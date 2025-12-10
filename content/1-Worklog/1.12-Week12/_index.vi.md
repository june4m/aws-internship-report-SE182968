---
title: "Worklog Tuần 12"
date: "2025-11-24"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu Tuần 12:

- **Bảo mật & Giám sát:** Rà soát lỗ hổng bảo mật và thiết lập Dashboard theo dõi hệ thống.
- **Tổng kết dự án:** Hoàn thiện tài liệu, tối ưu hóa lần cuối và chuẩn bị slide báo cáo.

### Các nhiệm vụ thực hiện trong tuần này:

| Ngày    | Nhiệm vụ                                                                                                                                                                                                      | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------- | :-------------- | :----------------- |
| **2-3** | - Cấu hình CloudWatch Alarms và Dashboard.<br>- Rà soát bảo mật với AWS Trusted Advisor và GuardDuty.                                                                                                         | 24/11/2025   | 25/11/2025      |                    |
| **4-5** | **Tối ưu hóa & Đóng gói:**<br>- Review và xóa các tài nguyên thừa để tối ưu chi phí.<br>- Viết tài liệu hướng dẫn vận hành (Runbook).<br>- Soạn thảo slide báo cáo tổng kết (Kiến trúc, Thách thức, Bài học). | 26/11/2025   | 27/11/2025      |                    |

---

### Thành tựu Tuần 12: Hệ thống Hoàn thiện & Sẵn sàng Bàn giao

#### 1. Đánh giá Hiệu năng & Tinh chỉnh

- **Giám sát:**
  - Đã thiết lập **CloudWatch Dashboard** hiển thị trực quan các chỉ số: CPU Utilization, Request Count, Database Connections, Error Rate (4xx, 5xx).
  - Cấu hình **SNS Alerts** để gửi email cảnh báo cho Admin khi hệ thống gặp sự cố.

#### 2. Tối ưu hóa & Bảo mật nâng cao

- **Bảo mật:**
  - Đã kích hoạt **WAF (Web Application Firewall)** để chặn các tấn công phổ biến (SQL Injection, XSS).
  - Review IAM Roles và thu hồi các quyền hạn không cần thiết.
- **Chi phí:** Dựa trên dữ liệu chạy thử 2 tuần, đã điều chỉnh lại loại Instance từ t3.medium xuống t3.micro cho môi trường Dev để tiết kiệm chi phí mà không ảnh hưởng hiệu năng.

#### 3. Hoàn tất Dự án

- **Tài liệu hóa:** Hoàn thành bộ tài liệu dự án bao gồm: Sơ đồ kiến trúc cập nhật, hướng dẫn triển khai lại và báo cáo chi phí.
- **Bài học kinh nghiệm:** Đúc kết được kinh nghiệm về việc xử lý "Cold Start" khi Auto Scaling và tầm quan trọng của việc cấu hình Health Check chính xác
