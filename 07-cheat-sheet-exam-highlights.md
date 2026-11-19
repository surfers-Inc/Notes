# 07 — Cheat Sheet and Exam Highlights

[← Analytics, integration, ML, and more](06-analytics-integration-ml-and-more.md) | [Index](README.md)

Use this file for the **final review** in the last days before the exam. Every row links to the full service card (AWS definition, simple definition, keywords, what they test, traps, and docs).

## Contents

[Trigger words A–Z](#trigger-words-az) · [One-line service map](#one-line-service-map) · [Most-tested comparisons](#most-tested-comparisons) · [Pricing triggers](#pricing-triggers) · [Support plans](#support-plans-at-a-glance) · [Frameworks and lists to memorise](#frameworks-and-lists-to-memorise) · [Root user quick list](#root-user-quick-list) · [Names you may see](#names-you-may-see-old-and-new) · [Final trap checklist](#final-trap-checklist) · [Official references](#official-aws-references-for-final-review)

---

## Trigger words A–Z

If you see the word on the left in a question, the answer is very likely on the right.

| Trigger word | Service |
|---|---|
| Active Directory | Directory Service |
| Alarm on a metric | CloudWatch |
| API calls audit, "who did it" | CloudTrail |
| Approved product catalogue | Service Catalog |
| Archive, retrieval in hours | S3 Glacier Flexible Retrieval / Deep Archive |
| Archive, retrieval in milliseconds | S3 Glacier Instant Retrieval |
| Automatic failover for a database | RDS Multi-AZ |
| Batch jobs | AWS Batch |
| Best-practice checks across the account | Trusted Advisor |
| Big data, Hadoop, Spark | EMR |
| Business case for migration | Migration Evaluator |
| Business intelligence, dashboards | Amazon Quick |
| Cache (database) | ElastiCache (DynamoDB → DAX) |
| CDN, edge caching | CloudFront |
| Certificates (SSL/TLS) | ACM |
| Chatbot | Lex |
| CI/CD pipeline | CodePipeline |
| Compile and test code | CodeBuild |
| Compliance reports (SOC, PCI, ISO) | Artifact |
| Configuration history / compliance rules | Config |
| Contact centre | Amazon Connect |
| Container images | ECR |
| Containers without servers | Fargate |
| Data warehouse, OLAP | Redshift |
| DDoS | Shield |
| Decouple, queue | SQS |
| Dedicated private network link | Direct Connect |
| Dependencies of on-premises servers | Application Discovery Service |
| Desktop in the cloud | WorkSpaces |
| Disaster recovery for servers, continuous replication | Elastic Disaster Recovery |
| DNS, domain registration, Health Checks | Route 53 |
| Document (MongoDB) database | DocumentDB |
| Email at scale | SES |
| Encryption keys | KMS (dedicated hardware → CloudHSM) |
| Estimate cost before building | Pricing Calculator |
| ETL, data catalogue | Glue |
| Event routing by rules, schedules | EventBridge |
| Extract text/forms from documents | Textract |
| Firewall rules across accounts | Firewall Manager |
| Generative AI assistant | Amazon Q |
| Graph, relationships | Neptune |
| Hub for many VPCs | Transit Gateway |
| Image/video analysis, faces | Rekognition |
| Infrastructure as code | CloudFormation |
| Investigate root cause | Detective |
| IoT devices | IoT Core |
| Kubernetes | EKS |
| Landing zone, multi-account guardrails | Control Tower |
| Lift and shift servers | Application Migration Service |
| Licences tracking | License Manager |
| Linux shared file system, NFS | EFS |
| Load balancing HTTP / path routing | Application Load Balancer |
| Low-cost simple VPS / WordPress | Lightsail |
| Managed relational database | RDS (high performance → Aurora) |
| Migrate a database | DMS (convert schema → SCT) |
| Multiple accounts, consolidated billing, SCPs | Organizations |
| NoSQL key-value, serverless | DynamoDB |
| Object storage | S3 |
| On-premises AWS racks | Outposts |
| On-premises access to cloud storage | Storage Gateway |
| One sign-in for employees | IAM Identity Center |
| Patch servers, run commands, no SSH | Systems Manager |
| PII in S3 | Macie |
| Private access to a service without the internet | PrivateLink / VPC endpoint |
| Publish/subscribe, push notifications | SNS |
| Quota increase | Service Quotas |
| Real-time streaming data | Kinesis |
| Report abuse | AWS Trust and Safety |
| Rightsizing | Compute Optimizer |
| Rotate database passwords | Secrets Manager |
| Search, log analytics | OpenSearch Service |
| Security findings in one place | Security Hub |
| Sentiment analysis | Comprehend |
| Share resources across accounts | RAM |
| Sign-in for app customers usually mentions web and mobile logins| Cognito |
| Speech to text | Transcribe |
| SQL injection, XSS, OWASP top 10 | WAF |
| SQL queries on S3 | Athena |
| Static IPs, global TCP/UDP acceleration | Global Accelerator |
| Stream an application | AppStream 2.0 |
| Text to speech | Polly |
| Threat detection | GuardDuty |
| Trace requests across microservices | X-Ray |
| Translate languages | Translate |
| Unknown access pattern (storage) | S3 Intelligent-Tiering |
| Upload code, AWS builds the environment | Elastic Beanstalk |
| Virtual server | EC2 |
| Vulnerability scanning | Inspector |
| Web and mobile app toolkit | Amplify |
| Windows file share, SMB | FSx for Windows File Server |
| Workflow of steps | Step Functions |

---

## One-line service map

### Compute and containers

| Requirement | Best answer |
|---|---|
| Virtual server with OS control | [EC2](02-compute-and-networking.md#amazon-ec2) |
| Pick the cheapest EC2 pricing option | [EC2 purchase options](02-compute-and-networking.md#ec2-purchase-options) |
| Add/remove capacity automatically | [Auto Scaling](02-compute-and-networking.md#amazon-ec2-auto-scaling-and-aws-auto-scaling) |
| Spread traffic across healthy targets | [Elastic Load Balancing](02-compute-and-networking.md#elastic-load-balancing-elb) |
| Event-driven serverless code | [Lambda](02-compute-and-networking.md#aws-lambda) |
| Upload code, platform built for you | [Elastic Beanstalk](02-compute-and-networking.md#aws-elastic-beanstalk) |
| Simple bundled server | [Lightsail](02-compute-and-networking.md#amazon-lightsail) |
| Batch jobs at scale | [Batch](02-compute-and-networking.md#aws-batch) |
| AWS in your own data centre | [Outposts](02-compute-and-networking.md#aws-outposts) |
| AWS-native containers / Kubernetes / serverless containers / images | [ECS](02-compute-and-networking.md#amazon-ecs) · [EKS](02-compute-and-networking.md#amazon-eks) · [Fargate](02-compute-and-networking.md#aws-fargate) · [ECR](02-compute-and-networking.md#amazon-ecr) |

### Networking and content delivery

| Requirement | Best answer |
|---|---|
| Private network in AWS | [VPC](02-compute-and-networking.md#amazon-vpc) |
| Instance vs subnet firewall | [Security groups vs NACLs](02-compute-and-networking.md#security-groups-vs-network-acls) |
| Link two VPCs | [VPC peering](02-compute-and-networking.md#vpc-peering) |
| Hub for many VPCs and on-premises | [Transit Gateway](02-compute-and-networking.md#aws-transit-gateway) |
| Private access to a service | [PrivateLink / VPC endpoints](02-compute-and-networking.md#aws-privatelink-and-vpc-endpoints) |
| Encrypted tunnel (office or user) | [Site-to-Site VPN / Client VPN](02-compute-and-networking.md#aws-vpn-site-to-site-vpn-and-client-vpn) |
| Dedicated private connection | [Direct Connect](02-compute-and-networking.md#aws-direct-connect) |
| DNS and routing policies | [Route 53](02-compute-and-networking.md#amazon-route-53) |
| CDN | [CloudFront](02-compute-and-networking.md#amazon-cloudfront) |
| Static IPs, faster global path | [Global Accelerator](02-compute-and-networking.md#aws-global-accelerator) |
| Publish APIs | [API Gateway](02-compute-and-networking.md#amazon-api-gateway) |

### Storage

| Requirement | Best answer |
|---|---|
| Object storage | [S3](03-storage-and-databases.md#amazon-s3) |
| Choose a storage class / archive | [S3 storage classes and Glacier](03-storage-and-databases.md#s3-storage-classes-and-s3-glacier) |
| EC2 block disk (persistent) | [EBS](03-storage-and-databases.md#amazon-ebs) |
| Temporary disk on the host | [Instance store](03-storage-and-databases.md#ec2-instance-store) |
| Shared Linux files | [EFS](03-storage-and-databases.md#amazon-efs) |
| Windows/NetApp/OpenZFS/Lustre files | [FSx](03-storage-and-databases.md#amazon-fsx) |
| Hybrid storage with local cache | [Storage Gateway](03-storage-and-databases.md#aws-storage-gateway) |
| Central backups | [AWS Backup](03-storage-and-databases.md#aws-backup) |
| Fast failover of servers | [Elastic Disaster Recovery](03-storage-and-databases.md#aws-elastic-disaster-recovery) |
| Move lots of data into AWS | [Moving large data sets](03-storage-and-databases.md#moving-large-data-sets-into-aws) |

### Databases

| Requirement | Best answer |
|---|---|
| Managed relational | [RDS](03-storage-and-databases.md#amazon-rds) |
| AWS-built MySQL/PostgreSQL-compatible | [Aurora](03-storage-and-databases.md#amazon-aurora) |
| Serverless NoSQL | [DynamoDB](03-storage-and-databases.md#amazon-dynamodb) |
| MongoDB-compatible | [DocumentDB](03-storage-and-databases.md#amazon-documentdb) |
| Graph | [Neptune](03-storage-and-databases.md#amazon-neptune) |
| In-memory cache | [ElastiCache](03-storage-and-databases.md#amazon-elasticache) |
| Data warehouse | [Redshift](03-storage-and-databases.md#amazon-redshift) |
| Migrate data / convert schema | [DMS](03-storage-and-databases.md#aws-database-migration-service-aws-dms) · [SCT](03-storage-and-databases.md#aws-schema-conversion-tool-aws-sct) |

### Security, identity, and compliance

| Requirement | Best answer |
|---|---|
| Who does what in AWS | [Shared Responsibility Model](04-security-and-compliance.md#shared-responsibility-model) |
| AWS permissions | [IAM](04-security-and-compliance.md#aws-identity-and-access-management-iam) |
| Workforce SSO | [IAM Identity Center](04-security-and-compliance.md#aws-iam-identity-center) |
| Many accounts, SCPs, one bill | [Organizations](04-security-and-compliance.md#aws-organizations) |
| Share resources across accounts | [RAM](04-security-and-compliance.md#aws-resource-access-manager-aws-ram) |
| App customer sign-in | [Cognito](04-security-and-compliance.md#amazon-cognito) |
| Microsoft AD | [Directory Service](04-security-and-compliance.md#aws-directory-service) |
| Keys / dedicated HSM / secrets / certificates | [KMS](04-security-and-compliance.md#aws-key-management-service-aws-kms) · [CloudHSM](04-security-and-compliance.md#aws-cloudhsm) · [Secrets Manager](04-security-and-compliance.md#aws-secrets-manager) · [ACM](04-security-and-compliance.md#aws-certificate-manager-acm) |
| DDoS / web attacks / rules across accounts | [Shield](04-security-and-compliance.md#aws-shield) · [WAF](04-security-and-compliance.md#aws-waf) · [Firewall Manager](04-security-and-compliance.md#aws-firewall-manager) |
| Detect / scan / PII / investigate / collect | [GuardDuty](04-security-and-compliance.md#amazon-guardduty) · [Inspector](04-security-and-compliance.md#amazon-inspector) · [Macie](04-security-and-compliance.md#amazon-macie) · [Detective](04-security-and-compliance.md#amazon-detective) · [Security Hub](04-security-and-compliance.md#aws-security-hub) |
| Compliance reports | [Artifact](04-security-and-compliance.md#aws-artifact) |

### Management, governance, and cost

| Requirement | Best answer |
|---|---|
| Metrics, alarms, logs / API audit / config history | [CloudWatch](05-management-and-governance.md#amazon-cloudwatch) · [CloudTrail](05-management-and-governance.md#aws-cloudtrail) · [Config](05-management-and-governance.md#aws-config) |
| Account recommendations | [Trusted Advisor](05-management-and-governance.md#aws-trusted-advisor) |
| AWS events affecting me | [Health Dashboard](05-management-and-governance.md#aws-health-dashboard) |
| Rightsizing | [Compute Optimizer](05-management-and-governance.md#aws-compute-optimizer) |
| Console, CLI, SDK | [Ways to access AWS](05-management-and-governance.md#ways-to-access-aws-console-cli-sdks) |
| Infrastructure as code | [CloudFormation](05-management-and-governance.md#aws-cloudformation) |
| Operate fleets | [Systems Manager](05-management-and-governance.md#aws-systems-manager) |
| Landing zone / approved catalogue / licences / quotas | [Control Tower](05-management-and-governance.md#aws-control-tower) · [Service Catalog](05-management-and-governance.md#aws-service-catalog) · [License Manager](05-management-and-governance.md#aws-license-manager) · [Service Quotas](05-management-and-governance.md#service-quotas) |
| Cost tools | [Pricing Calculator, Cost Explorer, Budgets, CUR, tags](05-management-and-governance.md#cost-tools-at-a-glance) |

### Analytics, integration, ML, and other categories

| Requirement | Best answer |
|---|---|
| Analytics services | [Athena, Glue, EMR, Kinesis, OpenSearch, Quick](06-analytics-integration-ml-and-more.md#analytics-chooser) |
| Messaging and events | [SNS, SQS, EventBridge, Step Functions](06-analytics-integration-ml-and-more.md#integration-chooser) |
| AI/ML tasks | [AI/ML chooser](06-analytics-integration-ml-and-more.md#aiml-chooser) |
| Build / release / trace | [CodeBuild](06-analytics-integration-ml-and-more.md#aws-codebuild) · [CodePipeline](06-analytics-integration-ml-and-more.md#aws-codepipeline) · [X-Ray](06-analytics-integration-ml-and-more.md#aws-x-ray) |
| Contact centre / email | [Connect](06-analytics-integration-ml-and-more.md#amazon-connect) · [SES](06-analytics-integration-ml-and-more.md#amazon-ses) |
| Desktop / app / browser in the cloud | [WorkSpaces](06-analytics-integration-ml-and-more.md#amazon-workspaces) · [AppStream 2.0](06-analytics-integration-ml-and-more.md#amazon-appstream-20) · [Secure Browser](06-analytics-integration-ml-and-more.md#amazon-workspaces-secure-browser) |
| Web/mobile toolkit / IoT | [Amplify](06-analytics-integration-ml-and-more.md#aws-amplify) · [IoT Core](06-analytics-integration-ml-and-more.md#aws-iot-core) |

### Cloud concepts, migration, support

| Requirement | Best answer |
|---|---|
| Regions, AZs, edge locations | [Global infrastructure](01-cloud-concepts.md#aws-global-infrastructure) |
| Six pillars / review tool / CAF | [Well-Architected](01-cloud-concepts.md#aws-well-architected-framework) · [W-A Tool](01-cloud-concepts.md#aws-well-architected-tool) · [CAF](01-cloud-concepts.md#aws-cloud-adoption-framework-aws-caf) |
| 7 Rs / migration tools | [7 Rs](01-cloud-concepts.md#migration-strategies-the-7-rs) · [Migration tools](01-cloud-concepts.md#aws-migration-tools) |
| Support plans / where to get help / Marketplace | [Support plans](01-cloud-concepts.md#aws-support-plans) · [Resources](01-cloud-concepts.md#technical-resources-and-where-to-get-help) · [Marketplace and Partners](01-cloud-concepts.md#aws-marketplace-and-the-aws-partner-network) |

---

## Most-tested comparisons

| Pair | Remember |
|---|---|
| RDS **Multi-AZ** vs **read replica** | Availability/failover (sync, standby) vs read scaling (async, readable, can be cross-Region) |
| S3 **Standard-IA** vs **One Zone-IA** | ≥3 AZs vs 1 AZ (only for re-creatable data) |
| **Glacier Instant** vs **Flexible** vs **Deep Archive** | Milliseconds vs minutes–hours vs up to 12–48 hours (cheapest) |
| **EBS** vs **EFS** vs **S3** | Block, one AZ, one instance vs shared Linux files across AZs vs objects over HTTP |
| **EBS** vs **instance store** | Persistent vs lost on stop |
| **EFS** vs **FSx for Windows** | Linux/NFS vs Windows/SMB/AD |
| **AWS Backup** vs **Elastic Disaster Recovery** | Periodic backups vs continuous replication + fast failover |
| **DMS** vs **SCT** | Moves data vs converts schema |
| **DynamoDB** vs **RDS** | NoSQL key-value at scale vs relational SQL with joins |
| **Redshift** vs **Athena** | Permanent warehouse vs serverless SQL on S3 |
| **ElastiCache** vs **DAX** | Cache for any DB vs cache for DynamoDB only |
| **On-Demand** vs **Reserved/Savings Plans** vs **Spot** | Flexible vs committed steady use vs interruptible cheapest |
| **Standard RI** vs **Convertible RI** | Biggest discount, can't change family, can sell on RI Marketplace vs smaller discount, can exchange, can't sell |
| **Regional RI** vs **Zonal RI** | Discount in any AZ + size flexibility vs discount **plus capacity reservation** in one AZ |
| **Dedicated Host** vs **Dedicated Instance** | Socket/core visibility for licensing vs just single-tenant hardware |
| **Auto Scaling** vs **ELB** | Changes capacity vs distributes traffic |
| **ALB** vs **NLB** vs **GWLB** | HTTP/path routing vs extreme performance/static IP vs third-party appliances |
| **Lambda** vs **Batch** vs **Fargate** | Short event functions (≤15 min) vs queued batch jobs vs serverless containers |
| **ECS** vs **EKS** | AWS-native vs Kubernetes |
| **Beanstalk** vs **CloudFormation** | Deploy an app vs build any infrastructure from code |
| **Security group** vs **NACL** | Stateful, allow only, instance vs stateless, allow + deny, subnet |
| **IGW** vs **NAT gateway** | Two-way public internet vs outbound only for private subnets |
| **VPC peering** vs **Transit Gateway** vs **PrivateLink** | Two VPCs, not transitive vs hub for many vs one service, one way |
| **Site-to-Site VPN** vs **Direct Connect** | Encrypted, internet, minutes vs private, consistent, weeks |
| **Site-to-Site VPN** vs **Client VPN** | Networks vs individual users |
| **CloudFront** vs **Global Accelerator** | Caches HTTP content vs static IPs, no caching, TCP/UDP |
| **Route 53 latency** vs **geolocation** | Fastest Region vs user's country |
| **IAM** vs **Identity Center** vs **Cognito** | AWS permissions vs employee SSO vs app customers |
| **SCP** vs **IAM policy** | Ceiling (never grants) vs grant/deny |
| **KMS** vs **CloudHSM** | Shared managed keys vs dedicated hardware you control |
| **KMS** vs **Secrets Manager** vs **ACM** | Keys vs passwords + rotation vs TLS certificates |
| **Shield** vs **WAF** | DDoS vs SQL injection/XSS |
| **WAF** vs **Firewall Manager** | Applies rules vs manages rules across accounts |
| **GuardDuty / Inspector / Macie / Detective / Security Hub** | Detects threats / scans vulnerabilities / finds PII in S3 / investigates / collects |
| **CloudWatch** vs **CloudTrail** vs **Config** | Performance vs who did it vs configuration + compliance |
| **Trusted Advisor** vs **Well-Architected Tool** vs **Compute Optimizer** | Account checks vs one-workload review vs rightsizing |
| **Health Dashboard** vs **CloudWatch** | AWS's events vs your metrics |
| **Organizations** vs **Control Tower** | Accounts + SCPs vs automated landing zone |
| **Service Catalog** vs **Marketplace** | Internal approved products vs buy third-party software |
| **Pricing Calculator** vs **Cost Explorer** vs **Budgets** vs **CUR** | Estimate before vs analyse past vs alert vs most detailed data |
| **SNS** vs **SQS** vs **EventBridge** vs **Step Functions** | Push to many vs queue pulled vs route by rules vs ordered workflow |
| **Kinesis** vs **SQS** | Real-time streams vs message queue |
| **Glue** vs **EMR** | ETL + catalogue vs Hadoop/Spark clusters |
| **Polly** vs **Transcribe** vs **Translate** | Text→speech vs speech→text vs language→language |
| **Comprehend** vs **Textract** vs **Rekognition** | Understand text vs extract text from documents vs images/video |
| **SageMaker AI** vs pre-trained AI services | Build your own model vs ready-made API |
| **WorkSpaces** vs **AppStream 2.0** vs **Secure Browser** | Desktop vs one app vs browser |
| **Well-Architected** vs **CAF** | Workload design vs organisation's adoption |
| **Application Migration Service** vs **Elastic Disaster Recovery** | Migrate for good vs recover after a disaster |

---

## Pricing triggers

| Phrase | Answer |
|---|---|
| Unpredictable, short-term, cannot be interrupted | On-Demand |
| Steady usage, flexible across instance types/Lambda/Fargate | Savings Plans |
| Steady, predictable EC2 for 1–3 years | Reserved Instances |
| Fault-tolerant, interruptible, cheapest | Spot Instances |
| Per-socket/core licences (BYOL), compliance | Dedicated Hosts |
| Guarantee capacity in an AZ | On-Demand Capacity Reservation |
| Estimate a future design | Pricing Calculator |
| Analyse past spend, forecast | Cost Explorer |
| Alert before forecast spend passes $X | Budgets |
| Most detailed line-item billing | Cost and Usage Reports |
| One bill, volume discounts, shared RIs | Consolidated billing (Organizations) |
| Costs by team/project | Cost allocation tags |

Data transfer: **in = free**, **out to the internet = charged**, **between Regions = charged**, between AZs usually charged. Public IPv4 addresses are charged per hour.

## Support plans at a glance

| Plan | Remember |
|---|---|
| **Basic** (free) | Customer service, docs, re:Post, core Trusted Advisor, Health Dashboard; **no technical cases** |
| **Business Support+** | Production minimum; 24/7 technical support; **full Trusted Advisor**; Health API; critical-system-down **< 30 min** |
| **Enterprise Support** | Business-critical; **TAM**; Well-Architected and security reviews; **< 15 min** |
| **Unified Operations** | Mission-critical; specialist engineers, 24/7 monitoring; **< 5 min** |

## Frameworks and lists to memorise

**Six advantages of cloud:** variable expense · economies of scale · stop guessing capacity · speed and agility · stop running data centres · go global in minutes.

**Six Well-Architected pillars = CROPSS:** **C**ost Optimisation · **R**eliability · **O**perational Excellence · **P**erformance Efficiency · **S**ecurity · **S**ustainability. (Six healthy *crops*; Security protects the system, Sustainability protects the surroundings.)

**Six CAF perspectives = GBP + OPS:** **GBP** (business side, think money) = **G**overnance · **B**usiness · **P**eople; **OPS** (technical side) = **O**perations · **P**latform · **S**ecurity. People = business P, Platform = tech P. Outcomes: reduced risk, improved ESG, more revenue, more efficiency.

**7 Rs:** Retire · Retain · Rehost · Relocate · Repurchase · Replatform · Refactor.

**Four Region factors:** compliance/data residency · latency to users · service availability · price.

**Shared Responsibility:** AWS = security **of** the cloud; customer = security **in** the cloud; **data is always yours**.

## Root user quick list

Only the root user can: change root email/password/access keys; **close the account**; **restore IAM permissions** when admins are locked out; activate IAM access to Billing; sign up for GovCloud; register as an RI Marketplace seller; configure **S3 MFA Delete**; fix a bucket policy that denies everyone. Protect it with **MFA**, create **no root access keys**, and do not use it daily. Details: [Protecting the root user](04-security-and-compliance.md#protecting-the-root-user).

## Names you may see (old and new)

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
| AWS DDoS Response Team (DRT) | Shield Response Team (SRT) |
| Snowcone / Snowball Edge | Legacy; AWS recommends DataSync and Data Transfer Terminal |

## Final trap checklist

- **Explicit deny wins** in IAM.
- EC2 needs AWS access → **IAM role**, never stored access keys.
- An SCP is a **guardrail**, not a grant.
- **Data** is always the customer's responsibility.
- RDS **Multi-AZ = availability**; **read replica = reads**.
- One Zone-IA only for **re-creatable** data.
- Bucket **names** are global; bucket **data** stays in its Region.
- **Instance store is temporary**; EBS persists; EBS is **one AZ**.
- **Spot** only for interruptible work.
- Auto Scaling adds capacity; ELB distributes traffic.
- SNS **pushes**; SQS **waits to be pulled**; EventBridge **routes**; Step Functions **orders steps**.
- VPC peering is **not transitive**.
- Direct Connect is **not encrypted by default** and is **not quick** to set up.
- CloudFront **caches**; Global Accelerator **does not**.
- CloudTrail keeps **90 days** by default; create a trail for longer.
- Trusted Advisor's **full** checks need **Business Support+**.
- A budget alert does not stop spending unless you configure **budget actions**.
- Serverless means no server **management**, not no servers.
- Answer every question: **no penalty for guessing**.

## Official AWS references for final review

- [CLF-C02 exam guide](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02.html) · [In-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-in-scope-services.html) · [Out-of-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-out-of-scope-services.html)
- [Overview of Amazon Web Services (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html)
- Decision guides: [Compute](https://docs.aws.amazon.com/decision-guides/latest/compute-on-aws-how-to-choose/choosing-aws-compute-service.html) · [Storage](https://docs.aws.amazon.com/decision-guides/latest/storage-on-aws-how-to-choose/choosing-aws-storage-service.html) · [Databases](https://docs.aws.amazon.com/decision-guides/latest/databases-on-aws-how-to-choose/databases-on-aws-how-to-choose.html) · [Security](https://docs.aws.amazon.com/decision-guides/latest/security-on-aws-how-to-choose/choosing-aws-security-services.html) · [Analytics](https://docs.aws.amazon.com/decision-guides/latest/analytics-on-aws-how-to-choose/analytics-on-aws-how-to-choose.html) · [Machine learning](https://docs.aws.amazon.com/decision-guides/latest/machine-learning-on-aws-how-to-choose/guide.html)
- [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/pillars.html) · [Cloud Adoption Framework](https://docs.aws.amazon.com/whitepapers/latest/overview-aws-cloud-adoption-framework/welcome.html)
- [AWS Pricing](https://aws.amazon.com/pricing/) · [Compare support plans](https://aws.amazon.com/premiumsupport/plans/) · [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

[← Analytics, integration, ML, and more](06-analytics-integration-ml-and-more.md) | [Index](README.md)
