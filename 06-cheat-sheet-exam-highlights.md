# 06 — Cheat Sheet and Exam Highlights

[← Management and analytics](05-management-and-analytics.md) | [Index](README.md)

Use this file for final review. Follow the links for each full service template.

## One-line service map

| Requirement / keyword | Best answer |
|---|---|
| Virtual server with OS control | [Amazon EC2](02-compute-and-networking.md#amazon-ec2) |
| Event-driven serverless code | [AWS Lambda](02-compute-and-networking.md#aws-lambda) |
| Easy application platform deployment | [Elastic Beanstalk](02-compute-and-networking.md#aws-elastic-beanstalk) |
| AWS-native container orchestration | [Amazon ECS](02-compute-and-networking.md#amazon-ecs) |
| Managed Kubernetes | [Amazon EKS](02-compute-and-networking.md#amazon-eks) |
| Serverless container compute | [AWS Fargate](02-compute-and-networking.md#aws-fargate) |
| Simple bundled VPS | [Amazon Lightsail](02-compute-and-networking.md#amazon-lightsail) |
| Object storage / static files | [Amazon S3](03-storage-and-databases.md#amazon-s3) |
| EC2 block disk | [Amazon EBS](03-storage-and-databases.md#amazon-ebs) |
| Shared Linux file system | [Amazon EFS](03-storage-and-databases.md#amazon-efs) |
| Central backup policy | [AWS Backup](03-storage-and-databases.md#aws-backup) |
| Offline physical data transfer | [AWS Snow Family](03-storage-and-databases.md#aws-snow-family-snowcone-and-snowball-edge) |
| Managed relational database | [Amazon RDS](03-storage-and-databases.md#amazon-rds) |
| AWS-built MySQL/PostgreSQL-compatible database | [Amazon Aurora](03-storage-and-databases.md#amazon-aurora) |
| Serverless NoSQL key-value/document database | [Amazon DynamoDB](03-storage-and-databases.md#amazon-dynamodb) |
| Data warehouse / OLAP | [Amazon Redshift](03-storage-and-databases.md#amazon-redshift) |
| In-memory cache | [Amazon ElastiCache](03-storage-and-databases.md#amazon-elasticache) |
| Database migration with low downtime | [AWS DMS](03-storage-and-databases.md#aws-database-migration-service-aws-dms) |
| Private AWS network | [Amazon VPC](02-compute-and-networking.md#amazon-vpc) |
| DNS, domain, health-based routing | [Amazon Route 53](02-compute-and-networking.md#amazon-route-53) |
| CDN / cached edge content | [Amazon CloudFront](02-compute-and-networking.md#amazon-cloudfront) |
| Dedicated private connection | [AWS Direct Connect](02-compute-and-networking.md#aws-direct-connect) |
| Static IPs and global network acceleration | [AWS Global Accelerator](02-compute-and-networking.md#aws-global-accelerator) |
| AWS permissions | [IAM](04-security-and-compliance.md#aws-identity-and-access-management-iam) |
| Application customer identities | [Amazon Cognito](04-security-and-compliance.md#amazon-cognito) |
| Multi-account governance | [AWS Organizations](04-security-and-compliance.md#aws-organizations) |
| Encryption keys | [AWS KMS](04-security-and-compliance.md#aws-key-management-service-aws-kms) |
| Store and rotate secret values | [AWS Secrets Manager](04-security-and-compliance.md#aws-secrets-manager) |
| DDoS protection | [AWS Shield](04-security-and-compliance.md#aws-shield) |
| Filter malicious web requests | [AWS WAF](04-security-and-compliance.md#aws-waf) |
| Threat detection | [Amazon GuardDuty](04-security-and-compliance.md#amazon-guardduty) |
| Vulnerability scanning | [Amazon Inspector](04-security-and-compliance.md#amazon-inspector) |
| Compliance reports | [AWS Artifact](04-security-and-compliance.md#aws-artifact) |
| Metrics, alarms, dashboards, logs | [Amazon CloudWatch](05-management-and-analytics.md#amazon-cloudwatch) |
| API and account activity audit | [AWS CloudTrail](05-management-and-analytics.md#aws-cloudtrail) |
| Resource configuration compliance | [AWS Config](05-management-and-analytics.md#aws-config) |
| Environment recommendations | [AWS Trusted Advisor](05-management-and-analytics.md#aws-trusted-advisor) |
| Analyse past cost and forecast | [AWS Cost Explorer](05-management-and-analytics.md#aws-cost-explorer) |
| Cost/usage threshold alert | [AWS Budgets](05-management-and-analytics.md#aws-budgets) |
| Query S3 data using SQL | [Amazon Athena](05-management-and-analytics.md#amazon-athena) |
| Infrastructure as code | [AWS CloudFormation](05-management-and-analytics.md#aws-cloudformation) |

## Most-tested comparisons

### Storage

| S3 | EBS | EFS |
|---|---|---|
| Object | Block | File |
| Access by API/HTTP | Attached disk | Shared NFS mount |
| Regional bucket | One-AZ volume | Regional or One Zone |
| Backups, media, static assets | Boot volume, database disk | Shared Linux files |

### Database

| RDS / Aurora | DynamoDB | Redshift | ElastiCache |
|---|---|---|---|
| Relational / SQL | NoSQL key-value/document | Analytics warehouse | In-memory cache |
| Transactions and joins | Massive scale, low latency | OLAP and BI | Accelerate reads/sessions |

### Security

| Pair | Difference |
|---|---|
| Shield vs. WAF | DDoS protection vs. HTTP request filtering |
| GuardDuty vs. Inspector | Suspicious behaviour vs. software vulnerabilities |
| KMS vs. Secrets Manager | Encryption keys vs. secret values and rotation |
| IAM vs. Cognito | AWS access vs. application customer identities |
| Security group vs. NACL | Stateful resource firewall vs. stateless subnet firewall |

### Monitoring and governance

| Pair | Difference |
|---|---|
| CloudWatch vs. CloudTrail | Performance/logs vs. API audit history |
| CloudTrail vs. Config | Who did what vs. how a resource was configured |
| Trusted Advisor vs. Well-Architected Tool | Environment recommendations vs. guided workload review |
| Cost Explorer vs. Budgets | Analyse/forecast vs. alert on thresholds |
| Organizations SCP vs. IAM policy | Permission ceiling vs. permission grant/deny |

### Networking

| Pair | Difference |
|---|---|
| CloudFront vs. Global Accelerator | Caches content vs. improves network path without caching |
| Direct Connect vs. Site-to-Site VPN | Dedicated private link vs. encrypted tunnel over internet |
| Internet gateway vs. NAT gateway | Public internet path vs. private-subnet outbound IPv4 |
| Route 53 vs. load balancer | DNS routing vs. distributing traffic to targets |

## Pricing triggers

| Phrase | Likely model/tool |
|---|---|
| Unpredictable, short-term EC2 | On-Demand |
| Steady compute commitment, flexible usage | Savings Plans |
| Predictable EC2 and possible capacity reservation | Reserved Instances |
| Fault-tolerant and interruptible | Spot Instances |
| Estimate a future architecture | AWS Pricing Calculator |
| Explore existing spending trends | Cost Explorer |
| Notify before forecast spend exceeds limit | AWS Budgets |
| Most detailed billing export | Cost and Usage Report |

## Global infrastructure triggers

- **Multiple AZs:** high availability inside one Region.
- **Multiple Regions:** disaster recovery, global latency, sovereignty, or business continuity.
- **Edge locations:** CloudFront caching and edge networking close to users.
- A Region contains multiple isolated AZs; a subnet belongs to exactly one AZ.

## Six Well-Architected pillars

**Operational Excellence · Security · Reliability · Performance Efficiency · Cost Optimization · Sustainability**

Memory aid: **Operate Secure, Reliable, Performing, Cost-aware, Sustainable systems.**

## Six AWS CAF perspectives

| Business capabilities | Technical capabilities |
|---|---|
| Business, People, Governance | Platform, Security, Operations |

## Final trap checklist

- **Explicit deny wins** in IAM policy evaluation.
- IAM **roles use temporary credentials**; avoid embedded long-term access keys.
- An SCP is a **guardrail**, not a permission grant.
- The customer always owns responsibility for **data and access configuration**.
- RDS **Multi-AZ = availability**; read replica = read scaling.
- S3 bucket names are global, but S3 data is stored in the selected Region.
- Auto Scaling adds/removes capacity; a load balancer distributes traffic.
- Serverless means no server management, not no servers and not always zero cost.
- A budget alert does not automatically shut down every resource.
- Answer every exam question: there is no penalty for guessing.



[← Management and analytics](05-management-and-analytics.md) | [Index](README.md)