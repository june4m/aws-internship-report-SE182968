---
title: "Blog 3"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Triển khai Okta làm nhà cung cấp danh tính tùy chỉnh cho AWS Transfer Family

**Bởi:** Yongki Kim và Tamelly Lim
**Ngày:** 11 THÁNG 8, 2025
**Chuyên mục:** [AWS Transfer Family](https://aws.amazon.com/blogs/storage/category/migration/aws-transfer-family/), [Storage](https://aws.amazon.com/blogs/storage/category/storage/), [Technical How-to](https://aws.amazon.com/blogs/storage/category/post-types/technical-how-to/), [Permalink](https://aws.amazon.com/blogs/storage/deploy-okta-as-a-custom-identity-provider-for-aws-transfer-family/) | [Comments](https://aws.amazon.com/blogs/storage/deploy-okta-as-a-custom-identity-provider-for-aws-transfer-family/#Comments) | [Share](https://aws.amazon.com/vi/blogs/storage/deploy-okta-as-a-custom-identity-provider-for-aws-transfer-family/#)

---

Khi các dự án và số lượng người dùng trong một tổ chức ngày càng tăng, việc quản lý quyền truy cập chi tiết cho tài nguyên dữ liệu đồng thời tuân thủ các quy định bảo mật trở nên vô cùng quan trọng. Môi trường nhiều người dùng yêu cầu phải phân quyền truy cập phù hợp để đảm bảo tính riêng tư và an toàn dữ liệu.

Tuy nhiên, quản lý quyền truy cập cho tài liệu chia sẻ có thể phức tạp và tốn thời gian, đặc biệt khi số lượng người dùng ngày càng nhiều và phân tán trên nhiều nhà cung cấp danh tính (identity providers) khác nhau. Các tổ chức cần một giải pháp có khả năng mở rộng để thực thi kiểm soát truy cập chi tiết, xác định người dùng nào có thể truy cập dữ liệu nhạy cảm, và đảm bảo mức độ truy cập phù hợp đối với tệp chia sẻ — từ đó giảm rủi ro truy cập trái phép.

[AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/) là dịch vụ được quản lý đầy đủ cho phép bạn truyền tải tệp vào và ra khỏi các dịch vụ lưu trữ AWS như [Amazon S3](https://aws.amazon.com/s3/) và [Amazon EFS](https://aws.amazon.com/efs/) mà không cần quản lý bất kỳ máy chủ SFTP nào. Dịch vụ này cũng hỗ trợ tích hợp các nhà cung cấp danh tính như Okta, giúp quản lý người dùng cuối có thể trao đổi tệp thông qua AWS managed SFTP server.

Trong một bài viết trước trên blog Storage, chúng tôi đã giới thiệu [AWS Transfer Family Custom IdP solution](https://docs.aws.amazon.com/transfer/latest/userguide/custom-idp-intro.html), nhằm cung cấp các thành phần có thể tái sử dụng và ví dụ để đơn giản hóa việc thiết lập Transfer Family cho các kịch bản phổ biến, cũng như cách sử dụng nó với Microsoft Active Directory authentication.

Trong bài viết này, chúng tôi sẽ hướng dẫn triển khai một giải pháp Identity Provider (IdP) tùy chỉnh với Okta, cho phép các tổ chức sử dụng xác thực Okta cho máy chủ Transfer Family của họ. Bạn có thể sử dụng các tính năng tích hợp sẵn của giải pháp Custom IdP, chẳng hạn như phân quyền theo từng người dùng (per-user permissions).

## Kiến trúc giải pháp

Kiến trúc được đề xuất cung cấp một nền tảng có thể tái sử dụng và thể hiện các best practices trong việc triển khai IdP tùy chỉnh, đồng thời cho phép kiểm soát chi tiết cấu hình phiên theo từng người dùng, như minh họa trong hình sau. Kiến trúc dạng mô-đun này thúc đẩy khả năng tái sử dụng mã, dễ bảo trì và khả năng mở rộng. Điều này giúp dễ dàng thích ứng với yêu cầu thay đổi hoặc tích hợp với các hệ thống khác.

Theo chuẩn, kiến trúc này cung cấp các tính năng sau:

- Một sơ đồ chuẩn trong [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) để lưu trữ metadata về các nhà cung cấp danh tính, cài đặt liên quan, thông tin đăng nhập của người dùng, và cấu hình phiên của người dùng như HomeDirectoryDetails, Role, và Policy.
- Hỗ trợ nhiều IdP kết nối đến một máy chủ Transfer Family.
- Hỗ trợ nhiều máy chủ Transfer Family sử dụng cùng một lần triển khai giải pháp này.
- Hỗ trợ các nhà cung cấp danh tính sau: Okta, LDAP (bao gồm Microsoft Active Directory), Entra ID, Argon2, Cognito, Public and Private Key, và [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/).
- Danh sách cho phép IP theo từng người dùng (Per user IP allow-listing).
- Mẫu logging theo best practice đã chuẩn hóa, với khả năng cấu hình log-level và hỗ trợ tracing.
- Khả năng triển khai [Amazon API Gateway](https://aws.amazon.com/api-gateway/) cho các trường hợp nâng cao.

![Hình 1: Kiến trúc giải pháp nhà cung cấp danh tính (Identity Provider) tùy chỉnh](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcMtdFqbk3vX5JgjVJsbTTNRCgwrKwii4saqjbeOwqwF2RvFKKEfKzii2yzOPXchQLueU1hNCL9_sfwV32TDCkdKHPEO36Mv7W54Z1einlJfru53LQZj1n34HBOIHslRvK8GCnf-A?key=R4ysRt4XoNv_sIjTNMv9AA)
_Hình 1: Kiến trúc giải pháp nhà cung cấp danh tính (Identity Provider) tùy chỉnh_

Trong ví dụ này, chúng ta sẽ đi qua các bước triển khai và cấu hình Okta làm nhà cung cấp danh tính tùy chỉnh (custom IdP) cho AWS Transfer Family.

## Điều kiện tiên quyết

Các điều kiện tiên quyết sau là cần thiết để triển khai giải pháp này:

1.  Một tài khoản AWS có quyền truy cập vào [AWS CloudShell](https://aws.amazon.com/cloudshell/).
2.  Quyền phù hợp trong [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) để cung cấp các loại tài nguyên sau:
    - Tạo IAM roles
    - Tạo DynamoDB tables
    - Tạo [AWS Lambda](https://aws.amazon.com/lambda/) functions
    - Tạo API Gateway (nếu được bật)
3.  Một [Okta organization](https://developer.okta.com/docs/concepts/okta-organizations/).

## Hướng dẫn triển khai

Trong giải pháp này, chúng tôi minh họa một trường hợp sử dụng khi một tổ chức cần xác thực Okta kèm MFA dựa trên TOTP (time-based one-time password) cho máy chủ Transfer Family của họ. Tổ chức cần sự linh hoạt để gán cho mỗi người dùng một cấu hình phiên Transfer Family riêng, chẳng hạn như Role, HomeDirectoryDetails và PosixProfile.

Trong walkthrough này, chúng tôi sẽ bao gồm các bước sau:

1.  **Thiết lập cấu hình Okta:**
    - Tạo người dùng và nhóm mới trong Okta
    - Cấu hình MFA
2.  **Triển khai bộ công cụ (Deploying the toolkit):**
    - Thiết lập custom IdP
    - Cung cấp tất cả tài nguyên cần thiết cho giải pháp này
3.  **Định nghĩa IdP và người dùng (Defining the IdP and user):**
    - Cấu hình cài đặt IdP
    - Quản lý bản ghi người dùng trong IdP
4.  **Kiểm thử IdP (Testing the IdP):**
    - Xác minh chức năng của giải pháp đã triển khai

---

### 1. Thiết lập cấu hình Okta

Trong phần này, bạn sẽ tạo một người dùng và một nhóm mới trong tổ chức Okta, đồng thời cấu hình MFA.

![Hình 2: Bảng điều khiển quản trị Okta cho Okta Identity Engine](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcqhN_SdSKgjy-6vDoFGHjqOutlhkzMYbtSSRRgJ69frblIQWwqDNsqyAgsoOg9ZCp6AOl72Go58JM4J931fcpMyyi8SJdx1bm0KIeAFFGUb-KlEZuJbjA2wjKmBsHZMIFVhX-pgw?key=R4ysRt4XoNv_sIjTNMv9AA)
_Hình 2: Bảng điều khiển quản trị Okta cho Okta Identity Engine_

_Lưu ý: Các hướng dẫn này áp dụng riêng cho Okta Identity Engine. Nếu bạn đang sử dụng Okta Classic, thì giao diện và một số thiết lập sẽ khác biệt._

1.  Đăng nhập vào tổ chức Okta với vai trò quản trị viên bằng bảng điều khiển quản trị (ví dụ: `dev-xxxx-admin.okta.com/admin`).

    ![Hình 3: Thêm người dùng trong bảng điều khiển quản trị Okta](https://lh7-rt.googleusercontent.com/docsz/AD_4nXczttsk2zYAMoOHHQt-S4NNh0F2eoWZbqRWPi2YZ1SlJvKl8vadYMwy10b48a6VpQ6XmrtrdquNgO1-cbgkTDIPeG-Kbh_-aBvRk4wSn6hkxDiZDCNzcOZMBabPlWoabajGgODjLQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 3: Thêm người dùng trong bảng điều khiển quản trị Okta_

2.  Điều hướng đến **Directory** ở thanh bên, chọn **People** và tạo một người dùng Okta với tên đăng nhập `john@demosftp.com`. Đặt mật khẩu mong muốn cho người dùng này.
3.  Trong cùng tab **Directory**, chọn **Groups**, nhấp **Add group**, và tạo một nhóm Okta với tên `sftp`.
4.  Chọn nhóm vừa tạo, sau đó chọn **Assign people** và thêm `john@demosftp.com` bằng cách chọn dấu cộng (+) bên cạnh tên của họ.

    ![Hình 4: Thêm người dùng vào nhóm sftp trong bảng điều khiển quản trị Okta](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdMZmhaHEzE8J4TtowCnI5QjU624nDnuHQFB3BzQiRuaJlrQH-ExvQQSNeveenPIee8CN7GS3zPwtV1OzxqZekMHHBHeX5M0uSWhG9MIT28_dSMyiWIJ9sceUyM0gOBr69eBAMrnQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 4: Thêm người dùng vào nhóm sftp trong bảng điều khiển quản trị Okta_

5.  Điều hướng đến **Security** ở thanh bên và chọn **Authenticators**. Trong tab **Setup**, chọn **Add authenticator** và thêm trình xác thực TOTP mà tổ chức của bạn sử dụng. _Lưu ý: Chỉ các trình xác thực dựa trên TOTP mới được hỗ trợ với Transfer Family, các trình xác thực dạng push (push-based) hiện chưa được hỗ trợ._ Trong trường hợp này, bạn thêm **Google Authenticator** làm trình xác thực.
6.  Để bắt buộc xác thực đa yếu tố (MFA) với Google Authenticator cho người dùng SFTP, điều hướng đến tab **Enrollment** và chọn **Add a policy**. Nhập các thông tin sau:
    - **Policy name:** SFTP Users MFA
    - **Assign to groups:** SFTP
    - **Authenticators:** Chuyển Google Authenticator thành **Required**.
    - Chọn **Create policy**. Trong hộp thoại _Add Rule_ xuất hiện, đặt **Rule name** là `SFTP Users MFA Enrollment` và chọn **Create rule**.
7.  Để bắt buộc MFA cho các phiên SFTP của người dùng, điều hướng đến **Security** ở thanh bên và đi đến **Global Session Policy**. Chọn **Add policy** và đặt **Policy name** thành `sftp policy`. Trong mục **Assign to groups**, thêm `sftp`, sau đó chọn **Create policy** và **Add rule**.
8.  Trong hộp thoại _Add Rule_ xuất hiện, đặt **Rule name** là `sftp rule`. Đặt **Multifactor authentication (MFA)** thành **Required**, sau đó chọn **Create rule**.
9.  Để hoàn tất việc thiết lập Google Authenticator cho người dùng Okta, đăng nhập vào miền Okta với tư cách người dùng `john@demosftp.com`. Mở một cửa sổ duyệt web riêng tư (private browsing) và điều hướng đến URL của tổ chức Okta (ví dụ: `dev-xxxx.okta.com`). Tại màn hình đăng nhập, nhập `john@demosftp.com` và mật khẩu đã tạo ở Bước 1.2. Khi được nhắc, chọn **Set up** dưới mục Google Authenticator và làm theo hướng dẫn để hoàn tất cài đặt. Bạn chỉ cần cấu hình phương thức Google Authenticator. Sau khi hoàn tất, bạn có thể bỏ qua các phương thức MFA khác và chọn **Continue** để tiếp tục đăng nhập.

    ![Hình 5: Thiết lập MFA cho một người dùng Okta](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcteOeCl8hnVbCX1LPQm78rs9F1-FZ0D7hBKxerkzuboqfxl-TjyNx6aQfqUWcY9yInTDGPDTrHLvrFwRLhjzWJ42FkrwgD0ezTiZYKKSH2yW622MRv5xNmHSc8maT4M5VD4eh1Vg?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 5: Thiết lập MFA cho một người dùng Okta_

_Lưu ý: Bạn đã thêm thành công một người dùng Okta vào nhóm mới tạo và cũng đã thiết lập MFA cho việc xác thực. Trước khi tiến hành giải pháp, người dùng Okta mới được tạo phải đăng nhập vào miền Okta để xác thực và thiết lập mật khẩu mới._

### 2. Triển khai giải pháp toolkit

Đăng nhập vào tài khoản AWS mà bạn muốn triển khai giải pháp, chuyển sang AWS Region mong muốn để chạy Transfer Family, và khởi động một phiên CloudShell.

1.  Chạy lệnh sau để clone (sao chép) kho lưu trữ của giải pháp vào môi trường CloudShell của bạn:

    ```bash
    cd ~
    git clone https://github.com/aws-samples/toolkit-for-aws-transfer-family.git
    ```

2.  Chạy lệnh sau để thực thi build script, lệnh này sẽ tải xuống tất cả các gói phụ thuộc và tạo các tệp nén (archives) cho Lambda layer và function được sử dụng trong giải pháp:

    ```bash
    cd ~/toolkit-for-aws-transfer-family/solutions/custom-idp
    ./build.sh
    ```

    _Theo dõi quá trình thực thi và xác minh rằng script đã hoàn tất thành công._

3.  Triển khai stack bằng SAM:

    ```bash
    sam deploy --guided --template \
    ./examples/okta/transfer_okta_customIdp_server_template.yaml --\
    capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND\
    CAPABILITY_NAMED_IAM --disable-rollback
    ```

4.  Khi được nhắc, hãy cung cấp các thông tin sau như ví dụ:

    - **Stack Name:** `transferidp`
    - **Parameter UserNameDelimiter:** `@@`
    - **Parameter LogLevel:** `INFO`
    - **Parameter ProvisionApi:** `false`
    - **Parameter EnableTracing:** `false`
    - **Parameter UsersTableName:** `empty`
    - **Parameter IdentityProvidersTableName:** `empty`
    - Đối với các tùy chọn khác, nhập `Y` để giữ nguyên giá trị mặc định.

    ![Hình 7: Triển khai AWS CloudFormation cho custom IdP](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcgPEP4HDdSgoQsmgz1g8hobs2bE_e28nM9nibvzPK-CoEUKYLp1rGjzbIRvmDKlcOMIrwRbDhAjoi0BaVK_WunZAFyHEz8XkBG5Hecnn1PtXsfYytt59_hfPHd1uQ6AKrQaFFWkQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 7: Triển khai AWS CloudFormation cho custom IdP_

5.  Khi quá trình triển khai hoàn tất thành công, hãy sao chép/dán các **AWS CloudFormation outputs** từ stack vừa được triển khai sang một trình soạn thảo văn bản và lưu lại để sử dụng về sau.

    **Các thông tin đầu ra bao gồm:**

    - **DDBIdentityProvidersTableName:** Tên bảng DynamoDB Identity Providers
    - **DDBUsersTableName:** Tên bảng DynamoDB Users
    - **SFTPServerS3Bucket:** Tên S3 bucket được tạo để làm bộ nhớ backend cho Transfer Family server
    - **TransferUserRole:** Amazon Resource Name (ARN) của IAM Role được tạo để ánh xạ đến người dùng
    - **TransferServerEndpoint:** Endpoint (điểm cuối) của Transfer Family server

### 3. Định nghĩa IdP

Khi giải pháp Transfer Family custom IdP được triển khai, bạn cần định nghĩa một hoặc nhiều IdP trong bảng `identity_providers` của DynamoDB. Để sử dụng Okta làm IdP, bạn cần thêm các thuộc tính Okta vào bảng này.

| Table name                                      | Comment                                                                                                                 |
| :---------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------- |
| `[StackName]-CustomIdP-XXXX_identity_providers` | Bảng DynamoDB chứa chi tiết của từng Transfer Family custom IdP (ví dụ: tên IdP, server URL, các tham số).              |
| `[StackName]-CustomIdP-XXXX_users`              | Bảng DynamoDB chứa chi tiết của từng Transfer Family user (ví dụ: IdP được sử dụng, IAM role, danh sách thư mục logic). |

1.  Trong AWS Management Console, điều hướng đến DynamoDB Item Explorer và chọn bảng `identity_providers` trong menu chọn bảng. Chọn **Create item**.

    ![Hình 8: Bảng DynamoDB cho custom IdP](https://lh7-rt.googleusercontent.com/docsz/AD_4nXefWm0xJf3OORTJhxUN4n27GQ1-vp4AqGv2zVfAN9yQs4MrEnWYgYJLOPtWtREwYzECBSGy33MHulcsUUiJG4lLo0BxqAY37vZdMbpTtgHlZkPzITgwEjQFWXlp60ZOADFAY3M7eg?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 8: Bảng DynamoDB cho custom IdP_

2.  Trong màn hình Create item, chọn **JSON view**, sau đó sao chép và dán bản ghi sau:

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

3.  Chọn nút **Form** để chuyển về chế độ xem form. Thay thế giá trị placeholder trong trường `okta_domain` bằng tên miền của tổ chức Okta của bạn (ví dụ: `dev-xxxxx.okta.com`).

    ![Hình 9: Tạo bản ghi IdP trong DynamoDB](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfLnVcbq-JdapL1tmqclvLT-7uJw81PsHoxklo_pxnxSTI_RaO9xhcvNKd6kgY-4XEophitLg_mRixfqw4lkULUu0qi26ydEJtzEjrETsL88BXgHacRRx0Cg7_vpJaEvZSLNaj4g?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 9: Tạo bản ghi IdP trong DynamoDB_

    - **provider:** Tên của IdP (ví dụ: `okta`).
    - **module:** Tên module IdP cần sử dụng (`okta`).
    - **config:** Chứa chi tiết cấu hình. `mfa` đặt là `true` nghĩa là người dùng cần nhập password + mã TOTP.

4.  Chọn **Create item**.

### 4. Định nghĩa một người dùng

Tiếp theo, bạn cần định nghĩa người dùng và ánh xạ tên người dùng đó đến IdP Okta vừa tạo.

1.  Trong console, điều hướng đến DynamoDB Item Explorer, chọn bảng `users` (ví dụ: `[StackName]-CustomIdP-XXXX_users`). Chọn nút **Create item**.

    ![Hình 10: Bảng DynamoDB cho users](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdmnXmED6yoRn2CShfnghH5buN4o29DkJy07xWwfr5DGjgOhr28mInBIZ0aO40EbPwjp7Qs7JmxmGHGUmY9CSRBwFyyALZVKuijjaeD-fZSZaKPh2Mjk2pbymnZssxKG67zcWMX?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 10: Bảng DynamoDB cho users_

2.  Trong màn hình Create item, chọn **JSON view**, sau đó sao chép và dán bản ghi sau:

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

3.  Chọn nút **Form**. Mở rộng phần `config` và thay thế `[SFTPServerS3Bucket]` và `[TransferUserRole]` bằng các giá trị CloudFormation outputs tương ứng ở Bước 2.5.

    ![Hình 11: Tạo bản ghi người dùng trong DynamoDB](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcu2nZuDcClUIyqF-hLHdf0KxK9L1B4L0GdD4yLJdyJRf0mnA_DHJzAvsnDAuzWoazdZImn6EtVmH_TYx5Q0fHyH6nZFLDBeMar1Smpkic1Q6CeQ70BVrgpGIZMy2xKUd5rFLbpTQ?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 11: Tạo bản ghi người dùng trong DynamoDB_

4.  Chọn **Create item**.

### 5. Kiểm thử IdP

1.  Điều hướng đến AWS Transfer Family console và chọn server đã được triển khai.
2.  Ở góc trên bên phải, chọn **Actions**, sau đó chọn **Test** từ menu thả xuống.

    ![Hình 12: Kiểm thử quyền truy cập SFTP](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfmdHjecbWuyso3osbbhdURE2ha7T3NxPfEHvH9Q6Rj8rgNR3EM1JZRMKZTsstL1-vGpl3f9OVUYLhMaSj5OWzC7isRIW7ACOKR3S9rnoOVFAhx3ObjztsqinxoaaCPBL3G7f6moA?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 12: Kiểm thử quyền truy cập SFTP_

3.  Nhập `john@demosftp.com` vào trường **username**. Đối với trường **password**, nhập mật khẩu của bạn nối tiếp với mã TOTP từ Google Authenticator (Ví dụ: nếu mật khẩu là `TestP@55w0rd` và mã MFA là `201483`, nhập `TestP@55w0rd201483`). **Source IP** có thể là bất kỳ giá trị nào. Chọn **Test**.

    ![Hình 13: Kiểm thử xác thực với mã MFA](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd7QHH1_ceUh393j-jAbXVGf8Tq60jlvVbSH6PppKhLoDUuzxA1rJpvJYkVAy5CpMCVd_56jc6WVIHYCpAi3MUgAOtnx2p61AqLcnBTkpzdFWmU30i3gl8oBrWPrfkjqp7yw7_seg?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 13: Kiểm thử xác thực với mã MFA_

4.  Nếu thành công, phản hồi HTTP 200 sẽ được trả về. Sau đó, bạn có thể kết nối đến máy chủ Transfer Family thông qua một client SFTP bằng cách sử dụng `TransferServerEndpoint` từ CloudFormation output.

    ![Hình 14: Kiểm thử với lệnh SFTP](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdRynxVJXo9ivmL07BsOj3OIwVwKgGX1txl1Xs9x43MJaLvNZfhrVkcGweo_q_icGDn98WW4l0Z4zhU4C-f9lBUEvp1LqE_W_ZeoyfRx11BAge4ABaYytt_pPMT7H9AtqDuU9wPiw?key=R4ysRt4XoNv_sIjTNMv9AA)
    _Hình 14: Kiểm thử với lệnh SFTP (lưu ý sử dụng định dạng "user@domain" trong dấu ngoặc kép nếu cần)_

## Dọn dẹp

Để xóa các tài nguyên đã tạo:

1.  Chạy lệnh: `sam delete --stack-name <stack-name>`
2.  Xóa thủ công hai bảng DynamoDB (`identity_providers` và `users`) trong console.
3.  Xóa thủ công AWS Lambda Layer (`handler-layer`) trong console.

## Kết luận

Việc chuẩn hóa quy trình triển khai trên nhiều IdP khác nhau giúp cải thiện khả năng tương tác và nâng cao hiệu quả quản lý. Trong bài viết này, chúng tôi đã hướng dẫn cách triển khai Okta làm custom IdP cho AWS Transfer Family, tích hợp MFA để tăng cường bảo mật. Bạn có thể tham khảo thêm về giải pháp và các mô-đun khác (như Active Directory, Entra ID, Cognito) trong [GitHub toolkit repository](https://github.com/aws-samples/toolkit-for-aws-transfer-family).

**TAGS:** AWS Storage Blog, AWS Transfer Family

---

**Về tác giả:**

**Yongki Kim**
Yongki là một APJ Storage Specialist Solutions Architect với chuyên môn sâu rộng về danh mục lưu trữ toàn diện của AWS. Anh đam mê làm việc với khách hàng để giải quyết những thách thức trong kiến trúc.

**Tamelly Lim**
Tamelly là một Specialist Solutions Architect làm việc tại Singapore, hỗ trợ khách hàng thiết kế và triển khai các giải pháp dựa trên AWS. Cô bắt đầu hành trình với đám mây vào năm 2022.
