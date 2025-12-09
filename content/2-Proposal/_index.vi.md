---
title: "Bản đề xuất"
date: "2025-09-08"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hệ thống Hiến máu & Cấp cứu Đại Việt (DaiVietBlood)

**Thực hiện bởi:** Skyline Team – Đại học FPT TP.HCM

**Ngày lập:** 07/12/2025

[Download PDF](/images/2-Proposal/AWS-Proposal.pdf)

---

## MỤC LỤC

1. **BỐI CẢNH VÀ ĐỘNG LỰC**
   - 1.1 Tóm tắt điều hành
   - 1.2 Tiêu chí thành công của dự án
   - 1.3 Các giả định
2. **KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC**
   - 2.1 Sơ đồ kiến trúc kỹ thuật
   - 2.2 Kế hoạch kỹ thuật
   - 2.3 Kế hoạch dự án
   - 2.4 Các cân nhắc về bảo mật
3. **HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO**
   - 3.1 Hoạt động và Sản phẩm bàn giao
   - 3.2 Ngoài phạm vi dự án (Out of Scope)
   - 3.3 Lộ trình tiến lên Production
4. **BẢNG PHÂN TÍCH CHI PHÍ AWS DỰ KIẾN**
5. **ĐỘI NGŨ THỰC HIỆN**
6. **NGUỒN LỰC & ƯỚC TÍNH CHI PHÍ NHÂN SỰ**
7. **NGHIỆM THU**

---

## 1. BỐI CẢNH VÀ ĐỘNG LỰC

### 1.1 TÓM TẮT ĐIỀU HÀNH (EXECUTIVE SUMMARY)

**Bối cảnh Khách hàng:**
Hệ thống DaiVietBlood được thiết kế để phục vụ cộng đồng, bao gồm người hiến máu tình nguyện, bệnh nhân cần máu khẩn cấp và các chuyên gia y tế tại Việt Nam. Khách hàng chính của hệ thống là người hiến máu, gia đình bệnh nhân và nhân viên y tế chịu trách nhiệm quản lý kho máu và lịch hiến. Họ cần một nền tảng tập trung, tin cậy để tối ưu hóa quy trình khớp nối giữa người hiến và người nhận, cải thiện khả năng liên lạc trong các trường hợp khẩn cấp. Trong bối cảnh chuyển đổi số y tế, DaiVietBlood cung cấp giải pháp an toàn, dễ tiếp cận nhằm giải quyết tình trạng khan hiếm máu cục bộ.

**Mục tiêu Kinh doanh và Kỹ thuật:**
Việc di chuyển hệ thống DaiVietBlood từ môi trường cục bộ (Local/On-premise) lên AWS mang lại lợi thế vượt trội:

- **Về kinh doanh:** AWS cho phép ứng dụng mở rộng linh hoạt theo số lượng người dùng, giảm chi phí vận hành hạ tầng cứng và đảm bảo hiệu suất ổn định trên toàn quốc.
- **Về kỹ thuật:** AWS cung cấp độ sẵn sàng cao (High Availability) và bảo mật dữ liệu y tế. Việc áp dụng kiến trúc **Serverless** (AWS Lambda, API Gateway, Cognito, RDS) giúp đơn giản hóa quản lý backend, tăng tốc độ phát triển và giảm chi phí bảo trì. Hệ thống được tích hợp giám sát toàn diện (CloudWatch) và tuân thủ các tiêu chuẩn bảo mật nghiêm ngặt.

**Tóm tắt các Use Cases chính:**

| Vai trò                   | Chức năng chính                  | Mô tả ngắn                                                                                   |
| :------------------------ | :------------------------------- | :------------------------------------------------------------------------------------------- |
| **Khách (Guest)**         | Truy cập thông tin công khai     | Xem hướng dẫn hiến máu, bảng tương thích nhóm máu, bài viết giáo dục mà không cần đăng nhập. |
| **Thành viên (Member)**   | Đăng ký/Đăng nhập, Quản lý hồ sơ | Tạo tài khoản, cập nhật thông tin cá nhân và nhóm máu.                                       |
|                           | Đặt lịch hiến máu                | Chọn thời gian và địa điểm để hiến máu.                                                      |
|                           | Gửi yêu cầu cấp cứu              | Gửi yêu cầu máu khẩn cấp, hệ thống tự động tìm người hiến phù hợp.                           |
| **Nhân viên (Staff)**     | Quản lý yêu cầu & Kho máu        | Duyệt yêu cầu cấp cứu, xác nhận lịch hiến, cập nhật tồn kho máu.                             |
| **Quản trị viên (Admin)** | Quản trị hệ thống                | Quản lý tài khoản người dùng, cấu hình khung giờ hiến, xem báo cáo tổng quan.                |

