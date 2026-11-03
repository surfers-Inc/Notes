# 06 — Cheat Sheet and Exam Highlights

[← Management and analytics](05-management-and-analytics.md) | [Index](README.md)

Use this file for final review. Follow the links for each full service template, and use the 📖 links at the end for official AWS reading.

## One-line service map

### Compute and containers

| Requirement / keyword | Best answer |
|---|---|
| Virtual server with OS control | [Amazon EC2](02-compute-and-networking.md#amazon-ec2) |
| Add or remove EC2 capacity automatically | [EC2 Auto Scaling](02-compute-and-networking.md#ec2-auto-scaling) |
| Spread traffic across healthy targets | [Elastic Load Balancing](02-compute-and-networking.md#elastic-load-balancing-elb) |
| Event-driven serverless code | [AWS Lambda](02-compute-and-networking.md#aws-lambda) |
| Easy application platform deployment | [Elastic Beanstalk](02-compute-and-networking.md#aws-elastic-beanstalk) |
| AWS-native container orchestration | [Amazon ECS](02-compute-and-networking.md#amazon-ecs) |
| Managed Kubernetes | [Amazon EKS](02-compute-and-networking.md#amazon-eks) |
| Serverless container compute | [AWS Fargate](02-compute-and-networking.md#aws-fargate) |
| Store container images | [Amazon ECR](02-compute-and-networking.md#amazon-ecr) |
| Batch jobs at scale | [AWS Batch](02-compute-and-networking.md#aws-batch) |
| AWS infrastructure in your own data centre | [AWS Outposts](02-compute-and-networking.md#aws-outposts) |
| Simple bundled VPS | [Amazon Lightsail](02-compute-and-networking.md#amazon-lightsail) |

### Networking and content delivery

| Requirement / keyword | Best answer |
|---|---|
| Private AWS network | [Amazon VPC](02-compute-and-networking.md#amazon-vpc) |
| Private link between two VPCs | VPC peering (section in [02](02-compute-and-networking.md#amazon-vpc-peering)) |
| Hub for many VPCs and on-premises networks | [AWS Transit Gateway](02-compute-and-networking.md#aws-transit-gateway) |
| Private access to one service, no internet | [AWS PrivateLink](02-compute-and-networking.md#aws-privatelink) |
| Quick encrypted link over the internet | [Site-to-Site VPN](02-compute-and-networking.md#aws-site-to-site-vpn) |
| Remote individual users | [Client VPN](02-compute-and-networking.md#aws-client-vpn) |
| Dedicated private connection | [AWS Direct Connect](02-compute-and-networking.md#aws-direct-connect) |
| DNS, domain, health-based routing | [Amazon Route 53](02-compute-and-networking.md#amazon-route-53) |
| CDN / cached edge content | [Amazon CloudFront](02-compute-and-networking.md#amazon-cloudfront) |
| Static IPs and global network acceleration | [AWS Global Accelerator](02-compute-and-networking.md#aws-global-accelerator) |
| Create and publish APIs | [Amazon API Gateway](02-compute-and-networking.md#amazon-api-gateway) |

### Storage

| Requirement / keyword | Best answer |
|---|---|
| Object storage / static files | [Amazon S3](03-storage-and-databases.md#amazon-s3) |
| Archive classes | [S3 Glacier](03-storage-and-databases.md#s3-glacier-storage-classes) |
| EC2 block disk (persistent) | [Amazon EBS](03-storage-and-databases.md#amazon-ebs) |
| Temporary disk on the EC2 host | [Instance store](03-storage-and-databases.md#ec2-instance-store) |
| Shared Linux file system | [Amazon EFS](03-storage-and-databases.md#amazon-efs) |
| Windows file shares, HPC, NetApp, OpenZFS | [Amazon FSx](03-storage-and-databases.md#amazon-fsx) |
| On-premises apps with cloud storage and local cache | [Storage Gateway](03-storage-and-databases.md#aws-storage-gateway) |
| Central backup policy | [AWS Backup](03-storage-and-databases.md#aws-backup) |
| Fast server recovery after a disaster | [Elastic Disaster Recovery](03-storage-and-databases.md#aws-elastic-disaster-recovery) |
| Offline physical data transfer (legacy) | [Snow Family](03-storage-and-databases.md#aws-snow-family-legacy) |

### Databases

| Requirement / keyword | Best answer |
|---|---|
| Managed relational database | [Amazon RDS](03-storage-and-databases.md#amazon-rds) |
| AWS-built MySQL/PostgreSQL-compatible database | [Amazon Aurora](03-storage-and-databases.md#amazon-aurora) |
| Serverless NoSQL key-value/document database | [Amazon DynamoDB](03-storage-and-databases.md#amazon-dynamodb) |
| MongoDB-compatible document database | [Amazon DocumentDB](03-storage-and-databases.md#amazon-documentdb) |
| Graph / relationships | [Amazon Neptune](03-storage-and-databases.md#amazon-neptune) |
| Data warehouse / OLAP | [Amazon Redshift](03-storage-and-databases.md#amazon-redshift) |
| In-memory cache | [Amazon ElastiCache](03-storage-and-databases.md#amazon-elasticache) |
| Database migration with low downtime | [AWS DMS](03-storage-and-databases.md#aws-database-migration-service-aws-dms) |
| Convert a database schema | [AWS SCT](03-storage-and-databases.md#aws-schema-conversion-tool-aws-sct) |

### Security, identity, and compliance

| Requirement / keyword | Best answer |
|---|---|
| AWS permissions | [IAM](04-security-and-compliance.md#aws-identity-and-access-management-iam) |
| Workforce single sign-on | [IAM Identity Center](04-security-and-compliance.md#aws-iam-identity-center) |
| Application customer identities | [Amazon Cognito](04-security-and-compliance.md#amazon-cognito) |
| Managed Microsoft Active Directory | [Directory Service](04-security-and-compliance.md#aws-directory-service) |
| Multi-account governance | [AWS Organizations](04-security-and-compliance.md#aws-organizations) |
| Share resources across accounts | [AWS RAM](04-security-and-compliance.md#aws-resource-access-manager-aws-ram) |
| Encryption keys | [AWS KMS](04-security-and-compliance.md#aws-key-management-service-aws-kms) |
| Dedicated hardware for keys | [AWS CloudHSM](04-security-and-compliance.md#aws-cloudhsm) |
| Store and rotate secret values | [AWS Secrets Manager](04-security-and-compliance.md#aws-secrets-manager) |
| TLS certificates | [AWS Certificate Manager](04-security-and-compliance.md#aws-certificate-manager-acm) |
| DDoS protection | [AWS Shield](04-security-and-compliance.md#aws-shield) |
| Filter malicious web requests | [AWS WAF](04-security-and-compliance.md#aws-waf) |
| Firewall rules across accounts | [Firewall Manager](04-security-and-compliance.md#aws-firewall-manager) |
| Threat detection | [Amazon GuardDuty](04-security-and-compliance.md#amazon-guardduty) |
| Vulnerability scanning | [Amazon Inspector](04-security-and-compliance.md#amazon-inspector) |
| Sensitive data (PII) in S3 | [Amazon Macie](04-security-and-compliance.md#amazon-macie) |
| Investigate cause of a finding | [Amazon Detective](04-security-and-compliance.md#amazon-detective) |
| Central view of security findings | [AWS Security Hub](04-security-and-compliance.md#aws-security-hub) |
| Compliance reports | [AWS Artifact](04-security-and-compliance.md#aws-artifact) |

### Management, cost, and governance

| Requirement / keyword | Best answer |
|---|---|
| Metrics, alarms, dashboards, logs | [Amazon CloudWatch](05-management-and-analytics.md#amazon-cloudwatch) |
| API and account activity audit | [AWS CloudTrail](05-management-and-analytics.md#aws-cloudtrail) |
| Resource configuration compliance | [AWS Config](05-management-and-analytics.md#aws-config) |
| Environment recommendations | [AWS Trusted Advisor](05-management-and-analytics.md#aws-trusted-advisor) |
| Rightsize EC2, EBS, Lambda | [Compute Optimizer](05-management-and-analytics.md#aws-compute-optimizer) |
| Secure multi-account landing zone | [Control Tower](05-management-and-analytics.md#aws-control-tower) |
| AWS events affecting my resources | [Health Dashboard](05-management-and-analytics.md#aws-health-dashboard) |
| Approved catalogue, licences, quotas | [Service Catalog, License Manager, Service Quotas](05-management-and-analytics.md#other-management-services-quick-reference) |
| Analyse past cost and forecast | [AWS Cost Explorer](05-management-and-analytics.md#aws-cost-explorer) |
| Cost/usage threshold alert | [AWS Budgets](05-management-and-analytics.md#aws-budgets) |
| Infrastructure as code | [AWS CloudFormation](05-management-and-analytics.md#aws-cloudformation) |
| Fleet operations and patching | [AWS Systems Manager](05-management-and-analytics.md#aws-systems-manager) |

### Analytics, integration, and other categories

| Requirement / keyword | Best answer |
|---|---|
| Query S3 data using SQL | [Amazon Athena](05-management-and-analytics.md#amazon-athena) |
| Real-time streaming data | [Amazon Kinesis](05-management-and-analytics.md#amazon-kinesis) |
| ETL, Spark/Hadoop, search, dashboards | [Glue, EMR, OpenSearch, Quick](05-management-and-analytics.md#other-analytics-services-quick-reference) |
| Push notifications to many subscribers | [Amazon SNS](05-management-and-analytics.md#amazon-sns-simple-notification-service) |
| Message queue / decoupling | [Amazon SQS](05-management-and-analytics.md#amazon-sqs-simple-queue-service) |
| Route events or schedules | [Amazon EventBridge](05-management-and-analytics.md#amazon-eventbridge) |
| Coordinate workflow steps | [AWS Step Functions](05-management-and-analytics.md#aws-step-functions) |
| AI/ML tasks (SageMaker AI, Lex, Polly, Rekognition and more) | [AI and ML services](05-management-and-analytics.md#ai-and-machine-learning-services) |
| Build, CI/CD, trace requests | [Developer tools](05-management-and-analytics.md#developer-tools) |
| Contact centre / send email | [Business applications](05-management-and-analytics.md#business-applications) |
| Virtual desktops, app streaming, secure browser | [End-user computing](05-management-and-analytics.md#end-user-computing) |
| Web and mobile frontend | [Amplify](05-management-and-analytics.md#frontend-web-and-mobile) |
| IoT device connectivity | [AWS IoT Core](05-management-and-analytics.md#internet-of-things-iot) |

### Migration, support, and marketplace

| Requirement / keyword | Best answer |
|---|---|
| Discover, track, evaluate, lift and shift | [Migration tools](01-cloud-concepts.md#aws-migration-tools) |
| Buy third-party software; report abuse; find help | [Technical resources](01-cloud-concepts.md#technical-resources-and-help) |
| Support plan levels | [Support plans](01-cloud-concepts.md#support-plans) |

## Most-tested comparisons

### Compute and storage

| Pair | Difference |
|---|---|
| Lambda vs Batch vs Fargate | Short event-driven functions vs queued batch jobs vs serverless containers |
| ECS vs EKS | AWS-native containers vs Kubernetes |
| ECR vs ECS | Image registry vs container runner |
| Outposts vs Direct Connect | AWS hardware on-premises vs a network link to AWS |

| S3 | EBS | Instance store | EFS | FSx |
|---|---|---|---|---|
| Object | Block, persistent | Block, **temporary** | Linux file (NFS) | Windows/HPC/NetApp/OpenZFS file |
| Access by API/HTTP | Attached disk, one AZ | Lost when instance stops or terminates | Shared mount, multi-AZ | Managed file servers |

### Database

| RDS / Aurora | DynamoDB | DocumentDB | Neptune | Redshift | ElastiCache |
|---|---|---|---|---|---|
| Relational / SQL | NoSQL key-value/document | MongoDB-compatible documents | Graph | Analytics warehouse | In-memory cache |
| Transactions and joins | Massive scale, low latency | Existing MongoDB apps | Relationships | OLAP and BI | Accelerate reads/sessions |

### Security

| Pair | Difference |
|---|---|
| Shield vs. WAF | DDoS protection vs. HTTP request filtering |
| WAF vs. Firewall Manager | Applies web rules vs. manages those rules across accounts |
| GuardDuty vs. Inspector vs. Macie | Suspicious behaviour vs. software vulnerabilities vs. sensitive data in S3 |
| GuardDuty vs. Detective vs. Security Hub | Detects threats vs. investigates root cause vs. collects findings in one place |
| KMS vs. CloudHSM | Managed encryption keys vs. dedicated hardware you control |
| KMS vs. Secrets Manager | Encryption keys vs. secret values and rotation |
| ACM vs. KMS | TLS certificates (in transit) vs. keys for data at rest |
| IAM vs. Cognito vs. Identity Center | AWS permissions vs. app customer sign-in vs. workforce SSO |
| Security group vs. NACL | Stateful resource firewall vs. stateless subnet firewall |

### Monitoring and governance

| Pair | Difference |
|---|---|
| CloudWatch vs. CloudTrail | Performance/logs vs. API audit history |
| CloudTrail vs. Config | Who did what vs. how a resource was configured |
| Trusted Advisor vs. Compute Optimizer | Broad best-practice checks vs. ML rightsizing of compute |
| Trusted Advisor vs. Well-Architected Tool | Environment recommendations vs. guided workload review |
| Health Dashboard vs. CloudWatch | AWS-side events affecting you vs. your own metrics and logs |
| Organizations vs. Control Tower | Accounts and SCPs vs. automated secure landing zone |
| Cost Explorer vs. Budgets | Analyse/forecast vs. alert on thresholds |
| Organizations SCP vs. IAM policy | Permission ceiling vs. permission grant/deny |

### Networking

| Pair | Difference |
|---|---|
| CloudFront vs. Global Accelerator | Caches content vs. improves network path without caching |
| Direct Connect vs. Site-to-Site VPN | Dedicated private link vs. encrypted tunnel over internet |
| Site-to-Site VPN vs. Client VPN | Connects networks vs. connects individual users |
| VPC peering vs. Transit Gateway vs. PrivateLink | Two VPCs, not transitive vs. hub for many networks vs. one service, one-way |
| Internet gateway vs. NAT gateway | Public internet path vs. private-subnet outbound IPv4 |
| Route 53 vs. load balancer | DNS routing vs. distributing traffic to targets |

### Messaging and data flow

| Pair | Difference |
|---|---|
| SNS vs. SQS | Push to many subscribers vs. queue consumers pull from |
| SNS vs. EventBridge | Broadcast messages vs. route events by rules and schedules |
| SQS vs. Step Functions | Moves messages vs. coordinates workflow steps |
| Kinesis vs. SQS vs. Athena | Real-time stream vs. message queue vs. SQL on stored S3 data |
| Glue vs. EMR | ETL and data catalogue vs. big data frameworks (Spark, Hadoop) |

### AI/ML: match the task

| Task | Service |
|---|---|
| Build custom ML models | SageMaker AI |
| Chatbot | Lex |
| Text to speech | Polly |
| Speech to text | Transcribe |
| Translate text | Translate |
| Text sentiment and entities | Comprehend |
| Image and video analysis | Rekognition |
| Extract data from documents | Textract |
| Generative AI assistant | Amazon Q |

### Desktops and apps

| Service | Memory aid |
|---|---|
| WorkSpaces | A **desktop** in the cloud |
| AppStream 2.0 | An **application** in the cloud |
| WorkSpaces Secure Browser | A **browser** in the cloud |

## Pricing triggers

| Phrase | Likely model/tool |
|---|---|
| Unpredictable, short-term EC2 | On-Demand |
| Steady compute commitment, flexible usage | Savings Plans |
| Predictable EC2 and possible capacity reservation | Reserved Instances (Standard = biggest discount, Convertible = exchangeable) |
| Fault-tolerant and interruptible | Spot Instances |
| Dedicated physical server for licences/compliance | Dedicated Hosts |
| Hardware used by only one customer, no server visibility | Dedicated Instances |
| Guarantee EC2 capacity in an AZ | On-Demand Capacity Reservation |
| Estimate a future architecture | AWS Pricing Calculator |
| Explore existing spending trends | Cost Explorer |
| Notify before forecast spend exceeds limit | AWS Budgets |
| Most detailed billing export | Cost and Usage Report |
| One bill, shared RI/Savings Plans discounts | Consolidated billing (Organizations) |
| Group costs by project or team | Cost allocation tags (activate them) |
| Right-size to cut cost | Compute Optimizer / rightsizing |
| Use licences you already own | BYOL (vs. License Included) |

Data transfer: **inbound is usually free**; outbound to the internet, between Regions, and often between AZs costs money. Public IPv4 addresses are charged per hour.

## Support plans at a glance

| Plan | Remember |
|---|---|
| **Basic** | Free; customer service, docs, re:Post, core Trusted Advisor checks, AWS Health |
| **Business Support+** | Production minimum; 24/7 technical support; full Trusted Advisor; critical-system response under 30 minutes |
| **Enterprise Support** | Business-critical; designated **TAM**; Well-Architected reviews; under 15 minutes |


## Global infrastructure triggers

- **Multiple AZs:** high availability inside one Region. AZs do not share single points of failure.
- **Multiple Regions:** disaster recovery, global latency, **data sovereignty**, or business continuity.
- **Edge locations:** CloudFront caching and edge networking close to users.
- A Region contains multiple isolated AZs; a subnet belongs to exactly one AZ.

## Six Well-Architected pillars

**Operational Excellence · Security · Reliability · Performance Efficiency · Cost Optimization · Sustainability**

Memory aid: **Operate Secure, Reliable, Performing, Cost-aware, Sustainable systems.**

## Six AWS CAF perspectives

| Business capabilities | Technical capabilities |
|---|---|
| Business, People, Governance | Platform, Security, Operations |

Outcomes: reduced business risk, improved ESG performance, increased revenue, increased operational efficiency.

## 7 Rs of migration

Retire · Retain · Rehost · Relocate · Repurchase · Replatform · Refactor

## Root user quick list

Only the root user can: change the root email, password, or access keys; close a standalone account; **restore IAM user permissions** (admin locked out); activate IAM access to Billing; sign up for GovCloud; register as a Reserved Instance Marketplace seller; configure S3 MFA Delete; fix a policy that denies all principals. Use MFA, no root access keys, and do not use root every day. See [Protecting the root user](04-security-and-compliance.md#protecting-the-root-user).

## Names you may see 

| You may see | Newer or related name |
|---|---|
| Developer / Business / Enterprise On-Ramp support | Business Support+ / Enterprise Support / Unified Operations |
| AWS Application Migration Service (MGN) | AWS Transform MGN (renamed June 2026) |
| Amazon QuickSight | Amazon Quick |
| Amazon SageMaker | Amazon SageMaker AI |
| Amazon CodeWhisperer | Amazon Q Developer |
| Amazon AppStream 2.0 | Amazon WorkSpaces Applications (in some newer AWS documents) |
| Kinesis Data Firehose | Amazon Data Firehose |
| CloudWatch Events | Amazon EventBridge |
| AWS Single Sign-On (SSO) | AWS IAM Identity Center |
| Amazon Elasticsearch Service | Amazon OpenSearch Service |
| Security Hub (original posture checks) | Security Hub CSPM (and a broader Security Hub) |
| Snowcone / Snowball Edge | Legacy; AWS recommends DataSync and Data Transfer Terminal |

## Final trap checklist

- **Explicit deny wins** in IAM policy evaluation.
- IAM **roles use temporary credentials**; avoid embedded long-term access keys.
- An SCP is a **guardrail**, not a permission grant.
- The customer always owns responsibility for **data and access configuration**.
- RDS **Multi-AZ = availability**; read replica = read scaling.
- S3 bucket names are global, but S3 data is stored in the selected Region.
- **Instance store is temporary**; EBS persists.
- Auto Scaling adds/removes capacity; a load balancer distributes traffic.
- SNS **pushes**; SQS **holds until pulled**; EventBridge **routes by rules**; Step Functions **coordinates steps**.
- VPC peering is **not transitive**; use Transit Gateway for many networks.
- Serverless means no server management, not no servers and not always zero cost.
- A budget alert does not automatically shut down every resource.
- Compliance varies by **geography, industry, and service**; AWS secures the cloud, you secure your workload.
- Answer every exam question: there is no penalty for guessing.

## 📖 Official AWS references for final review

- [CLF-C02 exam guide](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02.html) and [in-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-in-scope-services.html)
- [Overview of Amazon Web Services (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html)
- Decision guides: [Compute](https://docs.aws.amazon.com/decision-guides/latest/compute-on-aws-how-to-choose/choosing-aws-compute-service.html) · [Security](https://docs.aws.amazon.com/decision-guides/latest/security-on-aws-how-to-choose/choosing-aws-security-services.html) · [Analytics](https://docs.aws.amazon.com/decision-guides/latest/analytics-on-aws-how-to-choose/analytics-on-aws-how-to-choose.html) · [Application integration](https://docs.aws.amazon.com/decision-guides/latest/application-integration-on-aws-how-to-choose/application-integration-on-aws-how-to-choose.html) · [Machine learning](https://docs.aws.amazon.com/decision-guides/latest/machine-learning-on-aws-how-to-choose/guide.html)
- [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/definitions.html) · [Cloud Adoption Framework](https://aws.amazon.com/cloud-adoption-framework/)
- [AWS Pricing](https://aws.amazon.com/pricing/) · [Compare support plans](https://aws.amazon.com/premiumsupport/plans/) · [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)


[← Management and analytics](05-management-and-analytics.md) | [Index](README.md)