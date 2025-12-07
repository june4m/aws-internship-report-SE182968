---
title: "Worklog Tuần 7"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 7:

- Kết nối, làm quen với các thành viên trong First Cloud Journey.
- Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:

# Tuần 7 – Kỹ thuật Dữ liệu Dự án & Triển khai Gợi ý Thông minh với AWS Personalize

| Ngày  | Nhiệm vụ                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo     |
| :---- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :--------------------- |
| **2** | - Thiết kế, xây dựng dữ liệu cho dự án<br>- user (người dùng)<br>- club (câu lạc bộ)<br>- court (sân)<br>- booking (đặt sân)<br>- operation (vận hành)                                                                                                                              | 03/11/2025   | 03/11/2025      |                        |
| **3** | - Tìm hiểu về các mô hình cho hệ thống gợi ý:<br>- Lọc cộng tác (Collaborative Filtering)<br>- Lọc dựa trên nội dung (Content based)<br>- Phân rã ma trận (Matrix Factorization CF)                                                                                                 | 04/11/2025   | 04/11/2025      | tài liệu               |
| **4** | - Tìm hiểu về dữ liệu cần thiết để huấn luyện (train) Personalize, cách nhập (import) dữ liệu lên S3 buckets và cách Personalize lấy dữ liệu từ bucket, định dạng schema<br>- Hiểu về các chỉ số (metrics) đánh giá, loại độ chính xác và Siêu tham số (Hyperparameter) của mô hình | 05/11/2025   | 05/11/2025      | Video 1<br><br>Video 2 |
| **5** | - Xử lý dữ liệu cho mô hình                                                                                                                                                                                                                                                         | 06/11/2025   | 07/11/2025      |                        |
| **6** |                                                                                                                                                                                                                                                                                     |              |                 |                        |

---

### Kết quả đạt được trong tuần 7

Tuần này tập trung vào phần “bộ não” của dự án: thiết kế kiến trúc dữ liệu và chuẩn bị tập dữ liệu (dataset) để huấn luyện mô hình gợi ý bằng Amazon Personalize.

**1. Thiết kế Sơ đồ Cơ sở dữ liệu & Tạo Dữ liệu (Database Schema Design & Data Generation)**

- Xác định và xây dựng cấu trúc dữ liệu cho các thực thể chính: Users (Người dùng), Clubs (Câu lạc bộ), Courts (Sân), Bookings (Đặt sân), Operations (Vận hành).
- Tạo dữ liệu tổng hợp (synthetic data) mô phỏng môi trường thực tế.
- Đảm bảo dữ liệu có tính liên kết và phản ánh đúng hành vi người dùng.

**2. Các nguyên tắc cơ bản của Hệ thống Gợi ý (Recommendation System Fundamentals)**

- Nghiên cứu và so sánh các thuật toán gợi ý:
  - Collaborative Filtering (Lọc cộng tác)
  - Content-based Filtering (Lọc dựa trên nội dung)
  - Matrix Factorization (Phân rã ma trận)
- Hiểu rõ cách mỗi mô hình hoạt động và trường hợp sử dụng phù hợp.

**3. Triển khai AWS Personalize (AWS Personalize Implementation)**

- Nắm vững quy trình đầu cuối (end-to-end) của Amazon Personalize:
  - **Data Ingestion (Nhập dữ liệu):** chuẩn hóa schema JSON, tải dữ liệu lên S3, nhập vào Personalize.
  - **Model Training (Huấn luyện mô hình):** hiểu Hyperparameters (Siêu tham số) và cách điều chỉnh.
  - **Evaluation (Đánh giá):** đọc và phân tích các Metrics (Chỉ số) đánh giá để xác định độ chính xác của mô hình.

**4. Mô hình hóa Tương tác (Interaction Modeling)**

- Giải quyết bài toán "cold start" (khởi động lạnh) và thiếu dữ liệu bằng cách chọn Booking làm tín hiệu tương tác chính (high-intent interaction - tương tác có ý định cao).
- Điều chỉnh chiến lược mô hình để phù hợp với dữ liệu tổng hợp, trong bối cảnh thiếu các tương tác như xem/nhấp chuột (view/click).

**5. Chiến lược Tiền xử lý Dữ liệu (Data Pre-processing Strategy)**

- Xây dựng hệ thống logic để gán điểm cho các tương tác.
- Làm sạch dữ liệu, chuẩn hóa định dạng và đảm bảo tuân thủ yêu cầu nghiêm ngặt của Personalize.
- Chuẩn bị dataset hoàn chỉnh để sẵn sàng cho bước train mô hình.