**Tóm tắt Dịch vụ Chuyên nghiệp của Đối tác:**
Skyline Team sẽ cung cấp dịch vụ chuyển đổi số toàn diện, bao gồm đánh giá ứng dụng hiện tại, thiết kế kiến trúc Cloud-native, và thực hiện di chuyển (migration) hệ thống sang môi trường AWS Serverless. Chúng tôi cam kết bàn giao một hệ thống an toàn, có khả năng mở rộng, kèm theo quy trình CI/CD tự động hóa và tài liệu vận hành chi tiết.

### 1.2 TIÊU CHÍ THÀNH CÔNG CỦA DỰ ÁN

1.  **Chức năng:** 100% chức năng cốt lõi (đăng ký, đặt lịch, yêu cầu khẩn cấp, quản trị) hoạt động ổn định trên AWS, không phát sinh lỗi hồi quy.
2.  **Độ sẵn sàng:** Hệ thống đạt Uptime ≥ 99.9%, đảm bảo truy cập liên tục 24/7.
3.  **Hiệu năng:** Thời gian phản hồi ứng dụng cải thiện ít nhất 30% so với phiên bản chạy cục bộ. Thời gian xử lý yêu cầu cấp cứu giảm 40%.
4.  **Chi phí:** Tối ưu hóa chi phí hạ tầng ít nhất 20% nhờ mô hình Serverless và Auto-scaling.
5.  **Trải nghiệm người dùng:** Tỷ lệ chấp nhận UAT đạt tối thiểu 95% cho mọi vai trò người dùng.
6.  **Bảo mật:** Tuân thủ đầy đủ các yêu cầu về mã hóa dữ liệu, quản lý truy cập (IAM) và bảo mật API.
7.  **Vận hành:** Quy trình CI/CD tự động hóa hoàn toàn với thời gian deploy < 10 phút. Hệ thống giám sát (Monitoring) bao phủ 100% dịch vụ quan trọng.

### 1.3 CÁC GIẢ ĐỊNH

**Giả định về Kỹ thuật & Kiến trúc:**

- **Mã nguồn:** Ứng dụng chạy Local hiện tại (Frontend & Backend) đã hoàn thiện về mặt logic chức năng. Dự án tập trung vào việc Refactoring (tái cấu trúc) để đưa lên Cloud (Serverless), không bao gồm phát triển tính năng mới.
- **AWS Region:** Toàn bộ hạ tầng được triển khai tại **Singapore (ap-southeast-1)** để tối ưu độ trễ cho người dùng Việt Nam. _Lưu ý: Trong giai đoạn thử nghiệm, do sử dụng cấu hình VPC và tài nguyên hạn chế của gói Free Tier/Student, độ trễ có thể dao động (ước tính ~3.5s/request)._
- **Hạn mức Dịch vụ (Service Limits):** Tài khoản AWS sử dụng hạn mức mặc định (Soft limits). Việc tăng hạn mức để giảm độ trễ sẽ do Khách hàng phê duyệt khi cần thiết.
- **Tích hợp bên thứ ba:** Hệ thống sử dụng API Gemini cho các tính năng AI hỗ trợ.
- **Quyền truy cập:** Nhóm Skyline được cấp quyền Admin (IAM Role) để khởi tạo tài nguyên.

**Giả định về Vận hành & Tài chính:**

- **Tên miền:** Khách hàng sở hữu tên miền (ví dụ: daivietblood.com) và quyền cấu hình DNS.
- **Chi phí:** Bảng ước tính chi phí dựa trên giả định lưu lượng khoảng 50.000 yêu cầu API/tháng. Chi phí thực tế phụ thuộc vào mức sử dụng (Pay-as-you-go).

---

## 2. KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC

### 2.1 SƠ ĐỒ KIẾN TRÚC KỸ THUẬT

