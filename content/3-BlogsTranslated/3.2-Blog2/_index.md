---
title: "Blog 2"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Streamlining SAP Operations with CloudWatch MCP Server and Amazon Q CLI – Part 4

by Ferry Mulyadi and Vijay Sitaram | AUGUST 12, 2025 | in [Amazon CloudWatch](https://aws.amazon.com/blogs/awsforsap/category/management-tools/amazon-cloudwatch/), [Amazon Q](https://aws.amazon.com/blogs/awsforsap/category/amazon-q/), [AWS Systems Manager](https://aws.amazon.com/blogs/awsforsap/category/management-tools/aws-systems-manager/), [AWS Well-Architected](https://aws.amazon.com/blogs/awsforsap/category/aws-well-architected/), [How-To](https://aws.amazon.com/blogs/awsforsap/category/how-to/), [SAP on AWS](https://aws.amazon.com/blogs/awsforsap/category/sap/), [Technical How-to](https://aws.amazon.com/blogs/awsforsap/category/post-types/technical-how-to/), [Thought Leadership](https://aws.amazon.com/blogs/awsforsap/category/post-types/thought-leadership/) | [Permalink](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4/) | [Share](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4/#)

## Introduction

Optimizing SAP operations on AWS requires effective monitoring, troubleshooting, and maintenance capabilities. Continuing our previous discussions on Amazon CloudWatch Application Insights in [Part 1](https://aws.amazon.com/blogs/awsforsap/improve-your-sap-business-process-resilience-with-amazon-cloudwatch-application-insights/), how we used CloudWatch Application Insights to monitor SAP High Availability in [Part 2](https://aws.amazon.com/blogs/awsforsap/enhance-your-business-process-resilience-with-amazon-cloudwatch-application-insights-observability-for-sap-high-availability-part-2/), and [Amazon CloudWatch Model Context Protocol (MCP) Server](https://github.com/awslabs/mcp) combined with [Amazon Q for command line (Q CLI)](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html) in [Part 3](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-3/), this fourth article illustrates advanced real-world applications of these tools.

We will explore how this integration streamlines SAP maintenance planning and accelerates root cause analysis through practical use cases.

Through detailed examples, we will demonstrate how CloudWatch MCP Server and Q CLI work together to:

- Orchestrate planned maintenance events with minimal disruption.
- Accelerate issue diagnosis through intelligent log analysis.
- Provide context-aware recommendations for SAP operations.
- Reduce Mean Time To Resolution (MTTR) for SAP incidents.

---

## Use-case 3: Planned Maintenance Events for SAP

In this case, we will execute a planned maintenance event within an SAP application cluster. Workflow integration allows engineering teams to diagnose issues without disruptive context switching, by viewing CloudWatch metrics directly within the IDE while using Q CLI to generate precise remediation commands.

In this scenario, Q CLI generated the following task list:

1.  Prompt to check the status of the App Server.
2.  Run `aws ec2 describe-instances` to check the SAP cluster status.
3.  Run `ssm crm_mon` command to check the ASCS and ERS cluster.
4.  Run `ssm ps` command to get the process status of ASCS and ERS.
5.  Run `ssm sapcontrol` command to get the process list.
6.  Run `ssm` command to check `pacemaker.log`.
7.  Run `ssm crm` command to check cluster resource status, constraints, and configuration.
8.  Summarize that ERS is disabled due to cluster misconfiguration.
9.  After prompt to check CloudWatch logs:
    - Run `describe_log_groups`.
    - Run `analyze_log_groups`.
    - Run search `execute_log_insights_query` related to ASCS and ERS.
    - Run search `execute_log_insights_query` related to current ERS issues.
    - Summarize all log entries related to the ERS issue.
10. After prompt to start the SAP ERS service:
    - Run `ssm crm` command to check cluster resource status.
    - Run `ssm crm` command to enable the ERS resource (failed as expected).
    - Run `ssm` command to update resource configuration.
    - Run `ssm crm` command to re-enable the ERS resource (after fixing configuration).
    - Run `ssm sapcontrol` command to get the ASCS and ERS process list.
    - Summarize that the ERS service has been started successfully.

> **Important Note:** The prompts and responses below have been abbreviated. You can download the [Use-case 3 – Planned maintenance events for SAP](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/17/Use-case-3-Planned-maintenance-events-for-SAP.pdf) document to view the full prompts and responses from Q CLI.

**Prompt: Check the status of the SAP ASCS/ERS cluster for the SAPDemo SAP application without restarting any services.**

```text
> I'll check the status of the SAP ASCS/ERS cluster for the SAPDemo SAP application without restarting any services.
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ec2 describe-instances --filters "Name=tag:Name,Values=*SAP-FIS-CS*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,InstanceType,PrivateIpAddress]" --output table
 ⋮
 ↳ Purpose: Checking EC2 instances that are part of the SAP cluster

-------------------------------------------------------------------------------------------
|                                    DescribeInstances                                    |
+----------------------+-------------------------+----------+-------------+---------------+
|  i-064751ba1b4c8340f |  SAP-FIS-CS-sapclnode2  |  running |  c5a.xlarge |  10.0.155.180 |
|  i-0e08f883d5c3b3747 |  SAP-FIS-CS-sapclnode1  |  running |  c5a.xlarge |  10.0.134.80  |
+----------------------+-------------------------+----------+-------------+---------------+

 ⋮
 ● Completed in 1.666s
……

## Conclusion
The SAP ASCS/ERS cluster for the SAPDemo application is partially operational:
• The ASCS instance is running correctly on sapclnode2
• The ERS instance is intentionally disabled (target-role=Stopped in the configuration)
• The virtual IP for ERS is still active on sapclnode1
• Both cluster nodes are online and the cluster has quorum
This configuration appears to be intentional based on the cluster configuration, but it means that enqueue replication is not currently active. In a normal high-availability setup, both ASCS and ERS would be running to ensure enqueue table replication in case of a failover.
```

In this next scenario, we use CloudWatch MCP Server to investigate pacemaker cluster logs to determine the status of SAP ASCS. CloudWatch MCP Server invokes tools such as `describe_log_groups`, `analyze_log_groups`, and `execute_log_insights_query` to perform the analysis.

**Prompt: Check CloudWatch logs related to SAPDemo application for SAP ASCS ERS Pacemaker cluster events, alarms and issues.**

```text
> Check CloudWatch logs related to SAPDemo  application for SAP ASCS ERS Pacemaker cluster events, alarms and issues. only use cloudwatch mcp server for this analysis.
> I'll analyze the CloudWatch logs related to the SAPDemo application for SAP ASCS ERS Pacemaker cluster events, alarms, and issues using only the CloudWatch MCP server.
Let's start by identifying the relevant log groups for the SAPDemo application:
🛠️  Using tool: describe_log_groups (trusted) from mcp server awslabscloudwatch_mcp_server
 ⋮
 ● Running describe_log_groups with the param:
 ⋮  {
 ⋮    "arguments": {
 ⋮      "log_group_name_prefix": "PACEMAKER",
 ⋮      "region": "us-east-1"
 ⋮    },
 ⋮    "name": "describe_log_groups"
 ⋮  }

 ⋮
 ● Completed in 0.760s
……
### Analysis:
This log data shows that the ERS component (rsc_sap_ECD_ERS11) has been successfully started on sapclnode1. The cluster first cleared previous failure records, then migrated the IP resource (rsc_ip_ECD_ERS11) from node 2 to node 1, and finally started the ERS service on node 1.
The successful start is confirmed by the @rc-code=0 in the logs, indicating no errors. The monitoring operation is also successful, showing that the service is running properly.
This aligns with our previous findings where we had identified issues with the ERS component and cleared CloudWatch alarms for it. The logs now confirm that the ERS service has been successfully started and is
being monitored by the cluster, which explains why the service is now running despite previous alarms.
Would you like me to analyze any specific aspect of these logs in more detail or provide recommendations for ongoing monitoring of the ERS component?
```

**Prompt: start SAP ERS service on node sapclnode1.**

```text
> start SAP ERS services on node sapclnode1
> I'll start the SAP ERS services on node sapclnode1.
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ssm send-command --instance-ids i-0e08f883d5c3b3747 --document-name "AWS-RunShellScript" --parameters 'commands=["sudo crm resource status rsc_sap_ECD_ERS11"]' --output json
 ⋮
 ↳ Purpose: Checking the current status of the ERS resource

..
……

## Conclusion
The SAP ERS services have been successfully started on node sapclnode1. The enqueue replication is now active and functioning correctly, providing high availability for the SAP enqueue service. The cluster is now in a fully operational state with all resources running as expected.
```

---

## Use-case 4: Accelerate Root-Cause Analysis for SAP

In this scenario, we have an SAP cluster where ERS encountered an error due to misconfiguration. With the integration between Q CLI and CloudWatch MCP Server, traditional diagnostic challenges in complex SAP systems – where issues often propagate through multiple system layers – are eliminated. Q CLI generated the following task list:

1.  Prompt to check the status of the App Server.
2.  Run `aws ec2 describe-instances` to check the SAP cluster status.
3.  Run `ssm crm_mon` command to check the ASCS and ERS cluster.
4.  Run `ssm sapcontrol` command to get the process list on node 1 and node 2.
5.  Run `ssm crm` command to check cluster resource status and logs.
6.  Run `ssm` command to check `pacemaker.log`.
7.  Run `ssm crm` command to cleanup resources.
8.  Run `ssm crm_mon` command to check cluster status.
9.  Run `ssm sapcontrol` command to get the process list on node 2.
10. Summarize that the analysis results indicate the issue stems from a configuration problem.
11. After "Prompt to troubleshoot this problem":
    - Run `ssm` command to check SAP System Logs for ASCS and ERS on node 2.
    - Run `ssm` command to check SAP profiles of ASCS and ERS.
    - Run `ssm` command to check hostname configuration in hosts file on node 2.
    - Run `ssm` command to check IP addresses of node 1 and node 2.
    - Run `ssm` command to update hosts file on node 1 and node 2.
    - Run `ssm crm_mon` command to cleanup resources.
    - Run `ssm sapcontrol` command to get the process list on node 2.
    - Run `ssm sapcontrol` command to start ERS service, then check logs.
    - Run `ssm sapcontrol` command to start ASCS, then check logs.
    - Run `ssm crm_mon` command to stop cluster, then check status.
    - Run `ssm crm_mon` command to cleanup cluster resources.
    - Run `ssm crm_mon` command to start cluster, wait and check.
    - Run `ssm sapcontrol` command to get ASCS process list on node 1.
    - Run `ssm sapcontrol` command to start ASCS service, then check.
    - Summarize recommendations to fix the issue.
12. After "Prompt to start the SAP ERS service":
    - Run `ssm crm` command to stop ASCS and ERS cluster resources.
    - Run `ssm` command to verify and update hosts file on node 1 and node 2.
    - Run `ssm` command to check hostname resolution on node 1 and node 2 (first update failed, but retried with a different method).
    - Run `ssm` command to check SAP Profiles of ASCS and ERS.
    - Run `ssm crm_mon` command to cleanup cluster resources.
    - Run `ssm sapcontrol` command to start SAP ASCS and ERS resources, then verify.
    - Summarize that ASCS and ERS services have been started successfully.

> **Important Note:** The prompts and responses below have been abbreviated. You can download the [Use-case 4 – Accelerates Root-Cause Analysis for SAP](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/17/Use-case-4-Accelerates-Root-Cause-Analysis-for-SAP.pdf) document to view the full prompts and responses from Q CLI.

_[Figure 5. Failed status of SAP Central Services (ASCS/ERS)]_

**Prompt: What is the current status of SAP ASCS ERS cluster nodes for SAP application SAPDemo.**

```text
> what is the current status of SAP ASCS ERS cluster nodes for sap application SAPDemo.

> I'll check the current status of the SAP ASCS/ERS cluster nodes for the SAPDemo SAP application.
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ec2 describe-instances --filters "Name=tag:Name,Values=*SAP-FIS-CS*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,InstanceType,PrivateIpAddress]" --output table
 ⋮
 ↳ Purpose: Checking EC2 instances that are part of the SAP cluster

-------------------------------------------------------------------------------------------
|                                    DescribeInstances                                    |
+----------------------+-------------------------+----------+-------------+---------------+
|  i-064751ba1b4c8340f |  SAP-FIS-CS-sapclnode2  |  running |  c5a.xlarge |  10.0.155.180 |
|  i-0e08f883d5c3b3747 |  SAP-FIS-CS-sapclnode1  |  running |  c5a.xlarge |  10.0.134.80  |
+----------------------+-------------------------+----------+-------------+---------------+

 ⋮
 ● Completed in 1.815s
……..

## Current State Summary

The SAP ASCS/ERS cluster for SAPDemo is currently in a degraded state:
• The cluster infrastructure (corosync/pacemaker) is functioning correctly with quorum
• Both nodes are online and communicating
• The virtual IP addresses for both ASCS and ERS are properly assigned
• However, the actual SAP services (ASCS and ERS) are failing to start
• The application servers are showing YELLOW status, likely due to their dependency on the central services

This indicates a problem with the SAP services themselves rather than with the cluster infrastructure. The services may be failing to start due to configuration issues, file system problems, or other SAP-specific issues that need to be investigated further.
```

**Prompt: Help me troubleshoot this problem.**

```text
> help me troubleshoot this problem
> I'll help you troubleshoot the issues with the SAP ASCS/ERS cluster for the SAPDemo application. Based on the current state, both the ASCS and ERS services are failing to start properly, even though the cluster infrastructure itself is functioning correctly.
Let's take a systematic approach to troubleshooting:
## Step 1: Check SAP-specific error logs
First, let's check the SAP system logs to understand why the services are failing to start:
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ssm send-command --instance-ids i-064751ba1b4c8340f --document-name "AWS-RunShellScript" --parameters 'commands=["sudo su - ecdadm -c \"tail -50 /usr/sap/ECD/ASCS10/work/dev_ms\""]' --output json
 ⋮
 ↳ Purpose: Checking SAP system logs for ASCS on node 2
..
> Now let's check the ERS logs:
……..
> Based on my troubleshooting, I've identified the root cause of the issues with your SAP ASCS/ERS cluster and can provide you with a comprehensive solution:
# SAP ASCS/ERS Cluster Troubleshooting Report
## Root Cause Analysis
After thorough investigation, I've identified that the primary issue is related to hostname resolution. The error message MsSInit: NihostToAddr rc=(NIEHOST_UNKNOWN) in the ASCS logs clearly indicates that the
Message Server cannot resolve its hostname.
### Specific Issues Identified:

1. Hostname Resolution Problems:
   • The SAP profiles are configured to use the hostnames ecdascsnode and ecdersnode, but these hostnames were not properly defined in the /etc/hosts files on both nodes.
   • We've updated the hosts files, but the SAP services are still failing to start properly.

2. IP Address Configuration:
   • The virtual IPs (192.168.48.201 for ASCS and 192.168.48.202 for ERS) are not being properly managed by the cluster.
   • Both virtual IPs are currently assigned to node 1 (sapclnode1), which is causing conflicts.
3. SAP Profile Configuration:
   • The SAP profiles are using hostnames that don't match the actual hostnames of the cluster nodes.
   • The ERS service is trying to connect to the ASCS service but failing with "Connection refused" errors.

## Solution Steps

To fully resolve these issues, we need to take the following steps:

### 1. Fix the Hostname Resolution

We've already updated the /etc/hosts files on both nodes with the correct mappings, but we need to ensure that the SAP services can properly resolve the hostnames.

### 2. Update the SAP Profiles

The SAP profiles need to be updated to use the correct hostnames or IP addresses. This is a more involved task that requires careful editing of the SAP profiles.

### 3. Restart the Cluster Services in the Correct Order

The cluster services need to be restarted in the correct order to ensure proper operation:
1. Stop all SAP services
2. Stop the cluster
3. Fix the configuration
4. Start the cluster
5. Start the SAP services

### 4. Verify DNS Resolution

We need to ensure that DNS resolution is working correctly on both nodes.

## Recommended Actions

Here's a step-by-step plan to resolve the issues:

1. Stop the cluster resources:
  bash
   sudo crm resource stop rsc_sap_ECD_ASCS10
   sudo crm resource stop rsc_sap_ECD_ERS11

2. Verify the hosts file configuration on both nodes:
  bash
   cat /etc/hosts
  Ensure that both nodes have the following entries:
   127.0.0.1 localhost
   10.0.134.80 sapclnode1.ec2.internal sapclnode1
   10.0.155.180 sapclnode2.ec2.internal sapclnode2
   192.168.48.201 ecdascsnode
   192.168.48.202 ecdersnode

3. Test hostname resolution:
  bash
   ping -c 3 ecdascsnode
   ping -c 3 ecdersnode
   ping -c 3 sapclnode1
   ping -c 3 sapclnode2

4. Update the SAP profiles if necessary:
   • Review the SAP profiles to ensure they're using the correct hostnames
   • Consider updating the profiles to use IP addresses instead of hostnames if resolution continues to be an issue

5. Clean up the cluster resources:
  bash
   sudo crm resource cleanup rsc_sap_ECD_ASCS10
   sudo crm resource cleanup rsc_sap_ECD_ERS11

6. Start the resources in the correct order:
  bash
   sudo crm resource start rsc_sap_ECD_ASCS10
   # Wait for ASCS to fully start
   sudo crm resource start rsc_sap_ECD_ERS11

7. Verify the SAP services are running:
  bash
   sudo su - ecdadm -c "sapcontrol -nr 10 -function GetProcessList"
   sudo su - ecdadm -c "sapcontrol -nr 11 -function GetProcessList"


## Long-term Recommendations
1. Implement DNS Resolution: Consider implementing proper DNS resolution instead of relying on /etc/hosts files.
2. Review Cluster Configuration: Review the cluster configuration to ensure it's properly set up for SAP ASCS/ERS high availability.
3. Monitor the Cluster: Set up proper monitoring for the cluster to detect and alert on issues.
4. Document the Configuration: Document the cluster configuration and troubleshooting steps for future reference.
5. Regular Testing: Regularly test failover scenarios to ensure the cluster is working correctly.
By following these steps, you should be able to resolve the issues with your SAP ASCS/ERS cluster and ensure it's properly configured for high availability.
```

**Prompt: Apply the above solution to fix the problem.**

```text
> apply the above solution to fix the problem
> I'll apply the solution to fix the SAP ASCS/ERS cluster issues. Let's follow the step-by-step plan:
## Step 1: Stop the cluster resources
> I need to use SSM to execute these commands on the cluster nodes:
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ssm send-command --instance-ids i-0e08f883d5c3b3747 --document-name "AWS-RunShellScript" --parameters 'commands=["sudo crm resource stop rsc_sap_ECD_ASCS10 && sudo crm resource stop rsc_sap_ECD_ERS11"]' --output json
 ⋮
 ↳ Purpose: Stopping the cluster resources using SSM
..
> ## Step 2: Verify the hosts file configuration on both nodes
……
> Great! The solution has been successfully applied and the SAP ASCS/ERS cluster is now working correctly. Here's a summary of what we did:

## Summary of the Fix
1. Fixed Hostname Resolution:
   • Updated the /etc/hosts files on both nodes with the correct mappings for all hostnames.
   • Verified that hostname resolution was working correctly by pinging the virtual hostnames.
2. Cleaned Up and Restarted Cluster Resources:
   • Stopped the cluster resources to ensure a clean state.
   • Cleaned up any failed resource actions.
   • Started the resources in the correct order: ASCS first, then ERS.
3. Verified the Services:
   • Confirmed that both the ASCS and ERS services are now running correctly.
   • The system instance list shows all components are GREEN (running).
## Current Status
• **Cluster Status**: All resources are running correctly.
• **ASCS Status**: The Message Server and Enqueue Server are both running on node 2 (sapclnode2).
• **ERS Status**: The Enqueue Replication Server is running on node 1 (sapclnode1).
• **Virtual IPs**: The virtual IPs are correctly assigned to the appropriate nodes.
## Recommendations for Long-term Stability
1. Regular Monitoring: Set up monitoring for the cluster to detect any issues early.
2. Failover Testing: Regularly test failover scenarios to ensure the cluster works correctly.
3. Documentation: Document the cluster configuration and troubleshooting steps for future reference.
4. DNS Resolution: Consider implementing proper DNS resolution instead of relying on /etc/hosts files.
The SAP ASCS/ERS cluster for the SAPDemo application is now functioning correctly. The issue was primarily related to hostname resolution, which we've fixed by properly configuring the hosts files on both nodes.
```

_[Figure 6. Final healthy status of SAP Central Services (ASCS/ERS) cluster]_

---

## Cost Considerations

To deploy this solution, you need to consider the following factors:

- **Amazon Q CLI Cost** is shown in [[this link]](https://aws.amazon.com/q/developer/pricing/#topic-0). We recommend using at least the Pro tier with 1,000 agentic requests per month ($19/month/user).
- **Amazon CloudWatch Cost** is shown in [[this link]](https://aws.amazon.com/cloudwatch/pricing/). In this experiment, we assume that the Free tier is sufficient.
- **AWS Systems Manager Cost** is shown in [[this link]](https://aws.amazon.com/systems-manager/pricing/). For the HA experiment, assuming there are a minimum of 6 nodes to manage:
  $0.01137 \text{ USD} \times 6 \text{ nodes} \times 8 \text{ hours} = 3.27456 \text{ USD}$
- **No additional cost** when using Amazon CloudWatch MCP Server.
- **Estimated HA setup cost for SAP NetWeaver** (calculated in us-east-1 region):
  - 2 × m5.large = 148.16 USD (ASCS and ERS)
  - 2 × m5.xlarge = 280.32 USD (2 App Servers)
  - 2 × r5.2xlarge = 756.32 USD (SAP HANA Primary and Secondary)

**Total Estimated Cost** = 19 USD + 3.27456 USD + 39.4933 USD ≈ **61.76786 USD** for a one-day experiment.

---

## Conclusion

CloudWatch MCP Server delivers unified observability across the entire SAP landscape within the customer environment. It consolidates SAP application logs, SAP HANA metrics, and AWS EC2 performance data. The system automatically correlates metrics across SAP components. Q CLI complements this capability by enabling natural language interaction with this rich dataset, allowing teams to easily investigate session response times, compare HANA memory consumption patterns, or identify waiting SAP processes during performance incidents.

CloudWatch MCP Server helps troubleshoot complex SAP issues by identifying anomalies through pattern analysis in system logs and transaction errors. It links infrastructure alarms to application behavior. Additionally, it provides historical context from similar past incidents. Q CLI enhances this capability by interpreting SAP error codes, generating diagnostic commands based on observed patterns, suggesting root causes, and recommending relevant SAP transaction codes for deeper investigation.

This integration bridges the knowledge gap between SAP BASIS administrators and AWS infrastructure teams by creating a shared operational view of the SAP system and translating between SAP terminology and AWS concepts. This combination reduces mean time to resolution by eliminating context switching, providing intelligent analysis of SAP-specific patterns, and enabling natural language interaction with monitoring data — leading to faster troubleshooting, higher reliability, and more efficient operations for critical SAP workloads on AWS.

As a next step, try applying this solution; you can achieve at least a 6x gain in productivity. Stay tuned for upcoming innovations regarding [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [AWS Model Context Protocol Servers](https://github.com/awslabs/mcp), and [Amazon Q for command line](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html).

---

## Join the SAP on AWS Discussion

In addition to your customer account team and AWS Support channels, we recently launched [re:Post](https://repost.aws/) – A reimagined Q&A experience for the AWS community. Our AWS for SAP Solution Architecture team regularly monitors the AWS for SAP topic to discuss and answer questions to support customers and partners. If your question is not related to technical support, join the discussion at re:Post and contribute to the community knowledge base.

---

## Acknowledgments

I would like to thank the following team members for their contributions: **Sreenath Middhi** and **Adam Hill**.

**TAGS:** [#SAP Netweaver](https://aws.amazon.com/blogs/awsforsap/tag/sap-netweaver/), [AWS Systems Manager](https://aws.amazon.com/blogs/awsforsap/tag/aws-systems-manager/), [CloudWatch](https://aws.amazon.com/blogs/awsforsap/tag/cloudwatch/), [SAP](https://aws.amazon.com/blogs/awsforsap/tag/sap/), [SAP High Availability](https://aws.amazon.com/blogs/awsforsap/tag/sap-high-availability/)
