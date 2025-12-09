---
title: "Worklog Tuần 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

- Cài đặt môi trường.
- Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:

| Ngày  | Nhiệm vụ                                                                                                                                                                                                                                       | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                      |
| :---- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :---------------------------------------------------------------------- |
| **2** | - Học cách vẽ quy trình sử dụng bộ icon AWS<br>- Chuẩn bị cho khóa học (cài đặt các gói yêu cầu: powershell, winget, go, hugo)<br>- Học cách viết markdown và tạo website với hugo                                                             | 08/09/2025   | 08/09/2025      |                                                                         |
| **3** | - Làm quen với AWS<br>- AWS là gì<br>- Cơ sở hạ tầng AWS (Infrastructure)<br>- Tối ưu hóa chi phí và Hỗ trợ AWS<br>- Tạo tài khoản AWS                                                                                                         | 09/09/2025   | 09/09/2025      | [Tạo tài khoản AWS đầu tiên của bạn](https://000001.awsstudygroup.com/) |
| **4** | - Tìm hiểu về Amazon Virtual Private Cloud (VPC)<br>- VPC<br>- Subnet<br>- Bảng định tuyến (Route table)<br>- Giao diện mạng đàn hồi (ENI)<br>- VPC Endpoint<br>- Internet Gateway<br>- NAT Gateway                                            | 10/09/2025   | 10/09/2025      |                                                                         |
| **5** | - Bảo mật VPC và tính năng Multi VPC<br>- Nhóm bảo mật (Security Group - SG)<br>- Danh sách kiểm soát truy cập mạng (NACL)<br>- Nhật ký lưu lượng VPC (VPC Flow Logs)<br>- VPC Peering & Transit Gateway<br>- VPC Peering<br>- Transit Gateway | 11/09/2025   | 11/09/2025      |                                                                         |
| **6** | - VPN & Direct Connect<br>- VPN Site to Site<br>- VPN Client to Site<br>- AWS Direct Connect<br>- Cân bằng tải đàn hồi (Elastic Load Balancer)<br>- Elastic Load Balancing<br>- Network Load Balancer<br>- Gateway Load Balancer               | 12/09/2025   | 12/09/2025      |                                                                         |

---

### Thu Hoạch Tuần 1: Nắm Vững Nền Tảng Mạng và Tư Duy Bảo Mật

**Kiến thức Cốt lõi:**
Đã xây dựng khung hiểu biết vững chắc về Cơ sở hạ tầng toàn cầu của AWS (AWS Global Infrastructure) và Amazon VPC.

**Kỹ năng Thực hành:**

- **Thiết lập:** Hoàn tất việc tạo tài khoản an toàn (sử dụng IAM User) và thiết lập môi trường tài liệu (Hugo/Markdown).
- **VPC:** Tự tay tạo một VPC với các Subnet (Public/Private), IGW, và NAT Gateway hoạt động ổn định.
- **Bảo mật Mạng:** Phân biệt và biết cách áp dụng hiệu quả Security Group và NACL để kiểm soát lưu lượng.
- **Tư duy Cloud:** Bắt đầu hình thành tư duy về bảo mật (Least Privilege - Đặc quyền tối thiểu) và tối ưu đường truyền (VPC Endpoint).

**Khó khăn:**
Việc tính toán các khối CIDR (CIDR Blocks) và cập nhật các Bảng định tuyến (Route Table) khi thiết lập NAT Gateway và VPC Peering đòi hỏi sự chính xác cao và dễ nhầm lẫn ban đầu. Cần luyện tập thêm.