Hệ thống DaiVietBlood sử dụng kiến trúc **Serverless-First** trên AWS Cloud, ưu tiên khả năng mở rộng, bảo mật và tối ưu vận hành.

![Ảnh kiến trúc AWS](/images/AWS_Architecture.png)

**Các thành phần chính:**

1.  **Hạ tầng Mạng (VPC):**
    - _Public Subnet:_ Chứa Internet Gateway và NAT Gateway.
    - _Private Subnet:_ Chứa AWS Lambda và Amazon RDS để cô lập và bảo mật dữ liệu, ngăn chặn truy cập trực tiếp từ Internet.
2.  **Ứng dụng & Dữ liệu:**
    - _Frontend:_ Host trên **AWS Amplify**, phân phối qua **Amazon CloudFront** (CDN) và lưu trữ assets trên **S3**.
    - _Authentication:_ **Amazon Cognito** quản lý định danh và cấp phát JWT token.
    - _API & Compute:_ **Amazon API Gateway** tiếp nhận request, chuyển đến **AWS Lambda** để xử lý logic nghiệp vụ.
    - _Database:_ **Amazon RDS** lưu trữ dữ liệu có cấu trúc, đặt trong Private Subnet.
3.  **DevOps & Giám sát:**
    - _CI/CD:_ Sử dụng **AWS CodePipeline, CodeBuild, CodeDeploy** để tự động hóa quy trình triển khai.
    - _Monitoring:_ **Amazon CloudWatch** thu thập logs và metrics tập trung.

### 2.2 KẾ HOẠCH KỸ THUẬT

Quy trình triển khai kỹ thuật tuân theo phương pháp Infrastructure-as-Code (IaC):

- **Tự động hóa Hạ tầng:** Sử dụng **AWS CloudFormation** để khởi tạo VPC, Lambda, RDS, API Gateway, đảm bảo tính nhất quán giữa các môi trường (Dev/Staging/Prod).
- **Phát triển Ứng dụng:** Refactor backend thành các hàm Lambda module hóa (NodeJS/Python). Các biến môi trường và thông tin nhạy cảm (DB credentials) được mã hóa an toàn.
- **Quy trình CI/CD:**
  - Source (GitHub) -> Build (CodeBuild) -> Deploy (CloudFormation/CodeDeploy).
  - Bao gồm bước phê duyệt thủ công (Manual Approval) trước khi deploy ra môi trường Production.
- **Chiến lược Kiểm thử:** Unit Test cho Lambda, Integration Test cho API và Load Test để đảm bảo khả năng chịu tải.

### 2.3 KẾ HOẠCH DỰ ÁN

Dự án áp dụng mô hình Agile Scrum trong 8 tuần (4 Sprints):

- **Sprint 1 (Nền tảng):** Thiết lập AWS Account, VPC, RDS.
- **Sprint 2 (Backend Core):** Phát triển Lambda, API Gateway, Cognito.
- **Sprint 3 (Tích hợp):** Deploy Frontend (Amplify), hoàn thiện CI/CD Pipeline.
- **Sprint 4 (Ổn định hóa):** UAT, Tối ưu hiệu năng, Bàn giao.

### 2.4 CÁC CÂN NHẮC VỀ BẢO MẬT

1.  **Quản lý Truy cập:** Sử dụng Cognito cho xác thực người dùng và IAM Role cho phân quyền dịch vụ (Least Privilege).
2.  **Cô lập Mạng:** Database và Lambda nằm trong Private Subnet, chỉ truy cập Internet qua NAT Gateway.
3.  **Bảo vệ Dữ liệu:** Mã hóa dữ liệu khi lưu trữ (At-rest) trên RDS/S3 và khi truyền tải (In-transit) qua HTTPS.
4.  **Giám sát An ninh:** CloudWatch Logs ghi lại toàn bộ hoạt động để phục vụ kiểm tra (audit) và phát hiện xâm nhập.

---

## 3. HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO

### 3.1 HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO

