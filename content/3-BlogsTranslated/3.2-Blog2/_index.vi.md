---
title: "Blog 2"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Tinh gọn vận hành SAP với CloudWatch MCP Server và Amazon Q CLI – Phần 4

bởi Ferry Mulyadi và Vijay Sitaram | 12 THÁNG 8 2025 | in [Amazon CloudWatch](https://aws.amazon.com/vi/blogs/awsforsap/category/management-tools/amazon-cloudwatch/), [Amazon Q](https://aws.amazon.com/vi/blogs/awsforsap/category/amazon-q/), [AWS Systems Manager](https://aws.amazon.com/vi/blogs/awsforsap/category/management-tools/aws-systems-manager/), [AWS Well-Architected](https://aws.amazon.com/vi/blogs/awsforsap/category/aws-well-architected/), [How-To](https://aws.amazon.com/vi/blogs/awsforsap/category/how-to/), [SAP on AWS](https://aws.amazon.com/vi/blogs/awsforsap/category/sap/), [Technical How-to](https://aws.amazon.com/vi/blogs/awsforsap/category/post-types/technical-how-to/), [Thought Leadership](https://aws.amazon.com/vi/blogs/awsforsap/category/post-types/thought-leadership/) | [Permalink](https://aws.amazon.com/vi/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4/) | [Share](https://aws.amazon.com/vi/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4/#)

## Giới thiệu

Tối ưu hóa vận hành SAP trên AWS đòi hỏi khả năng giám sát, xử lý sự cố và bảo trì hiệu quả. Tiếp nối phần thảo luận trước của chúng tôi về Amazon CloudWatch Application Insight trong [phần 1](https://aws.amazon.com/blogs/awsforsap/improve-your-sap-business-process-resilience-with-amazon-cloudwatch-application-insights/), cách chúng tôi sử dụng CloudWatch Application Insight để giám sát SAP High Availability trong [phần 2](http://aws.amazon.com/vi/blogs/awsforsap/enhance-your-business-process-resilience-with-amazon-cloudwatch-application-insights-observability-for-sap-high-availability-part-2/), và [Amazon CloudWatch Model Context Protocol (MCP) Server](https://github.com/awslabs/mcp) cùng [Amazon Q for command line (Q CLI)](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html) trong [phần 3](https://aws.amazon.com/vi/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-3/), bài viết thứ tư này minh họa các ứng dụng nâng cao trong thực tế của những công cụ này.

Chúng tôi sẽ khám phá cách tích hợp này tinh gọn việc lập kế hoạch bảo trì SAP và tăng tốc phân tích nguyên nhân gốc thông qua các trường hợp sử dụng thực tiễn.

Thông qua các ví dụ chi tiết, chúng tôi sẽ chỉ ra cách CloudWatch MCP Server và Q CLI phối hợp để:

- Điều phối các sự kiện bảo trì có kế hoạch với gián đoạn tối thiểu.
- Tăng tốc chẩn đoán sự cố nhờ phân tích log thông minh.
- Cung cấp khuyến nghị nhận thức theo ngữ cảnh cho vận hành SAP.
- Giảm thời gian trung bình để khắc phục (MTTR) cho các sự cố SAP.

---

## Use-case 3: Các sự kiện bảo trì có kế hoạch cho SAP

Trong trường hợp này, chúng ta sẽ thực hiện một sự kiện bảo trì có kế hoạch trong cụm ứng dụng SAP. Tích hợp quy trình làm việc cho phép nhóm kỹ sư chẩn đoán sự cố mà không cần chuyển đổi ngữ cảnh gây gián đoạn, nhờ vào việc xem trực tiếp các chỉ số CloudWatch trong IDE, đồng thời sử dụng Q CLI để tạo ra các lệnh khắc phục chính xác.

Trong kịch bản này, Q CLI đã tạo danh sách tác vụ như sau:

1.  Yêu cầu (Prompt) để kiểm tra trạng thái của App Server.
2.  Chạy `aws ec2 describe-instances` để kiểm tra trạng thái cụm SAP.
3.  Chạy lệnh `ssm crm_mon` để kiểm tra cụm ASCS và ERS.
4.  Chạy lệnh `ssm ps` để lấy trạng thái tiến trình ASCS và ERS.
5.  Chạy lệnh `ssm sapcontrol` để lấy danh sách tiến trình.
6.  Chạy lệnh `ssm` để kiểm tra `pacemaker.log`.
7.  Chạy lệnh `ssm crm` để kiểm tra trạng thái tài nguyên cụm, ràng buộc và cấu hình.
8.  Tóm tắt rằng ERS bị vô hiệu hóa do cấu hình cụm sai.
9.  Sau yêu cầu kiểm tra log CloudWatch:
    - Chạy `describe_log_groups`.
    - Chạy `analyze_log_groups`.
    - Chạy search `execute_log_insights_query` liên quan đến ASCS và ERS.
    - Chạy search `execute_log_insights_query` liên quan đến các vấn đề ERS hiện tại.
    - Tóm tắt tất cả các mục log liên quan đến vấn đề ERS.
10. Sau yêu cầu khởi động dịch vụ SAP ERS:
    - Chạy lệnh `ssm crm` để kiểm tra trạng thái tài nguyên cụm.
    - Chạy lệnh `ssm crm` để bật tài nguyên ERS (thất bại như dự kiến).
    - Chạy lệnh `ssm` để cập nhật cấu hình tài nguyên.
    - Chạy lệnh `ssm crm` để bật lại tài nguyên ERS (sau khi sửa cấu hình).
    - Chạy lệnh `ssm sapcontrol` để lấy danh sách tiến trình ASCS và ERS.
    - Tóm tắt rằng dịch vụ ERS đã được khởi động thành công.

> **Lưu ý quan trọng:** Các yêu cầu và phản hồi dưới đây đã được rút gọn. Bạn có thể tải về tài liệu [Use-case 3 – Planned maintenance events for SAP](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/17/Use-case-3-Planned-maintenance-events-for-SAP.pdf) để xem toàn bộ nội dung yêu cầu và phản hồi từ Q CLI.

**Prompt: Check the status of the SAP ASCS/ERS cluster for the SAPDemo SAP application without restarting any services.**

```text
> I'll check the status of the SAP ASCS/ERS cluster for the SAPDemo SAP application without restarting any services.
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ec2 describe-instances --filters "Name=tag:Name,Values=*SAP-FIS-CS*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,InstanceType,PrivateIpAddress]" --output table
 ⋮
 ↳ Purpose: Checking EC2 instances that are part of the SAP cluster

-------------------------------------------------------------------------------------------
|                                    DescribeInstances                                    |
+----------------------+-------------------------+----------+-------------+---------------+
|  i-064751ba1b4c8340f |  SAP-FIS-CS-sapclnode2  |  running |  c5a.xlarge |  10.0.155.180 |
|  i-0e08f883d5c3b3747 |  SAP-FIS-CS-sapclnode1  |  running |  c5a.xlarge |  10.0.134.80  |
+----------------------+-------------------------+----------+-------------+---------------+

 ⋮
 ● Completed in 1.666s
……

## Conclusion
The SAP ASCS/ERS cluster for the SAPDemo application is partially operational:
• The ASCS instance is running correctly on sapclnode2
• The ERS instance is intentionally disabled (target-role=Stopped in the configuration)
• The virtual IP for ERS is still active on sapclnode1
• Both cluster nodes are online and the cluster has quorum
This configuration appears to be intentional based on the cluster configuration, but it means that enqueue replication is not currently active. In a normal high-availability setup, both ASCS and ERS would be running to ensure enqueue table replication in case of a failover.
```

Trong kịch bản tiếp theo này, chúng ta sử dụng CloudWatch MCP Server để điều tra log của cụm pacemaker nhằm xác định trạng thái của SAP ASCS. CloudWatch MCP Server gọi các công cụ như `describe_log_groups`, `analyze_log_groups`, và `execute_log_insights_query` để thực hiện phân tích.

**Prompt: Check CloudWatch logs related to SAPDemo application for SAP ASCS ERS Pacemaker cluster events, alarms and issues.**

```text
> Check CloudWatch logs related to SAPDemo  application for SAP ASCS ERS Pacemaker cluster events, alarms and issues. only use cloudwatch mcp server for this analysis.
> I'll analyze the CloudWatch logs related to the SAPDemo application for SAP ASCS ERS Pacemaker cluster events, alarms, and issues using only the CloudWatch MCP server.
Let's start by identifying the relevant log groups for the SAPDemo application:
🛠️  Using tool: describe_log_groups (trusted) from mcp server awslabscloudwatch_mcp_server
 ⋮
 ● Running describe_log_groups with the param:
 ⋮  {
 ⋮    "arguments": {
 ⋮      "log_group_name_prefix": "PACEMAKER",
 ⋮      "region": "us-east-1"
 ⋮    },
 ⋮    "name": "describe_log_groups"
 ⋮  }

 ⋮
 ● Completed in 0.760s
……
### Analysis:
This log data shows that the ERS component (rsc_sap_ECD_ERS11) has been successfully started on sapclnode1. The cluster first cleared previous failure records, then migrated the IP resource (rsc_ip_ECD_ERS11) from node 2 to node 1, and finally started the ERS service on node 1.
The successful start is confirmed by the @rc-code=0 in the logs, indicating no errors. The monitoring operation is also successful, showing that the service is running properly.
This aligns with our previous findings where we had identified issues with the ERS component and cleared CloudWatch alarms for it. The logs now confirm that the ERS service has been successfully started and is
being monitored by the cluster, which explains why the service is now running despite previous alarms.
Would you like me to analyze any specific aspect of these logs in more detail or provide recommendations for ongoing monitoring of the ERS component?
```

**Prompt: start SAP ERS service on node sapclnode1.**

```text
> start SAP ERS services on node sapclnode1
> I'll start the SAP ERS services on node sapclnode1.
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ssm send-command --instance-ids i-0e08f883d5c3b3747 --document-name "AWS-RunShellScript" --parameters 'commands=["sudo crm resource status rsc_sap_ECD_ERS11"]' --output json
 ⋮
 ↳ Purpose: Checking the current status of the ERS resource

..
……

## Conclusion
The SAP ERS services have been successfully started on node sapclnode1. The enqueue replication is now active and functioning correctly, providing high availability for the SAP enqueue service. The cluster is now in a fully operational state with all resources running as expected.
```

---

## Use-case 4: Tăng tốc phân tích nguyên nhân gốc (Root-Cause Analysis) cho SAP

Trong kịch bản này, chúng ta có một cụm ứng dụng SAP nơi ERS gặp lỗi do cấu hình sai. Với sự tích hợp giữa Q CLI và CloudWatch MCP Server, các thách thức chẩn đoán truyền thống trong những hệ thống SAP phức tạp – nơi sự cố thường lan truyền qua nhiều lớp hệ thống – được loại bỏ. Q CLI đã tạo danh sách tác vụ như sau:

1.  Yêu cầu để kiểm tra trạng thái của App Server.
2.  Chạy `aws ec2 describe-instances` để kiểm tra trạng thái cụm SAP.
3.  Chạy lệnh `ssm crm_mon` để kiểm tra cụm ASCS và ERS.
4.  Chạy lệnh `ssm sapcontrol` để lấy danh sách tiến trình trên node 1 và node 2.
5.  Chạy lệnh `ssm crm` để kiểm tra trạng thái tài nguyên cụm và log.
6.  Chạy lệnh `ssm` để kiểm tra `pacemaker.log`.
7.  Chạy lệnh `ssm crm` để dọn dẹp (cleanup) tài nguyên.
8.  Chạy lệnh `ssm crm_mon` để kiểm tra trạng thái cụm.
9.  Chạy lệnh `ssm sapcontrol` để lấy danh sách tiến trình trên node 2.
10. Tóm tắt rằng kết quả phân tích cho thấy sự cố xuất phát từ vấn đề cấu hình.
11. Sau yêu cầu xử lý sự cố này (Prompt to troubleshoot this problem):
    - Chạy lệnh `ssm` để kiểm tra SAP System Logs cho ASCS và ERS trên node 2.
    - Chạy lệnh `ssm` để kiểm tra SAP profile của ASCS và ERS.
    - Chạy lệnh `ssm` để kiểm tra cấu hình hostname trong hosts file ở node 2.
    - Chạy lệnh `ssm` để kiểm tra địa chỉ IP của node 1 và node 2.
    - Chạy lệnh `ssm` để cập nhật hosts file ở node 1 và node 2.
    - Chạy lệnh `ssm crm_mon` để dọn dẹp tài nguyên.
    - Chạy lệnh `ssm sapcontrol` để lấy danh sách tiến trình trên node 2.
    - Chạy lệnh `ssm sapcontrol` để khởi động dịch vụ ERS, sau đó kiểm tra log.
    - Chạy lệnh `ssm sapcontrol` để khởi động ASCS, sau đó kiểm tra log.
    - Chạy lệnh `ssm crm_mon` để dừng cụm, sau đó kiểm tra trạng thái.
    - Chạy lệnh `ssm crm_mon` để dọn dẹp tài nguyên cụm.
    - Chạy lệnh `ssm crm_mon` để khởi động cụm, chờ và kiểm tra.
    - Chạy lệnh `ssm sapcontrol` để lấy danh sách tiến trình ASCS trên node 1.
    - Chạy lệnh `ssm sapcontrol` để khởi động dịch vụ ASCS, sau đó kiểm tra.
    - Tóm tắt các khuyến nghị để khắc phục sự cố.
12. Sau yêu cầu khởi động dịch vụ SAP ERS (Prompt to start the SAP ERS service):
    - Chạy lệnh `ssm crm` để dừng tài nguyên cụm ASCS và ERS.
    - Chạy lệnh `ssm` để xác minh và cập nhật hosts file trên node 1 và node 2.
    - Chạy lệnh `ssm` để kiểm tra phân giải tên miền (hostname resolution) trên node 1 và node 2 (lần đầu cập nhật thất bại, nhưng thử lại với cách khác).
    - Chạy lệnh `ssm` để kiểm tra SAP Profile của ASCS và ERS.
    - Chạy lệnh `ssm crm_mon` để dọn dẹp tài nguyên cụm.
    - Chạy lệnh `ssm sapcontrol` để khởi động tài nguyên SAP ASCS và ERS, sau đó xác minh.
    - Tóm tắt rằng các dịch vụ ASCS và ERS đã được khởi động thành công.

> **Lưu ý quan trọng:** Các yêu cầu và phản hồi dưới đây đã được rút gọn. Bạn có thể tải về tài liệu [Use-case 4 – Accelerates Root-Cause Analysis for SAP](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/17/Use-case-4-Accelerates-Root-Cause-Analysis-for-SAP.pdf) để xem toàn bộ nội dung yêu cầu và phản hồi từ Q CLI.

_[Hình 5. Trạng thái lỗi của SAP Central Services (ASCS/ERS)]_

**Prompt: What is the current status of SAP ASCS ERS cluster nodes for SAP application SAPDemo.**

```text
> what is the current status of SAP ASCS ERS cluster nodes for sap application SAPDemo.

> I'll check the current status of the SAP ASCS/ERS cluster nodes for the SAPDemo SAP application.
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ec2 describe-instances --filters "Name=tag:Name,Values=*SAP-FIS-CS*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,InstanceType,PrivateIpAddress]" --output table
 ⋮
 ↳ Purpose: Checking EC2 instances that are part of the SAP cluster

-------------------------------------------------------------------------------------------
|                                    DescribeInstances                                    |
+----------------------+-------------------------+----------+-------------+---------------+
|  i-064751ba1b4c8340f |  SAP-FIS-CS-sapclnode2  |  running |  c5a.xlarge |  10.0.155.180 |
|  i-0e08f883d5c3b3747 |  SAP-FIS-CS-sapclnode1  |  running |  c5a.xlarge |  10.0.134.80  |
+----------------------+-------------------------+----------+-------------+---------------+

 ⋮
 ● Completed in 1.815s
……..

## Current State Summary

The SAP ASCS/ERS cluster for SAPDemo is currently in a degraded state:
• The cluster infrastructure (corosync/pacemaker) is functioning correctly with quorum
• Both nodes are online and communicating
• The virtual IP addresses for both ASCS and ERS are properly assigned
• However, the actual SAP services (ASCS and ERS) are failing to start
• The application servers are showing YELLOW status, likely due to their dependency on the central services

This indicates a problem with the SAP services themselves rather than with the cluster infrastructure. The services may be failing to start due to configuration issues, file system problems, or other SAP-specific issues that need to be investigated further.
```

**Prompt: Help me troubleshoot this problem.**

```text
> help me troubleshoot this problem
> I'll help you troubleshoot the issues with the SAP ASCS/ERS cluster for the SAPDemo application. Based on the current state, both the ASCS and ERS services are failing to start properly, even though the cluster infrastructure itself is functioning correctly.
Let's take a systematic approach to troubleshooting:
## Step 1: Check SAP-specific error logs
First, let's check the SAP system logs to understand why the services are failing to start:
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ssm send-command --instance-ids i-064751ba1b4c8340f --document-name "AWS-RunShellScript" --parameters 'commands=["sudo su - ecdadm -c \"tail -50 /usr/sap/ECD/ASCS10/work/dev_ms\""]' --output json
 ⋮
 ↳ Purpose: Checking SAP system logs for ASCS on node 2
..
> Now let's check the ERS logs:
……..
> Based on my troubleshooting, I've identified the root cause of the issues with your SAP ASCS/ERS cluster and can provide you with a comprehensive solution:
# SAP ASCS/ERS Cluster Troubleshooting Report
## Root Cause Analysis
After thorough investigation, I've identified that the primary issue is related to hostname resolution. The error message MsSInit: NihostToAddr rc=(NIEHOST_UNKNOWN) in the ASCS logs clearly indicates that the
Message Server cannot resolve its hostname.
### Specific Issues Identified:

1. Hostname Resolution Problems:
   • The SAP profiles are configured to use the hostnames ecdascsnode and ecdersnode, but these hostnames were not properly defined in the /etc/hosts files on both nodes.
   • We've updated the hosts files, but the SAP services are still failing to start properly.

2. IP Address Configuration:
   • The virtual IPs (192.168.48.201 for ASCS and 192.168.48.202 for ERS) are not being properly managed by the cluster.
   • Both virtual IPs are currently assigned to node 1 (sapclnode1), which is causing conflicts.
3. SAP Profile Configuration:
   • The SAP profiles are using hostnames that don't match the actual hostnames of the cluster nodes.
   • The ERS service is trying to connect to the ASCS service but failing with "Connection refused" errors.

## Solution Steps

To fully resolve these issues, we need to take the following steps:

### 1. Fix the Hostname Resolution

We've already updated the /etc/hosts files on both nodes with the correct mappings, but we need to ensure that the SAP services can properly resolve the hostnames.

### 2. Update the SAP Profiles

The SAP profiles need to be updated to use the correct hostnames or IP addresses. This is a more involved task that requires careful editing of the SAP profiles.

### 3. Restart the Cluster Services in the Correct Order

The cluster services need to be restarted in the correct order to ensure proper operation:
1. Stop all SAP services
2. Stop the cluster
3. Fix the configuration
4. Start the cluster
5. Start the SAP services

### 4. Verify DNS Resolution

We need to ensure that DNS resolution is working correctly on both nodes.

## Recommended Actions

Here's a step-by-step plan to resolve the issues:

1. Stop the cluster resources:
  bash
   sudo crm resource stop rsc_sap_ECD_ASCS10
   sudo crm resource stop rsc_sap_ECD_ERS11

2. Verify the hosts file configuration on both nodes:
  bash
   cat /etc/hosts
  Ensure that both nodes have the following entries:
   127.0.0.1 localhost
   10.0.134.80 sapclnode1.ec2.internal sapclnode1
   10.0.155.180 sapclnode2.ec2.internal sapclnode2
   192.168.48.201 ecdascsnode
   192.168.48.202 ecdersnode

3. Test hostname resolution:
  bash
   ping -c 3 ecdascsnode
   ping -c 3 ecdersnode
   ping -c 3 sapclnode1
   ping -c 3 sapclnode2

4. Update the SAP profiles if necessary:
   • Review the SAP profiles to ensure they're using the correct hostnames
   • Consider updating the profiles to use IP addresses instead of hostnames if resolution continues to be an issue

5. Clean up the cluster resources:
  bash
   sudo crm resource cleanup rsc_sap_ECD_ASCS10
   sudo crm resource cleanup rsc_sap_ECD_ERS11

6. Start the resources in the correct order:
  bash
   sudo crm resource start rsc_sap_ECD_ASCS10
   # Wait for ASCS to fully start
   sudo crm resource start rsc_sap_ECD_ERS11

7. Verify the SAP services are running:
  bash
   sudo su - ecdadm -c "sapcontrol -nr 10 -function GetProcessList"
   sudo su - ecdadm -c "sapcontrol -nr 11 -function GetProcessList"


## Long-term Recommendations
1. Implement DNS Resolution: Consider implementing proper DNS resolution instead of relying on /etc/hosts files.
2. Review Cluster Configuration: Review the cluster configuration to ensure it's properly set up for SAP ASCS/ERS high availability.
3. Monitor the Cluster: Set up proper monitoring for the cluster to detect and alert on issues.
4. Document the Configuration: Document the cluster configuration and troubleshooting steps for future reference.
5. Regular Testing: Regularly test failover scenarios to ensure the cluster is working correctly.
By following these steps, you should be able to resolve the issues with your SAP ASCS/ERS cluster and ensure it's properly configured for high availability.
```

**Prompt: Apply the above solution to fix the problem.**

```text
> apply the above solution to fix the problem
> I'll apply the solution to fix the SAP ASCS/ERS cluster issues. Let's follow the step-by-step plan:
## Step 1: Stop the cluster resources
> I need to use SSM to execute these commands on the cluster nodes:
🛠️  Using tool: execute_bash (trusted)
 ⋮
 ● I will run the following shell command:
aws ssm send-command --instance-ids i-0e08f883d5c3b3747 --document-name "AWS-RunShellScript" --parameters 'commands=["sudo crm resource stop rsc_sap_ECD_ASCS10 && sudo crm resource stop rsc_sap_ECD_ERS11"]' --output json
 ⋮
 ↳ Purpose: Stopping the cluster resources using SSM
..
> ## Step 2: Verify the hosts file configuration on both nodes
……
> Great! The solution has been successfully applied and the SAP ASCS/ERS cluster is now working correctly. Here's a summary of what we did:

## Summary of the Fix
1. Fixed Hostname Resolution:
   • Updated the /etc/hosts files on both nodes with the correct mappings for all hostnames.
   • Verified that hostname resolution was working correctly by pinging the virtual hostnames.
2. Cleaned Up and Restarted Cluster Resources:
   • Stopped the cluster resources to ensure a clean state.
   • Cleaned up any failed resource actions.
   • Started the resources in the correct order: ASCS first, then ERS.
3. Verified the Services:
   • Confirmed that both the ASCS and ERS services are now running correctly.
   • The system instance list shows all components are GREEN (running).
## Current Status
• **Cluster Status**: All resources are running correctly.
• **ASCS Status**: The Message Server and Enqueue Server are both running on node 2 (sapclnode2).
• **ERS Status**: The Enqueue Replication Server is running on node 1 (sapclnode1).
• **Virtual IPs**: The virtual IPs are correctly assigned to the appropriate nodes.
## Recommendations for Long-term Stability
1. Regular Monitoring: Set up monitoring for the cluster to detect any issues early.
2. Failover Testing: Regularly test failover scenarios to ensure the cluster works correctly.
3. Documentation: Document the cluster configuration and troubleshooting steps for future reference.
4. DNS Resolution: Consider implementing proper DNS resolution instead of relying on /etc/hosts files.
The SAP ASCS/ERS cluster for the SAPDemo application is now functioning correctly. The issue was primarily related to hostname resolution, which we've fixed by properly configuring the hosts files on both nodes.
```

_[Hình 6. Trạng thái cuối cùng của cụm SAP Central Services (ASCS/ERS) trong tình trạng khỏe mạnh]_

---

## Cân nhắc chi phí

Để triển khai giải pháp này, bạn cần cân nhắc các yếu tố sau:

- **Chi phí Amazon Q CLI** được thể hiện trong [[liên kết này]](https://aws.amazon.com/vi/q/developer/pricing/#topic-0). Chúng tôi khuyến nghị sử dụng ít nhất bản Pro với 1.000 yêu cầu agentic mỗi tháng (19 USD/tháng/người dùng).
- **Chi phí Amazon CloudWatch** được thể hiện trong [[liên kết này]](https://aws.amazon.com/vi/cloudwatch/pricing/). Trong thử nghiệm này, chúng tôi giả định rằng gói Free tier là đủ.
- **Chi phí AWS Systems Manager** được thể hiện trong [[liên kết này]](https://aws.amazon.com/vi/systems-manager/pricing/). Với thử nghiệm HA, giả định có tối thiểu 6 node cần quản lý:
  $0.01137 \text{ USD} \times 6 \text{ nodes} \times 8 \text{ giờ} = 3.27456 \text{ USD}$
- **Không có chi phí bổ sung** nào khi sử dụng Amazon CloudWatch MCP Server.
- **Chi phí thiết lập HA cho SAP NetWeaver** (tính ở vùng us-east-1) ước tính:
  - 2 × m5.large = 148.16 USD (ASCS và ERS)
  - 2 × m5.xlarge = 280.32 USD (2 App Server)
  - 2 × r5.2xlarge = 756.32 USD (SAP HANA Primary và Secondary)

**Tổng chi phí ước tính** = 19 USD + 3.27456 USD + 39.4933 USD ≈ **61.76786 USD** cho một ngày thử nghiệm.

---

## Kết luận

CloudWatch MCP Server mang lại khả năng quan sát thống nhất trên toàn bộ hệ thống SAP trong môi trường khách hàng. Nó hợp nhất SAP application logs, SAP HANA metrics, và AWS EC2 performance data. Hệ thống tự động tương quan các chỉ số giữa các thành phần SAP. Q CLI bổ sung khả năng này bằng cách cho phép tương tác bằng ngôn ngữ tự nhiên với tập dữ liệu phong phú, cho phép nhóm dễ dàng điều tra thời gian phản hồi của phiên làm việc, so sánh mẫu tiêu thụ bộ nhớ HANA, hoặc xác định tiến trình SAP ở trạng thái chờ trong các sự cố hiệu năng.

CloudWatch MCP Server giúp xử lý sự cố SAP phức tạp bằng cách xác định các bất thường thông qua phân tích mẫu trong nhật ký hệ thống và lỗi giao dịch. Nó liên kết cảnh báo hạ tầng với hành vi ứng dụng. Ngoài ra, nó cung cấp bối cảnh lịch sử từ các sự cố tương tự trong quá khứ. Q CLI tăng cường khả năng này bằng cách diễn giải mã lỗi SAP, tạo lệnh chẩn đoán dựa trên các mẫu quan sát được, gợi ý nguyên nhân gốc rễ, và đề xuất các mã giao dịch SAP liên quan để điều tra sâu hơn.

Sự tích hợp này thu hẹp khoảng cách kiến thức giữa SAP BASIS administrators và AWS infrastructure teams bằng cách tạo ra một góc nhìn vận hành chung của hệ thống SAP và chuyển đổi giữa thuật ngữ SAP và khái niệm AWS. Sự kết hợp này giảm mean time to resolution bằng cách loại bỏ chuyển đổi ngữ cảnh, cung cấp phân tích thông minh về các mẫu đặc thù SAP, và cho phép tương tác bằng ngôn ngữ tự nhiên với dữ liệu giám sát — dẫn đến xử lý sự cố nhanh hơn, độ tin cậy cao hơn, và vận hành hiệu quả hơn cho các workload SAP quan trọng trên AWS.

Bước tiếp theo, hãy thử áp dụng giải pháp này, bạn có thể đạt được năng suất cao gấp ít nhất 6 lần. Hãy đón chờ những đổi mới sắp tới liên quan đến [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [AWS Model Context Protocol Servers]() và [Amazon Q for command line](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html).

---

## Tham gia thảo luận SAP trên AWS

Ngoài đội ngũ quản lý tài khoản khách hàng và kênh AWS Support, chúng tôi vừa ra mắt [re:Post](https://repost.aws/) – Một trải nghiệm Hỏi & Đáp được tái thiết kế cho cộng đồng AWS. Nhóm AWS for SAP Solution Architecture thường xuyên theo dõi chủ đề AWS for SAP để tham gia thảo luận và trả lời các câu hỏi nhằm hỗ trợ khách hàng và đối tác. Nếu câu hỏi của bạn không liên quan đến hỗ trợ kỹ thuật, hãy tham gia thảo luận tại re:Post và đóng góp vào cơ sở tri thức cộng đồng.

---

## Lời cảm ơn

Xin gửi lời cảm ơn đến các thành viên trong nhóm đã đóng góp: **Sreenath Middhi** và **Adam Hill**.

**TAGS:** [#SAP Netweaver](https://aws.amazon.com/blogs/awsforsap/tag/sap-netweaver/), [AWS Systems Manager](https://aws.amazon.com/blogs/awsforsap/tag/aws-systems-manager/), [CloudWatch](https://aws.amazon.com/blogs/awsforsap/tag/cloudwatch/), [SAP](https://aws.amazon.com/blogs/awsforsap/tag/sap/), [SAP High Availability](https://aws.amazon.com/blogs/awsforsap/tag/sap-high-availability/)
