---
title: "Worklog Tuần 9"
date: "2025-11-10"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 9:

- Xây dựng và huấn luyện mô hình gợi ý (Recommendation Model).
- Tinh chỉnh dữ liệu và xử lý các vấn đề phát sinh trong quá trình huấn luyện.

### Các công việc cần triển khai trong tuần này:

| Ngày  | Nhiệm vụ                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo       |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-------------- | :----------------------- |
| **2** | **Xây dựng mô hình (Model Building):**<br>- Thiết lập cấu hình mô hình (Recipe)<br>- Nhập dữ liệu (Import job)<br>- Chạy huấn luyện (Training)                                   | 10/11/2025   | 11/11/2025      | Tài liệu AWS Personalize |
| **3** | Kiểm tra sơ bộ kết quả mô hình và phân tích lỗi                                                                                                                                  | 12/11/2025   | 12/11/2025      |                          |
| **4** | **Cập nhật & Xử lý lại dữ liệu (Data Refinement):**<br>- Thêm các trường dữ liệu mới vào Schema<br>- Làm sạch và đồng bộ lại dataset<br>- Huấn luyện lại mô hình với dữ liệu mới | 12/11/2025   | 13/11/2025      |                          |
| **5** | Đánh giá lại các chỉ số sau khi cập nhật dữ liệu                                                                                                                                 | 14/11/2025   | 14/11/2025      |                          |
| **6** |                                                                                                                                                                                  |              |                 |                          |

---

### Thu Hoạch Tuần 9: Thách Thức trong Huấn Luyện Mô Hình và Chất Lượng Dữ Liệu

- **Hạn chế về Dữ liệu (Data Scarcity & Interaction types):**

  - **Vấn đề:** Lượng dữ liệu còn khá ít và quan trọng hơn là chỉ có một loại tương tác duy nhất là "Booking" (Đặt sân). Việc thiếu các tương tác dẫn dắt như "View" (Xem) hay "Click" khiến mô hình khó nắm bắt được sở thích tiềm ẩn của người dùng.
  - **Hệ quả:** Điểm số đánh giá (metrics) của mô hình sau khi train khá thấp, gây ra sự hoài nghi lớn về độ chính xác và hiệu quả thực tế của các gợi ý.

- **Vấn đề Kiểm chứng (Validation Blockers):**

  - Do giao diện Website (Frontend) chưa hoàn thiện, tôi chưa thể thực hiện kiểm tra thực tế (Visual check) để xem các gợi ý có hợp lý với mắt thường hay không. Hiện tại chỉ có thể dựa vào các chỉ số khô khan.

- **Quản lý thay đổi Dữ liệu (Schema Evolution):**
  - Việc phát sinh thêm các trường thông tin mới trong quá trình phát triển đã gây khó khăn cho luồng xử lý dữ liệu (pipeline). Mỗi lần thay đổi cấu trúc dữ liệu đòi hỏi phải làm sạch lại, map lại schema và train lại mô hình từ đầu, tốn khá nhiều thời gian và công sức.
