---
title: "Bản đề xuất"
date: "2025-09-08"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# IoT Weather Platform for Lab Research

## Giải pháp AWS Serverless hợp nhất cho giám sát thời tiết thời gian thực

### 1. Tóm tắt điều hành

IoT Weather Platform được thiết kế dành cho nhóm _ITea Lab_ tại TP. Hồ Chí Minh nhằm nâng cao khả năng thu thập và phân tích dữ liệu thời tiết. Nền tảng hỗ trợ tối đa 5 trạm thời tiết, có khả năng mở rộng lên 10–15 trạm, sử dụng thiết bị biên Raspberry Pi kết hợp cảm biến ESP32 để truyền dữ liệu qua MQTT. Nền tảng tận dụng các dịch vụ AWS Serverless để cung cấp giám sát thời gian thực, phân tích dự đoán và tiết kiệm chi phí, với quyền truy cập giới hạn cho 5 thành viên phòng lab thông qua Amazon Cognito.

### 2. Tuyên bố vấn đề

_Vấn đề hiện tại_  
Các trạm thời tiết hiện tại yêu cầu thu thập dữ liệu thủ công, khó quản lý khi có nhiều trạm. Không có hệ thống tập trung cho dữ liệu hoặc phân tích thời gian thực, và các nền tảng bên thứ ba thường tốn kém và quá phức tạp.

_Giải pháp_  
Nền tảng sử dụng AWS IoT Core để tiếp nhận dữ liệu MQTT, AWS Lambda và API Gateway để xử lý, Amazon S3 để lưu trữ (bao gồm data lake), và AWS Glue Crawlers cùng các tác vụ ETL để trích xuất, chuyển đổi, tải dữ liệu từ S3 data lake sang một S3 bucket khác để phân tích. AWS Amplify với Next.js cung cấp giao diện web, và Amazon Cognito đảm bảo quyền truy cập an toàn. Tương tự như Thingsboard và CoreIoT, người dùng có thể đăng ký thiết bị mới và quản lý kết nối, nhưng nền tảng này hoạt động ở quy mô nhỏ hơn và phục vụ mục đích sử dụng nội bộ. Các tính năng chính bao gồm bảng điều khiển thời gian thực, phân tích xu hướng và chi phí vận hành thấp.

_Lợi ích và hoàn vốn đầu tư (ROI)_  
Giải pháp tạo nền tảng cơ bản để các thành viên phòng lab phát triển một nền tảng IoT lớn hơn, đồng thời cung cấp nguồn dữ liệu cho những người nghiên cứu AI phục vụ huấn luyện mô hình hoặc phân tích. Nền tảng giảm bớt báo cáo thủ công cho từng trạm thông qua hệ thống tập trung, đơn giản hóa quản lý và bảo trì, đồng thời cải thiện độ tin cậy dữ liệu. Chi phí hàng tháng ước tính 0,66 USD (theo AWS Pricing Calculator), tổng cộng 7,92 USD cho 12 tháng. Tất cả thiết bị IoT đã được trang bị từ hệ thống trạm thời tiết hiện tại, không phát sinh chi phí phát triển thêm. Thời gian hoàn vốn 6–12 tháng nhờ tiết kiệm đáng kể thời gian thao tác thủ công.

### 3. Kiến trúc giải pháp

Nền tảng áp dụng kiến trúc AWS Serverless để quản lý dữ liệu từ 5 trạm dựa trên Raspberry Pi, có thể mở rộng lên 15 trạm. Dữ liệu được tiếp nhận qua AWS IoT Core, lưu trữ trong S3 data lake và xử lý bởi AWS Glue Crawlers và ETL jobs để chuyển đổi và tải vào một S3 bucket khác cho mục đích phân tích. Lambda và API Gateway xử lý bổ sung, trong khi Amplify với Next.js cung cấp bảng điều khiển được bảo mật bởi Cognito.

![IoT Weather Station Architecture](/images/2-Proposal/edge_architecture.jpeg)

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

_Dịch vụ AWS sử dụng_

- _AWS IoT Core_: Tiếp nhận dữ liệu MQTT từ 5 trạm, mở rộng lên 15.
- _AWS Lambda_: Xử lý dữ liệu và kích hoạt Glue jobs (2 hàm).
- _Amazon API Gateway_: Giao tiếp với ứng dụng web.
- _Amazon S3_: Lưu trữ dữ liệu thô (data lake) và dữ liệu đã xử lý (2 bucket).
- _AWS Glue_: Crawlers lập chỉ mục dữ liệu, ETL jobs chuyển đổi và tải dữ liệu.
- _AWS Amplify_: Lưu trữ giao diện web Next.js.
- _Amazon Cognito_: Quản lý quyền truy cập cho người dùng phòng lab.

_Thiết kế thành phần_

