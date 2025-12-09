---
title: "Worklog Tuần 3"
date: "2025-09-22"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** vào báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}

### Mục tiêu Tuần 3:

- **Làm chủ nền tảng điện toán (Compute):** Tìm hiểu sâu về kiến trúc Amazon EC2, hiểu rõ các trường hợp sử dụng riêng biệt cho từng loại Instance (Instance Types).
- **An ninh mạng:** Triển khai các quy tắc tường lửa chặt chẽ bằng cách sử dụng Security Groups để kiểm soát lưu lượng mạng.
- **Triển khai đa nền tảng:** Thực hành triển khai các instance trên cả môi trường Linux và Windows sử dụng SSH và RDP.
- **Thiết lập ứng dụng:** Cấu hình môi trường máy chủ web (LAMP/XAMPP) để mô phỏng việc lưu trữ ứng dụng trong thực tế.

### Các nhiệm vụ thực hiện trong tuần này:

| Ngày  | Nhiệm vụ                                                                                                                                                                                              | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo         |
| :---- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :------------------------- |
| **2** | **Lý thuyết & An ninh mạng:**<br>- Nghiên cứu các khái niệm cốt lõi của EC2 (Instance Types, AMI, EBS, User Data).<br>- Cấu hình VPC Security Groups (Quy tắc Inbound/Outbound).                      | 22/09/2025   | 22/09/2025      | Module 02-Lab03-04.1       |
| **3** | **Thao tác cơ bản:**<br>- Thực hành khởi chạy EC2 instance trong public subnet.<br>- Thiết lập kết nối thông qua SSH key pairs.                                                                       | 22/09/2025   | 23/09/2025      | Introduction to Amazon EC2 |
| **4** | **Chuyên sâu môi trường Linux:**<br>- Triển khai Amazon Linux EC2.<br>- **Lab nâng cao:** Khôi phục quyền truy cập vào instance sau khi mất Key Pair.<br>- Cài đặt LAMP Stack (Apache, MariaDB, PHP). | 24/09/2025   | 24/09/2025      | EC2 Linux Guide            |
| **5** | **Thiết lập môi trường Windows:**<br>- Triển khai Windows Server instance.<br>- Kết nối qua RDP (Remote Desktop Protocol).<br>- Cài đặt giải pháp XAMPP.                                              | 25/09/2025   | 25/09/2025      | EC2 Windows Guide          |

---

### Thành tựu Tuần 3: Làm chủ hạ tầng EC2, Cấu hình bảo mật & Triển khai Web Server

#### 1. Tìm hiểu sâu về Kiến trúc & Thành phần EC2

- **Mô hình dịch vụ điện toán:** Hiểu rõ EC2 là giải pháp IaaS cho khả năng tính toán có thể mở rộng theo nhu cầu.
- **Các thành phần cốt lõi:** Phân tích hệ sinh thái cần thiết để vận hành một instance hiệu quả:
  - **Instance Types:** Lựa chọn dựa trên khối lượng công việc (Tối ưu hóa CPU, Bộ nhớ, hoặc Lưu trữ).
  - **AMI (Amazon Machine Image):** Sử dụng các mẫu được cấu hình sẵn để triển khai nhanh chóng.
  - **Lưu trữ:** Phân biệt được EBS (lưu trữ khối bền vững) và Instance Store (lưu trữ tạm thời).
  - **Bootstrapping:** Sử dụng **User Data** để chạy các tập lệnh tự động ngay khi khởi tạo instance.

#### 2. Triển khai An ninh mạng

- **Quản lý Security Group:**
  - Cấu hình Security Groups như một **tường lửa có trạng thái (stateful firewall)** ở cấp độ instance.
  - Xác định chính xác các **quy tắc Inbound** để cho phép các dịch vụ cần thiết (SSH cổng 22, HTTP cổng 80, RDP cổng 3389).
  - Hiểu cơ chế mặc định của quy tắc Outbound và cách theo dõi kết nối.

#### 3. Quản trị Linux & Xử lý sự cố

- **Kết nối:** Thành thạo kết nối SSH sử dụng private keys (`.pem`).
- **Khôi phục thảm họa (Mất Key Pair):** Thực hiện thành công việc khôi phục thủ công một instance bị "mất quyền truy cập" bằng cách:
  1. Tạo một Key Pair mới.
  2. Sử dụng `PuTTYgen` để trích xuất Public Key.
  3. Chèn Public Key mới vào tập lệnh `user-data` (`cloud-config`) của instance trong khi instance đang dừng (stop).
  4. Kiểm chứng việc khôi phục quyền truy cập sau khi khởi động lại.
- **Triển khai Web Stack:** Cài đặt và cấu hình thành công LAMP stack (Linux, Apache, MariaDB, PHP) để host một trang web thử nghiệm.

#### 4. Quản trị Windows Server

- **Quản lý truy cập:** Lấy mật khẩu Administrator bằng cách giải mã với file Key Pair.
- **Truy cập từ xa:** Sử dụng Microsoft Remote Desktop (RDP/mstsc) để quản lý qua giao diện đồ họa.
- **Tầng ứng dụng:** Triển khai XAMPP để kiểm tra chức năng web server trên môi trường Windows.

#### 5. Khó khăn & Giải pháp

- **Vấn đề 1 (Kết nối):** Kết nối SSH ban đầu bị time out.
  - _Giải pháp:_ Kiểm tra lại Security Group và thêm quy tắc Inbound cho cổng 22 từ địa chỉ IP của tôi ("My IP").
- **Vấn đề 2 (Cài đặt phần mềm):** Cài đặt Apache thất bại trên **Amazon Linux 2023** do lệnh `amazon-linux-extras` đã bị loại bỏ/không còn được hỗ trợ.
  - _Giải pháp:_ Tra cứu tài liệu và chuyển sang sử dụng lệnh `dnf` cùng các kho lưu trữ (repositories) distro-stream cụ thể tương thích với AL2023.