| Giai đoạn                     | Thời gian | Hoạt động chính                                                              | Sản phẩm bàn giao                          | Ước tính (Man-day) |
| :---------------------------- | :-------- | :--------------------------------------------------------------------------- | :----------------------------------------- | :----------------- |
| **Phân tích & Thiết kế**      | Tuần 1    | Đánh giá hiện trạng Local, thiết kế kiến trúc Cloud, lên kế hoạch migration. | Tài liệu SRS, Sơ đồ kiến trúc, Đặc tả API. | 5                  |
| **Phát triển Local**          | Tuần 2-3  | Xây dựng logic backend, database schema, unit test cục bộ.                   | Prototype Backend, Database Schema.        | 10                 |
| **Frontend & Tích hợp**       | Tuần 4-5  | Phát triển Frontend, tích hợp API local, chuẩn bị code để refactor.          | Ứng dụng hoàn chỉnh chạy Local.            | 10                 |
| **Thiết lập Hạ tầng AWS**     | Tuần 6    | Viết script CloudFormation, khởi tạo VPC, RDS, IAM.                          | Template IaC, Môi trường VPC an toàn.      | 5                  |
| **Refactor & Deploy Backend** | Tuần 7-8  | Chuyển đổi sang Lambda, cấu hình API Gateway, Cognito.                       | Backend Serverless hoạt động trên AWS.     | 10                 |
| **Deploy Frontend & CI/CD**   | Tuần 9-10 | Host Frontend trên Amplify, thiết lập Pipeline tự động.                      | URL Production, CI/CD Pipeline.            | 10                 |
| **Testing & Go-live**         | Tuần 11   | UAT, Kiểm thử bảo mật, Tối ưu hiệu năng.                                     | Báo cáo UAT, Báo cáo bảo mật.              | 5                  |
| **Bàn giao & Đào tạo**        | Tuần 12   | Chuyển giao tài khoản, đào tạo vận hành, bàn giao tài liệu.                  | Tài liệu hướng dẫn vận hành, Nghiệm thu.   | 5                  |

### 3.2 NGOÀI PHẠM VI DỰ ÁN (OUT OF SCOPE)

Do giới hạn về thời gian và tài nguyên của giai đoạn MVP, các hạng mục sau chưa được bao gồm:

- Thuật toán tìm kiếm người dùng tối ưu theo vị trí thực (Geo-location) (Hiện tại sử dụng logic đơn giản hóa).
- Khả năng tự động mở rộng (Auto-scaling) phức tạp cho tầng Database (Hiện tại dùng RDS cơ bản).
- Tối ưu hóa độ trễ chuyên sâu (Latency Optimization) cho các khu vực ngoài Singapore.
- Các tiêu chuẩn bảo mật nâng cao (Advanced Security Compliance) như HIPAA/PCI-DSS.

### 3.3 LỘ TRÌNH TIẾN LÊN PRODUCTION (PATH TO PRODUCTION)

Để nâng cấp từ bản MVP hiện tại lên hệ thống Production quy mô lớn, cần thực hiện:

1.  **Chiến lược Môi trường:** Tách biệt hoàn toàn môi trường Dev/Staging/Prod trên các tài khoản AWS khác nhau (Multi-account strategy).
2.  **Mở rộng Database:** Chuyển sang Amazon Aurora Serverless hoặc sử dụng Read Replicas để tăng khả năng chịu tải đọc/ghi.
3.  **Nâng cao Giám sát:** Tích hợp AWS X-Ray để truy vết (trace) request và phát hiện điểm nghẽn hiệu năng.
4.  **Tăng cường Bảo mật:** Triển khai AWS WAF với các tập luật chặn DDoS và bot tự động; sử dụng Amazon Inspector để quét lỗ hổng định kỳ.

---

## 4. BẢNG PHÂN TÍCH CHI PHÍ AWS DỰ KIẾN

Dưới đây là bản dịch tiếng Việt, giữ nguyên định dạng liên kết:

