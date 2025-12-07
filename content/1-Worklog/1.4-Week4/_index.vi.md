---
title: "Worklog Tuần 4"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 4:

- Kết nối, làm quen với các thành viên trong First Cloud Journey.
- Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:

# Tuần 4 – Làm chủ Lưu trữ Đối tượng: Amazon S3, Di chuyển Dữ liệu & Chiến lược Phục hồi Thảm họa

| Ngày  | Nhiệm vụ                                                                                                                                                                                                                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :---- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :----------------- |
| **2** | - Tìm hiểu lý thuyết module 4: Amazon Simple Storage (S3)<br>- Access point<br>- Lớp lưu trữ (Storage Class)<br>- Website tĩnh & CORS<br>- Kiểm soát truy cập (Control Access)<br>- Endpoint & Lập phiên bản (Versioning)<br>- Object Key & Hiệu năng<br>- Glacier<br>- Snow Family - Snowmobile<br>- AWS Storage Gateway<br>- RTO/RPO<br>- Phục hồi thảm họa trên AWS (Disaster Recovery)<br>- AWS Backup | 29/09/2025   | 29/09/2025      | S3                 |
| **3** | Thực hành module 4 - bài Lab 13                                                                                                                                                                                                                                                                                                                                                                            | 30/09/2025   | 30/09/2025      |                    |
| **4** | Thực hành module 4 - bài Lab 14                                                                                                                                                                                                                                                                                                                                                                            | 01/10/2025   | 01/10/2025      |                    |
| **5** |                                                                                                                                                                                                                                                                                                                                                                                                            | 02/10/2025   | 02/10/2025      |                    |
| **6** |                                                                                                                                                                                                                                                                                                                                                                                                            | 03/10/2025   | 03/10/2025      |                    |

---

### Thu Hoạch Tuần 4: Nắm Vững Lưu Trữ Đối Tượng và Chiến Lược DR

- **Năng lực cốt lõi về Amazon S3 (Amazon S3 Core Competencies):** Đã làm chủ việc cấu hình S3, từ các Lớp lưu trữ (Storage Classes) để tối ưu chi phí đến tính năng Lập phiên bản (Versioning) để bảo vệ dữ liệu. Thành thạo việc triển khai Hosting Website tĩnh và cấu hình bảo mật với Bucket Policies và CORS.
- **Bảo vệ & Quản lý Dữ liệu (Data Protection & Management):** Hiểu rõ các giải pháp lưu trữ dữ liệu lớn và Hybrid (Snow Family, Storage Gateway). Quan trọng hơn, đã biết cách tự động hóa quản lý dữ liệu bằng các Quy tắc vòng đời (Life Cycle Rules) và thiết lập Sao chép chéo vùng (Cross-Region Replication).
- **Phục hồi Thảm họa (Disaster Recovery - DR):** Đã phân tích được mối quan hệ giữa các chỉ số kinh doanh (RTO/RPO) và 4 mô hình DR khác nhau. Đây là kiến thức nền tảng quan trọng cho việc thiết kế kiến trúc tin cậy (Reliability).
- **Ứng dụng Thực hành (Hands-on Application):** Việc hoàn thành các bài Lab về Hosting, Policies và Life Cycle đã củng cố khả năng triển khai thực tế S3 như một giải pháp lưu trữ và phân phối nội dung mạnh mẽ.
