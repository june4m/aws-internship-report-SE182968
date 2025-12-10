---
title: "Blog 3"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Weekly AWS Roundup: Amazon EKS, Amazon OpenSearch, Amazon API Gateway, and More (April 7, 2025)

**Author:** [Sébastien Stormacq](https://aws.amazon.com/blogs/aws/author/stormacq/)  
**Publication Date:** April 7, 2025  
**Services:** [Amazon API Gateway](https://aws.amazon.com/blogs/aws/category/application-services/amazon-api-gateway-application-services/), [Amazon Elastic Kubernetes Service](https://aws.amazon.com/blogs/aws/category/compute/amazon-kubernetes-service/), [Amazon Neptune](https://aws.amazon.com/blogs/aws/category/database/amazon-neptune/), [Amazon OpenSearch Service](https://aws.amazon.com/blogs/aws/category/analytics/amazon-elasticsearch-service/), [Amazon Q Developer](https://aws.amazon.com/blogs/aws/category/amazon-q/amazon-q-developer/), [Amazon Security Lake](https://aws.amazon.com/blogs/aws/category/security-identity-compliance/amazon-security-lake/), [Amazon Simple Email Service (SES)](https://aws.amazon.com/blogs/aws/category/messaging/amazon-simple-email-service-ses/), [Announcements](https://aws.amazon.com/blogs/aws/category/post-types/announcements/), [AWS Identity and Access Management (IAM)](https://aws.amazon.com/blogs/aws/category/security-identity-compliance/aws-identity-and-access-management-iam/), [Launch](https://aws.amazon.com/blogs/aws/category/news/launch/), [News](https://aws.amazon.com/blogs/aws/category/news/), [Resource Access Manager (RAM)](https://aws.amazon.com/blogs/aws/category/security-identity-compliance/resource-access-manager/)

The AWS Summit season kicks off this week! These free events are now being held around the world, bringing our cloud community together to connect, collaborate, and learn. Whether you prefer to join online or attend in person, these gatherings offer valuable opportunities to deepen your AWS knowledge.

I will be attending the [Summit in Paris](https://aws.amazon.com/fr/events/summits/paris/?trk=4b29643c-e00f-4ab6-ab9c-b1fb47aa1708&sc_channel=blog) this week — the largest cloud conference in France — and the [Summit in London](https://aws.amazon.com/events/summits/london/) later this month. We’ll have a small podcast studio 🎙️ where I’ll interview French and UK customers to produce new episodes of the [AWS Developers Podcast](https://developers.podcast.go-aws.com/web/index.html) and [le podcast 🎙 AWS ☁️ en 🇫🇷](https://francais.podcast.go-aws.com/web/index.html).

Register today!

But first, let’s take a look at last week’s announcements.

---

## Launches from Last Week

At [KubeCon London](https://events.linuxfoundation.org/kubecon-cloudnativecon-europe/), we announced the **[EKS Community Add-Ons Catalog](https://aws.amazon.com/blogs/containers/announcing-amazon-eks-community-add-ons-catalog/)**, making it easier for Kubernetes users to enhance their [Amazon EKS](https://aws.amazon.com/eks/) clusters with powerful open-source tools. The catalog simplifies installation of essential add-ons such as [metrics-server](https://github.com/kubernetes-sigs/metrics-server), [kube-state-metrics](https://kubernetes.io/docs/concepts/cluster-administration/kube-state-metrics/), [prometheus-node-exporter](https://prometheus.io/docs/guides/node-exporter/#monitoring-linux-host-metrics-with-the-node-exporter), [cert-manager](https://github.com/cert-manager/cert-manager), and [external-dns](https://github.com/kubernetes-sigs/external-dns). By integrating these community-developed add-ons directly into the [Amazon EKS console](https://console.aws.amazon.com/eks) and the [AWS Command Line Interface](https://aws.amazon.com/cli/) (CLI), customers can reduce operational complexity and accelerate deployment while maintaining flexibility and security. This release reflects AWS’s commitment to the Kubernetes community by offering seamless access to trusted open-source solutions without the burden of manual installation and maintenance.

**[Amazon Q Developer is now integrated with Amazon OpenSearch Service](https://aws.amazon.com/blogs/aws/accelerate-operational-analytics-with-amazon-q-developer-in-amazon-opensearch-service/)** to enhance operational analytics by enabling natural language exploration and AI-powered data visualization. This integration simplifies querying and visualizing data, reducing the challenges associated with traditional query languages and tools. During incident response, [Amazon Q Developer](https://aws.amazon.com/q/developer/) provides contextual summaries and insights directly in the alert interface, enabling faster analysis and resolution. This enhancement allows engineers to focus more on innovation by streamlining troubleshooting and improving monitoring infrastructure.

**[Amazon API Gateway now supports dual-stack endpoints (IPv4 and IPv6)](https://aws.amazon.com/blogs/aws/amazon-api-gateway-now-supports-dual-stack-ipv4-and-ipv6-endpoints/)** across all endpoint types, custom domain names, and managed APIs in both commercial regions and AWS GovCloud (US). This enhancement enables REST, HTTP, and WebSocket APIs—as well as custom domains—to receive requests from both IPv4 and IPv6 clients, easing IPv6 migration and addressing IPv4 address exhaustion. This update builds on recent IPv6-focused improvements, including [AWS Identity and Access Management (IAM) introducing dual-stack public endpoints](https://aws.amazon.com/about-aws/whats-new/2025/03/aws-identity-access-management-dual-stack-ipv4-ipv6-environments/) for seamless connectivity across both protocols, and [AWS Resource Access Manager (RAM) enabling resource sharing using IPv6 addresses](https://aws.amazon.com/about-aws/whats-new/2025/03/aws-ram-supports-ipv6/). Additionally, [Amazon Security Lake customers can now use IPv6](https://aws.amazon.com/about-aws/whats-new/2025/04/amazon-security-lake-internet-protocol-version-6/) through new dual-stack endpoints to configure and manage the service. Together, these enhancements ensure broader compatibility and prepare network infrastructure for the future.

**[Amazon SES has introduced attachment support in its v2 APIs](https://aws.amazon.com/about-aws/whats-new/2025/04/amazon-ses-attachments-sending-apis/)**, allowing users to attach files such as PDFs and images directly to their emails without manually generating MIME messages. This improvement simplifies sending rich email content and reduces implementation complexity. [Amazon Simple Email Service](https://aws.amazon.com/ses/) (Amazon SES) supports attachments in all AWS Regions where the service is available.

**[Amazon Neptune has updated its Service Level Agreement (SLA) to provide 99.99% monthly uptime for Multi-AZ DB Instance](https://aws.amazon.com/about-aws/whats-new/2025/04/amazon-neptune-99-99-availability-service-level-agreement/)**, Multi-AZ DB Cluster, and Multi-AZ Graph, up from the previous 99.9%. This enhancement reflects AWS’s commitment to delivering highly available and reliable graph database services for mission-critical applications. The improved SLA is now available in all AWS Regions where [Amazon Neptune](https://aws.amazon.com/neptune/) is offered.

For a full list of AWS announcements, keep an eye on the [What's New at AWS](https://aws.amazon.com/new/) page.

---

## Other AWS Events

Check your calendar and register for upcoming AWS events.

[AWS GenAI Lofts](https://aws.amazon.com/startups/lp/aws-gen-ai-lofts?trk=4b29643c-e00f-4ab6-ab9c-b1fb47aa1708&sc_channel=blog) are collaborative, immersive spaces showcasing AWS’s cloud and AI expertise. They give startups and developers the opportunity to experience AI products and services firsthand, attend exclusive sessions with industry leaders, and network with investors and peers. [Find a GenAI Loft location](https://aws.amazon.com/startups/lp/aws-gen-ai-lofts#locations?trk=4b29643c-e00f-4ab6-ab9c-b1fb47aa1708&sc_channel=blog) near you and don’t forget to register.

Browse all upcoming [AWS in-person and online events here.](https://aws.amazon.com/events/explore-aws-events/?trk=4b29643c-e00f-4ab6-ab9c-b1fb47aa1708&sc_channel=blog)

That’s all for this week. Come back next Monday for the next Weekly Roundup!

[— seb](https://linktr.ee/sebsto)

*This post is part of our [Weekly Roundup](https://aws.amazon.com/blogs/aws/tag/week-in-review/) series. Visit each week for a quick recap of exciting AWS news and announcements!*

---

Blog News: How does it work? Take this [1-minute survey](https://amazonmr.au1.qualtrics.com/jfe/form/SV_eyD5tC5xNGCdCmi)!

*(This [survey](https://amazonmr.au1.qualtrics.com/jfe/form/SV_eyD5tC5xNGCdCmi) is hosted by an external company. AWS processes your information as described in the [AWS Privacy Notice](https://aws.amazon.com/privacy/). AWS will own the data collected through this survey and will not share participants’ information.)*

---

# About the Author

[Sébastien Stormacq](https://aws.amazon.com/blogs/aws/author/stormacq/)

Seb has been writing code since he first touched a Commodore 64 in the mid-1980s. He inspires developers to unlock the value of the AWS Cloud with a unique mix of passion, enthusiasm, customer obsession, curiosity, and creativity.  
He cares deeply about software architecture, developer tools, and mobile computing.  
If you want to sell him something, make sure it has an API.  
Follow @sebsto on Bluesky, X, Mastodon, and other platforms.