[Bảng Ước tính Chi phí](https://calculator.aws/#/estimate?id=6f03c76acaabcf7f7d55ccb27d6c2600a04ad472) dưới đây được tính toán dựa trên khu vực _Châu Á Thái Bình Dương (Singapore)_, đây là khu vực tiêu chuẩn để đảm bảo độ trễ thấp nhất cho các truy cập từ Việt Nam.

| Hạng mục      | Dịch vụ      | Cấu hình ước tính                                             | Chi phí tháng (USD)  |
| :------------ | :----------- | :------------------------------------------------------------ | :------------------- |
| **Network**   | NAT Gateway  | 1 NAT Gateway (Bắt buộc cho Private Subnet) + Data Processing | ~$43.13              |
|               | VPC          | Subnets, Security Groups                                      | ~$13.14              |
|               | CloudFront   | 5GB Data Transfer (Tận dụng Free Tier)                        | ~$3.00               |
| **Compute**   | Lambda       | 1,000 requests, 512MB RAM (Free Tier)                         | ~$0.00               |
|               | API Gateway  | 1,000 requests                                                | ~$0.00               |
| **Database**  | RDS          | db.t3.micro, 20GB Storage                                     | ~$21.74              |
| **Storage**   | S3           | 5GB Storage, 200 requests                                     | ~$0.14               |
| **Hosting**   | Amplify      | Build & Hosting, WAF enabled                                  | ~$16.77              |
| **Ops**       | CloudWatch   | Logs, Metrics, Alarms                                         | ~$9.41               |
| **CI/CD**     | CodePipeline | 1 Active Pipeline                                             | ~$1.05               |
| **Tổng cộng** |              |                                                               | **~$108.38 / Tháng** |

---

### 5. ĐỘI NGŨ DỰ ÁN (TEAM)

**Cố vấn Chương trình AWS FCJ (AWS FCJ Program Lead)**

| Họ tên              | Chức danh                                                     | Mô tả                                                                                                  | Email / Thông tin liên hệ |
| :------------------ | :------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------- | :------------------------ |
| **Nguyễn Gia Hưng** | Trưởng nhóm Kiến trúc Giải pháp (Head of Solutions Architect) | Cung cấp hướng dẫn kỹ thuật, đánh giá kiến trúc và tư vấn các phương pháp thực hành tốt nhất trên AWS. | hunggia@amazon.com        |

**Các bên liên quan của Dự án (Project Stakeholders)**

| Nhóm                | Chức danh               | Vai trò liên quan                                                | Email / Thông tin liên hệ |
| :------------------ | :---------------------- | :--------------------------------------------------------------- | :------------------------ |
| **AWS FCJ Mentors** | Giảng viên Chương trình | Giám sát học thuật, đánh giá dự án và xác nhận tín chỉ thực tập. |                           |

**Nhóm Thực tập (Đại học FPT)**

| Họ tên                     | Chức danh                   | Vai trò & Trách nhiệm                                                                                                                                                                    | Email / Thông tin liên hệ    |
| :------------------------- | :-------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------- |
| **Nguyễn Đức Lân**         | Đồng Trưởng nhóm (PM)       | **Quản lý Dự án:** Chịu trách nhiệm điều phối nhóm và theo dõi tiến độ. Dẫn dắt cấu trúc mã nguồn, lập kế hoạch UAT và Tối ưu hóa chi phí. Hoàn thiện thiết kế kiến trúc AWS serverless. | lannguyen68609@gmail.com     |
| **Nguyễn Công Minh**       | Đồng Trưởng nhóm (Kỹ thuật) | **Lập trình viên (DevOps & Backend):** Triển khai CI/CD, CodePipeline, CDK Stack và AWS Lambda.                                                                                          | minhncse182968@fpt.edu.vn    |
| **Đỗ Khang**               | Thành viên                  | **Kiến trúc sư Đám mây:** Thiết kế Kiến trúc tổng thể, Chính sách Dịch vụ (Service Policy), đồng thiết kế kiến trúc AWS serverless.                                                      | dokhang307@gmail.com         |
| **Lê Hoàng Anh**           | Thành viên                  | **Lập trình viên (Fullstack):** Phát triển API, thiết kế giao diện UI/UX và cấu hình Bảo mật.                                                                                            | anhlhse170327@fpt.edu.vn     |
| **Nguyễn Quách Lam Giang** | Thành viên                  | **Kỹ sư Dữ liệu:** Quản trị RDS (MySQL), thiết lập kết nối Database, cấu hình mạng VPC và Subnet.                                                                                        | nguyenlamgiang2198@gmail.com |

**Đầu mối liên hệ xử lý sự cố (Project Escalation Contact)**

| Họ tên             | Chức danh   | Vai trò                                                                  | Email / Thông tin liên hệ |
| :----------------- | :---------- | :----------------------------------------------------------------------- | :------------------------ |
| **Nguyễn Đức Lân** | Trưởng nhóm | Đầu mối liên hệ chính về tình trạng dự án và xử lý các vấn đề phát sinh. | lannguyen68609@gmail.com  |

---

#### 6. NGUỒN LỰC & ƯỚC TÍNH CHI PHÍ

**Phân bổ Nguồn lực (Resource Breakdown)**

| Nguồn lực                  | Trách nhiệm                                                                                                                                                               |
| :------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Nguyễn Đức Lân**         | **Điều phối & PM:** Quản lý dự án, Tối ưu hóa chi phí, Phân tích Forensics, Cấu hình hệ thống và hỗ trợ lập trình Full-stack.                                             |
| **Nguyễn Công Minh**       | **DevOps & Hạ tầng:** Quy trình CI/CD, Bảo mật, CDK Stack, Triển khai Lambda, Đồng thiết kế cấu trúc API, Hạ tầng hệ thống.                                               |
| **Lê Hoàng Anh**           | **Frontend & UI/UX:** Phát triển Frontend, Tích hợp API, Thiết kế UI/UX, Bảo mật ứng dụng.                                                                                |
| **Nguyễn Quách Lam Giang** | **Kỹ thuật Dữ liệu:** Phân tích dữ liệu, Kết nối RDS với MySQL, Khởi tạo VPC, Giám sát CloudWatch, Cấu hình luồng Subnet và NAT Gateway.                                  |
| **Đỗ Khang**               | **Kiến trúc Đám mây:** Thiết kế kiến trúc, Các chính sách dịch vụ (Service Policies), Tài liệu hóa, Đồng thiết kế cấu trúc API, Tích hợp AI Chatbot, Giám sát CloudWatch. |

**Giờ làm việc theo Giai đoạn Dự án**

| Giai đoạn Dự án               | N.Đ. Lân | N.C. Minh | L.H. Anh | N.Q.L. Giang | Đ. Khang | Tổng số giờ |
| :---------------------------- | :------: | :-------: | :------: | :----------: | :------: | :---------: |
| **Nền tảng (Foundation)**     |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Điều phối Cốt lõi**         |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Lớp Phân tích (Analytics)** |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Kiểm thử & Xác thực**       |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Tài liệu & Bàn giao**       |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Tổng số giờ**               | **150**  |  **150**  | **150**  |   **150**    | **150**  |   **750**   |

**Phân bổ Chi phí**

| Bên tham gia             | Đóng góp (USD)                       | Tỷ lệ % trên Tổng số |
| :----------------------- | :----------------------------------- | :------------------- |
| **Chương trình AWS FCJ** | $0 (Thực tập phi lợi nhuận)          | 0%                   |
| **Đại học FPT**          | Nhân lực sinh viên (Tín chỉ học tập) | 0%                   |
| **Hạ tầng AWS**          | ~$15.00 (Chi phí chạy thử nghiệm)    | 100%                 |
| **Tổng chi phí Dự án**   | **~$15.00**                          | **100%**             |

**\*Ghi chú về Hiệu quả Chi phí:** Chi phí nhân lực cho dự án này được hỗ trợ như một phần của kỳ Thực tập tại Đại học FPT và chương trình AWS First Cloud Journey (FCJ). Khoản ước tính $15 đại diện cho chi phí vận hành hạ tầng (Sử dụng AWS Credits) cần thiết cho các môi trường phát triển và kiểm thử.

## 7. NGHIỆM THU

### 7.1 Bàn giao Sản phẩm:

Sau khi hoàn thành giai đoạn "Bàn giao", Skyline Team sẽ gửi toàn bộ Mã nguồn, Tài liệu Kiến trúc, Tài khoản Admin và Hướng dẫn vận hành cho Khách hàng/Mentor.

### 7.2 Thời gian & Quy trình Nghiệm thu:

Khách hàng có 05 ngày làm việc để kiểm tra và thực hiện UAT. Nếu sản phẩm đáp ứng Tiêu chí thành công (Mục 1.2), Khách hàng sẽ ký xác nhận nghiệm thu.

### 7.3 Xử lý Lỗi:

Nếu phát sinh lỗi nghiêm trọng hoặc thiếu tính năng so với phạm vi đã cam kết, Skyline Team có trách nhiệm khắc phục và gửi lại để nghiệm thu trong thời gian sớm nhất.
