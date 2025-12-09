---
title: "Blog 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Tối ưu hóa vận hành SAP với CloudWatch MCP server và Amazon Q CLI – Phần 3

**bởi Ferry Mulyadi và Vijay Sitaram | 11 THÁNG 8 2025 | in [Amazon CloudWatch](https://aws.amazon.com/blogs/awsforsap/category/management-tools/amazon-cloudwatch/), [Amazon Q](https://aws.amazon.com/blogs/awsforsap/category/amazon-q/), [Amazon Q Developer](https://aws.amazon.com/blogs/awsforsap/category/amazon-q/amazon-q-developer/), [SAP on AWS](https://aws.amazon.com/blogs/awsforsap/category/sap/), [Technical How-to](https://aws.amazon.com/blogs/awsforsap/category/post-types/technical-how-to/), [Thought Leadership](https://aws.amazon.com/blogs/awsforsap/category/post-types/thought-leadership/) | Permalink | Share**

## Giới thiệu

Trong các môi trường SAP phức tạp ngày nay, hiệu quả vận hành và khả năng khắc phục sự cố nhanh chóng là yếu tố then chốt để đảm bảo hoạt động kinh doanh liên tục. Tiếp nối những thảo luận trước về khả năng quan sát SAP ([phần 1](https://aws.amazon.com/blogs/awsforsap/improve-your-sap-business-process-resilience-with-amazon-cloudwatch-application-insights/)) và tính năng [Amazon CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html) ([phần 2](https://aws.amazon.com/blogs/awsforsap/enhance-your-business-process-resilience-with-amazon-cloudwatch-application-insights-observability-for-sap-high-availability-part-2/)), bài viết thứ ba này giới thiệu sự kết hợp mạnh mẽ của các công cụ đang thay đổi cách các nhóm quản lý hệ thống SAP: **Amazon CloudWatch Model Context Protocol (MCP) Server** và **Amazon Q cho command line (Q CLI)**.

Giải pháp sáng tạo này giải quyết ba thách thức lớn trong vận hành SAP:

- Cú pháp lệnh phức tạp làm chậm tiến trình vận hành
- Phân tích nguyên nhân gốc (root cause) tốn thời gian, cần nhiều công cụ và ngữ cảnh khác nhau
- Nhu cầu giám sát sức khỏe hệ thống và báo cáo một cách tinh gọn

Trong blog này, chúng ta sẽ:

- Tìm hiểu cách CloudWatch MCP Server và Q CLI phối hợp để đơn giản hóa vận hành SAP
- Trình bày các use case thực tế để tạo báo cáo sức khỏe hệ thống tự động
- Minh họa cách lệnh ngôn ngữ tự nhiên có thể tối ưu hóa tác vụ bảo trì thường kỳ
- Chỉ ra sự phù hợp của giải pháp này với [AWS Well-Architected Framework for SAP](https://docs.aws.amazon.com/wellarchitected/latest/sap-lens/sap-lens.html)

[Phần thứ tư](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4) của chuỗi bài sẽ tập trung vào lập kế hoạch bảo trì, phân tích nguyên nhân gốc nâng cao, và xem xét chi phí chi tiết khi triển khai giải pháp này.

![Hình 1. Sơ đồ kiến trúc Amazon CloudWatch MCP Server và Amazon Q cho CLI](https://lh7-us.googleusercontent.com/docs/AHkbwyIRCm0pSvViYxI1DLvPHBsIqs0Q23Y7tvS-MqIl4Fwyuf32EHYC7Jv2uUxjOzl5cKjPYFdyU3vOrEFzxVIviKZ4D9e2Tq57yVYR7-1Ztf5rVV4OlXIC=w1200-h630-p)
_Hình 1. Sơ đồ kiến trúc Amazon CloudWatch MCP Server và Amazon Q cho CLI_

## Amazon Q cho CLI (Q CLI) là gì?

Amazon Q CLI mang lại giá trị chuyển đổi cho khách hàng AWS bằng cách tích hợp trợ lý AI trực tiếp vào quy trình làm việc trên command line. Nó cho phép người dùng diễn đạt lệnh bằng ngôn ngữ tự nhiên, sau đó Q dịch thành cú pháp CLI chuẩn, rút ngắn đáng kể đường cong học tập. Công cụ cung cấp gợi ý lệnh theo ngữ cảnh dựa trên ý định của người dùng, loại bỏ việc phải tìm kiếm tài liệu tốn thời gian. Khi xảy ra lỗi, Q cung cấp giải thích rõ ràng và các bước khắc phục, tăng tốc quá trình xử lý sự cố.

Q phân tích lệnh thành các thành phần dễ hiểu, giúp nâng cao khả năng tiếp thu và kỹ năng. Bằng việc giữ người dùng trong môi trường terminal, Q tối ưu hóa quy trình bằng cách loại bỏ sự gián đoạn do phải chuyển đổi sang tài nguyên bên ngoài.

Sự cải thiện về năng suất này giúp giảm thời gian tìm kiếm ví dụ cú pháp và xử lý sự cố, cho phép các nhà phát triển tập trung vào việc xây dựng thay vì mất thời gian với cấu trúc lệnh. Tính tương tác của Q đẩy nhanh quá trình học tập, giúp người dùng thành thạo AWS CLI nhanh hơn. Cuối cùng, Amazon Q CLI biến trải nghiệm dòng lệnh từ một rào cản tiềm ẩn thành một giao diện trực quan, khiến các dịch vụ AWS trở nên dễ tiếp cận và hiệu quả hơn cho khách hàng ở mọi cấp độ chuyên môn.

![Hình 2. Giao diện nhắc lệnh Amazon Q CLI](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdLtgDgRtioSucxjMb39jtEQKuvezHzwXqM56tTQzjFLdaUeNJXujzcn69RKYhIiRIow9SsqbJZLL91xRQ4n26Gh5MnW-LYhCEz78N1rrgfdlk6VgSDFS_uixqwXBynMnMPUJ2xdQ?key=_rIm0DIFuPyNHJJhyMmm5Q)
_Hình 2. Giao diện nhắc lệnh Amazon Q CLI_

## Amazon CloudWatch Model Context Protocol (MCP) Server là gì?

Model Context Protocol (MCP) là một chuẩn mã nguồn mở cách mạng hóa cách công cụ phát triển giao tiếp với mô hình AI. Bằng cách cung cấp bối cách có cấu trúc về mã, tệp và dự án, MCP cho phép các trợ lý AI dưa ra các đề xuất được điều chỉnh chính xác. Một ứng dụng thực tế là [CloudWatch MCP Server](https://github.com/awslabs/mcp/tree/main/src/cloudwatch-mcp-server), tích hợp metrics và logs từ CloudWatch trực tiếp vào môi trường phát triển. Nhờ đó, developer có thể giám sát và khắc phục sự cố mà không rời khỏi IDE (Integrated Development Environment), tối ưu hóa quy trình vận hành.

AWS Labs CloudWatch MCP Server kết nối giữa AI troubleshooting agent và dữ liệu vận hành. Nó cho phép phân tích nguyên nhân gốc dựa trên AI và đưa ra khuyến nghị, sử dụng CloudWatch từ xa mà không cần tích hợp API tùy chỉnh.

Server có 4 công cụ chuyên biệt:

- **Alarm Based Troubleshooting** xác định cảnh báo đang hoạt động và phân tích mẫu lịch sử để tìm nguyên nhân gốc, đưa ra khuyến nghị khắc phục theo ngữ cảnh.
- **Log Analyzer** kiểm tra log group của CloudWatch để tìm bất thường và lỗi theo khoảng thời gian.
- **Metric Definition Analyzer** cung cấp mô tả về metrics, phương pháp tính toán và thống kê khuyến nghị.
- **Alarm Recommendations** đề xuất cấu hình cảnh báo tối ưu với ngưỡng và chu kỳ đánh giá phù hợp.

### Các công cụ trong CloudWatch MCP Server là gì?

Khi người dùng nhập một prompt bằng ngôn ngữ tự nhiên trong Q CLI, công cụ từ CloudWatch MCP Server sẽ được gọi để tương tác với dịch vụ AWS nhằm thực hiện công việc.
Bạn có thể bắt đầu Q từ client bằng cách gõ ‘q’ và dùng `/tools` hoặc `/mcp` để xem danh sách công cụ khả dụng và các MCP server đã nạp trong môi trường.

![Hình 3. Công cụ Q CLI Server được nạp trong phiên client](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcw0jCTtzQB8tI0BJNIUIcwYo1VCEjs4OJDiY_GatKFQeMEcq5MMWHoJcUuLJJvTS9lOBUugDBUZwEx_YmzLURf7LRGWDa1N3UQ6zktZgp8Ant62AAuR8foltIpYGCM2wYpxBQN?key=_rIm0DIFuPyNHJJhyMmm5Q)
_Hình 3. Công cụ Q CLI Server được nạp trong phiên client_

![Hình 4. CloudWatch MCP server được nạp trong phiên client](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfn5tDf-XMyj4IpBWqPu5GsX0QfSbo6xhirHRmzaIiGqi4-T21I0B8b2_cFfO1Q1jsAtHgn2hmnYM8lRhmbRJzAgiRpdWTBqswa1zTx8uzwMZEeFGRoTkVTxH4n-XHyQi84FQwKUA?key=_rIm0DIFuPyNHJJhyMmm5Q)
_Hình 4. CloudWatch MCP server được nạp trong phiên client_

## Giải pháp này phù hợp với Well-Architected Framework for SAP Lens như thế nào?

- **Vận hành SAP tinh gọn:** Q CLI cải tiến các quy trình vận hành SAP bằng cách cho phép kỹ sư sử dụng ngôn ngữ tự nhiên để thực hiện các thao tác AWS hỗ trợ khối lượng công việc SAP. Điều này phù hợp với trọng tâm của SAP Lens về tự động hóa và giảm nỗ lực thủ công trong quản lý các hệ thống kinh doanh quan trọng, khi các lệnh CLI cho hạ tầng SAP có thể được tạo thông qua các yêu cầu hội thoại đơn giản.
- **Tăng tốc xử lý sự cố SAP:** Sự kết hợp này giảm thời gian trung bình để xử lý sự cố (mean time to resolution) cho các tình huống SAP bằng cách cung cấp dữ liệu giám sát theo ngữ cảnh và phân tích thành phần SAP có hỗ trợ AI, từ đó hỗ trợ khôi phục nhanh khỏi các lỗi ảnh hưởng đến hoạt động kinh doanh.
- **Quan sát tích hợp dành cho SAP:** Giải pháp loại bỏ việc chuyển đổi ngữ cảnh bằng cách hợp nhất các chỉ số, log và cảnh báo đặc thù SAP trực tiếp vào môi trường máy khách, hỗ trợ khả năng giám sát đầu-cuối trên các hệ thống SAP phức tạp. Việc tích hợp này giúp các nhóm quan sát hành vi hệ thống SAP mà không bị gián đoạn quy trình làm việc.
- **Phân tích tác động thay đổi SAP:** Công cụ giúp các nhóm hiểu cách các thay đổi ảnh hưởng đến độ ổn định hệ thống SAP bằng cách cung cấp phản hồi tức thời về các chỉ số vận hành trên toàn bộ tầng công nghệ SAP.
- **Xác minh khả năng phục hồi SAP:** Các công cụ đơn giản hóa việc giám sát cơ chế khôi phục SAP và ranh giới cô lập lỗi trên các hệ thống SAP phân tán, cũng như kiểm tra độ tin cậy của các quy trình và giao dịch quan trọng.
- **Hiểu biết về kiến trúc SAP:** Q CLI có thể giải thích các mối quan hệ giữa dịch vụ AWS và ứng dụng SAP dựa trên dữ liệu CloudWatch, giúp nhóm của bạn có thêm góc nhìn về độ tin cậy của kiến trúc SAP.
- **Dân chủ hóa tri thức SAP:** Công cụ giúp tổ chức “vận hành SAP bằng code”. Khả năng giải thích dòng lệnh của Q CLI cho phép nhóm của bạn vận hành hiệu quả trên Landscape SAP cùng với thông tin đầy đủ để đưa ra quyết định dựa trên dữ liệu.

## Điều kiện tiên quyết

- Triển khai hệ thống SAP S/4 HANA có High Availability bằng [AWS Launch Wizard for SAP](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap.html)
- Kích hoạt khả năng quan sát full-stack cho SAP theo [Amazon CloudWatch Application Insights for SAP](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-tutorial-sap-netweaver.html)
- Cài đặt Amazon Q CLI theo các bước trong [Installing Amazon Q for command line](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html)
- Cấu hình [CloudWatch MCP Server](https://github.com/awslabs/mcp/tree/main/src/cloudwatch-mcp-server) trên máy của bạn theo hướng dẫn trong AWS Labs CloudWatch MCP repository
- Cấu hình AWS CLI trên máy của bạn theo hướng dẫn trong [configuration settings for the AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/cli-chap-configure.html)

Việc truy cập giải pháp được bảo mật bằng [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/), đồng thời xem xét các yếu tố [Q CLI Security considerations and best practices](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-chat-security.html) cũng như [Security best practices for Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-best-practices.html). Áp dụng mô hình truy cập tối thiểu (least-privilege) cho SAP Basis và Kỹ sư Hạ tầng AWS.

## Use-case 1: Tạo báo cáo tình trạng (Health Reports) cho SAP

Use case này sẽ tạo báo cáo sức khỏe cho cơ sở dữ liệu SAP HANA và cụm ABAP SAP Central Services (SAP ASCS). Q CLI tạo các lệnh dòng lệnh theo ngữ cảnh để phát hiện các phiên bản Amazon EC2 và xây dựng báo cáo tình trạng. Trong kịch bản này, Q CLI chạy nhiều lệnh để kiểm tra trạng thái SAP Central Services và tạo báo cáo hoàn chỉnh:

1.  Nhập prompt để tạo báo cáo sức khỏe của cụm SAP
2.  Chạy `aws ec2 describe-instances` cho các phiên bản thuộc cụm
3.  Chạy lệnh `ssm crm_mon` để kiểm tra trạng thái các node trong cụm
4.  Chạy lệnh `ssm` để kiểm tra `pacemaker.log`
5.  Chạy lệnh `ssm` để kiểm tra `corosync.log`
6.  Chạy lệnh `ssm` để kiểm tra `corosync.log`

**Lưu ý quan trọng:** Các prompt và phản hồi dưới đây đã được rút gọn, bạn có thể tải [Use-case 1 - Create Health Reports for SAP](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/16/Use-case-1-Create-Health-Reports-for-SAP.pdf) để xem đầy đủ prompt và phản hồi từ Q CLI.

**Prompt: Generate a health report for SAP HANA database, database cluster, and HSR.**

```
> generate a health report for SAP Hana database, database cluster and HSR

> # SAP HANA Health Report

## Executive Summary
Based on the analysis of CloudWatch logs for the SAP HANA system in the SAPDemo environment, the overall health status is GOOD. The SAP HANA database, cluster infrastructure, and HANA System Replication (HSR) are
all functioning properly after recovering from replication issues that occurred on July 11-12, 2025.
……

## Conclusion

The SAP HANA database, cluster infrastructure, and HSR are currently healthy and functioning as expected. The system has demonstrated resilience by recovering from previous replication issues. Regular monitoring
and proactive maintenance are recommended to maintain system stability and performance.
```

**Prompt: What is the current status of SAP ASCS ERS cluster nodes for sap application SAPDemo.**

```
> what is the current status of SAP ASCS ERS cluster nodes for sap application SAPDemo.

> I'll check the current status of the SAP ASCS and ERS cluster nodes for the SAPDemo SAP application.


🛠️ Using tool: execute_bash (trusted)
⋮
● I will run the following shell command:
aws ec2 describe-instances --filters "Name=tag:Name,Values=*SAP-FIS-CS*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,InstanceType,PrivateIpAddress]" --output table
⋮
↳ Purpose: Checking EC2 instances that are part of the SAP *SAPDemocluster

-------------------------------------------------------------------------------------------
| DescribeInstances |
+----------------------+-------------------------+----------+-------------+---------------+
| i-064751ba1b4c8340f | SAP-FIS-CS-sapclnode2 | running | c5a.xlarge | 10.0.155.180 |
| i-0e08f883d5c3b3747 | SAP-FIS-CS-sapclnode1 | running | c5a.xlarge | 10.0.134.80 |
+----------------------+-------------------------+----------+-------------+---------------+

⋮
● Completed in 1.737s

……

## Conclusion
The SAP ASCS/ERS cluster for the *SAPDemo application is functioning properly with the expected resource distribution:
• ASCS (Central Services) is active on sapclnode2
• ERS (Enqueue Replication Server) is active on sapclnode1
• Both nodes are online and healthy
• The cluster has quorum and is properly configured for high availability

This is the expected configuration for an SAP high-availability setup, where ASCS and ERS run on separate nodes to ensure enqueue replication functionality in case of a node failure. The cluster is operating normally and no immediate action is required.
```

Đối với các hoạt động phức tạp, Q CLI hướng dẫn các quy trình nhiều bước trong khi MCP cung cấp phản hồi thời gian thực về trạng thái hệ thống.

## Use-case 2: Đơn giản hóa vận hành SAP

Use case sau nhằm bảo trì SAP Application Servers. Q CLI tinh gọn quản lý SAP hàng ngày bằng cách tạo runbook cho các tác vụ bảo trì định kỳ. Quản trị viên SAP BASIS có thể dùng Q CLI để quản lý tác vụ hành chính đồng thời theo dõi trạng thái hệ thống theo thời gian thực thông qua CloudWatch MCP server trong thời gian bảo trì. Trong kịch bản này, QCLI tạo danh sách tác vụ như sau:

1.  Prompt để kiểm tra trạng thái App Server
2.  Chạy `aws ec2 describe-instances` để kiểm tra SAP application server
3.  Chạy lệnh `ssm ps` để kiểm tra SAP dispatcher và dialog process
4.  Chạy lệnh `ssm sapcontrol` để lấy trạng thái hệ thống
5.  Tóm tắt rằng SAP application server đang dừng
6.  Sau prompt để khởi động SAP application server
7.  Chạy lệnh `ssm sapcontrol` để khởi động SAP application server
8.  Chạy lệnh `ssm sapcontrol` để kiểm tra trạng thái các tiến trình
9.  Chạy lệnh `ssm ps` để kiểm tra dialog process SAP
10. Chạy lệnh `ssm sapcontrol` để lấy trạng thái hệ thống
11. Tóm tắt rằng SAP application server đã được khởi động

**Lưu ý quan trọng:** Các prompt và phản hồi dưới đây đã được rút gọn, bạn có thể tải [Use-case 2 - Simplify SAP Operations](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/17/Use-case-2-Simplify-SAP-Operations.pdf) để xem đầy đủ prompt và phản hồi từ Q CLI.

**Prompt: Get status of sap in application server sapbaseaas1. Do not start the server, only report on current status of SAP process.**

```
> get status of sap in application server sapbaseaas1. Do not start the server, Only report on current status of SAP process.

> I'll check the current status of SAP processes on the application server sapbaseaas1 without starting the server.

🛠️ Using tool: execute_bash (trusted)
⋮
● I will run the following shell command:
aws ec2 describe-instances --filters "Name=tag:Name,Values=*sapbaseaas1*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,PrivateIpAddress]" --output table
⋮
↳ Purpose: Finding the instance ID for sapbaseaas1
..
> Now I'll check the current status of the SAP processes on the running sapbaseaas1 server:
……..
## Conclusion
The SAP application server sapbaseaas1 is currently stopped. While the EC2 instance itself is running and the SAP support processes are active, the actual SAP application instance (dialog instance) is not running.
No work processes are active, and the instance is showing as GRAY in the system status. The instance would need to be started to become operational and process SAP transactions.
```

Trong kịch bản tiếp theo, quản trị viên SAP BASIS sẽ yêu cầu Amazon Q từ CLI khởi động một SAP application server, dịch vụ SAP liên quan và báo cáo trạng thái ứng dụng SAP. Công cụ `execute_bash` của Q CLI chạy 8 lệnh SSM để thực hiện yêu cầu.

**Prompt: Start SAP on the SAP application server sapbaseaas1.**

```
> start SAP on the SAP application server sapbaseaas1

> I'll start the SAP application on the sapbaseaas1 server.


🛠️ Using tool: execute_bash (trusted)
⋮
● I will run the following shell command:
aws ssm send-command --instance-ids i-0539ecbaf407c4886 --document-name "AWS-RunShellScript" --parameters 'commands=["sudo su - ecdadm -c \"sapcontrol -nr 00 -function Start\""]' --output json
⋮
↳ Purpose: Starting SAP instance on sapbaseaas1

..

> Now let's check if the SAP instance has started successfully:

……..

## Conclusion
The SAP application server sapbaseaas1 is now fully operational. All required processes are running, and the instance is properly integrated with the SAP system. The instance is ready to handle user workloads and
process SAP transactions.
```

## Kết luận

CloudWatch MCP Server mang đến khả năng quan sát hợp nhất trên toàn bộ landscape SAP trong môi trường máy khách. Nó hợp nhất log ứng dụng SAP, chỉ số SAP HANA và dữ liệu hiệu năng AWS EC2. Hệ thống tự động liên kết các chỉ số giữa các thành phần SAP. Q CLI bổ sung bằng cách cho phép tương tác ngôn ngữ tự nhiên với tập dữ liệu phong phú này, giúp các nhóm dễ dàng tạo log hệ thống và đơn giản hóa vận hành hệ thống SAP. Giải pháp này phù hợp với AWS Well-Architected Frameworks để giúp bạn tăng năng suất trong việc vận hành và duy trì Landscape SAP.

Trong các bài blog tiếp theo, chúng tôi sẽ thảo luận về cách bạn có thể tận dụng CloudWatch MCP Server và Q CLI để lập kế hoạch bảo trì và tăng tốc phân tích nguyên nhân gốc (root cause analysis) cho SAP, cũng như đi sâu vào cân nhắc chi phí của giải pháp.

Bước tiếp theo, hãy thử nghiệm giải pháp này — bạn có thể đạt năng suất gấp ít nhất 6 lần. Hãy đón chờ những đổi mới tiếp theo của chúng tôi về [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [AWS Model Context Protocol Servers](https://awslabs.github.io/mcp/) và [Amazon Q for command line](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html).

## Tham gia thảo luận SAP trên AWS

Ngoài đội ngũ tài khoản khách hàng và các kênh hỗ trợ AWS, chúng tôi gần đây đã ra mắt [re:Post](https://repost.aws/) – Trải nghiệm Hỏi & Đáp được tái tưởng dành cho Cộng đồng AWS. Đội **AWS for SAP Solution Architecture** của chúng tôi thường xuyên theo dõi chủ đề **AWS for SAP** để thảo luận và trả lời các câu hỏi nhằm hỗ trợ khách hàng và đối tác. Nếu câu hỏi của bạn không liên quan đến hỗ trợ kỹ thuật, hãy tham gia thảo luận tại re:Post và đóng góp vào kho tri thức cộng đồng.

## Lời cảm ơn

Tôi xin cảm ơn các thành viên sau vì những đóng góp của họ: **Sreenath Middhi** và **Adam Hill**.

TAGS: [#saponaws](https://aws.amazon.com/blogs/awsforsap/tag/saponaws/), [AWS Systems Manager](https://aws.amazon.com/blogs/awsforsap/tag/aws-systems-manager/), [CloudWatch](https://aws.amazon.com/blogs/awsforsap/tag/cloudwatch/), [SAP](https://aws.amazon.com/blogs/awsforsap/tag/sap/)
