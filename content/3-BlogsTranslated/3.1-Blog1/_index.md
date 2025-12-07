---
title: "Blog 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only, please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Optimizing SAP Operations with CloudWatch MCP server and Amazon Q CLI – Part 3

_by Ferry Mulyadi and Vijay Sitaram | AUGUST 11, 2025 | in [Amazon CloudWatch](https://aws.amazon.com/blogs/awsforsap/category/management-tools/amazon-cloudwatch/), [Amazon Q](https://aws.amazon.com/blogs/awsforsap/category/amazon-q/), [Amazon Q Developer](https://aws.amazon.com/blogs/awsforsap/category/amazon-q/amazon-q-developer/), [SAP on AWS](https://aws.amazon.com/blogs/awsforsap/category/sap/), [Technical How-to](https://aws.amazon.com/blogs/awsforsap/category/post-types/technical-how-to/), [Thought Leadership](https://aws.amazon.com/blogs/awsforsap/category/post-types/thought-leadership/) | [Permalink](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-3/) | [Share_](https://aws.amazon.com/vi/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-3/#)

## Introduction

In today's complex SAP environments, operational efficiency and rapid troubleshooting capabilities are key factors in ensuring business continuity. Following previous discussions on SAP observability ([Part 1](https://aws.amazon.com/blogs/awsforsap/improve-your-sap-business-process-resilience-with-amazon-cloudwatch-application-insights/)) and [Amazon CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html) features ([Part 2](https://aws.amazon.com/blogs/awsforsap/enhance-your-business-process-resilience-with-amazon-cloudwatch-application-insights-observability-for-sap-high-availability-part-2/)), this third article introduces a powerful combination of tools that is changing how teams manage SAP systems: **Amazon CloudWatch Model Context Protocol (MCP) Server** and **Amazon Q for command line (Q CLI)**.

This innovative solution addresses three major challenges in SAP operations:

- Complex command syntax slowing down operational progress
- Time-consuming root cause analysis requiring multiple tools and contexts
- The need for lean system health monitoring and reporting

In this blog, we will:

- Explore how CloudWatch MCP Server and Q CLI collaborate to simplify SAP operations
- Present real-world use cases for creating automated system health reports
- Illustrate how natural language commands can optimize routine maintenance tasks
- Demonstrate the alignment of this solution with the **AWS Well-Architected Framework for SAP**

[Part four](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4) of the series will focus on maintenance planning, advanced root cause analysis, and a detailed cost review when deploying this solution.

_Figure 1. Architecture diagram of Amazon CloudWatch MCP Server and Amazon Q for CLI_

## What is Amazon Q for CLI (Q CLI)?

Amazon Q CLI delivers transformative value to AWS customers by integrating an AI assistant directly into the command line workflow. It allows users to express commands in natural language, which Q then translates into standard CLI syntax, significantly shortening the learning curve. The tool provides contextual command suggestions based on user intent, eliminating time-consuming documentation searches. When errors occur, Q provides clear explanations and troubleshooting steps, accelerating the resolution process.

Q decomposes commands into easy-to-understand components, enhancing comprehension and skills. By keeping users within the terminal environment, Q optimizes workflows by eliminating interruptions caused by switching to external resources.

This productivity improvement helps reduce time spent searching for syntax examples and troubleshooting, allowing developers to focus on building rather than wrestling with command structures. Q's interactivity accelerates learning, helping users master the AWS CLI faster. Ultimately, Amazon Q CLI transforms the command line experience from a potential barrier into an intuitive interface, making AWS services more accessible and efficient for customers of all expertise levels.

_Figure 2. Amazon Q CLI prompt interface_

## What is Amazon CloudWatch Model Context Protocol (MCP) Server?

The Model Context Protocol (MCP) is an open-source standard that revolutionizes how development tools communicate with AI models. By providing structured context about code, files, and projects, MCP enables AI assistants to provide precisely tailored recommendations. A practical application is the **CloudWatch MCP Server**, which integrates metrics and logs from CloudWatch directly into the development environment. As a result, developers can monitor and troubleshoot without leaving their IDE (Integrated Development Environment), optimizing operational workflows.

The [AWS Labs CloudWatch MCP Server](https://github.com/awslabs/mcp/tree/main/src/cloudwatch-mcp-server) bridges the gap between AI troubleshooting agents and operational data. It enables AI-driven root cause analysis and recommendations, using CloudWatch remotely without needing custom API integrations.

The Server features 4 specialized tools:

- **Alarm Based Troubleshooting:** Identifies active alarms and analyzes historical patterns to find root causes, offering contextual remediation recommendations.
- **Log Analyzer:** Inspects CloudWatch log groups for anomalies and errors over specific time intervals.
- **Metric Definition Analyzer:** Provides descriptions of metrics, calculation methods, and recommended statistics.
- **Alarm Recommendations:** Suggests optimal alarm configurations with appropriate thresholds and evaluation periods.

## What are the tools in CloudWatch MCP Server?

When a user enters a natural language prompt in Q CLI, a tool from the CloudWatch MCP Server is invoked to interact with AWS services to perform the task.
You can start Q from the client by typing `q` and using `/tools` or `/mcp` to view the list of available tools and loaded MCP servers in the environment.

_Figure 3. Q CLI Server tools loaded in the client session_

_Figure 4. CloudWatch MCP server loaded in the client session_

## How does this solution align with the Well-Architected Framework for SAP Lens?

- **Lean SAP Operations:** Q CLI improves SAP operational processes by allowing engineers to use natural language to execute AWS operations supporting SAP workloads. This aligns with the [SAP Lens](https://docs.aws.amazon.com/wellarchitected/latest/sap-lens/sap-lens.html) focus on automation and reducing manual effort in managing critical business systems, as CLI commands for SAP infrastructure can be generated through simple conversational requests.
- **Accelerated SAP Troubleshooting:** This combination reduces the mean time to resolution (MTTR) for SAP scenarios by providing contextual monitoring data and AI-assisted analysis of SAP components, thereby supporting rapid recovery from errors affecting business operations.
- **Integrated Observability for SAP:** The solution eliminates context switching by consolidating SAP-specific metrics, logs, and alarms directly into the client environment, supporting end-to-end monitoring across complex SAP systems. This integration helps teams observe SAP system behavior without workflow interruptions.
- **SAP Change Impact Analysis:** The tool helps teams understand how changes affect SAP system stability by providing immediate feedback on operational metrics across the entire SAP technology stack.
- **SAP Resilience Verification:** The tools simplify monitoring of SAP recovery mechanisms and fault isolation boundaries across distributed SAP systems, as well as testing the reliability of critical processes and transactions.
- **Understanding SAP Architecture:** Q CLI can explain relationships between AWS services and SAP applications based on CloudWatch data, giving your team additional perspective on SAP architectural reliability.
- **Democratizing SAP Knowledge:** The tool helps organizations "operate SAP as code." Q CLI's command-line explanation capabilities allow your team to operate efficiently on the SAP Landscape with sufficient information to make data-driven decisions.

## Prerequisites

- Deployment of an **SAP S/4 HANA** system with High Availability using [AWS Launch Wizard for SAP](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap.html)
- Enablement of full-stack observability for SAP via [Amazon CloudWatch Application Insights for SAP](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-tutorial-sap-netweaver.html)
- Installation of **Amazon Q CLI** following steps in [Installing Amazon Q for command line](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html)
- Configuration of **CloudWatch MCP Server** on your machine following instructions in the [AWS Labs CloudWatch MCP repository](https://github.com/awslabs/mcp/tree/main/src/cloudwatch-mcp-server)
- Configuration of **AWS CLI** on your machine following instructions in [configuration settings for the AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/cli-chap-configure.html)

Access to the solution is secured by [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/); also review [Q CLI Security considerations and best practices](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-chat-security.html) as well as [Security best practices for Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-best-practices.html). Apply the least-privilege access model for SAP Basis and AWS Infrastructure Engineers.

## Use-case 1: Create Health Reports for SAP

This use case will create a health report for the SAP HANA database and the SAP Central Services (SAP ASCS) cluster. Q CLI generates contextual command-line commands to detect Amazon EC2 instances and build the health report. In this scenario, Q CLI runs multiple commands to check the status of SAP Central Services and generate a complete report:

1.  Enter prompt to generate the cluster health report.
2.  Run `aws ec2 describe-instances` for instances belonging to the cluster.
3.  Run `ssm crm_mon` command to check the status of nodes in the cluster.
4.  Run `ssm` command to check `pacemaker.log`.
5.  Run `ssm` command to check `corosync.log`.
6.  Run `ssm` command to check `corosync.log`.

**Important Note:** The prompts and responses below have been abbreviated. You can download [Use-case 1 - Create Health Reports for SAP](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/16/Use-case-1-Create-Health-Reports-for-SAP.pdf) to view the full prompt and response from Q CLI.

**Prompt: Generate a health report for SAP HANA database, database cluster, and HSR.**

> generate a health report for SAP Hana database, database cluster and HSR
>
> \# SAP HANA Health Report
>
> ## Executive Summary
>
> Based on the analysis of CloudWatch logs for the SAP HANA system in the SAPDemo environment, the overall health status is GOOD. The SAP HANA database, cluster infrastructure, and HANA System Replication (HSR) are
> all functioning properly after recovering from replication issues that occurred on July 11-12, 2025.
> ……
>
> ## Conclusion
>
> The SAP HANA database, cluster infrastructure, and HSR are currently healthy and functioning as expected. The system has demonstrated resilience by recovering from previous replication issues. Regular monitoring
> and proactive maintenance are recommended to maintain system stability and performance.

**Prompt: What is the current status of SAP ASCS ERS cluster nodes for sap application SAPDemo.**

> what is the current status of SAP ASCS ERS cluster nodes for sap application SAPDemo.
>
> I'll check the current status of the SAP ASCS and ERS cluster nodes for the SAPDemo SAP application.
>
> 🛠️ Using tool: execute*bash (trusted)
> ⋮
> ● I will run the following shell command:
> aws ec2 describe-instances --filters "Name=tag:Name,Values=\_SAP-FIS-CS*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,InstanceType,PrivateIpAddress]" --output table
> ⋮
> ↳ Purpose: Checking EC2 instances that are part of the SAP \*SAPDemocluster
>
> ---
>
> | DescribeInstances |
> +----------------------+-------------------------+----------+-------------+---------------+
> | i-064751ba1b4c8340f | SAP-FIS-CS-sapclnode2 | running | c5a.xlarge | 10.0.155.180 |
> | i-0e08f883d5c3b3747 | SAP-FIS-CS-sapclnode1 | running | c5a.xlarge | 10.0.134.80 |
> +----------------------+-------------------------+----------+-------------+---------------+
>
> ⋮
> ● Completed in 1.737s
>
> ……
>
> ## Conclusion
>
> The SAP ASCS/ERS cluster for the \*SAPDemo application is functioning properly with the expected resource distribution:
> • ASCS (Central Services) is active on sapclnode2
> • ERS (Enqueue Replication Server) is active on sapclnode1
> • Both nodes are online and healthy
> • The cluster has quorum and is properly configured for high availability
>
> This is the expected configuration for an SAP high-availability setup, where ASCS and ERS run on separate nodes to ensure enqueue replication functionality in case of a node failure. The cluster is operating normally and no immediate action is required.

For complex operations, Q CLI guides multi-step processes while MCP provides real-time feedback on system status.

## Use-case 2: Simplify SAP Operations

The following use case aims to maintain SAP Application Servers. Q CLI streamlines daily SAP management by creating runbooks for routine maintenance tasks. SAP BASIS administrators can use Q CLI to manage administrative tasks while monitoring real-time system status via the CloudWatch MCP server during maintenance windows. In this scenario, Q CLI creates a task list as follows:

1.  Prompt to check App Server status.
2.  Run `aws ec2 describe-instances` to check the SAP application server.
3.  Run `ssm ps` command to check SAP dispatcher and dialog processes.
4.  Run `ssm sapcontrol` command to get system status.
5.  Summarize that the SAP application server is stopped.
6.  Prompt to start the SAP application server.
7.  Run `ssm sapcontrol` command to start the SAP application server.
8.  Run `ssm sapcontrol` command to check the status of processes.
9.  Run `ssm ps` command to check SAP dialog processes.
10. Run `ssm sapcontrol` command to get system status.
11. Summarize that the SAP application server has been started.

**Important Note:** The prompts and responses below have been abbreviated. You can download [Use-case 2 - Simplify SAP Operations](https://d2908q01vomqb2.cloudfront.net/17ba0791499db908433b80f37c5fbc89b870084b/2025/07/17/Use-case-2-Simplify-SAP-Operations.pdf) to view the full prompt and response from Q CLI.

**Prompt: Get status of sap in application server sapbaseaas1. Do not start the server, only report on current status of SAP process.**

> get status of sap in application server sapbaseaas1. Do not start the server, Only report on current status of SAP process.
>
> I'll check the current status of SAP processes on the application server sapbaseaas1 without starting the server.
> 🛠️ Using tool: execute*bash (trusted)
> ⋮
> ● I will run the following shell command:
> aws ec2 describe-instances --filters "Name=tag:Name,Values=\_sapbaseaas1*" --query "Reservations[*].Instances[*].[InstanceId,Tags[?Key=='Name'].Value|[0],State.Name,PrivateIpAddress]" --output table
> ⋮
> ↳ Purpose: Finding the instance ID for sapbaseaas1
> ..
>
> > Now I'll check the current status of the SAP processes on the running sapbaseaas1 server:
> > ……..
>
> ## Conclusion
>
> The SAP application server sapbaseaas1 is currently stopped. While the EC2 instance itself is running and the SAP support processes are active, the actual SAP application instance (dialog instance) is not running.
> No work processes are active, and the instance is showing as GRAY in the system status. The instance would need to be started to become operational and process SAP transactions.

In the next scenario, the SAP BASIS administrator will ask Amazon Q from the CLI to start an SAP application server, the associated SAP service, and report on the SAP application status. Q CLI's `execute_bash` tool runs 8 SSM commands to execute the request.

**Prompt: Start SAP on the SAP application server sapbaseaas1.**

> start SAP on the SAP application server sapbaseaas1
>
> I'll start the SAP application on the sapbaseaas1 server.
>
> 🛠️ Using tool: execute_bash (trusted)
> ⋮
> ● I will run the following shell command:
> aws ssm send-command --instance-ids i-0539ecbaf407c4886 --document-name "AWS-RunShellScript" --parameters 'commands=["sudo su - ecdadm -c \"sapcontrol -nr 00 -function Start\""]' --output json
> ⋮
> ↳ Purpose: Starting SAP instance on sapbaseaas1
>
> ..
>
> > Now let's check if the SAP instance has started successfully:
>
> ……..
>
> ## Conclusion
>
> The SAP application server sapbaseaas1 is now fully operational. All required processes are running, and the instance is properly integrated with the SAP system. The instance is ready to handle user workloads and
> process SAP transactions.

## Conclusion

CloudWatch MCP Server delivers unified observability across the entire SAP landscape within the client environment. It consolidates SAP application logs, SAP HANA metrics, and AWS EC2 performance data. The system automatically correlates metrics across SAP components. Q CLI complements this by enabling natural language interaction with this rich dataset, making it easy for teams to generate system logs and simplify SAP system operations. This solution aligns with AWS Well-Architected Frameworks to help you increase productivity in operating and maintaining your SAP Landscape.

In [future blog posts](https://aws.amazon.com/blogs/awsforsap/streamline-sap-operation-with-cloudwatch-mcp-server-and-amazon-q-cli-part-4/), we will discuss how you can leverage CloudWatch MCP Server and Q CLI for maintenance planning and accelerating root cause analysis for SAP, as well as diving into the cost considerations of the solution.

As a next step, try out this solution — you can achieve at least a 6x gain in productivity. Stay tuned for our next innovations on [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [AWS Model Context Protocol Servers](https://awslabs.github.io/mcp/) and [Amazon Q for command line](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html).

## Join the SAP on AWS Discussion

In addition to your customer account team and AWS support channels, we recently launched [re:Post](https://repost.aws/) – A Reimagined Q&A Experience for the AWS Community. Our **AWS for SAP Solution Architecture** team regularly monitors the **AWS for SAP** topic to discuss and answer questions to support customers and partners. If your question is not related to technical support, join the discussion at re:Post and contribute to the community knowledge base.

## Acknowledgments

I would like to thank the following members for their contributions: Sreenath Middhi and Adam Hill.

TAGS: [#saponaws](https://aws.amazon.com/blogs/awsforsap/tag/saponaws/), [AWS Systems Manager](https://aws.amazon.com/blogs/awsforsap/tag/aws-systems-manager/), [CloudWatch](https://aws.amazon.com/blogs/awsforsap/tag/cloudwatch/), [SAP](https://aws.amazon.com/blogs/awsforsap/tag/sap/)
