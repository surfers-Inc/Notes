# 01 — Cloud Concepts

[← Exam overview](00-exam-overview.md) | [Index](README.md) | [Compute and networking →](02-compute-and-networking.md)

## Cloud fundamentals

Cloud computing is on-demand access to IT resources over the internet with pay-as-you-go pricing.

### Six cloud advantages

- **Trade fixed expense for variable expense:** pay for use instead of buying data centres.
- **Economies of scale:** AWS spreads costs across many customers.
- **Stop guessing capacity:** scale resources when demand changes.
- **Increase speed and agility:** provision in minutes instead of weeks.
- **Stop spending on data centres:** focus on customers, not hardware.
- **Go global quickly:** deploy to multiple Regions rapidly.

### Key terms

| Term | Meaning |
|---|---|
| High availability | Keep working when a component fails, commonly across multiple AZs |
| Elasticity | Automatically add or remove resources with demand |
| Scalability | Increase or decrease capacity to meet demand |
| Agility | Experiment and deliver quickly |
| Fault tolerance | Continue operating despite failures |
| Disaster recovery | Restore service after a major event |

### Deployment models

- **Cloud:** resources run in AWS.
- **On-premises:** resources run in the organisation's own data centre.
- **Hybrid:** connected mix of cloud and on-premises resources.

### Global infrastructure

- **Region:** separate geographic area containing multiple Availability Zones.
- **Availability Zone (AZ):** one or more isolated data centres in a Region.
- **Edge location:** site that caches or routes content closer to users.
- Use **multiple AZs** for high availability; use **multiple Regions** for global latency, sovereignty, or disaster recovery.
- Most services are **regional**; IAM, Route 53, and CloudFront are important global services.

## AWS Cloud Adoption Framework (AWS CAF)

AWS CAF helps organisations plan cloud transformation through six perspectives.

| Business-focused | Technology-focused |
|---|---|
| **Business:** business outcomes and value | **Platform:** architecture and cloud foundations |
| **People:** skills, culture, and roles | **Security:** confidentiality, integrity, compliance |
| **Governance:** benefits, risk, and control | **Operations:** operate and recover workloads |

⚠️ **Exam trap:** CAF plans organisational transformation. The Well-Architected Framework evaluates cloud workloads.

## AWS Well-Architected Framework

Six pillars guide secure, reliable, efficient, cost-effective, and sustainable workloads.

| Pillar | Main question |
|---|---|
| Operational Excellence | Can we run, monitor, and improve the workload? |
| Security | Can we protect data, systems, and assets? |
| Reliability | Can the workload recover and meet demand? |
| Performance Efficiency | Are resources used efficiently? |
| Cost Optimization | Are we avoiding unnecessary cost? |
| Sustainability | Are we reducing environmental impact? |

### AWS Well-Architected Tool

📌 **What is it?** A free self-service tool for reviewing workloads against AWS Well-Architected best practices. Think of it as an architecture health checklist.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses structured questions to identify **high-risk issues** and improvement actions.
- Supports the six pillars and can record milestones.
- The tool is free; recommended improvements may create service charges.

⚠️ **Exam Trap / Common Distractor:** It provides guidance; it does not automatically redesign or repair a workload.

## Identity and Access Management (IAM) foundations

Use least privilege, enable MFA, avoid daily root-user use, and prefer temporary credentials. See [Security and compliance](04-security-and-compliance.md#aws-identity-and-access-management-iam) for the full IAM service template.

| Identity item | Purpose |
|---|---|
| User | Long-term identity for one person or application |
| Group | Collection of users; permissions apply to its members |
| Role | Assumable identity with temporary credentials |
| Policy | JSON permission document stating allow or deny |

## Shared Responsibility Model

- AWS is responsible for security **of** the cloud: facilities, hardware, networking, and managed infrastructure.
- The customer is responsible for security **in** the cloud: identities, data, configuration, and—for EC2—the guest OS and applications.
- Customer work decreases with more managed services: EC2 → RDS → Lambda.

## Pricing and billing foundations

### Pricing principles

- **Pay as you go:** no long-term commitment for many services.
- **Save when you commit:** Savings Plans and Reserved Instances can reduce eligible compute cost.
- **Pay less at scale:** tiered pricing and volume discounts may apply.
- **Inbound data transfer** is usually free; outbound transfer and cross-Region transfer commonly cost money.
- Pricing varies by service, Region, usage, and purchase option.

### EC2 purchasing models at a glance

| Model | Best for | Commitment / interruption |
|---|---|---|
| On-Demand | Short, unpredictable workloads | None; not interrupted by AWS |
| Savings Plans | Steady compute usage | 1 or 3 years; flexible discount model |
| Reserved Instances | Predictable EC2 use and capacity options | 1 or 3 years |
| Spot Instances | Flexible, fault-tolerant batch work | Can be interrupted by AWS |
| Dedicated Hosts | Licensing or compliance requiring a physical server | Dedicated physical host; highest control/cost |

### Cost tools

- **AWS Pricing Calculator:** estimate cost before deploying.
- **AWS Cost Explorer:** analyse and forecast existing cost and usage.
- **AWS Budgets:** alert when actual or forecast cost/usage crosses thresholds.
- **Cost and Usage Report (CUR):** most detailed billing data for analysis.
- **Cost allocation tags:** group costs by project, team, or environment after activation.

### Support and guidance

- **AWS re:Post:** community knowledge and questions.
- **AWS Knowledge Center:** official answers to common technical questions.
- **AWS Support Center:** create and manage support cases where the plan permits.
- Support plan names, prices, and response times can change; learn the progression from self-service help to faster technical and business-critical support.

## Migration concepts

The common migration strategies are the **7 Rs**:

- **Retire:** remove what is no longer needed.
- **Retain:** keep it where it is for now.
- **Rehost:** “lift and shift” with minimal change.
- **Relocate:** move infrastructure without redesigning workloads.
- **Repurchase:** replace with another product, often SaaS.
- **Replatform:** make limited cloud optimisations.
- **Refactor / re-architect:** redesign to use cloud-native capabilities.


[← Exam overview](00-exam-overview.md) | [Index](README.md) | [Compute and networking →](02-compute-and-networking.md)