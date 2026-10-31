# 00 — CLF-C02 Exam Overview

[← Index](README.md) | [Next: Cloud concepts →](01-cloud-concepts.md)

## Exam at a glance

| Item | Detail |
|---|---|
| Questions | **65** in total: 50 scored + 15 unscored (unscored ones are not marked, so treat every question as scored) |
| Time | **90 minutes** (about 1.4 minutes per question) |
| Question types | **Multiple choice** (1 correct of 4) and **multiple response** (2 or more correct of 5 or more) |
| Pass mark | Scaled score **700 out of 1,000** |
| Scoring | Compensatory: you only need to pass overall, not each domain |
| Unanswered | Marked wrong; **no penalty for guessing**, so answer everything |
| Cost / validity | 100 USD; certificate valid for 3 years |
| Delivery | Pearson VUE test centre or online proctored |

📖 **AWS docs:** [Exam page](https://aws.amazon.com/certification/certified-cloud-practitioner/) · [Exam guide](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02.html) · [Certification FAQs](https://aws.amazon.com/certification/faqs/)

## Scored domains

| Domain | Weight | Study priority | Official task statements |
|---|---:|---|---|
| 1. Cloud Concepts | 24% | Cloud value, design principles, migration, economics | [Domain 1](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain1.html) |
| 2. Security and Compliance | 30% | Shared responsibility, IAM, governance, compliance | [Domain 2](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain2.html) |
| 3. Cloud Technology and Services | 34% | Core compute, network, storage, and database services, plus AI/ML, analytics, and other service categories | [Domain 3](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain3.html) |
| 4. Billing, Pricing, and Support | 12% | Pricing models, budgets, support plans, technical resources | [Domain 4](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain4.html) |

## What the exam tests

- Pick the **best-fit service** from business and technical keywords.
- Know **managed vs. unmanaged**, **serverless vs. provisioned**, and **global vs. regional**.
- Know who does what under the **Shared Responsibility Model**.
- Recognise cost tools, pricing models, support resources, and cloud benefits.
- **Out of scope** (per AWS): coding, designing architecture, troubleshooting, implementation, and load testing.
- The in-scope service list is **non-exhaustive and can change**; check it before the exam. AWS also lists services that are [out of scope](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-out-of-scope-services.html) (for example Network Firewall, CodeDeploy, CloudShell, MSK), so do not spend study time on those.

📖 **AWS docs:** [In-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-in-scope-services.html) · [Technologies and concepts](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-technologies-concepts.html)

## Test-taking strategy

1. Identify the requirement: cost, availability, security, performance, or least management.
2. Circle mental keywords: **object**, **block**, **file**, **relational**, **NoSQL**, **audit**, or **metrics**.
3. Eliminate services from the wrong category; distractors are usually plausible but belong to a different job.
4. Prefer the service that meets the need with the **least operational work**.
5. For multiple-response questions, select exactly the number of answers requested.
6. Flag long questions and return later; answer every question before time ends.

## Frequent question patterns

### Core services

| If the question says… | Think… | Study note |
|---|---|---|
| Audit API activity, who did what | CloudTrail | [CloudTrail](05-management-and-analytics.md#aws-cloudtrail) |
| Metric, alarm, or application log | CloudWatch | [CloudWatch](05-management-and-analytics.md#amazon-cloudwatch) |
| Object storage | S3 | [S3](03-storage-and-databases.md#amazon-s3) |
| Disk for one EC2 instance | EBS | [EBS](03-storage-and-databases.md#amazon-ebs) |
| Shared Linux file system | EFS | [EFS](03-storage-and-databases.md#amazon-efs) |
| Relational database | RDS / Aurora | [RDS](03-storage-and-databases.md#amazon-rds) |
| Key-value NoSQL at any scale | DynamoDB | [DynamoDB](03-storage-and-databases.md#amazon-dynamodb) |
| Protect web apps from SQL injection | WAF | [WAF](04-security-and-compliance.md#aws-waf) |
| DDoS protection | Shield | [Shield](04-security-and-compliance.md#aws-shield) |
| Dedicated private link to AWS | Direct Connect | [Direct Connect](02-compute-and-networking.md#aws-direct-connect) |
| DNS and domain registration | Route 53 | [Route 53](02-compute-and-networking.md#amazon-route-53) |

### Other in-scope services (full notes are in the linked files)

| If the question says… | Think… | Study note |
|---|---|---|
| Run batch jobs at scale | AWS Batch | [02](02-compute-and-networking.md) |
| Run AWS infrastructure in your own data centre | AWS Outposts | [02](02-compute-and-networking.md) |
| Store container images | Amazon ECR | [02](02-compute-and-networking.md) |
| Create and publish APIs | API Gateway | [02](02-compute-and-networking.md) |
| Private access to a service without the internet | PrivateLink | [02](02-compute-and-networking.md) |
| Hub that connects many VPCs and on-premises networks | Transit Gateway | [02](02-compute-and-networking.md) |
| Encrypted tunnel over the internet to AWS | Site-to-Site VPN (Client VPN for individual users) | [02](02-compute-and-networking.md) |
| Windows file shares / managed file systems | FSx | [03](03-storage-and-databases.md) |
| On-premises apps using cloud storage with a local cache | Storage Gateway | [03](03-storage-and-databases.md) |
| Disaster recovery for servers | Elastic Disaster Recovery | [03](03-storage-and-databases.md) |
| MongoDB-compatible database | DocumentDB | [03](03-storage-and-databases.md) |
| Graph database, highly connected data | Neptune | [03](03-storage-and-databases.md) |
| Migrate a database / convert its schema | DMS / SCT | [03](03-storage-and-databases.md#aws-database-migration-service-aws-dms) |
| Workforce single sign-on across accounts | IAM Identity Center | [04](04-security-and-compliance.md) |
| Central view of security findings | Security Hub | [04](04-security-and-compliance.md) |
| Find sensitive data (PII) in S3 | Macie | [04](04-security-and-compliance.md) |
| Investigate the cause of a security finding | Detective | [04](04-security-and-compliance.md) |
| Manage firewall rules across accounts | Firewall Manager | [04](04-security-and-compliance.md) |
| Dedicated hardware for encryption keys | CloudHSM | [04](04-security-and-compliance.md) |
| Right-size recommendations for compute | Compute Optimizer | [05](05-management-and-analytics.md) |
| Governed multi-account environment | Control Tower | [05](05-management-and-analytics.md) |
| AWS service events affecting your resources | Health Dashboard | [05](05-management-and-analytics.md) |
| Approved catalogue of products for users | Service Catalog | [05](05-management-and-analytics.md) |
| View or request quota increases | Service Quotas | [05](05-management-and-analytics.md) |
| Push notifications (pub/sub) | SNS | [05](05-management-and-analytics.md) |
| Message queue to decouple components | SQS | [05](05-management-and-analytics.md) |
| Route events or run on a schedule | EventBridge | [05](05-management-and-analytics.md) |
| Coordinate the steps of a workflow | Step Functions | [05](05-management-and-analytics.md) |
| Real-time streaming data | Kinesis | [05](05-management-and-analytics.md) |
| ETL and data catalogue | Glue | [05](05-management-and-analytics.md) |
| Cloud contact centre | Amazon Connect | [05](05-management-and-analytics.md) |
| Send email at scale | SES | [05](05-management-and-analytics.md) |
| Build code / CI/CD pipeline | CodeBuild / CodePipeline | [05](05-management-and-analytics.md) |
| Trace requests through an application | X-Ray | [05](05-management-and-analytics.md) |
| Virtual desktops | WorkSpaces | [05](05-management-and-analytics.md) |
| Stream desktop applications to users | AppStream 2.0 | [05](05-management-and-analytics.md) |
| Build web and mobile front ends | Amplify | [05](05-management-and-analytics.md) |
| Connect and manage IoT devices | IoT Core | [05](05-management-and-analytics.md) |
| Build, train, and deploy ML models | SageMaker AI | [05](05-management-and-analytics.md) |
| Chatbot / voice interface | Lex | [05](05-management-and-analytics.md) |
| Text to speech, speech to text, translation | Polly, Transcribe, Translate | [05](05-management-and-analytics.md) |
| Image analysis, text from documents, text sentiment | Rekognition, Textract, Comprehend | [05](05-management-and-analytics.md) |
| Discover on-premises servers for migration | Application Discovery Service | [01](01-cloud-concepts.md) |
| Track and manage a migration | Migration Hub | [01](01-cloud-concepts.md) |
| Lift and shift servers | Application Migration Service | [01](01-cloud-concepts.md) |
| Business case for migrating | Migration Evaluator | [01](01-cloud-concepts.md) |
| Buy third-party software | AWS Marketplace | [01](01-cloud-concepts.md) |
| Report abuse of AWS resources | AWS Trust and Safety | [01](01-cloud-concepts.md) |

## Official references

- [CLF-C02 exam guide](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02.html)
- [Exam appointment details](https://aws.amazon.com/certification/certified-cloud-practitioner/)
- [Exam policies](https://aws.amazon.com/certification/policies/)
- [AWS Documentation home](https://docs.aws.amazon.com/)
- Every service note in this repo ends with a **📖 AWS docs** line so you can read beyond the exam-level summary.