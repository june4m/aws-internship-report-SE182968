---
title: "Worklog Tuần 3"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 3:

- Kết nối, làm quen với các thành viên trong First Cloud Journey.
- Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:

# Tuần 3 – Làm chủ AWS Compute: Cơ sở hạ tầng EC2, Các tùy chọn lưu trữ & Khả năng mở rộng

| Ngày  | Nhiệm vụ                                                                                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :---- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :----------------- |
| **2** | Thiết lập Transit Gateway                                                                                                                                                                                                                                        | 22/09/2025   | 22/09/2025      | Transit Gateway    |
| **3** | - Tìm hiểu lý thuyết về AWS EC2<br>- Loại Instance (Instance type)<br>- AMI/ Sao lưu (Backup)/ Keypair<br>- Elastic Block Store (EBS)<br>- Instance Store<br>- User data/ Meta data<br>- EC2 Auto Scaling<br>- EFS/FSx, Lightsail, MGN<br>- Thực hành bài Lab 13 | 23/09/2025   | 23/09/2025      | EC2                |
| **4** | Thực hành Module 3 - bài Lab 24                                                                                                                                                                                                                                  | 24/09/2025   | 24/09/2025      |                    |
| **5** | Thực hành bài Lab 57                                                                                                                                                                                                                                             | 25/09/2025   | 25/09/2025      |                    |
| **6** |                                                                                                                                                                                                                                                                  | 26/09/2025   | 26/09/2025      |                    |

---

### Thu Hoạch Tuần 3: Nền Tảng Compute Vững Chắc và Quản Lý Lưu Trữ Đa Dạng

- **Kết nối mạng nâng cao (Advanced Network Connectivity):** Đã hoàn thiện cấu hình Transit Gateway (TGW), củng cố mô hình mạng trung tâm (hub-and-spoke) cho khả năng mở rộng trong tương lai.
- **Amazon EC2 & Khả năng mở rộng (Scalability):**
  - Nắm vững kiến trúc EC2, từ việc chọn Instance Type đến cơ chế tự động hóa bằng User Data.
  - Đặc biệt, đã làm chủ được quy trình triển khai EC2 Auto Scaling Group (ASG) kết hợp với ALB, đảm bảo hệ thống có Tính Sẵn Sàng Cao (HA) và Khả năng Mở rộng (Scalability) theo tải.
- **Giải pháp lưu trữ đám mây (Cloud Storage Solutions):** Đã phân biệt rõ và biết khi nào nên sử dụng EBS (Block Storage cho OS/DB), Instance Store (Cache tạm thời), và EFS (Shared File System).
