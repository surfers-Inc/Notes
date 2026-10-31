# 04 — Security and Compliance

[← Storage and databases](03-storage-and-databases.md) | [Index](README.md) | [Management and analytics →](05-management-and-analytics.md)

## Shared Responsibility Model

| AWS: security **of** the cloud | Customer: security **in** the cloud |
|---|---|
| Physical facilities and hardware | Data classification and protection |
| AWS global infrastructure | IAM users, roles, policies, and MFA |
| Host operating system and virtualisation | Network and resource configuration |
| Managed-service infrastructure | Guest OS and applications on EC2 |

- Responsibility changes with the service. Customers do more for **EC2**, less for **RDS**, and still less infrastructure work for **Lambda**.
- AWS and customers share areas such as patch management, configuration management, and security awareness—but each patches/configures its own part.

⚠️ **Exam trap:** AWS does not secure customer data, IAM policies, security groups, or EC2 guest operating systems for the customer.

## AWS Identity and Access Management (IAM)

📌 **What is it?** Controls who can access AWS and what they may do. Think of identities as staff badges and policies as the doors each badge can open.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Users** represent identities, **groups** collect users, **roles** provide temporary credentials, and **policies** define permissions.
- Apply **least privilege**, enable MFA, prefer roles over long-term access keys, and protect the root user.
- IAM is global and has no additional charge.

⚠️ **Exam Trap / Common Distractor:** Groups contain users, not other groups; roles are assumed and provide temporary credentials.

### Policy logic

- An implicit deny applies by default.
- An explicit allow grants access unless another applicable policy explicitly denies it.
- An **explicit deny overrides an allow**.
- Identity-based policies attach to identities; resource-based policies attach to resources.

## AWS Organizations

📌 **What is it?** Centrally manages and governs multiple AWS accounts. Think of a head office grouping branches under common billing and guardrails.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Organizational units (OUs)** group accounts; **Service Control Policies (SCPs)** set maximum available permissions.
- Supports consolidated billing, central governance, and account creation.
- Organizations itself has no additional charge; member accounts still pay for resources.

⚠️ **Exam Trap / Common Distractor:** An SCP does not grant permissions. IAM policies must still allow the action, and an SCP can restrict it.

## Amazon Cognito

📌 **What is it?** Adds sign-up, sign-in, and access control for web and mobile application users. Think of a managed customer login desk.

🎯 **Need to Know for the Exam (Key Keywords)**

- **User pools** provide user directories and authentication; **identity pools** provide temporary AWS credentials.
- Supports social and enterprise identity providers and MFA.
- Pricing is primarily based on monthly active users and enabled features.

⚠️ **Exam Trap / Common Distractor:** Cognito is for application end users; IAM manages access for AWS workforce identities and workloads.

## AWS Key Management Service (AWS KMS)

📌 **What is it?** Managed service for creating and controlling encryption keys. Think of a guarded key cabinet integrated with AWS services.

🎯 **Need to Know for the Exam (Key Keywords)**

- Creates and controls KMS keys; supports key policies, rotation options, and auditing through CloudTrail.
- Integrates with services for encryption at rest and supports envelope encryption.
- AWS owned keys are free; customer managed keys and API requests can incur charges.

⚠️ **Exam Trap / Common Distractor:** KMS manages encryption keys; Secrets Manager stores and rotates secret values such as passwords.

## AWS Secrets Manager

📌 **What is it?** Securely stores, retrieves, and can rotate credentials and other secrets. Think of a password vault applications can open with permission.

🎯 **Need to Know for the Exam (Key Keywords)**

- Stores database credentials, API keys, and tokens encrypted with KMS.
- Supports automatic rotation for supported secrets and fine-grained access.
- Pay per secret stored and API calls.

⚠️ **Exam Trap / Common Distractor:** Do not hard-code credentials. KMS protects keys; Secrets Manager manages secret values and rotation.

## AWS Shield

