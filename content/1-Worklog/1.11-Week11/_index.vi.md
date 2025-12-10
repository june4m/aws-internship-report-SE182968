---
title: "Worklog Tuần 11"
date: "2025-11-17"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu Tuần 11:

- **Triển khai Core Services:** Cấu hình Compute, Database.
- **Tự động hóa (DevOps):** Xây dựng quy trình CI/CD để tự động hóa việc build và deploy code lên môi trường AWS.

### Các nhiệm vụ thực hiện trong tuần này:

| Ngày    | Nhiệm vụ                                                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :----------------- |
| **2-3** | **Cấu hình Compute & DB:**<br>- Khởi tạo RDS với Single-AZ.<br>- Kết nối MySQL                                                                                                                                                                              | 17/11/2025   | 18/11/2025      |                    |
| **4-5** | **Xây dựng CI/CD Pipeline:**<br>- Thiết lập Repo trên CodeCommit/GitHub.<br>- Cấu hình CodeBuild để đóng gói ứng dụng.<br>- Sử dụng CodeDeploy/CodePipeline để đẩy code tự động xuống EC2 Instance.<br>- Kiểm tra luồng triển khai code từ Local lên Cloud. | 19/11/2025   | 20/11/2025      |                    |

---

### Thành tựu Tuần 11: Hệ thống Hoạt động & Pipeline Tự động

#### 1. Triển khai Ứng dụng Khả dụng cao

- **Database:** RDS đã hoạt động ổn định ở chế độ Multi-AZ, đảm bảo dữ liệu an toàn khi có sự cố tại một AZ. Đã kết nối thành công App Server với DB.

#### 2. Quy trình DevOps (CI/CD)

- **Pipeline hoàn chỉnh:** Xây dựng thành công luồng CI/CD (Continuous Integration/Continuous Deployment).
  - **Source:** Code được đẩy lên GitHub/CodeCommit.
  - **Build:** Tự động cài đặt dependencies và đóng gói artifact.
  - **Deploy:** Tự động cập nhật phiên bản mới lên toàn bộ EC2 instances trong ASG mà không gây gián đoạn dịch vụ (Zero Downtime Deployment - sử dụng chiến lược Rolling Update).