- _Thiết bị biên_: Raspberry Pi thu thập và lọc dữ liệu cảm biến, gửi tới IoT Core.
- _Tiếp nhận dữ liệu_: AWS IoT Core nhận tin nhắn MQTT từ thiết bị biên.
- _Lưu trữ dữ liệu_: Dữ liệu thô lưu trong S3 data lake; dữ liệu đã xử lý lưu ở một S3 bucket khác.
- _Xử lý dữ liệu_: AWS Glue Crawlers lập chỉ mục dữ liệu; ETL jobs chuyển đổi để phân tích.
- _Giao diện web_: AWS Amplify lưu trữ ứng dụng Next.js cho bảng điều khiển và phân tích thời gian thực.
- _Quản lý người dùng_: Amazon Cognito giới hạn 5 tài khoản hoạt động.

### 4. Triển khai kỹ thuật

_Các giai đoạn triển khai_  
Dự án gồm 2 phần — thiết lập trạm thời tiết biên và xây dựng nền tảng thời tiết — mỗi phần trải qua 4 giai đoạn:

1. _Nghiên cứu và vẽ kiến trúc_: Nghiên cứu Raspberry Pi với cảm biến ESP32 và thiết kế kiến trúc AWS Serverless (1 tháng trước kỳ thực tập).
2. _Tính toán chi phí và kiểm tra tính khả thi_: Sử dụng AWS Pricing Calculator để ước tính và điều chỉnh (Tháng 1).
3. _Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp_: Tinh chỉnh (ví dụ tối ưu Lambda với Next.js) để đảm bảo hiệu quả (Tháng 2).
4. _Phát triển, kiểm thử, triển khai_: Lập trình Raspberry Pi, AWS services với CDK/SDK và ứng dụng Next.js, sau đó kiểm thử và đưa vào vận hành (Tháng 2–3).

_Yêu cầu kỹ thuật_

- _Trạm thời tiết biên_: Cảm biến (nhiệt độ, độ ẩm, lượng mưa, tốc độ gió), vi điều khiển ESP32, Raspberry Pi làm thiết bị biên. Raspberry Pi chạy Raspbian, sử dụng Docker để lọc dữ liệu và gửi 1 MB/ngày/trạm qua MQTT qua Wi-Fi.
- _Nền tảng thời tiết_: Kiến thức thực tế về AWS Amplify (lưu trữ Next.js), Lambda (giảm thiểu do Next.js xử lý), AWS Glue (ETL), S3 (2 bucket), IoT Core (gateway và rules), và Cognito (5 người dùng). Sử dụng AWS CDK/SDK để lập trình (ví dụ IoT Core rules tới S3). Next.js giúp giảm tải Lambda cho ứng dụng web fullstack.

### 5. Lộ trình & Mốc triển khai

- _Trước thực tập (Tháng 0)_: 1 tháng lên kế hoạch và đánh giá trạm cũ.
- _Thực tập (Tháng 1–3)_:
  - Tháng 1: Học AWS và nâng cấp phần cứng.
  - Tháng 2: Thiết kế và điều chỉnh kiến trúc.
  - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.
- _Sau triển khai_: Nghiên cứu thêm trong vòng 1 năm.

### 6. Ước tính ngân sách

Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  
Hoặc tải [tệp ước tính ngân sách](../attachments/budget_estimation.pdf).

_Chi phí hạ tầng_

- AWS Lambda: 0,00 USD/tháng (1.000 request, 512 MB lưu trữ).
- S3 Standard: 0,15 USD/tháng (6 GB, 2.100 request, 1 GB quét).
- Truyền dữ liệu: 0,02 USD/tháng (1 GB vào, 1 GB ra).
- AWS Amplify: 0,35 USD/tháng (256 MB, request 500 ms).
- Amazon API Gateway: 0,01 USD/tháng (2.000 request).
- AWS Glue ETL Jobs: 0,02 USD/tháng (2 DPU).
- AWS Glue Crawlers: 0,07 USD/tháng (1 crawler).
- MQTT (IoT Core): 0,08 USD/tháng (5 thiết bị, 45.000 tin nhắn).

_Tổng_: 0,7 USD/tháng, 8,40 USD/12 tháng

- _Phần cứng_: 265 USD một lần (Raspberry Pi 5 và cảm biến).

### 7. Đánh giá rủi ro

_Ma trận rủi ro_

- Mất mạng: Ảnh hưởng trung bình, xác suất trung bình.
- Hỏng cảm biến: Ảnh hưởng cao, xác suất thấp.
- Vượt ngân sách: Ảnh hưởng trung bình, xác suất thấp.

_Chiến lược giảm thiểu_

- Mạng: Lưu trữ cục bộ trên Raspberry Pi với Docker.
- Cảm biến: Kiểm tra định kỳ, dự phòng linh kiện.
- Chi phí: Cảnh báo ngân sách AWS, tối ưu dịch vụ.

_Kế hoạch dự phòng_

- Quay lại thu thập thủ công nếu AWS gặp sự cố.
- Sử dụng CloudFormation để khôi phục cấu hình liên quan đến chi phí.

### 8. Kết quả kỳ vọng

_Cải tiến kỹ thuật_: Dữ liệu và phân tích thời gian thực thay thế quy trình thủ công. Có thể mở rộng tới 10–15 trạm.  
_Giá trị dài hạn_: Nền tảng dữ liệu 1 năm cho nghiên cứu AI, có thể tái sử dụng cho các dự án tương lai.
