# 00 — CLF-C02 Exam Overview

[← Index](README.md) | [Next: Cloud concepts →](01-cloud-concepts.md)

## Exam at a glance

| Item | Detail |
|---|---|
| Questions | **65** in total: 50 scored + 15 unscored (you can't tell which, so treat every question as scored) |
| Question types | **Multiple choice** (1 correct of 4) and **multiple response** (2+ correct of 5 or more) |
| Time | **90 minutes** (about 1.4 minutes per question) |
| Pass mark | Scaled score of **700 out of 1,000** |
| Scoring | **Compensatory**: you only need to pass overall, not each domain |
| Unanswered | Marked wrong; **no penalty for guessing**, so answer everything |
| Delivery | Pearson VUE test centre or online proctored |
| Exam guide version used for these notes | **1.3 (29 October 2026)** |

📖 **Read more:** [Exam page](https://aws.amazon.com/certification/certified-cloud-practitioner/) · [Exam guide](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02.html) · [Certification FAQs](https://aws.amazon.com/certification/faqs/)

## Scored domains

| Domain | Weight | Study notes in this repo | Official task statements |
|---|---:|---|---|
| 1. Cloud Concepts | 24% | [01](01-cloud-concepts.md) (benefits, Well-Architected, CAF, migration, economics) | [Domain 1](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain1.html) |
| 2. Security and Compliance | 30% | [04](04-security-and-compliance.md) + governance tools in [05](05-management-and-governance.md) | [Domain 2](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain2.html) |
| 3. Cloud Technology and Services | 34% | [02](02-compute-and-networking.md), [03](03-storage-and-databases.md), [05](05-management-and-governance.md), [06](06-analytics-integration-ml-and-more.md) | [Domain 3](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain3.html) |
| 4. Billing, Pricing, and Support | 12% | [01](01-cloud-concepts.md#aws-support-plans) (pricing, support) + [05 cost tools](05-management-and-governance.md#cost-tools-at-a-glance) | [Domain 4](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain4.html) |

## What the exam tests (and what it does not)

- Pick the **best-fit service** from business and technical keywords.
- Know **managed vs unmanaged**, **serverless vs provisioned**, and **global vs regional vs zonal**.
- Know who does what under the **Shared Responsibility Model**.
- Recognise cost tools, pricing models, support plans, and cloud benefits.
- **Not tested** (per AWS): writing code, designing architectures, troubleshooting, implementation, and load/performance testing.

## How to study with these notes

Every service card has the same six parts:

| Part | Use it to… |
|---|---|
| 📘 **AWS definition** | Learn the official wording; exam options often reuse it |
| 💡 **Simple definition** | Remember the job of the service in one picture or hook |
| 🔑 **Exam keywords** | Spot the service from the words in the question |
| 🎯 **What they test** | Learn the sub-topics that actually get asked (classes, types, comparisons) |
| ⚠️ **Exam traps** | Avoid the look-alike wrong answer |
| 📖 **Read more** | Go to the AWS documentation for a deeper understanding |

Suggested plan:

1. **Week 1:** 01 and 04 (Domains 1 and 2 = 54% of the score).
2. **Week 2:** 02 and 03 (core compute, networking, storage, databases).
3. **Week 3:** 05 and 06 (management, cost, and "match the task" services).
4. **Final days:** [07 cheat sheet](07-cheat-sheet-exam-highlights.md) + practice exams. For every wrong answer, re-read that service's ⚠️ traps.

⚠️ These notes are a summary to make study easier and help you find your weak spots. They do **not** guarantee a pass. Read the linked AWS documentation for a deeper understanding.

## Test-taking strategy

1. Find the **requirement word**: cheapest, most available, most secure, fastest, **least operational overhead**.
2. Find the **category keyword**: object/block/file, relational/NoSQL, audit/metrics, threat/vulnerability.
3. Eliminate answers from the wrong category; distractors are usually real services that do a **different** job.
4. "Least operational overhead" → prefer **managed or serverless** services.
5. Multiple response: select **exactly** the number asked.
6. Flag long questions and come back; answer every question before time runs out.

## Frequent question patterns

| If the question says… | Think… | Card |
|---|---|---|
| Who made this API call / audit | CloudTrail | [05](05-management-and-governance.md#aws-cloudtrail) |
| Metric, alarm, application log | CloudWatch | [05](05-management-and-governance.md#amazon-cloudwatch) |
| Resource configuration history / compliance rules | Config | [05](05-management-and-governance.md#aws-config) |
| Object storage, static website | S3 | [03](03-storage-and-databases.md#amazon-s3) |
| Choose an S3 class / archive | S3 storage classes | [03](03-storage-and-databases.md#s3-storage-classes-and-s3-glacier) |
| Disk for one EC2 instance | EBS | [03](03-storage-and-databases.md#amazon-ebs) |
| Shared Linux file system | EFS | [03](03-storage-and-databases.md#amazon-efs) |
| Relational database, Multi-AZ, read replicas | RDS / Aurora | [03](03-storage-and-databases.md#amazon-rds) |
| NoSQL key-value at any scale | DynamoDB | [03](03-storage-and-databases.md#amazon-dynamodb) |
| Cheapest EC2 option / RI types | EC2 purchase options | [02](02-compute-and-networking.md#ec2-purchase-options) |
| Run code without servers | Lambda | [02](02-compute-and-networking.md#aws-lambda) |
| Firewall: instance vs subnet | Security groups vs NACLs | [02](02-compute-and-networking.md#security-groups-vs-network-acls) |
| Dedicated private link to AWS | Direct Connect | [02](02-compute-and-networking.md#aws-direct-connect) |
| DNS, routing policies | Route 53 | [02](02-compute-and-networking.md#amazon-route-53) |
| CDN | CloudFront | [02](02-compute-and-networking.md#amazon-cloudfront) |
| Who is responsible for X | Shared Responsibility Model | [04](04-security-and-compliance.md#shared-responsibility-model) |
| Permissions, roles, MFA | IAM | [04](04-security-and-compliance.md#aws-identity-and-access-management-iam) |
| DDoS / SQL injection | Shield / WAF | [04](04-security-and-compliance.md#aws-shield) |
| Threat / vulnerability / PII | GuardDuty / Inspector / Macie | [04](04-security-and-compliance.md#the-detection-family-at-a-glance) |
| Estimate / analyse / alert on cost | Pricing Calculator / Cost Explorer / Budgets | [05](05-management-and-governance.md#cost-tools-at-a-glance) |
| Which support plan | Support plans | [01](01-cloud-concepts.md#aws-support-plans) |
| Which pillar / perspective | Well-Architected / CAF | [01](01-cloud-concepts.md#aws-well-architected-framework) |
| Queue / push / events / workflow | SQS / SNS / EventBridge / Step Functions | [06](06-analytics-integration-ml-and-more.md#integration-chooser) |
| AI task (speech, text, images) | AI/ML services | [06](06-analytics-integration-ml-and-more.md#aiml-chooser) |

The full A–Z keyword list is in the [cheat sheet](07-cheat-sheet-exam-highlights.md#trigger-words-az).

## Out of scope: recognise, don't study

AWS publishes an **out-of-scope** list. These services may appear in older practice tests or as **wrong answer options**, but you do not need to study them. Selected examples from exam guide version 1.3:

| Category | Out-of-scope examples |
|---|---|
| Analytics | AppFlow, Clean Rooms, Data Exchange, DataZone, Amazon MSK |
| Application integration | AppFabric, Simple Workflow Service (SWF) |
| Compute | AWS Copilot, AWS Wavelength |
| Cost / financial | Application Cost Profiler, DevPay, Billing Conductor |
| Customer enablement | AWS Activate, AWS IQ, Amazon Managed Services (AMS) |
| Database | Keyspaces, MemoryDB, AppConfig |
| Developer tools | Application Composer, CodeArtifact, **CodeDeploy**, CodeGuru, **CloudShell**, **Device Farm** |
| IoT / ML / games | IoT Device Defender, IoT Greengrass, Fraud Detector, Personalize, GameLift |
| Management | Chatbot, **Amazon Data Lifecycle Manager**, Launch Wizard |
| Media | Elemental services, Interactive Video Service (IVS) |
| Migration / networking | Migration Hub Refactor Spaces, Transfer Family, Cloud Map, Network Access Analyzer, Ground Station, VPC Lattice |
| Security / storage | Cloud Directory, **Network Firewall**, **FSx for Lustre** |

Also legacy and not on the in-scope list: the **Snow Family** (see [03](03-storage-and-databases.md#moving-large-data-sets-into-aws)).

📖 **Read more:** [In-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-in-scope-services.html) · [Out-of-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-out-of-scope-services.html) · [Technologies and concepts](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-technologies-concepts.html)

[← Index](README.md) | [Next: Cloud concepts →](01-cloud-concepts.md)
