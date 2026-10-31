# 01 — Cloud Concepts

[← Exam overview](00-exam-overview.md) | [Index](README.md) | [Compute and networking →](02-compute-and-networking.md)

## Cloud fundamentals

Cloud computing is on-demand access to IT resources over the internet with pay-as-you-go pricing. AWS owns and maintains the hardware; you provision and use only what you need.

📖 **AWS docs:** [What is cloud computing?](https://aws.amazon.com/what-is-cloud-computing/)

### Six cloud advantages

- **Trade fixed expense for variable expense:** pay for use instead of buying data centres.
- **Economies of scale:** AWS spreads costs across many customers, so unit prices fall.
- **Stop guessing capacity:** scale resources when demand changes.
- **Increase speed and agility:** provision in minutes instead of weeks.
- **Stop spending on data centres:** focus on customers, not hardware.
- **Go global quickly:** deploy to multiple Regions rapidly.

📖 **AWS docs:** [Six advantages of cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)

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

### Ways to access AWS

| Method | Use it for |
|---|---|
| **Management Console** | Point-and-click, learning, one-time tasks |
| **AWS CLI** | Commands and scripts from a terminal |
| **SDKs / APIs** | Calling AWS from application code (programmatic access) |



📖 **AWS docs:** [Tools to build on AWS](https://aws.amazon.com/tools/) · [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)

### Global infrastructure

- **Region:** separate geographic area containing multiple Availability Zones.
- **Availability Zone (AZ):** one or more isolated data centres in a Region. AZs do not share single points of failure.
- **Edge location:** site that caches or routes content closer to users.
- Use **multiple AZs** for high availability.
- Use **multiple Regions** for disaster recovery, business continuity, low latency for users, or **data sovereignty** (data must stay in a country).
- Most services are **regional**; IAM, Route 53, and CloudFront are important global services.

📖 **AWS docs:** [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)

## AWS Cloud Adoption Framework (AWS CAF)

AWS CAF helps organisations plan cloud transformation through six perspectives. The goal is better business outcomes: **reduced business risk, improved ESG (environmental, social, governance) performance, increased revenue, and improved operational efficiency**.

| Business-focused | Technology-focused |
|---|---|
| **Business:** business outcomes and value | **Platform:** architecture and cloud foundations |
| **People:** skills, culture, and roles | **Security:** confidentiality, integrity, compliance |
| **Governance:** benefits, risk, and control | **Operations:** operate and recover workloads |

⚠️ **Exam trap:** CAF plans organisational transformation. The Well-Architected Framework evaluates cloud workloads.

📖 **AWS docs:** [AWS CAF](https://aws.amazon.com/cloud-adoption-framework/) · [CAF Business perspective](https://docs.aws.amazon.com/whitepapers/latest/aws-caf-business-perspective/aws-caf-business-perspective.html)

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

📖 **AWS docs:** [Well-Architected Framework: definitions and pillars](https://docs.aws.amazon.com/wellarchitected/latest/framework/definitions.html)

### AWS Well-Architected Tool

📌 **What is it?** A free self-service tool for reviewing workloads against AWS Well-Architected best practices. Think of it as an architecture health checklist.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses structured questions to identify **high-risk issues (HRIs)** and medium-risk issues, with improvement actions.
- Supports the six pillars and can record milestones.
- The tool is free; recommended improvements may create service charges.

⚠️ **Exam Trap / Common Distractor:** It provides guidance; it does not automatically redesign or repair a workload.

📖 **AWS docs:** [Well-Architected Tool User Guide](https://docs.aws.amazon.com/wellarchitected/latest/userguide/well-architected.html)

## Identity and Access Management (IAM) foundations

Use least privilege, enable MFA, avoid daily root-user use, and prefer temporary credentials. See [Security and compliance](04-security-and-compliance.md#aws-identity-and-access-management-iam) for the full IAM service template.

| Identity item | Purpose |
|---|---|
| User | Long-term identity for one person or application |
| Group | Collection of users; permissions apply to its members |
| Role | Assumable identity with temporary credentials |
| Policy | JSON permission document stating allow or deny |

📖 **AWS docs:** [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

## Shared Responsibility Model

- AWS is responsible for security **of** the cloud: facilities, hardware, networking, and managed infrastructure.
- The customer is responsible for security **in** the cloud: identities, data, configuration, and, for EC2, the guest OS and applications.
- Customer work decreases with more managed services: EC2 → RDS → Lambda.

📖 **AWS docs:** [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

## Cloud economics

| Idea | What to remember |
|---|---|
| **Fixed vs variable cost** | Fixed (capital) cost means buying hardware up front. Variable cost means paying only for what you use. The cloud moves you to variable cost. |
| **On-premises costs** | Servers, storage, networking, power, cooling, floor space, and staff. You also over-buy to cover peak demand. |
| **Licensing** | **License Included:** AWS provides the licence in the price. **BYOL (Bring Your Own License):** you use licences you already own, subject to the vendor's terms (for example Microsoft products on Dedicated Hosts). |
| **Rightsizing** | Match instance type and size to the real workload so you are not paying for unused capacity. Do it continuously. |
| **Automation** | Scheduled start/stop, Auto Scaling, and IaC reduce manual errors and idle cost and make results repeatable. |
| **Economies of scale** | Aggregated usage from many customers lowers AWS's cost per unit, and savings are passed on. |

📖 **AWS docs:** [Licensing options on AWS](https://aws.amazon.com/windows/resources/licensing/) · [Right sizing](https://aws.amazon.com/aws-cost-management/aws-cost-optimization/right-sizing/) · [Six advantages](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)

## Pricing and billing foundations

### Pricing principles

- **Pay as you go:** no long-term commitment for many services.
- **Save when you commit:** Savings Plans and Reserved Instances can reduce eligible compute cost.
- **Pay less at scale:** tiered pricing and volume discounts may apply.
- Pricing varies by service, Region, usage, and purchase option.

📖 **AWS docs:** [AWS Pricing](https://aws.amazon.com/pricing/) · [AWS Free Tier](https://aws.amazon.com/free/)

### Data transfer costs

- **Inbound** data transfer from the internet is usually free.
- **Outbound** to the internet, **between Regions**, and often **between AZs** costs money.
- Traffic within one AZ over private IPs, and between some services in the same Region (for example EC2 to S3), is commonly free. Always check the service's pricing page.

📖 **AWS docs:** [Understanding data transfer charges](https://docs.aws.amazon.com/cur/latest/userguide/cur-data-transfers-charges.html)

### EC2 purchasing models at a glance

| Model | Best for | Commitment / interruption |
|---|---|---|
| On-Demand | Short, unpredictable workloads | None; not interrupted by AWS |
| Savings Plans | Steady compute usage | 1 or 3 years; flexible discount model |
| Reserved Instances | Predictable EC2 use and capacity options | 1 or 3 years |
| Spot Instances | Flexible, fault-tolerant batch work | Can be interrupted by AWS |
| Dedicated Instances | Instances on hardware used by only one customer | Pay by the hour; no view of the physical server |
| Dedicated Hosts | Licensing or compliance requiring a physical server | Dedicated physical host; highest control/cost |


**Flexibility to remember**

- **Savings Plans:** Compute Savings Plans cover EC2, Fargate, and Lambda (up to 66% off On-Demand). EC2 Instance Savings Plans are tied to an instance family but save more (up to 72%).
- **Reserved Instances:** **Standard** gives the biggest discount (up to 72%); **Convertible** lets you exchange for other attributes (up to 66%). **Regional** RIs apply across AZs and give instance size flexibility; **zonal** RIs reserve capacity in one AZ.
- **In AWS Organizations:** RI and Savings Plans discounts are shared across accounts under consolidated billing by default. The management account can turn sharing off for individual accounts. Zonal RI capacity reservations are not shared.

📖 **AWS docs:** [EC2 purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html) · [Savings Plans vs RIs](https://docs.aws.amazon.com/savingsplans/latest/userguide/sp-ris.html) · [RI and Savings Plans discount sharing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ri-turn-off.html)

### Cost tools

- **AWS Pricing Calculator:** estimate cost before deploying.
- **AWS Cost Explorer:** analyse and forecast existing cost and usage.
- **AWS Budgets:** alert when actual or forecast cost/usage crosses thresholds.
- **Cost and Usage Report (CUR):** most detailed billing data for analysis.
- **Consolidated billing (AWS Organizations):** one bill for all member accounts, combined volume discounts, shared RI/Savings Plans discounts.
- **Cost allocation tags:** group costs by project, team, or environment after activation. There are two types: **AWS-generated** tags (prefix `aws:`) and **user-defined** tags (prefix `user:`). Both must be **activated** in the Billing console, and can take up to 24 hours to appear. They show up in Cost Explorer and the CUR.

📖 **AWS docs:** [Pricing Calculator](https://calculator.aws/) · [Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-getting-started.html) · [Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) · [Cost and Usage Reports](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html) · [Consolidated billing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html) · [Cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)

### Support plans

AWS now offers four plans; each paid plan builds on the one below it. Older plans (Developer, Business, Enterprise On-Ramp) may still appear in older study material.

| Plan | For | Key features |
|---|---|---|
| **Basic** | All AWS customers (free) | 24/7 customer service, documentation, whitepapers, AWS re:Post, core Trusted Advisor checks, AWS Health |
| **Business Support+** | Minimum AWS recommends for **production** workloads | 24/7 technical support by phone, web, chat, and email; AI-powered troubleshooting; full Trusted Advisor checks; unlimited cases and contacts; third-party software support; critical-system-down response under 30 minutes |
| **Enterprise Support** | **Business-critical** workloads | Everything in Business Support+, plus a designated **Technical Account Manager (TAM)**, Well-Architected and security reviews, Trusted Advisor Priority, critical-system-down response under 15 minutes |
| **Unified Operations** | **Mission-critical** workloads | Everything in Enterprise Support, plus designated domain specialist engineers, Incident Management Engineers, 24/7 workload monitoring, response under 5 minutes |

⚠️ **Exam trap:** Response times and features differ by plan; learn the order **Basic → Business Support+ → Enterprise Support → Unified Operations**. The TAM starts at Enterprise level.

📖 **AWS docs:** [Compare AWS Support plans](https://aws.amazon.com/premiumsupport/plans/) · [AWS Support plans guide](https://docs.aws.amazon.com/awssupport/latest/user/aws-support-plans.html) · [Announcement of the new plans](https://aws.amazon.com/about-aws/whats-new/2025/12/aws-support-transformation-ai-powered-operations)

### Technical resources and help

| Resource | What it is for |
|---|---|
| **Documentation and whitepapers** | Official guides and deep dives |
| **AWS re:Post** | Community questions and answers |
| **AWS Knowledge Center** | Official answers to common technical questions |
| **AWS Prescriptive Guidance** | Proven strategies, guides, and patterns for migration and modernisation |
| **AWS Support Center** | Create and manage support cases where your plan allows |
| **AWS Health Dashboard / Health API** | Events that affect your AWS resources and services |
| **AWS Trust and Safety** | Report abuse of AWS resources (spam, malware, DDoS, and similar) |
| **AWS Professional Services / solutions architects** | AWS experts who help with projects and architecture |
| **AWS Partner Network (APN)** | Partners such as ISVs (software vendors) and system integrators. Partners get training and certification, events, and discounts |
| **AWS Marketplace** | Catalogue of third-party software you can buy and deploy; supports cost management, governance, and entitlement |

📖 **AWS docs:** [Documentation](https://docs.aws.amazon.com/) · [Whitepapers](https://aws.amazon.com/whitepapers/) · [re:Post](https://repost.aws/) · [Knowledge Center](https://repost.aws/knowledge-center) · [Prescriptive Guidance](https://aws.amazon.com/prescriptive-guidance/) · [Report abuse](https://www.repost.aws/knowledge-center/report-aws-abuse) · [Professional Services](https://aws.amazon.com/professional-services/) · [Partner Network](https://aws.amazon.com/partners/) · [Marketplace](https://aws.amazon.com/marketplace)

## Migration concepts

The common migration strategies are the **7 Rs**:

- **Retire:** remove what is no longer needed.
- **Retain:** keep it where it is for now.
- **Rehost:** “lift and shift” with minimal change.
- **Relocate:** move infrastructure without redesigning workloads.
- **Repurchase:** replace with another product, often SaaS.
- **Replatform:** make limited cloud optimisations.
- **Refactor / re-architect:** redesign to use cloud-native capabilities.

📖 **AWS docs:** [Migration strategies (7 Rs)](https://docs.aws.amazon.com/prescriptive-guidance/latest/large-migration-guide/migration-strategies)

### AWS migration tools

| Service | What it does |
|---|---|
| **AWS Application Discovery Service** | Collects data about on-premises servers and dependencies to plan a migration. ⚠️ AWS says it is no longer open to new customers and points to AWS Transform instead, but it is still on the exam's in-scope list. |
| **AWS Migration Hub** | One place to track migration progress across tools |
| **Migration Evaluator** | Builds a business case and cost estimate for moving to AWS |
| **AWS Application Migration Service (MGN)** | Automated **lift and shift (rehost)** of servers. ⚠️ Renamed **AWS Transform MGN** in June 2026; the exam guide may still use the old name. |
| **AWS DMS / SCT** | Move databases and convert schemas; see [Storage and databases](03-storage-and-databases.md#aws-database-migration-service-aws-dms) |

📖 **AWS docs:** [Migration Hub](https://docs.aws.amazon.com/migrationhub/latest/ug/whatishub.html) · [Application Discovery Service](https://docs.aws.amazon.com/application-discovery/latest/userguide/what-is-appdiscovery.html) · [Migration Evaluator](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-tools/discovery-migration-evaluator.html) · [AWS Transform MGN (Application Migration Service)](https://docs.aws.amazon.com/mgn/latest/ug/what-is-application-migration-service.html) · [MGN rename announcement](https://aws.amazon.com/about-aws/whats-new/2026/06/aws-transform-mgn-rebrand/)


[← Exam overview](00-exam-overview.md) | [Index](README.md) | [Compute and networking →](02-compute-and-networking.md)