📌 **What is it?** Managed protection against distributed denial-of-service (DDoS) attacks. Think of crowd control that keeps malicious traffic floods away.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Shield Standard** provides automatic baseline DDoS protection at no extra cost.
- **Shield Advanced** adds enhanced protection, visibility, specialist response, and cost protection for eligible resources.
- Standard is included; Advanced has a subscription and usage-related conditions.

⚠️ **Exam Trap / Common Distractor:** Shield protects against DDoS; WAF filters malicious web requests such as SQL injection.

## AWS WAF

📌 **What is it?** Web application firewall that filters HTTP(S) requests using rules. Think of a website bouncer checking each request.

🎯 **Need to Know for the Exam (Key Keywords)**

- Protects resources such as CloudFront, Application Load Balancer, and API Gateway.
- Rules can block or allow IPs, patterns, bots, **SQL injection**, and cross-site scripting.
- Pay for web ACLs, rules, and processed requests; managed rule groups may cost extra.

⚠️ **Exam Trap / Common Distractor:** WAF works at the web application layer; security groups filter network traffic and Shield addresses DDoS.

## Amazon GuardDuty

📌 **What is it?** Intelligent threat-detection service that continuously analyses AWS data sources for suspicious activity. Think of a security analyst watching behaviour for danger signs.

🎯 **Need to Know for the Exam (Key Keywords)**

- Detects threats using sources such as CloudTrail events, VPC Flow Logs, and DNS logs.
- Produces findings for compromised credentials, unusual access, malware-related behaviour, and other threats.
- Usage-based pricing; a trial is commonly available for evaluation.

⚠️ **Exam Trap / Common Distractor:** GuardDuty detects threats; it does not replace WAF, patch systems, or automatically fix every finding.

## Amazon Inspector

📌 **What is it?** Automated vulnerability management for supported compute workloads. Think of a building inspector looking for known structural weaknesses.

🎯 **Need to Know for the Exam (Key Keywords)**

- Continuously scans supported EC2 instances, ECR container images, and Lambda functions for vulnerabilities/exposure.
- Prioritises findings using vulnerability and reachability context.
- Usage-based charges depend on scanned resources.

⚠️ **Exam Trap / Common Distractor:** Inspector finds software vulnerabilities; GuardDuty looks for suspicious or malicious activity.

## AWS Artifact

📌 **What is it?** Self-service portal for AWS compliance reports and selected agreements. Think of a document library for auditors.

🎯 **Need to Know for the Exam (Key Keywords)**

- Download reports such as SOC and PCI documents and review compliance agreements.
- Helps customers support their own compliance evidence and audits.
- Artifact access has no charge.

⚠️ **Exam Trap / Common Distractor:** Artifact provides documents; it does not scan resources or automatically make a workload compliant.

## AWS Certificate Manager (ACM)

📌 **What is it?** Provisions and manages public and private SSL/TLS certificates for supported AWS services. Think of an automated issuer and renewer for website identity cards.

🎯 **Need to Know for the Exam (Key Keywords)**

- Integrates with services such as Elastic Load Balancing, CloudFront, and API Gateway.
- Public certificates used with integrated AWS services can be provisioned and renewed automatically.
- Public certificates for integrated services have no additional certificate charge; private CA features cost money.

⚠️ **Exam Trap / Common Distractor:** ACM manages certificates; KMS manages encryption keys used to protect data.

## Security service chooser

| Requirement | Service |
|---|---|
| Control AWS permissions | IAM |
| Govern many AWS accounts | Organizations / SCPs |
| Authenticate application customers | Cognito |
| Manage encryption keys | KMS |
| Store and rotate passwords/API keys | Secrets Manager |
| DDoS protection | Shield |
| Filter malicious HTTP requests | WAF |
| Detect suspicious behaviour | GuardDuty |
| Find compute vulnerabilities | Inspector |
| Download compliance reports | Artifact |
| Manage TLS certificates | ACM |


[← Storage and databases](03-storage-and-databases.md) | [Index](README.md) | [Management and analytics →](05-management-and-analytics.md)