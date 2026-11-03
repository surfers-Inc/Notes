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
- AWS and customers share areas such as patch management, configuration management, and security awareness and training, but each patches/configures its own part.

⚠️ **Exam trap:** AWS does not secure customer data, IAM policies, security groups, or EC2 guest operating systems for the customer.

📖 **AWS docs:** [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

## Security, governance, and compliance concepts

### Compliance and governance

- **Compliance** means meeting the laws, standards, and industry rules that apply to you (for example PCI DSS for card payments, HIPAA for health data, GDPR for EU personal data, SOC and ISO reports).
- Requirements **vary by geography and industry**, and they also **vary by AWS service**: a service may be covered by one compliance program but not another. Check the *Services in Scope* list for your program.
- AWS is responsible for compliance **of** the cloud infrastructure. You remain responsible for making **your** workload and data compliant.
- **Governance** means setting rules and checking they are followed: policies (IAM, SCPs), monitoring (CloudWatch), auditing (CloudTrail, Config), and reports.

📖 **AWS docs:** [AWS Compliance Programs](https://aws.amazon.com/compliance/programs/) · [Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)

### Encryption options

| Type | Protects | Typical AWS tools |
|---|---|---|
| **Encryption in transit** | Data moving across a network (HTTPS/TLS, VPN) | ACM certificates, TLS endpoints, Site-to-Site VPN |
| **Encryption at rest** | Data stored on disk or in a service | KMS keys with S3, EBS, RDS, DynamoDB encryption |

A benefit of cloud security: strong controls such as encryption, monitoring, and automation are built in and easy to turn on.

### Where to find security logs

| What you want to see | Where to look |
|---|---|
| Who called which AWS API | CloudTrail (Event history shows recent management events; trails deliver to S3 and CloudWatch Logs) |
| Application, OS, and service logs; metrics and alarms | CloudWatch Logs / CloudWatch |
| Network traffic to and from your VPC | VPC Flow Logs |
| Resource configuration history and compliance | AWS Config |
| Detected threats | GuardDuty findings |
| Findings from many security services in one place | Security Hub |

📖 **AWS docs:** [CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) · [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)

## AWS Identity and Access Management (IAM)

📌 **What is it?** Controls who can access AWS and what they may do. Think of identities as staff badges and policies as the doors each badge can open.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Users** represent identities, **groups** collect users, **roles** provide temporary credentials, and **policies** define permissions.
- Apply **least privilege** (grant only what is needed), enable MFA, prefer roles over long-term access keys, and protect the root user.
- IAM is global and has no additional charge.

⚠️ **Exam Trap / Common Distractor:** Groups contain users, not other groups; roles are assumed and provide temporary credentials.

📖 **AWS docs:** [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/) · [IAM overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/security-services.html#aws-identity-and-access-management)

### Policy logic

- An implicit deny applies by default.
- An explicit allow grants access unless another applicable policy explicitly denies it.
- An **explicit deny overrides an allow**.
- Identity-based policies attach to identities; resource-based policies attach to resources.

### Policy types

| Type | Meaning |
|---|---|
| **AWS managed policy** | Created and maintained by AWS (for example read-only access); reusable |
| **Customer managed policy** | A custom policy you write and attach to many identities; the best way to apply least privilege |
| **Inline policy** | Embedded in a single user, group, or role; one-to-one |

### Credentials and authentication

- **Password policy:** set rules for IAM user passwords such as length, complexity, and expiry.
- **Access keys** (access key ID and secret) give programmatic access (CLI, SDK, API). Rotate them, never share them, and never hard-code them in code. Prefer roles with temporary credentials.
- **Store credentials safely** in **AWS Secrets Manager** or **Systems Manager Parameter Store**, not in code or files.
- **MFA** adds a second factor on top of a password (for example an authenticator app or a hardware/security key).
- **Federated identity:** users sign in with an existing identity (corporate directory, social login) and receive temporary access, instead of having their own IAM user. Federation uses SAML 2.0 or OpenID Connect, or **IAM Identity Center**.
- **Cross-account IAM role:** lets a user or service in one account assume a role in another account without sharing long-term credentials.

### Access reports

- **Credential report:** an account-wide list of IAM users and the status of their passwords, access keys, and MFA.
- **Last accessed information:** shows when permissions were last used, so you can remove unused permissions and tighten least privilege.

📖 **AWS docs:** [Temporary credentials (STS)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) · [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

### Protecting the root user

- Use a strong, unique password and **MFA** (AWS now requires MFA for root users; you add it at sign-up or when prompted).
- **Do not create root access keys**, and delete any that exist.
- Do not use root for everyday work; use an administrative user (ideally in IAM Identity Center) instead. Monitor root sign-in activity.

**Tasks only the root user can perform** (AWS current list):

- Change the root user's email address, password, or access keys (standalone account).
- Close a standalone AWS account.
- **Restore IAM user permissions** when the only IAM administrator locks themselves out.
- Activate IAM access to the Billing and Cost Management console, and view certain tax invoices.
- Sign up for AWS GovCloud (US).
- Register as a seller in the EC2 Reserved Instance Marketplace.
- Configure an S3 bucket for MFA Delete.
- Edit or delete an S3 bucket policy (or SQS resource policy) that denies all principals.

⚠️ **Exam Trap / Common Distractor:** Older study material also lists "change the support plan" as root-only. AWS now also allows an IAM user with the right permissions to do this. Many other settings (account name, contact details) no longer need root either.

📖 **AWS docs:** [AWS account root user and tasks that require it](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) · [Root user best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)

## AWS IAM Identity Center

📌 **What is it?** Central single sign-on (SSO) for your workforce across multiple AWS accounts and business applications. Think of one front door and one badge for every account. It was formerly called AWS Single Sign-On.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Workforce SSO** across accounts in AWS Organizations; users sign in once at a user portal.
- Connects to a corporate directory or uses its own; supports SAML 2.0 business apps (for example Salesforce, Microsoft 365).
- AWS recommends it for daily administrator access instead of using the root user.

⚠️ **Exam Trap / Common Distractor:** Identity Center is for your employees and AWS access. Cognito is for the customers of your application.

📖 **AWS docs:** [IAM Identity Center](https://aws.amazon.com/iam/identity-center/) · [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html)

## AWS Organizations

📌 **What is it?** Centrally manages and governs multiple AWS accounts. Think of a head office grouping branches under common billing and guardrails.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Organizational units (OUs)** group accounts; **Service Control Policies (SCPs)** set maximum available permissions.
- Supports consolidated billing, central governance, and account creation.
- Organizations itself has no additional charge; member accounts still pay for resources.

⚠️ **Exam Trap / Common Distractor:** An SCP does not grant permissions. IAM policies must still allow the action, and an SCP can restrict it.

📖 **AWS docs:** [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html)

## AWS Resource Access Manager (AWS RAM)

📌 **What is it?** Securely shares AWS resources with other accounts, your organization, or OUs, so you create a resource once instead of duplicating it. Think of lending one tool to several teams.

🎯 **Need to Know for the Exam (Key Keywords)**

- Share resources such as subnets, transit gateways, License Manager configurations, and Route 53 Resolver rules.
- Works within AWS Organizations or with specific accounts.
- No additional charge for RAM itself.

⚠️ **Exam Trap / Common Distractor:** RAM shares resources. It does not grant IAM permissions across accounts (roles do that).

📖 **AWS docs:** [What is AWS RAM?](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html) · [Shareable resources](https://docs.aws.amazon.com/ram/latest/userguide/shareable.html)

## Amazon Cognito

📌 **What is it?** Adds sign-up, sign-in, and access control for web and mobile application users. Think of a managed customer login desk.

🎯 **Need to Know for the Exam (Key Keywords)**

- **User pools** provide user directories and authentication; **identity pools** provide temporary AWS credentials.
- Supports social and enterprise identity providers and MFA.
- Pricing is primarily based on monthly active users and enabled features.

⚠️ **Exam Trap / Common Distractor:** Cognito is for application end users; IAM manages access for AWS workforce identities and workloads.

📖 **AWS docs:** [What is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)

## AWS Directory Service

📌 **What is it?** Managed Microsoft Active Directory (AWS Managed Microsoft AD) in the AWS Cloud. Think of a company user directory that AWS runs for you.

🎯 **Need to Know for the Exam (Key Keywords)**

- Built on real Microsoft Active Directory; supports Group Policy and SSO.
- Lets EC2, RDS for SQL Server, and WorkSpaces join a domain and use existing AD users and groups.
- Can connect to IAM Identity Center; pay per directory.

⚠️ **Exam Trap / Common Distractor:** Directory Service is for Active Directory workloads. Cognito handles app customers and IAM Identity Center handles AWS workforce sign-in.

📖 **AWS docs:** [AWS Directory Service](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/security-services.html#aws-directory-service) · [Admin guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html)

## AWS Key Management Service (AWS KMS)

📌 **What is it?** Managed service for creating and controlling encryption keys. Think of a guarded key cabinet integrated with AWS services.

🎯 **Need to Know for the Exam (Key Keywords)**

- Creates and controls KMS keys; supports key policies, rotation options, and auditing through CloudTrail.
- Integrates with services for encryption at rest and supports envelope encryption.
- AWS owned keys are free; customer managed keys and API requests can incur charges.

⚠️ **Exam Trap / Common Distractor:** KMS manages encryption keys; Secrets Manager stores and rotates secret values such as passwords.

📖 **AWS docs:** [What is AWS KMS?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)

## AWS CloudHSM

📌 **What is it?** A cloud-based **hardware security module (HSM)** for generating and using your own encryption keys on dedicated hardware. Think of renting a private, tamper-resistant safe just for your keys.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Dedicated** FIPS 140-2 Level 3 validated HSMs; you manage your own keys.
- Used when rules or contracts require single-tenant hardware key storage.
- AWS handles provisioning, patching, and backups; pay per HSM per hour.

⚠️ **Exam Trap / Common Distractor:** KMS is the simpler managed key service. Choose CloudHSM when the question says dedicated hardware or you must control the HSM.

📖 **AWS docs:** [AWS CloudHSM](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/security-services.html#aws-cloudhsm) · [User guide](https://docs.aws.amazon.com/cloudhsm/latest/userguide/introduction.html)

## AWS Secrets Manager

📌 **What is it?** Securely stores, retrieves, and can rotate credentials and other secrets. Think of a password vault applications can open with permission.

🎯 **Need to Know for the Exam (Key Keywords)**

- Stores database credentials, API keys, and tokens encrypted with KMS.
- Supports automatic rotation for supported secrets (for example RDS, Redshift, DocumentDB) and fine-grained access.
- Pay per secret stored and API calls.

⚠️ **Exam Trap / Common Distractor:** Do not hard-code credentials. KMS protects keys; Secrets Manager manages secret values and rotation.

📖 **AWS docs:** [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)

## AWS Certificate Manager (ACM)

📌 **What is it?** Provisions and manages public and private SSL/TLS certificates for supported AWS services. Think of an automated issuer and renewer for website identity cards.

🎯 **Need to Know for the Exam (Key Keywords)**

- Integrates with services such as Elastic Load Balancing, CloudFront, and API Gateway.
- Public certificates used with integrated AWS services can be provisioned and renewed automatically.
- Public certificates for integrated services have no additional certificate charge; private CA features cost money.

⚠️ **Exam Trap / Common Distractor:** ACM manages certificates (encryption in transit); KMS manages encryption keys used to protect data at rest.

📖 **AWS docs:** [What is AWS Certificate Manager?](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)

## Amazon Macie

📌 **What is it?** Discovers and protects sensitive data in Amazon S3 using machine learning and pattern matching. Think of a scanner that spots personal data lying around in your buckets.

🎯 **Need to Know for the Exam (Key Keywords)**

- Finds **sensitive data** such as personally identifiable information (PII), financial data, and health information in **S3**.
- Also reports on bucket security: unencrypted, publicly accessible, or shared outside your organization.
- Findings can go to Security Hub and EventBridge; pay for bucket evaluation and data inspected.

⚠️ **Exam Trap / Common Distractor:** Macie looks at data in S3. GuardDuty detects threats, and Inspector finds software vulnerabilities.

📖 **AWS docs:** [What is Amazon Macie?](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)

## AWS Shield

📌 **What is it?** Managed protection against distributed denial-of-service (DDoS) attacks. Think of crowd control that keeps malicious traffic floods away.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Shield Standard** provides automatic baseline DDoS protection at no extra cost.
- **Shield Advanced** adds enhanced protection, visibility, specialist response (the AWS DDoS Response Team), and cost protection for eligible resources.
- Standard is included; Advanced has a subscription and usage-related conditions.

⚠️ **Exam Trap / Common Distractor:** Shield protects against DDoS; WAF filters malicious web requests such as SQL injection.

📖 **AWS docs:** [AWS Shield](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/security-services.html#aws-shield) · [Shield overview](https://docs.aws.amazon.com/waf/latest/developerguide/ddos-overview.html)

## AWS WAF

📌 **What is it?** Web application firewall that filters HTTP(S) requests using rules. Think of a website bouncer checking each request.

🎯 **Need to Know for the Exam (Key Keywords)**

- Protects resources such as CloudFront, Application Load Balancer, and API Gateway.
- Rules can block or allow IPs, patterns, bots, **SQL injection**, and cross-site scripting.
- Pay for web ACLs, rules, and processed requests; managed rule groups may cost extra.

⚠️ **Exam Trap / Common Distractor:** WAF works at the web application layer; security groups filter network traffic and Shield addresses DDoS.

📖 **AWS docs:** [What is AWS WAF?](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)

## AWS Firewall Manager

📌 **What is it?** Centrally configures and manages firewall rules across all accounts and applications in AWS Organizations. Think of one security rulebook enforced everywhere.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Central administration** of firewall rules (for example AWS WAF rules) across accounts.
- New accounts and resources automatically fall under the common policies.
- Requires AWS Organizations; pay per policy per Region.

⚠️ **Exam Trap / Common Distractor:** Firewall Manager does not filter traffic itself. It deploys and enforces rules that WAF, Shield Advanced, and security groups apply.

📖 **AWS docs:** [AWS Firewall Manager](https://docs.aws.amazon.com/waf/latest/developerguide/fms-chapter.html)

## Amazon GuardDuty

📌 **What is it?** Intelligent threat-detection service that continuously analyses AWS data sources for suspicious activity. Think of a security analyst watching behaviour for danger signs.

🎯 **Need to Know for the Exam (Key Keywords)**

- Detects threats using sources such as CloudTrail events, VPC Flow Logs, and DNS logs.
- Produces findings for compromised credentials, unusual API calls, malware-related behaviour, and other threats.
- Usage-based pricing; a 30-day free trial is available for new accounts.

⚠️ **Exam Trap / Common Distractor:** GuardDuty detects threats; it does not replace WAF, patch systems, or automatically fix every finding.

📖 **AWS docs:** [What is Amazon GuardDuty?](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)

## Amazon Inspector

📌 **What is it?** Automated vulnerability management for supported compute workloads. Think of a building inspector looking for known structural weaknesses.

🎯 **Need to Know for the Exam (Key Keywords)**

- Continuously scans supported EC2 instances, ECR container images, and Lambda functions for vulnerabilities/exposure.
- Prioritises findings using vulnerability and reachability context.
- Usage-based charges depend on scanned resources.

⚠️ **Exam Trap / Common Distractor:** Inspector finds software vulnerabilities; GuardDuty looks for suspicious or malicious activity.

📖 **AWS docs:** [What is Amazon Inspector?](https://docs.aws.amazon.com/inspector/latest/user/what-is-inspector.html)

## Amazon Detective

📌 **What is it?** Helps you investigate and find the **root cause** of security findings or suspicious activity. Think of a detective piecing together the story behind an alert.

🎯 **Need to Know for the Exam (Key Keywords)**

- Automatically collects logs (VPC Flow Logs, CloudTrail, GuardDuty) and builds a linked, interactive view of resources, users, and their interactions over time.
- Used **after** a finding appears, to investigate it.
- Pay based on data ingested; a 30-day free trial is available for new accounts.

⚠️ **Exam Trap / Common Distractor:** GuardDuty and Macie raise findings. Detective investigates them to find the cause.

📖 **AWS docs:** [What is Amazon Detective?](https://docs.aws.amazon.com/detective/latest/userguide/what-is-detective.html)

## AWS Security Hub

📌 **What is it?** A central place to see, prioritise, and respond to security findings and your security posture across accounts. Think of a single security dashboard.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Aggregates findings** from services such as GuardDuty, Inspector, Macie, and partner products into one view.
- Runs automated **security best-practice checks** against standards (for example AWS Foundational Security Best Practices, CIS, PCI DSS).
- Works across multiple accounts with AWS Organizations; supports automated responses.

⚠️ **Exam Trap / Common Distractor:** Security Hub collects and prioritises findings; it does not detect threats itself (GuardDuty does). AWS renamed the original service **Security Hub CSPM** (posture checks) and made **Security Hub** a broader unified service, so you may see both names.

📖 **AWS docs:** [Security Hub and Security Hub CSPM](https://docs.aws.amazon.com/securityhub/latest/userguide/what-are-securityhub-services.html)

## AWS Artifact

📌 **What is it?** Self-service portal for AWS compliance reports and selected agreements. Think of a document library for auditors.

🎯 **Need to Know for the Exam (Key Keywords)**

- Download reports such as SOC and PCI documents and review agreements such as the Business Associate Addendum (BAA).
- Helps customers support their own compliance evidence and audits.
- Artifact access has no charge.

⚠️ **Exam Trap / Common Distractor:** Artifact provides documents; it does not scan resources or automatically make a workload compliant.

📖 **AWS docs:** [What is AWS Artifact?](https://docs.aws.amazon.com/artifact/latest/ug/what-is-aws-artifact.html)

## Security information and resources

| Resource | What it gives you |
|---|---|
| **AWS Knowledge Center** | Official answers to common technical and security questions |
| **AWS Security Center / Cloud Security page** | Overview of AWS security services, compliance, and best practices |
| **AWS Security Blog** | News, how-tos, and announcements about AWS security |
| **AWS Trusted Advisor** | Checks your account for security issues such as open ports or exposed resources |
| **AWS Marketplace** | Third-party security products you can buy and deploy on AWS |

📖 **AWS docs:** [Knowledge Center](https://repost.aws/knowledge-center) · [AWS Cloud Security](https://aws.amazon.com/security/) · [AWS Security Blog](https://aws.amazon.com/blogs/security/) · [AWS Marketplace](https://aws.amazon.com/marketplace) · [Choosing AWS security services](https://docs.aws.amazon.com/decision-guides/latest/security-on-aws-how-to-choose/choosing-aws-security-services.html)

## Security service chooser

| Requirement | Service |
|---|---|
| Control AWS permissions | IAM |
| Workforce single sign-on across accounts | IAM Identity Center |
| Govern many AWS accounts | Organizations / SCPs |
| Share resources across accounts | RAM |
| Authenticate application customers | Cognito |
| Managed Microsoft Active Directory | Directory Service |
| Manage encryption keys | KMS |
| Dedicated hardware for encryption keys | CloudHSM |
| Store and rotate passwords/API keys | Secrets Manager |
| Manage TLS certificates | ACM |
| DDoS protection | Shield |
| Filter malicious HTTP requests | WAF |
| Manage firewall rules across accounts | Firewall Manager |
| Detect suspicious behaviour | GuardDuty |
| Find compute vulnerabilities | Inspector |
| Find sensitive data in S3 | Macie |
| Investigate the root cause of a finding | Detective |
| Central view of security findings and posture | Security Hub |
| Download compliance reports | Artifact |


[← Storage and databases](03-storage-and-databases.md) | [Index](README.md) | [Management and analytics →](05-management-and-analytics.md)