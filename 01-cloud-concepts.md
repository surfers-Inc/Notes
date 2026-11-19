# 01 — Cloud Concepts, Pricing, and Support

[← Exam overview](00-exam-overview.md) | [Index](README.md) | [Compute and networking →](02-compute-and-networking.md)

> This file covers **Domain 1 (Cloud Concepts, 24%)** and most of **Domain 4 (Billing, Pricing, and Support, 12%)**. Service cards use the same layout as the rest of the repo: 📘 how AWS describes it · 💡 how to remember it · 🔑 keywords · 🎯 what they test · ⚠️ traps · 📖 read more.

## Contents

- **Cloud fundamentals:** [What is cloud computing?](#what-is-cloud-computing) · [Six advantages](#six-advantages-of-cloud-computing) · [Key terms](#key-terms-that-appear-in-questions) · [Deployment and service models](#deployment-models-and-service-models)
- **Global infrastructure:** [Regions, AZs, edge locations](#aws-global-infrastructure)
- **Frameworks:** [Well-Architected Framework](#aws-well-architected-framework) · [Well-Architected Tool](#aws-well-architected-tool) · [Cloud Adoption Framework (CAF)](#aws-cloud-adoption-framework-aws-caf)
- **Migration:** [7 Rs](#migration-strategies-the-7-rs) · [AWS migration tools](#aws-migration-tools)
- **Economics and pricing:** [Cloud economics](#cloud-economics) · [Pricing principles](#pricing-principles) · [Data transfer costs](#data-transfer-costs) · [Purchase options summary](#compute-purchase-options-summary) · [Free Tier](#aws-free-tier)
- **Support and help:** [Support plans](#aws-support-plans) · [Technical resources](#technical-resources-and-where-to-get-help) · [AWS Marketplace and Partners](#aws-marketplace-and-the-aws-partner-network)
- Shared Responsibility Model → [04](04-security-and-compliance.md#shared-responsibility-model) · Cost tools (Cost Explorer, Budgets, CUR…) → [05](05-management-and-governance.md#cost-tools-at-a-glance) · Console/CLI/SDK → [05](05-management-and-governance.md#ways-to-access-aws-console-cli-sdks)
- [Top traps on this page](#top-traps-on-this-page)

---

## What is cloud computing?

**📘 AWS definition:** Cloud computing is the on-demand delivery of IT resources over the internet with pay-as-you-go pricing.

**💡 Simple definition:** **Renting IT instead of buying it**, like using electricity from the grid rather than building your own power station. You switch it on when needed and pay for what you use.

📖 **Read more:** [What is cloud computing?](https://aws.amazon.com/what-is-cloud-computing/) · [Overview of Amazon Web Services (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html)

## Six advantages of cloud computing

Expect at least one question that describes a benefit and asks you to name it.

| Advantage | Plain meaning | Question clue |
|---|---|---|
| **Trade fixed expense for variable expense** | Pay only for what you use instead of buying servers up front (**CapEx → OpEx**) | "no upfront investment", "pay as you go" |
| **Benefit from massive economies of scale** | AWS buys at huge scale, so prices are lower | "lower variable cost than you could get yourself" |
| **Stop guessing capacity** | Scale up or down with real demand | "avoid over- or under-provisioning" |
| **Increase speed and agility** | New resources in minutes, so you can experiment | "reduce time to provision from weeks to minutes" |
| **Stop spending money running and maintaining data centres** | Focus on customers, not racking servers | "focus on business differentiators" |
| **Go global in minutes** | Deploy in Regions worldwide with a few clicks | "low latency for customers around the world" |

Memory hook: **"Variable Scale, No Guessing, Fast, No Data centre, Global."**

📖 **Read more:** [Six advantages of cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)

## Key terms that appear in questions

| Term | Meaning | Example |
|---|---|---|
| **High availability** | The system keeps running with minimal downtime | Run across **multiple AZs** behind a load balancer |
| **Fault tolerance** | Keeps working **even while** a component fails, with no visible interruption | Aurora's six copies of data across three AZs |
| **Elasticity** | Resources grow and shrink **automatically** with demand | Auto Scaling adds instances at peak and removes them at night |
| **Scalability** | The ability to handle more load by adding resources (up or out) | Move to a bigger instance (vertical) or add more instances (horizontal) |
| **Agility** | Speed of trying, building, and delivering new things | Spin up a test environment in minutes |
| **Disaster recovery (DR)** | Restoring service after a major event | Backups or standby resources in another Region |
| **Durability** | Data will not be lost | S3 is designed for 11 nines of durability |
| **Decoupling / loose coupling** | Components do not depend directly on each other | SQS queue between web and worker tiers |

⚠️ **Trap:** **Elasticity** = automatic, short-term matching of demand. **Scalability** = ability to grow. Pick elasticity when the question says "automatically" or "only pay for what is needed at that moment".

## Deployment models and service models

| Deployment model | Where it runs | Clue |
|---|---|---|
| **Cloud (cloud-native)** | Fully in AWS | "built entirely in the cloud" |
| **On-premises (private cloud)** | Your own data centre | "keep everything in our own building" |
| **Hybrid** | Cloud + on-premises connected | "connect our data centre to AWS", Outposts, Direct Connect, Storage Gateway |

| Service model | You manage | AWS example |
|---|---|---|
| **IaaS** (Infrastructure as a Service) | OS, apps, data | **EC2**, VPC, EBS |
| **PaaS** (Platform as a Service) | Apps and data | **Elastic Beanstalk**, RDS |
| **SaaS** (Software as a Service) | Just use it | Amazon WorkSpaces, Amazon Connect, software from AWS Marketplace |

📖 **Read more:** [Types of cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/types-of-cloud-computing.html)

---

## AWS global infrastructure

**📘 AWS definition:** The AWS Cloud infrastructure is built around AWS Regions and Availability Zones; a Region is a physical location in the world with multiple Availability Zones, and each Availability Zone consists of one or more discrete data centres with redundant power, networking, and connectivity.

**💡 Simple definition:** **Region = a city. Availability Zone = separate buildings in that city. Edge location = small shops in many towns** that keep popular items close to customers.

**🔑 Exam keywords:** `Region` · `Availability Zone (AZ)` · `edge location` · `Local Zone` · `data sovereignty / residency` · `multi-AZ` · `multi-Region`

**🎯 What they test**

| Piece | What it is | Use it for |
|---|---|---|
| **Region** | A geographic area (for example `eu-west-1`, Ireland), fully isolated from other Regions; usually **three or more AZs** | Data residency, latency to users, DR in another Region |
| **Availability Zone (AZ)** | One or more data centres with separate power, cooling, and networking, physically apart but linked by low-latency fibre | **High availability**: run in **at least two AZs** |
| **Edge location / point of presence** | Hundreds of sites in many more cities than Regions; used by **CloudFront, Route 53, Global Accelerator, Shield, WAF** | Low-latency content delivery and DNS |
| **Regional edge cache** | Larger caches between edge locations and the origin | CloudFront content that is less popular |
| **Local Zone** | An extension of a Region placed close to a big city | Single-digit-millisecond latency to users in that city |
| **Outposts** | AWS racks in **your** data centre | Hybrid; see [02](02-compute-and-networking.md#aws-outposts) |

**How to choose a Region (four factors):**

1. **Compliance / data sovereignty:** laws may require data to stay in a country. (Usually the deciding factor.)
2. **Proximity / latency:** close to your customers.
3. **Service availability:** not every service or feature is in every Region.
4. **Pricing:** prices differ between Regions.

**Scope of services:** most services are **regional** (EC2, RDS, VPC…). Key **global** services: **IAM, Route 53, CloudFront, Organizations**, WAF when used with CloudFront. **S3** has globally unique bucket names but each bucket lives in one Region. **EBS** volumes and **subnets** are **AZ-scoped**.

**⚠️ Exam traps**

- **High availability** → multiple **AZs**. **Disaster recovery from a regional outage** or **data sovereignty** → multiple / specific **Regions**.
- Data never leaves a Region unless **you** move or replicate it.
- Edge locations are **not** where you run EC2 instances.

📖 **Read more:** [Regions and Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/) · [Global infrastructure overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/global-infrastructure.html) · [What is a Local Zone?](https://docs.aws.amazon.com/local-zones/latest/ug/what-is-aws-local-zones.html)

---

## AWS Well-Architected Framework

**📘 AWS definition:** The AWS Well-Architected Framework helps you understand the pros and cons of decisions you make while building systems on AWS, describing key concepts, design principles, and architectural best practices for designing and running workloads in the cloud.

**💡 Simple definition:** **A health checklist for a cloud workload**, organised into six pillars.

**🧮 Formula: Well-Architected = CROPSS**

> Picture a farmer checking six **crops** before harvest: a workload is only "well-architected" when all six are healthy.

| Letter | Pillar | One-line meaning | Think of it as… |
|---|---|---|---|
| **C** | **Cost Optimisation** | Deliver value at the lowest price; no waste | "Am I paying for anything I don't use?" |
| **R** | **Reliability** | Recover from failure and meet demand | "Does it keep working when something breaks?" |
| **O** | **Operational Excellence** | Run, monitor, and keep improving operations | "Can we run and improve it day to day?" |
| **P** | **Performance Efficiency** | Use the right resources efficiently as needs change | "Is it fast and right-sized?" |
| **S** | **Security** | Protect data, systems, and assets | "Is it locked down?" |
| **S** | **Sustainability** | Reduce environmental impact | "Is it kind to the planet?" |

Trick to remember the two S's: **S**ecurity protects the **system**, **S**ustainability protects the **surroundings** (the environment). Sustainability is the newest pillar (added in 2021), so it comes last.

**🔑 Exam keywords:** `six pillars` · `best practices` · `design principles` · `workload review` · `high-risk issues`

**🎯 What they test: match the scenario to the pillar**

| Pillar | Main question | Design principles / clues to match |
|---|---|---|
| **Operational Excellence** | Can we run, monitor, and improve the workload? | **Perform operations as code** (IaC), make **small, frequent, reversible changes**, refine procedures, **anticipate failure**, learn from operational events |
| **Security** | Can we protect data, systems, and assets? | Strong identity foundation (**least privilege**), **traceability** (logs), security at **all layers**, **automate** security, protect data **in transit and at rest**, prepare for incidents |
| **Reliability** | Can it recover from failure and meet demand? | **Automatically recover** from failure, **test recovery** procedures, **scale horizontally**, **stop guessing capacity**, manage change through automation |
| **Performance Efficiency** | Are we using the right resources efficiently as demand changes? | **Democratise advanced technologies** (use managed services), **go global in minutes**, use **serverless**, **experiment more often**, consider mechanical sympathy |
| **Cost Optimisation** | Are we avoiding unnecessary cost? | **Adopt a consumption model**, measure efficiency, **stop spending on undifferentiated heavy lifting**, analyse and attribute expenditure (tags) |
| **Sustainability** | Are we reducing environmental impact? | Understand your impact, maximise **utilisation**, use **managed services**, adopt more efficient hardware/software, reduce downstream impact |

**⚠️ Exam traps**

- "Multi-AZ, automatic failover, backups" → **Reliability**, not Performance.
- "Choose the right instance type, use serverless/CDN for speed" → **Performance Efficiency**.
- "Turn off idle resources, rightsizing, Savings Plans" → **Cost Optimisation**.
- "Maximise utilisation to reduce energy" → **Sustainability**.
- "Infrastructure as code, runbooks, small changes" → **Operational Excellence**.

📖 **Read more:** [Well-Architected Framework: the pillars](https://docs.aws.amazon.com/wellarchitected/latest/framework/pillars.html) · [Definitions](https://docs.aws.amazon.com/wellarchitected/latest/framework/definitions.html) · [Well-Architected home](https://aws.amazon.com/architecture/well-architected/)

### AWS Well-Architected Tool

**📘 AWS definition:** The AWS Well-Architected Tool is a service in the cloud that provides a consistent process for measuring your architecture using AWS best practices.

**💡 Simple definition:** **The Well-Architected checklist as a free questionnaire in the console.** Answer questions about one workload; get a list of risks and fixes.

**🔑 Exam keywords:** `review a workload` · `high-risk issues (HRIs)` · `six pillars` · `milestones` · `improvement plan`

**🎯 What they test**

- Free self-service tool; identifies **high and medium-risk issues** per pillar, tracks **milestones** over time, and offers lenses (for example Serverless, SaaS).

**⚠️ Exam traps**

- Well-Architected Tool = review **one workload** by answering questions. **Trusted Advisor** = automatic checks of your **whole account**.
- It gives guidance; it does not fix anything for you.

📖 **Read more:** [What is the AWS Well-Architected Tool?](https://docs.aws.amazon.com/wellarchitected/latest/userguide/intro.html)

---

## AWS Cloud Adoption Framework (AWS CAF)

**📘 AWS definition:** The AWS Cloud Adoption Framework (AWS CAF) leverages AWS experience and best practices to help you digitally transform and accelerate your business outcomes through innovative use of AWS.

**💡 Simple definition:** **A roadmap for the organisation (people, process, technology) moving to the cloud.** Well-Architected judges a **workload**; CAF guides the **company**.

**🔑 Exam keywords:** `cloud transformation` · `six perspectives` · `business outcomes` · `organisational readiness` · `foundational capabilities`

**🎯 What they test**

**🧮 Formula: CAF = GBP + OPS**

> **GBP** is the British pound, money, so it's the **business side**. **OPS** is short for operations, so it's the **technical side**. The cloud journey needs both the money people and the ops people.

| Formula part | Letter | Perspective | One-line meaning |
|---|---|---|---|
| **GBP** (business side) | **G** | **Governance** | Manage risk, budgets, and the cloud programme |
| | **B** | **Business** | Make sure cloud spending delivers business value |
| | **P** | **People** | Skills, culture, training, and change (the **bridge** between business and tech) |
| **OPS** (technical side) | **O** | **Operations** | Run cloud services reliably day to day |
| | **P** | **Platform** | Build the cloud platform and architecture |
| | **S** | **Security** | Protect data and workloads |

Do not mix up the two **P**s: **P**eople sits with the business (GBP), **P**latform sits with the technology (OPS).

**Six perspectives in detail** (who owns each one):

| Business-focused | Who owns it | Technical-focused | Who owns it |
|---|---|---|---|
| **Business**: cloud investments drive business outcomes | CEO, CFO, CIO, strategy | **Platform**: build an enterprise-grade, scalable cloud platform | CTO, architects, engineers |
| **People**: culture, skills, roles, change management (the **bridge** between business and technology) | HR, staffing, people managers | **Security**: confidentiality, integrity, availability of data and workloads | CISO, security engineers |
| **Governance**: maximise benefits, minimise risk; programme and portfolio management, **financial management** | CFO, chief risk officer, programme managers | **Operations**: deliver cloud services to agreed business levels; monitoring, incident management | IT operations, site reliability |

**Benefits (business outcomes) CAF lists:** **reduced business risk**, **improved ESG performance** (environmental, social, governance), **increased revenue**, and **increased operational efficiency**.

**Transformation journey phases:** **Envision → Align → Launch → Scale**.
**Transformation domains:** Technology → Process → Organisation → Product.

**⚠️ Exam traps**

- "Staff training, skills gaps, organisational change" → **People** perspective.
- "Align IT and business, measure cloud spend, manage risk" → **Governance**.
- CAF plans an **organisation's adoption**; Well-Architected reviews a **workload's design**.

📖 **Read more:** [AWS CAF overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/overview-aws-cloud-adoption-framework/welcome.html) · [AWS CAF home](https://aws.amazon.com/cloud-adoption-framework/)

---

## Migration strategies: the 7 Rs

| Strategy | Plain meaning | Clue |
|---|---|---|
| **Retire** | Turn it off; no longer needed | "decommission unused applications" |
| **Retain** | Keep it where it is for now (revisit later) | "not ready to migrate", "recent upgrade" |
| **Rehost** | **Lift and shift**, no changes | "move quickly without changes", Application Migration Service |
| **Relocate** | Move a whole platform without changing it (for example VMware Cloud on AWS) | "move VMs at hypervisor level" |
| **Repurchase** | **Drop and shop**: switch to a different product, often SaaS | "replace CRM with a SaaS product" |
| **Replatform** | **Lift, tinker, and shift**: small optimisations | "move the database to RDS without changing the app" |
| **Refactor / re-architect** | Redesign to be cloud-native | "rebuild as microservices/serverless for agility" |

Memory hook: **"Two stay (Retire, Retain), three move as-is-ish (Rehost, Relocate, Replatform), one replaces (Repurchase), one rebuilds (Refactor)."**

**Benefits of migrating** the exam likes: lower TCO, agility, elasticity, reduced operational overhead, better security and resilience, global reach, faster innovation.

📖 **Read more:** [Migration strategies (7 Rs)](https://docs.aws.amazon.com/prescriptive-guidance/latest/large-migration-guide/migration-strategies.html)

## AWS migration tools

| Service | 💡 Simple definition | 🔑 Keyword | ⚠️ Trap |
|---|---|---|---|
| **AWS Application Discovery Service** | Collects data about on-premises servers, usage, and **dependencies** to plan a migration | "discover", "inventory", "dependencies" | AWS says it is no longer open to new customers (it points to **AWS Transform**), but it is still on the in-scope list |
| **Migration Evaluator** | Builds a **business case** and cost projection for moving to AWS | "business case", "TCO estimate" | Estimates value; does not move anything |
| **AWS Migration Hub** | One dashboard to **track** migration progress across tools | "track", "single place" | Tracks; others do the moving |
| **AWS Application Migration Service (MGN)** | Automated **lift-and-shift (rehost)** of servers with continuous replication | "rehost", "minimal downtime", "lift and shift" | Renamed **AWS Transform MGN** in June 2026; the exam may use the old name. Not for DR (that is Elastic Disaster Recovery) |
| **AWS DMS** and **AWS SCT** | Move databases / convert schemas | "migrate database" | See [03](03-storage-and-databases.md#aws-database-migration-service-aws-dms) |

Order to remember: **Discover → Evaluate (business case) → Migrate (MGN/DMS) → Track (Migration Hub)**.

📖 **Read more:** [What is Application Discovery Service?](https://docs.aws.amazon.com/application-discovery/latest/userguide/what-is-appdiscovery.html) · [Migration Evaluator](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-tools/discovery-migration-evaluator.html) · [What is AWS Migration Hub?](https://docs.aws.amazon.com/migrationhub/latest/ug/whatishub.html) · [What is AWS Application Migration Service?](https://docs.aws.amazon.com/mgn/latest/ug/what-is-application-migration-service.html) · [MGN rename announcement](https://aws.amazon.com/about-aws/whats-new/2026/06/aws-transform-mgn-rebrand/)

---

## Cloud economics

| Idea | What to remember |
|---|---|
| **CapEx vs OpEx** | **Capital expense** = buy hardware up front (on-premises). **Operational expense** = pay as you use (cloud). |
| **Total cost of ownership (TCO)** | On-premises costs include servers, storage, network, **power, cooling, floor space, staff, licences**, and over-buying for peak demand. Cloud removes most of these. |
| **Licensing** | **License Included**: the licence is in the AWS price. **BYOL (Bring Your Own License)**: reuse licences you own, subject to vendor terms (often with **Dedicated Hosts**; track with **License Manager**). |
| **Rightsizing** | Match the resource size to real usage; do it continuously (**Compute Optimizer**). |
| **Automation** | Scheduled start/stop, Auto Scaling, and infrastructure as code reduce idle cost and human error. |
| **Managed services** | Shift undifferentiated work (patching, backups) to AWS, reducing staff cost. |

📖 **Read more:** [Licensing options on AWS](https://aws.amazon.com/windows/resources/licensing/) · [Right sizing](https://aws.amazon.com/aws-cost-management/aws-cost-optimization/right-sizing/)

## Pricing principles

1. **Pay as you go:** no long-term contracts for most services.
2. **Save when you commit:** Savings Plans and Reserved Instances.
3. **Pay less by using more:** tiered pricing (for example S3 per-GB price drops at higher volumes) and volume discounts across accounts with consolidated billing.

**Usually free:** IAM, VPC itself, Organizations and consolidated billing, CloudFormation, Elastic Beanstalk, Auto Scaling (pay for the resources they create), Shield Standard, Cost Explorer UI, Well-Architected Tool, Trusted Advisor core checks.

📖 **Read more:** [How AWS pricing works](https://docs.aws.amazon.com/whitepapers/latest/how-aws-pricing-works/welcome.html) · [AWS Pricing](https://aws.amazon.com/pricing/)

## Data transfer costs

- **Inbound** data from the internet: **free** (in most cases).
- **Outbound** to the internet: **charged** (tiered).
- **Between Regions**: charged. **Between AZs** in the same Region: usually charged.
- Within the same AZ using private IPs, and many same-Region service-to-service transfers (for example EC2 to S3): commonly free.
- **CloudFront** can reduce outbound costs and origin load; **Direct Connect** can lower transfer cost for large, steady volumes.

📖 **Read more:** [Understanding data transfer charges](https://docs.aws.amazon.com/cur/latest/userguide/cur-data-transfers-charges.html)

## Compute purchase options summary

Full detail with traps is in [02 → EC2 purchase options](02-compute-and-networking.md#ec2-purchase-options). The short version:

| Option | One-line rule |
|---|---|
| **On-Demand** | Flexible, no commitment, can't be interrupted |
| **Savings Plans** | Commit to $/hour for 1–3 years; flexible across instance families, Regions, **Fargate, and Lambda** (Compute SP) |
| **Reserved Instances** | Commit to an instance configuration for 1–3 years; **Standard** (biggest discount) or **Convertible** (can change type) |
| **Spot** | Up to 90% off; **can be interrupted** with 2 minutes' notice |
| **Dedicated Host / Instance** | Physical isolation; **Host** for socket/core licensing (BYOL) |

In **AWS Organizations**, RI and Savings Plans discounts are **shared** across accounts by default (the management account can turn sharing off).

📖 **Read more:** [Savings Plans vs Reserved Instances](https://docs.aws.amazon.com/savingsplans/latest/userguide/sp-ris.html) · [RI and Savings Plans discount sharing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ri-turn-off.html)

## AWS Free Tier

- New customers get free usage of many services to try AWS (for example credits or limited monthly usage), plus some **always-free** offers (for example Lambda's 1 million requests per month).
- The structure of the Free Tier changes over time; check the current page before the exam rather than memorising amounts.
- Use **AWS Budgets** alerts to avoid surprise bills when the free allowance runs out.

📖 **Read more:** [AWS Free Tier](https://aws.amazon.com/free/)

---

## AWS Support plans

**📘 AWS definition:** AWS Support offers a range of plans that provide access to tools and expertise that support the success and operational health of your AWS solutions.

**💡 Simple definition:** **Levels of help, from self-service to a dedicated team watching your systems.** The more critical your workload, the higher the plan.

**🔑 Exam keywords:** `Basic` · `Business Support+` · `Enterprise Support` · `Unified Operations` · `Technical Account Manager (TAM)` · `response time` · `production workload` · `mission-critical`

**🎯 What they test**

AWS now offers **four** plans; each paid plan includes everything in the one before it.

| Plan | For | Key features |
|---|---|---|
| **Basic** (free, all customers) | Getting started | 24/7 **customer service** (billing and account), documentation, whitepapers, **AWS re:Post**, **core Trusted Advisor** checks, **AWS Health Dashboard**. **No technical support cases** |
| **Business Support+** | **Production** workloads (AWS's recommended minimum) | 24/7 technical support by phone, web, chat, and email; AI-powered troubleshooting; **full Trusted Advisor** checks; **AWS Health API**; unlimited cases and contacts; third-party software support; critical-system-down response **under 30 minutes** |
| **Enterprise Support** | **Business-critical** workloads | Everything above, plus a designated **Technical Account Manager (TAM)**, Well-Architected and security reviews, Trusted Advisor Priority, critical-system-down response **under 15 minutes** |
| **Unified Operations** | **Mission-critical** workloads | Everything above, plus designated domain specialist engineers, Incident Management Engineers, 24/7 workload monitoring, response **under 5 minutes** |

Memory hook: **"B, B+, E, U"** → **B**asic, **B**usiness **+**, **E**nterprise, **U**nified, getting faster: **none → 30 min → 15 min → 5 min**.

**⚠️ Exam traps**

- **Basic** includes **no** technical support cases; account and billing questions only.
- The **TAM** starts at **Enterprise Support**.
- Older materials and practice tests list **Developer, Business, and Enterprise On-Ramp** plans. Recognise those names, but learn the four current plans above for the exam.
- The **full** set of Trusted Advisor checks and the **Health API** need **Business Support+ or higher**.

📖 **Read more:** [Compare AWS Support plans](https://aws.amazon.com/premiumsupport/plans/) · [AWS Support plans guide](https://docs.aws.amazon.com/awssupport/latest/user/aws-support-plans.html) · [Announcement of the new plans](https://aws.amazon.com/about-aws/whats-new/2025/12/aws-support-transformation-ai-powered-operations)

## Technical resources and where to get help

| Need | Go to | Memory hook |
|---|---|---|
| Official guides and deep dives | **AWS Documentation**, **whitepapers**, **AWS blogs** | The textbook |
| Ask the community a question | **AWS re:Post** | Community Q&A forum |
| Official answers to common questions | **AWS Knowledge Center** (on re:Post) | AWS's FAQ for problems |
| Proven migration/modernisation patterns and guides | **AWS Prescriptive Guidance** | Recipes from AWS experts |
| Open and manage support cases | **AWS Support Center** | Help desk |
| AWS events affecting my resources | **AWS Health Dashboard** / Health API | Weather report |
| Report **abuse** of AWS resources (spam, phishing, malware, DDoS from AWS IPs) | **AWS Trust and Safety** team | Report misuse |
| Hands-on expert help for a project | **AWS Professional Services**, **AWS solutions architects** | AWS consultants |
| Third-party consultancy or software | **AWS Partner Network (APN)** partners | Certified helpers |
| Security news and how-tos | **AWS Security Blog**, Security Center | Security newspaper |

⚠️ **Trap:** "Report that an AWS IP is sending spam/attacking us" → **AWS Trust and Safety**, not AWS Support.

📖 **Read more:** [Documentation](https://docs.aws.amazon.com/) · [Whitepapers](https://aws.amazon.com/whitepapers/) · [re:Post](https://repost.aws/) · [Knowledge Center](https://repost.aws/knowledge-center) · [Prescriptive Guidance](https://aws.amazon.com/prescriptive-guidance/) · [Report abuse](https://repost.aws/knowledge-center/report-aws-abuse) · [Professional Services](https://aws.amazon.com/professional-services/)

## AWS Marketplace and the AWS Partner Network

**📘 AWS definition:** AWS Marketplace is a curated digital catalogue that customers can use to find, buy, deploy, and manage third-party software, data, and services to build solutions and run their businesses.

**💡 Simple definition:** **An app store for business software that runs on AWS**, billed on your AWS invoice.

**🔑 Exam keywords:** `third-party software` · `ISV` · `buy and deploy` · `pay on the AWS bill` · `private offers` · `BYOL` · `APN partners`

**🎯 What they test**

- Thousands of listings from **independent software vendors (ISVs)**: security tools, AMIs, SaaS, data, ML models, professional services.
- Pricing options: free, hourly/usage, annual, **BYOL**, **private offers** (custom negotiated prices). Billing goes on the AWS bill.
- Marketplace helps with **cost management** (one bill), **governance** (private marketplace of approved products), and **entitlement** (licence management).
- **AWS Partner Network (APN):** consulting partners and **system integrators** (help you build/migrate) and **software partners/ISVs** (build products). **Benefits of being a partner:** training and certification, partner events, marketing and funding programmes, **partner volume discounts**.

**⚠️ Exam traps**

- **Buy third-party** software → Marketplace. **Internal approved** catalogue → Service Catalog.

📖 **Read more:** [What is AWS Marketplace?](https://docs.aws.amazon.com/marketplace/latest/buyerguide/what-is-marketplace.html) · [AWS Marketplace](https://aws.amazon.com/marketplace) · [AWS Partner Network](https://aws.amazon.com/partners/)

---

## Top traps on this page

1. **Elasticity** = automatic scaling with demand; **scalability** = ability to grow.
2. Multiple **AZs** = high availability; multiple **Regions** = DR / data sovereignty.
3. Most important Region factor is usually **compliance / data residency**.
4. IAM, Route 53, CloudFront, Organizations are **global**.
5. Well-Architected = **workload**; CAF = **organisation**.
6. "Skills/training" = CAF **People**; "risk/portfolio/finance" = CAF **Governance**.
7. Rehost = **lift and shift**; Replatform = **lift, tinker, and shift**; Repurchase = **SaaS**.
8. Data **in** is free; data **out** is charged.
9. Basic Support has **no technical cases**; TAM starts at **Enterprise**.
10. Abuse reports → **Trust and Safety**.

[← Exam overview](00-exam-overview.md) | [Index](README.md) | [Compute and networking →](02-compute-and-networking.md)
