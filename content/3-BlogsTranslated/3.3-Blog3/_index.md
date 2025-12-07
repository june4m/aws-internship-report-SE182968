---
title: "Blog 3"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Deploy Okta as a Custom Identity Provider for AWS Transfer Family

**By:** Yongki Kim and Tamelly Lim
**Date:** AUGUST 11, 2025
**Categories:** [AWS Transfer Family](https://aws.amazon.com/blogs/storage/category/migration/aws-transfer-family/), [Storage](https://aws.amazon.com/blogs/storage/category/storage/), [Technical How-to](https://aws.amazon.com/blogs/storage/category/post-types/technical-how-to/), [Permalink](https://aws.amazon.com/blogs/storage/deploy-okta-as-a-custom-identity-provider-for-aws-transfer-family/) | [Comments](https://aws.amazon.com/blogs/storage/deploy-okta-as-a-custom-identity-provider-for-aws-transfer-family/#Comments) | [Share](https://aws.amazon.com/vi/blogs/storage/deploy-okta-as-a-custom-identity-provider-for-aws-transfer-family/#)

---

As projects and the number of users within an organization grow, managing granular access to data resources while adhering to security regulations becomes critically important. Multi-user environments require appropriate access delegation to ensure data privacy and security.

However, managing access to shared files can be complex and time-consuming, especially as the user base expands and is distributed across multiple identity providers. Organizations need a scalable solution to enforce granular access controls, define which users can access sensitive data, and ensure appropriate access levels for shared files — thereby reducing the risk of unauthorized access.

[AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/) is a fully managed service that enables you to transfer files into and out of AWS storage services like [Amazon S3](https://aws.amazon.com/s3/) and [Amazon EFS](https://aws.amazon.com/efs/) without managing any SFTP servers. The service also supports integration with identity providers like Okta, enabling end-user management for file exchange through an AWS managed SFTP server.

In a previous post on the Storage blog, we introduced the [AWS Transfer Family Custom IdP solution](https://docs.aws.amazon.com/transfer/latest/userguide/custom-idp-intro.html), aimed at providing reusable components and examples to simplify setting up Transfer Family for common scenarios, as well as how to use it with Microsoft Active Directory authentication.

In this post, we will guide you through deploying a custom Identity Provider (IdP) solution with Okta, allowing organizations to use Okta authentication for their Transfer Family servers. You can leverage built-in features of the Custom IdP solution, such as per-user permissions.

## Solution Architecture

The proposed architecture provides a reusable foundation and demonstrates best practices for deploying a custom IdP while allowing granular control over session configuration on a per-user basis, as illustrated in the following figure. This modular architecture promotes code reusability, ease of maintenance, and scalability. This makes it easy to adapt to changing requirements or integrate with other systems.

By default, this architecture provides the following features:

- A standard schema in [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) to store metadata about identity providers, related settings, user credentials, and user session configurations such as HomeDirectoryDetails, Role, and Policy.
- Support for multiple IdPs connecting to a single Transfer Family server.
- Support for multiple Transfer Family servers using the same deployment of this solution.
- Support for the following identity providers: Okta, LDAP (including Microsoft Active Directory), Entra ID, Argon2, Cognito, Public and Private Key, and [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/).
- Per-user IP allow-listing.
- Standardized best-practice logging patterns, with configurable log levels and tracing support.
- Capability to deploy [Amazon API Gateway](https://aws.amazon.com/api-gateway/) for advanced use cases.

![Figure 1: Custom Identity Provider Solution Architecture](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcMtdFqbk3vX5JgjVJsbTTNRCgwrKwii4saqjbeOwqwF2RvFKKEfKzii2yzOPXchQLueU1hNCL9_sfwV32TDCkdKHPEO36Mv7W54Z1einlJfru53LQZj1n34HBOIHslRvK8GCnf-A?key=R4ysRt4XoNv_sIjTNMv9AA)
_Figure 1: Custom Identity Provider Solution Architecture_

In this example, we will walk through the steps to deploy and configure Okta as a custom IdP for AWS Transfer Family.

## Prerequisites

The following prerequisites are required to deploy this solution:

1.  An AWS account with access to [AWS CloudShell](https://aws.amazon.com/cloudshell/).
2.  Appropriate permissions in [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) to provision the following resource types:
    - Create IAM roles
    - Create DynamoDB tables
    - Create [AWS Lambda](https://aws.amazon.com/lambda/) functions
    - Create API Gateway (if enabled)
3.  An [Okta organization](https://developer.okta.com/docs/concepts/okta-organizations/).

## Deployment Guide

In this solution, we illustrate a use case where an organization requires Okta authentication with TOTP (time-based one-time password) based MFA for their Transfer Family server. The organization needs the flexibility to assign each user a unique Transfer Family session configuration, such as Role, HomeDirectoryDetails, and PosixProfile.

In this walkthrough, we will cover the following steps:

1.  **Configure Okta:**
    - Create a new user and group in Okta
    - Configure MFA
2.  **Deploy the toolkit:**
    - Set up the custom IdP
    - Provision all necessary resources for this solution
3.  **Define the IdP and user:**
    - Configure IdP settings
    - Manage user records in the IdP
4.  **Test the IdP:**
    - Verify the functionality of the deployed solution

---

### 1. Configure Okta

In this section, you will create a new user and group in your Okta organization and configure MFA.

![Figure 2: Okta Admin Console for Okta Identity Engine](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcqhN_SdSKgjy-6vDoFGHjqOutlhkzMYbtSSRRgJ69frblIQWwqDNsqyAgsoOg9ZCp6AOl72Go58JM4J931fcpMyyi8SJdx1bm0KIeAFFGUb-KlEZuJbjA2wjKmBsHZMIFVhX-pgw?key=R4ysRt4XoNv_sIjTNMv9AA)
_Figure 2: Okta Admin Console for Okta Identity Engine_

_Note: These instructions apply specifically to Okta Identity Engine. If you are using Okta Classic, the interface and some settings will differ._

1.  Log in to your Okta organization as an administrator using the admin console (e.g., `dev-xxxx-admin.okta.com/admin`).

    ![Figure 3: Adding a user in Okta Admin Console](https://lh7-rt.googleusercontent.com/docsz/AD_4nXczttsk2zYAMoOHHQt-S4NNh0F2eoWZbqRWPi2YZ1SlJvKl8vadYMwy10b48a6VpQ6XmrtrdquNgO1-cbgkTDIPeG-Kbh_-aBvRk4wSn6hkxDiZDCNzcOZMBabPlWoabajGgODjLQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 3: Adding a user in Okta Admin Console_

2.  Navigate to **Directory** in the sidebar, select **People**, and create an Okta user with the username `john@demosftp.com`. Set a desired password for this user.
3.  In the same **Directory** tab, select **Groups**, click **Add group**, and create an Okta group named `sftp`.
4.  Select the newly created group, then choose **Assign people** and add `john@demosftp.com` by selecting the plus sign (+) next to their name.

    ![Figure 4: Adding user to sftp group in Okta Admin Console](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdMZmhaHEzE8J4TtowCnI5QjU624nDnuHQFB3BzQiRuaJlrQH-ExvQQSNeveenPIee8CN7GS3zPwtV1OzxqZekMHHBHeX5M0uSWhG9MIT28_dSMyiWIJ9sceUyM0gOBr69eBAMrnQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 4: Adding user to sftp group in Okta Admin Console_

5.  Navigate to **Security** in the sidebar and select **Authenticators**. In the **Setup** tab, choose **Add authenticator** and add the TOTP authenticator used by your organization. _Note: Only TOTP-based authenticators are supported with Transfer Family; push-based authenticators are not currently supported._ In this case, you add **Google Authenticator** as an authenticator.
6.  To enforce Multi-Factor Authentication (MFA) with Google Authenticator for SFTP users, navigate to the **Enrollment** tab and select **Add a policy**. Enter the following information:
    - **Policy name:** SFTP Users MFA
    - **Assign to groups:** SFTP
    - **Authenticators:** Switch Google Authenticator to **Required**.
    - Select **Create policy**. In the appearing _Add Rule_ dialog, set the **Rule name** to `SFTP Users MFA Enrollment` and select **Create rule**.
7.  To enforce MFA for user SFTP sessions, navigate to **Security** in the sidebar and go to **Global Session Policy**. Select **Add policy** and set the **Policy name** to `sftp policy`. In the **Assign to groups** section, add `sftp`, then select **Create policy** and **Add rule**.
8.  In the appearing _Add Rule_ dialog, set the **Rule name** to `sftp rule`. Set **Multifactor authentication (MFA)** to **Required**, then select **Create rule**.
9.  To complete the Google Authenticator setup for the Okta user, log in to the Okta domain as `john@demosftp.com`. Open a private browsing window and navigate to your Okta organization URL (e.g., `dev-xxxx.okta.com`). At the login screen, enter `john@demosftp.com` and the password created in Step 1.2. When prompted, select **Set up** under Google Authenticator and follow the instructions to complete the setup. You only need to configure the Google Authenticator method. Once done, you can skip other MFA methods and select **Continue** to proceed with login.

    ![Figure 5: Setting up MFA for an Okta user](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcteOeCl8hnVbCX1LPQm78rs9F1-FZ0D7hBKxerkzuboqfxl-TjyNx6aQfqUWcY9yInTDGPDTrHLvrFwRLhjzWJ42FkrwgD0ezTiZYKKSH2yW622MRv5xNmHSc8maT4M5VD4eh1Vg?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 5: Setting up MFA for an Okta user_

_Note: You have successfully added an Okta user to the newly created group and set up MFA for authentication. Before proceeding with the solution, the newly created Okta user must log in to the Okta domain to authenticate and set a new password._

### 2. Deploy the Toolkit

Log in to the AWS account where you want to deploy the solution, switch to the desired AWS Region to run Transfer Family, and launch a CloudShell session.

1.  Run the following command to clone the solution repository into your CloudShell environment:

    ```bash
    cd ~
    git clone https://github.com/aws-samples/toolkit-for-aws-transfer-family.git
    ```

2.  Run the following command to execute the build script, which downloads all dependencies and creates archives for the Lambda layer and function used in the solution:

    ```bash
    cd ~/toolkit-for-aws-transfer-family/solutions/custom-idp
    ./build.sh
    ```

    _Monitor the execution process and verify that the script completes successfully._

3.  Deploy the stack using SAM:

    ```bash
    sam deploy --guided --template \
    ./examples/okta/transfer_okta_customIdp_server_template.yaml --\
    capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND\
    CAPABILITY_NAMED_IAM --disable-rollback
    ```

4.  When prompted, provide the following information as an example:

    - **Stack Name:** `transferidp`
    - **Parameter UserNameDelimiter:** `@@`
    - **Parameter LogLevel:** `INFO`
    - **Parameter ProvisionApi:** `false`
    - **Parameter EnableTracing:** `false`
    - **Parameter UsersTableName:** `empty`
    - **Parameter IdentityProvidersTableName:** `empty`
    - For other options, enter `Y` to keep the default values.

    ![Figure 7: Deploying AWS CloudFormation for custom IdP](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcgPEP4HDdSgoQsmgz1g8hobs2bE_e28nM9nibvzPK-CoEUKYLp1rGjzbIRvmDKlcOMIrwRbDhAjoi0BaVK_WunZAFyHEz8XkBG5Hecnn1PtXsfYytt59_hfPHd1uQ6AKrQaFFWkQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 7: Deploying AWS CloudFormation for custom IdP_

5.  When the deployment completes successfully, copy/paste the **AWS CloudFormation outputs** from the deployed stack to a text editor and save them for later use.

    **The output information includes:**

    - **DDBIdentityProvidersTableName:** DynamoDB Identity Providers table name
    - **DDBUsersTableName:** DynamoDB Users table name
    - **SFTPServerS3Bucket:** S3 bucket name created as backend storage for the Transfer Family server
    - **TransferUserRole:** Amazon Resource Name (ARN) of the IAM Role created to map to users
    - **TransferServerEndpoint:** Endpoint of the Transfer Family server

### 3. Define the IdP

Once the Transfer Family custom IdP solution is deployed, you need to define one or more IdPs in the DynamoDB `identity_providers` table. To use Okta as an IdP, you need to add Okta attributes to this table.

| Table name                                      | Comment                                                                                                            |
| :---------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- |
| `[StackName]-CustomIdP-XXXX_identity_providers` | DynamoDB table containing details of each Transfer Family custom IdP (e.g., IdP name, server URL, parameters).     |
| `[StackName]-CustomIdP-XXXX_users`              | DynamoDB table containing details of each Transfer Family user (e.g., IdP used, IAM role, logical directory list). |

1.  In the AWS Management Console, navigate to DynamoDB Item Explorer and select the `identity_providers` table from the table selection menu. Select **Create item**.

    ![Figure 8: DynamoDB table for custom IdP](https://lh7-rt.googleusercontent.com/docsz/AD_4nXefWm0xJf3OORTJhxUN4n27GQ1-vp4AqGv2zVfAN9yQs4MrEnWYgYJLOPtWtREwYzECBSGy33MHulcsUUiJG4lLo0BxqAY37vZdMbpTtgHlZkPzITgwEjQFWXlp60ZOADFAY3M7eg?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 8: DynamoDB table for custom IdP_

2.  In the Create item screen, select **JSON view**, then copy and paste the following record:

    ```json
    {
      "provider": {
        "S": "okta"
      },
      "config": {
        "M": {
          "mfa": {
            "BOOL": true
          },
          "okta_domain": {
            "S": "[OKTA DOMAIN]"
          }
        }
      },
      "module": {
        "S": "okta"
      }
    }
    ```

3.  Select the **Form** button to switch back to form view. Replace the placeholder value in the `okta_domain` field with your Okta organization domain (e.g., `dev-xxxxx.okta.com`).

    ![Figure 9: Creating IdP record in DynamoDB](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfLnVcbq-JdapL1tmqclvLT-7uJw81PsHoxklo_pxnxSTI_RaO9xhcvNKd6kgY-4XEophitLg_mRixfqw4lkULUu0qi26ydEJtzEjrETsL88BXgHacRRx0Cg7_vpJaEvZSLNaj4g?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 9: Creating IdP record in DynamoDB_

    - **provider:** IdP name (e.g., `okta`).
    - **module:** IdP module name to use (`okta`).
    - **config:** Contains configuration details. `mfa` set to `true` means users must enter password + TOTP code.

4.  Select **Create item**.

### 4. Define a User

Next, you need to define a user and map that username to the newly created Okta IdP.

1.  In the console, navigate to DynamoDB Item Explorer, select the `users` table (e.g., `[StackName]-CustomIdP-XXXX_users`). Select the **Create item** button.

    ![Figure 10: DynamoDB table for users](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdmnXmED6yoRn2CShfnghH5buN4o29DkJy07xWwfr5DGjgOhr28mInBIZ0aO40EbPwjp7Qs7JmxmGHGUmY9CSRBwFyyALZVKuijjaeD-fZSZaKPh2Mjk2pbymnZssxKG67zcWMX?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 10: DynamoDB table for users_

2.  In the Create item screen, select **JSON view**, then copy and paste the following record:

    ```json
    {
      "user": {
        "S": "john@demosftp.com"
      },
      "identity_provider_key": {
        "S": "okta"
      },
      "config": {
        "M": {
          "HomeDirectoryDetails": {
            "L": [
              {
                "M": {
                  "Entry": {
                    "S": "/"
                  },
                  "Target": {
                    "S": "/[SFTPServerS3Bucket]/${transfer:UserName}"
                  }
                }
              }
            ]
          },
          "HomeDirectoryType": {
            "S": "LOGICAL"
          },
          "Role": {
            "S": "[TransferUserRole]"
          }
        }
      }
    }
    ```

3.  Select the **Form** button. Expand the `config` section and replace `[SFTPServerS3Bucket]` and `[TransferUserRole]` with the corresponding CloudFormation output values from Step 2.5.

    ![Figure 11: Creating user record in DynamoDB](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcu2nZuDcClUIyqF-hLHdf0KxK9L1B4L0GdD4yLJdyJRf0mnA_DHJzAvsnDAuzWoazdZImn6EtVmH_TYx5Q0fHyH6nZFLDBeMar1Smpkic1Q6CeQ70BVrgpGIZMy2xKUd5rFLbpTQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 11: Creating user record in DynamoDB_

4.  Select **Create item**.

### 5. Test the IdP

1.  Navigate to the AWS Transfer Family console and select the deployed server.
2.  In the top right corner, select **Actions**, then select **Test** from the dropdown menu.

    ![Figure 12: Testing SFTP access](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfmdHjecbWuyso3osbbhdURE2ha7T3NxPfEHvH9Q6Rj8rgNR3EM1JZRMKZTsstL1-vGpl3f9OVUYLhMaSj5OWzC7isRIW7ACOKR3S9rnoOVFAhx3ObjztsqinxoaaCPBL3G7f6moA?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 12: Testing SFTP access_

3.  Enter `john@demosftp.com` in the **username** field. For the **password** field, enter your password concatenated with the TOTP code from Google Authenticator (Example: if your password is `TestP@55w0rd` and the MFA code is `201483`, enter `TestP@55w0rd201483`). **Source IP** can be any value. Select **Test**.

    ![Figure 13: Testing authentication with MFA code](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd7QHH1_ceUh393j-jAbXVGf8Tq60jlvVbSH6PppKhLoDUuzxA1rJpvJYkVAy5CpMCVd_56jc6WVIHYCpAi3MUgAOtnx2p61AqLcnBTkpzdFWmU30i3gl8oBrWPrfkjqp7yw7_seg?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 13: Testing authentication with MFA code_

4.  If successful, an HTTP 200 response will be returned. You can then connect to the Transfer Family server via an SFTP client using the `TransferServerEndpoint` from the CloudFormation output.

    ![Figure 14: Testing with SFTP command](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdRynxVJXo9ivmL07BsOj3OIwVwKgGX1txl1Xs9x43MJaLvNZfhrVkcGweo_q_icGDn98WW4l0Z4zhU4C-f9lBUEvp1LqE_W_ZeoyfRx11BAge4ABaYytt_pPMT7H9AtqDuU9wPiw?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Figure 14: Testing with SFTP command (note using "user@domain" format in quotes if needed)_

## Cleanup

To delete the created resources:

1.  Run the command: `sam delete --stack-name <stack-name>`
2.  Manually delete the two DynamoDB tables (`identity_providers` and `users`) in the console.
3.  Manually delete the AWS Lambda Layer (`handler-layer`) in the console.

## Conclusion

Standardizing deployment processes across multiple IdPs improves interoperability and enhances management efficiency. In this post, we guided you on how to deploy Okta as a custom IdP for AWS Transfer Family, integrating MFA for enhanced security. You can explore more about the solution and other modules (such as Active Directory, Entra ID, Cognito) in the [GitHub toolkit repository](https://github.com/aws-samples/toolkit-for-aws-transfer-family).

**TAGS:** AWS Storage Blog, AWS Transfer Family

---

**About the Authors:**

**Yongki Kim**
Yongki is an APJ Storage Specialist Solutions Architect with deep expertise in the comprehensive AWS storage portfolio. He is passionate about working with customers to solve architectural challenges.

**Tamelly Lim**
Tamelly is a Specialist Solutions Architect based in Singapore, helping customers design and implement AWS-based solutions. She began her cloud journey in 2022.
