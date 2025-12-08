---
title: "Worklog Tuần 7"
date: "2025-10-20"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 7:

- Hệ thống hóa toàn bộ kiến thức đã học ở nửa đầu chương trình.
- Tập trung sâu vào hai trụ cột chính: Bảo mật (Security) và Tính kiên cường (Resiliency).
- Chuẩn bị nền tảng vững chắc cho kỳ thi giữa kỳ.

### Các công việc cần triển khai trong tuần này:

| Ngày  | Nhiệm vụ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                             |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :----------------------------------------------------------------------------- |
| **2** | **Ôn tập Giữa kỳ (Midterm Review):**<br><br>**Phần 1: Kiến trúc Bảo mật (Secure Architecture)**<br>- IAM (Quản lý danh tính & truy cập)<br>- KMS (Quản lý khóa mã hóa)<br>- Security Group & NACLs (Tường lửa mạng)<br>- Secrets Manager (Quản lý bí mật)<br>- GuardDuty (Phát hiện mối đe dọa)<br>- Shield & WAF (Bảo vệ DDoS & Ứng dụng Web)<br><br>**Phần 2: Kiến trúc Kiên cường (Resilient Architecture)**<br>- Các chỉ số cốt lõi cần ghi nhớ (RTO/RPO)<br>- Multi-AZ RDS (Cơ sở dữ liệu đa vùng)<br>- Các chiến lược Phục hồi thảm họa (DR Strategies)<br>- Auto Scaling và Load Balancing<br>- Route 53 và DNS<br>- AWS Backup | 20/10/2025   | 20/10/2025      | [Link ôn tập](https://ruskicoder.github.io/fcj-midterm-2025/)                  |
| **3** | Tự ôn tập: Hệ thống hóa lại các dịch vụ bảo mật và thực hành Lab mô phỏng                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 21/10/2025   | 21/10/2025      |                                                                                |
| **4** | Tự ôn tập: Vẽ lại sơ đồ kiến trúc High Availability (HA) và Disaster Recovery (DR)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 22/10/2025   | 22/10/2025      |                                                                                |
| **5** | Làm các bài kiểm tra thử (Practice Tests) để rà soát kiến thức                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 23/10/2025   | 23/10/2025      | [Luyện tập](https://ezse.net/aws/certified-cloud-practitioner/examtopics.html) |
| **6** | Tổng hợp các câu hỏi thắc mắc và xem lại các ghi chú quan trọng                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 24/10/2025   | 24/10/2025      | [Tips](https://ruskicoder.github.io/fcj-midterm-2025/6-additional-resources/)  |

---

### Thu Hoạch Tuần 7: Củng Cố Tư Duy Bảo Mật và Kiến Trúc Bền Vững

- **Tư duy Bảo mật Đa lớp (Defense in Depth):**

  - Đã phân biệt rõ ràng sự khác biệt và phạm vi áp dụng của **Security Group** (Stateful - cấp Instance) và **NACLs** (Stateless - cấp Subnet).
  - Hiểu sâu về cách bảo vệ ứng dụng ở các tầng khác nhau: **WAF** cho tầng ứng dụng (Layer 7), **Shield** cho hạ tầng (Layer 3/4), và **GuardDuty** để giám sát thông minh các hành vi bất thường.
  - Nắm vững tầm quan trọng của **KMS** và **Secrets Manager** trong việc bảo vệ dữ liệu nhạy cảm "at rest" và "in transit".

- **Kiến trúc Bền vững & Khả năng Phục hồi (Resiliency & Recovery):**
  - Ghi nhớ và hiểu bản chất của các chỉ số **RTO (Recovery Time Objective)** và **RPO (Recovery Point Objective)** để lựa chọn chiến lược DR phù hợp (Backup & Restore, Pilot Light, Warm Standby, hay Multi-site Active/Active).
  - Nắm vững cơ chế **Multi-AZ RDS** để đảm bảo tính sẵn sàng cao (High Availability) so với Read Replica (dùng để mở rộng khả năng đọc).
  - Hiểu rõ sự phối hợp giữa **Route 53, ELB và Auto Scaling** để tạo nên một hệ thống linh hoạt, tự động phục hồi khi có sự cố và chịu tải tốt.
