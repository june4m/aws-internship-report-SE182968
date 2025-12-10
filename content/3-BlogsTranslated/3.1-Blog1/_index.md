---
title: "Blog 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Varsity Yearbooks Overcomes Market Challenges Through SaaS and CleanSlate

**Authors: Bill Tarr, Anthony McClure, Brad Laughlin, and Rowena Branch**

**Publication Date: March 13, 2025**

**Services:** Americas, AWS Well-Architected, AWS Well-Architected Framework, AWS Well-Architected Tool, Best Practices, Customer Solutions, DevOps, Education, Experience-Based Acceleration, Migration Solutions, SaaS

*by Brad Laughlin, VP of Strategy and Operations – CleanSlate Technology Group*  
*By Rowena Branch, SaaS Partner Manager – AWS SaaS Factory*  
*By Anthony McClure, Senior Partner Solutions Architect – AWS SaaS Factory*  
*By Bill Tarr, Principal Solutions Architect – AWS SaaS Factory*

Software providers have evolved to survive. This has never been more true for education services during the 2020 pandemic. The changing needs of the institutions they serve pushed companies like [Varsity Yearbooks](https://www.herffjones.com/), a leading provider of graduation products since the 1920s, to focus on faster innovation and shorter time-to-market through a SaaS delivery model.

In partnership with AWS SaaS Competency Partner [CleanSlate Technology Group](https://www.cleanslatetg.com/), Varsity Yearbooks carried out a strategic transformation of their products to adapt to the new online landscape, beginning with their yearbook line. This collaboration resulted in a SaaS transformation that reshaped Varsity Yearbooks’ approach by adopting a SaaS mindset to drive revenue and market share.

According to Varsity Yearbooks, they were simply trying to stay afloat before building a SaaS solution. CleanSlate helped them shift into a SaaS-oriented mindset, which ultimately increased revenue and market share. The result provided Varsity with a solution they felt completely confident in, enabling them to focus on the future and explore what comes next. Today, their team prioritizes delivering features to market within weeks instead of months and is achieving rapid market growth. The [SaaS solution](https://www.cleanslatetg.com/success_stories/herff-jones-story/) allowed Varsity Yearbooks to reimagine the yearbook production process and opened up possibilities for future markets. The team now constantly asks, “What else is possible?”

CleanSlate’s work contributed to their recognition at the [TechPoint Digital Transformation Awards](https://techpoint.org/techpoint-announces-nominees-for-24th-annual-mira-awards-honoring-the-best-of-tech-in-indiana/) for successfully helping Varsity Yearbooks thrive in the $1.9 trillion education services industry.

CleanSlate has extensive experience designing and building SaaS solutions for customers through a comprehensive approach to designing and developing modern applications. CleanSlate’s expertise includes product design, DevOps automation, cloud-native and mobile development. Their core values drive their success, with the ultimate goal of leaving customers in a better position than when they started. Their team of technologists is inspired to innovate and deliver. By collaborating with clients on modernization efforts, they help identify SaaS opportunities and guide organizations to think differently. Customers must ask how a modern application built using the SaaS model can bring greater value, such as becoming a growth driver, revenue engine, superior customer experience platform, or accelerating feature delivery. Thinking differently about modernization and transforming it into SaaS and a product strategy is what differentiates CleanSlate Technology Group from its competitors.

---

# Solution Overview

CleanSlate’s collaboration with Varsity Yearbooks was driven by the imminent risk of failure of their electronic yearbook design software. The project focused on modernizing and transforming their 14-year-old platform and infrastructure into a modern SaaS solution, while re-platforming the legacy system to run in parallel until the new SaaS MVP (Minimum Viable Product) was released.

By leveraging AWS services and the AWS SaaS Factory framework, we successfully modernized Varsity’s legacy software into a true SaaS product, as illustrated in Figure 1. CleanSlate’s modernization approach applied a SaaS product mindset, combining business planning with modern cloud-native application development. The multi-tenant solution designed and built by the team enabled business growth, competitive advantage, and enhanced customer flexibility and creativity.

![][image1]  
*Figure 1: Architecture model enabling SaaS delivery*

CleanSlate evaluated the overall solution from both a business and technology perspective.

## Business Perspective

CleanSlate collaborated with Varsity Yearbooks to achieve their business goals, creating a marketable AWS SaaS solution. This process included building a product roadmap and go-to-market strategy, defining commercialization and packaging, establishing customer success metrics, using an MVP approach, and prioritizing modern UX/UI to improve usability and functionality.

## Technology Perspective

CleanSlate developed the technical foundation in alignment with Varsity Yearbooks’ business vision, delivering a well-built and future-proof solution. This included profiling, analytics, identity/access management, tenant isolation/multi-tenant storage, SaaS-focused DevOps automation, microservice architecture design, partner integrations, and modernization using AWS, Angular, and Java.

CleanSlate built a scalable SaaS application using more than 20 AWS services, including a multi-tenant architecture with [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) and microservices for seamless tenant connectivity. [Amazon Elastic Container Service](https://aws.amazon.com/ecs/) (ECS) and [Amazon Relational Database Service](https://aws.amazon.com/rds/) (RDS) enabled automatic horizontal scaling, while auto-scaling rules handled demand spikes. Application profiling, analytics, and monitoring tools such as [AWS CloudTrail](https://aws.amazon.com/cloudtrail/), [New Relic](https://newrelic.com/), and [Splunk](https://www.splunk.com/) ensured governance and scalability.

The SaaS DevOps approach automated the entire infrastructure with [Hashicorp Terraform](https://developer.hashicorp.com/terraform), integrated quality checks, and continuous integration/deployment pipelines. This enabled Varsity Yearbooks to scale their development team, run multiple environments simultaneously, and accelerate production releases with on-demand deployment.

Initial challenges included operational inefficiencies hindering innovation, slow time-to-market impacting customer satisfaction, limited scalability and reliability, long bug resolution cycles, lack of monitoring/logging tools, and manual onboarding processes. CleanSlate addressed these challenges through modernization and a new SaaS product mindset.

---

# Lessons Learned

Overall, companies like CleanSlate must embody a passionate partnership mindset, demonstrating empathy toward customer challenges and working tirelessly to solve them with sustainable, long-term solutions. CleanSlate’s commitment to building a culture of service quality helps customers resolve both short-term and long-term challenges through improvements in processes, technology, and people. Partners must strive to understand the unique business needs of their customers, build trust, and collaborate to deliver the best solutions.

From a technology perspective, leveraging SaaS Factory frameworks and performing [AWS Well-Architected Reviews](https://aws.amazon.com/architecture/well-architected/) throughout the process truly helps companies succeed by building and deploying high-quality projects that take full advantage of the cloud. Investing in SaaS component design and strong SaaS DevOps principles enables sustainable and transformative SaaS product development.

The biggest lesson from this large-scale digital transformation is that change is genuinely difficult to accept, and it requires the right partnership to embrace it. Varsity Yearbooks and CleanSlate encountered several challenges but ultimately built trust, integrated quality into their processes, advised effectively, adapted well, and worked tirelessly to meet expectations — all of which fueled success.

---

# Conclusion

The successful transformation of Varsity Yearbooks demonstrates how traditional businesses can reinvent themselves through strategic SaaS adoption. Through collaboration with CleanSlate and AWS, what began as a pandemic-driven necessity evolved into a transformative business opportunity, enabling faster feature delivery, improved scalability, and greater operational efficiency. This shift positioned Varsity Yearbooks for long-term growth and continuous innovation.

This success story highlights the value of [AWS SaaS Competency Partners](https://aws.amazon.com/partners/saas-on-aws/partner-solutions/), who bring deep expertise in designing and building cloud-native solutions on AWS. Just as CleanSlate has helped organizations ease the burden of legacy migrations and establish strong foundations for SaaS solutions on AWS, they also provide comprehensive architectural and deployment support for enterprises pursuing digital transformation.

[![][image2]](https://partners.amazonaws.com/contactpartner?partnerId=0010L00001pBtKDQA0&partnerName=CleanSlate%20Technology%20Group)

---

# Cleanslate Technology – Featured Partner

At CleanSlate Technology Group, we specialize in designing and building innovative SaaS products, data-driven solutions, and AI-powered applications that help clients achieve their business goals. As a trusted AWS partner, we provide cloud-native application development, DevOps services, and deep architectural expertise to unlock the full potential of modern technologies.

Our expertise also includes migrating and modernizing legacy applications, ensuring they are future-ready and optimized for cloud environments. Whether you are looking to build scalable SaaS solutions, harness the power of data and AI, or modernize your infrastructure, we are committed to delivering transformative outcomes that drive growth, innovation, and long-term adaptability for your organization.

[Contact CleanSlate Technology](https://partners.amazonaws.com/contactpartner?partnerId=0010L00001pBtKDQA0&partnerName=CleanSlate%20Technology%20Group) | [CleanSlate Overview](https://partners.amazonaws.com/partners/0010L00001pBtKDQA0/CleanSlate%20Technology%20Group)
