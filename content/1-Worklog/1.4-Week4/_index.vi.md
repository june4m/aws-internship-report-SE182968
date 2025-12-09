---
title: "Woklog Tuần 4"
date: "2025-09-29"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4:

- **Lưu trữ & Phân phối nội dung:** Hiểu và thực hành triển khai hosting website tĩnh sử dụng Amazon S3 kết hợp với mạng phân phối nội dung CloudFront.
- **Công cụ phát triển (DevTools):** Làm quen với việc sử dụng AWS CLI thông qua VSCode để tương tác với các dịch vụ AWS một cách chuyên nghiệp và hiệu quả hơn.
- **Cơ sở dữ liệu:** Thực hành khởi tạo và quản lý cơ sở dữ liệu quan hệ với Amazon RDS (MySQL).

### Các nhiệm vụ thực hiện trong tuần này:

| Ngày  | Nhiệm vụ                                                                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo    |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :-------------------- |
| **2** | **S3 & CloudFront:**<br>- Tìm hiểu lý thuyết cơ bản về S3 và CloudFront.<br>- **Thực hành:** Biến S3 bucket thành website tĩnh, cấu hình quyền truy cập, tạo CloudFront distribution và thực hành Bucket Versioning. | 29/09/2025   | 30/09/2025      | Amazon S3 Guide       |
| **3** | **AWS CLI & VSCode:**<br>- Thiết lập môi trường phát triển: Kết nối AWS với VSCode.<br>- Sử dụng AWS CLI để thao tác với dịch vụ thay vì dùng Console.                                                               | 30/09/2025   | 30/09/2025      | AWS CLI Configuration |
| **4** | **Cơ sở dữ liệu:**<br>- Thực hành tạo RDS Instance.<br>- Kết nối và làm việc với MySQL trên hạ tầng AWS.                                                                                                             | 01/10/2025   | 01/10/2025      | Amazon RDS (MySQL)    |

---

### Thành tựu Tuần 4: Website tĩnh, CDN & Quản trị Database

#### 1. Cơ chế lưu trữ S3 và Hosting Website tĩnh

- **Hiểu về S3:** Nắm vững S3 là dịch vụ lưu trữ đối tượng (Object Storage), cực kỳ phù hợp để lưu trữ các tài nguyên web tĩnh (HTML/CSS/JS).
- **Quy trình triển khai:**
  - Kích hoạt tính năng **Static Website Hosting** trên Bucket.
  - Cấu hình các tài liệu chính: `index.html` (trang chủ) và `error.html` (trang lỗi).
  - **Quản lý quyền truy cập:** Thiết lập Bucket Policy để cho phép công chúng (public) đọc dữ liệu hoặc giới hạn chỉ truy cập thông qua CloudFront để bảo mật hơn.
  - Kiểm thử thành công việc truy cập website thông qua Endpoint URL của S3.

#### 2. Tối ưu hóa phân phối nội dung với CloudFront

- **Cấu hình Distribution:**
  - **Origin:** Trỏ về S3 bucket chứa source code web.
  - **Behavior:** Thiết lập các chính sách caching, thời gian tồn tại của cache (TTL) và giao thức HTTP/HTTPS.
- **Lợi ích thực tế:**
  - **Hiệu năng:** Tăng tốc độ tải trang nhờ mạng lưới Edge Locations.
  - **Bảo mật:** Ẩn S3 bucket gốc khỏi người dùng cuối (người dùng chỉ tương tác với CloudFront), hỗ trợ HTTPS qua ACM Certificate.
- **Vận hành:** Hiểu cách sử dụng **Invalidations** để xóa cache cũ khi cập nhật nội dung mới cho website.

#### 3. Tương tác chuyên nghiệp với AWS CLI & VSCode

- **Thiết lập môi trường:**
  - Cài đặt thành công AWS Toolkit extension cho VSCode.
  - Cấu hình xác thực người dùng (`aws configure`) với Access Key và Secret Key.
- **Thao tác dòng lệnh:** Chuyển đổi từ việc click giao diện (Console) sang sử dụng lệnh CLI để quản lý các tài nguyên như S3, EC2 và RDS, giúp tăng tốc độ làm việc.

#### 4. Quản trị Amazon RDS (MySQL)

- **Triển khai Database:**
  - Khởi tạo thành công RDS MySQL instance.
  - Cấu hình mạng lưới: Thiết lập **Subnet Group** và **Security Group** để kiểm soát luồng truy cập vào database.
- **Kết nối & Truy vấn:**
  - Sử dụng MySQL Workbench để kết nối vào RDS endpoint.
  - Thực hiện các thao tác quản trị cơ sở dữ liệu cơ bản (Tạo DB, chạy các câu lệnh SQL).
