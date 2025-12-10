---
title: "Worklog Tuần 10"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu Tuần 10:

- **Thiết kế giải pháp (Solution Architecture):** Hoàn thiện bản vẽ kiến trúc High-Level (HLD) và Low-Level (LLD) dựa trên AWS Well-Architected Framework.
- **Quy hoạch tài nguyên & Chi phí:** Ước tính chi phí vận hành (TCO) và lựa chọn dịch vụ phù hợp.
- **Khởi tạo hạ tầng mạng (Networking):** Thiết lập nền móng VPC, Subnet và các chính sách bảo mật cơ bản.

### Các nhiệm vụ thực hiện trong tuần này:

| Ngày    | Nhiệm vụ                                                                                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :----------------- |
| **2-3** | **Thiết kế & Quy hoạch:**<br>- Phân tích yêu cầu nghiệp vụ dự án.<br>- Vẽ sơ đồ kiến trúc hệ thống (Architecture Diagram).<br>- Sử dụng AWS Pricing Calculator để lập bảng dự toán chi phí.<br>- Review kiến trúc theo 5 trụ cột của Well-Architected Framework. | 10/11/2025   | 11/11/2025      |                    |
| **4-5** | **Triển khai Hạ tầng mạng (IaC):**<br>- Viết code (Terraform/CloudFormation) để khởi tạo VPC, Public/Private Subnets, NAT Gateway, Route Tables.<br>- Thiết kế và cấu hình các Security Group cơ bản cho Bastion Host, Web Server và Database.                   | 12/11/2025   | 13/11/2025      |                    |

---

### Thành tựu Tuần 10: Bản vẽ Kiến trúc & Nền móng Hạ tầng

#### 1. Thiết kế Kiến trúc Hệ thống

- **Sơ đồ kiến trúc:** Hoàn thành sơ đồ luồng dữ liệu và bố trí tài nguyên. Hệ thống được thiết kế theo mô hình **Single-AZ**.
- **Lựa chọn dịch vụ:**
  - **Database:** RDS MySQL (Multi-AZ) cho dữ liệu quan hệ.
  - **Storage:** S3 cho static assets và backup.
- **Tối ưu chi phí:** Đã lập bảng dự toán ngân sách hàng tháng và xác định các phương án tiết kiệm (như sử dụng Spot Instances cho môi trường Dev, Savings Plans cho Prod).

#### 2. Triển khai Mạng lưới (VPC)

- **Cấu trúc mạng:** Thiết lập thành công VPC với dải CIDR tùy chỉnh, chia tách rõ ràng giữa Public Subnet và Private Subnet (cho App, DB).
- **Bảo mật mạng:**
  - Thiết lập **Security Groups** theo nguyên tắc đặc quyền tối thiểu (Least Privilege).
