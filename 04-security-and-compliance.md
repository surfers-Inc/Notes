# 04 — Security and Compliance

[← Storage and databases](03-storage-and-databases.md) | [Index](README.md) | [Management and governance →](05-management-and-governance.md)

> **How to read each card:** 📘 how AWS describes it · 💡 how to remember it · 🔑 words that point to it in a question · 🎯 what the exam actually asks about it · ⚠️ the wrong answer you will be tempted by · 📖 where to read more.
>
> Security and Compliance is the **heaviest-weighted domain after Technology (30%)**. Most questions are "which service does X?" or "is this AWS's job or mine?"

## Contents

- **Foundations:** [Shared Responsibility Model](#shared-responsibility-model) · [Compliance and governance](#compliance-and-governance) · [Encryption](#encryption-at-rest-and-in-transit) · [Where to find logs](#where-to-find-security-logs)
- **Identity and accounts:** [IAM](#aws-identity-and-access-management-iam) · [Root user](#protecting-the-root-user) · [IAM Identity Center](#aws-iam-identity-center) · [Organizations](#aws-organizations) · [RAM](#aws-resource-access-manager-aws-ram) · [Cognito](#amazon-cognito) · [Directory Service](#aws-directory-service)
- **Keys, secrets, and certificates:** [KMS](#aws-key-management-service-aws-kms) · [CloudHSM](#aws-cloudhsm) · [Secrets Manager](#aws-secrets-manager) · [ACM](#aws-certificate-manager-acm)
- **Network and application protection:** [Shield](#aws-shield) · [WAF](#aws-waf) · [Firewall Manager](#aws-firewall-manager)
- **Detection and investigation:** [The detection family](#the-detection-family-at-a-glance) · [GuardDuty](#amazon-guardduty) · [Inspector](#amazon-inspector) · [Macie](#amazon-macie) · [Detective](#amazon-detective) · [Security Hub](#aws-security-hub)
- **Compliance evidence:** [Artifact](#aws-artifact) · [Security resources](#security-information-and-resources)
- [Security service chooser](#security-service-chooser) · [Top traps on this page](#top-traps-on-this-page)

---

## Shared Responsibility Model

**📘 AWS definition:** Security and compliance is a shared responsibility between AWS and the customer: AWS is responsible for security **of** the cloud, and the customer is responsible for security **in** the cloud.

**💡 Simple definition:** **AWS is the landlord, you are the tenant.** The landlord secures the building, walls, and wiring; you lock your own door and decide who gets a key. **AWS = OF the cloud. You = IN the cloud.**

**🔑 Exam keywords:** `security of the cloud` · `security in the cloud` · `customer responsibility` · `AWS responsibility` · `shared controls`

**🎯 What they test**

| AWS: security **OF** the cloud | Customer: security **IN** the cloud |
|---|---|
| Physical data centres, guards, hardware | **Customer data** (classification, encryption choices) |
| Global infrastructure: Regions, AZs, edge locations | **IAM**: users, roles, policies, MFA |
| Host OS and virtualisation (hypervisor) | **Guest OS** patching and applications on EC2 |
| Network infrastructure | Security groups, NACLs, firewall configuration |
| Managed-service software (for example the RDS engine patching, Lambda runtime) | Client- and server-side encryption settings, network traffic protection |

**How it shifts by service** (the more managed the service, the less you do):

| Task | EC2 (IaaS) | RDS (managed) | Lambda / S3 / DynamoDB (serverless) |
|---|---|---|---|
| Physical hardware, host OS | AWS | AWS | AWS |
| Guest OS patching | **You** | AWS | AWS |
| Database engine patching | **You** (if self-installed) | AWS | — |
| Application code | **You** | — | **You** (your function code) |
| Network access rules (security groups) | **You** | **You** | **You** (IAM/resource policies) |
| Data, IAM permissions, encryption choices | **You** | **You** | **You** |

**Shared controls** (both parties, each on its own layer): **patch management**, **configuration management**, and **awareness and training**.

**⚠️ Exam traps**

- **Data is always the customer's responsibility**, no matter which service.
- AWS never patches the **guest OS on EC2** for you (unless you choose a tool such as Systems Manager to automate it; it is still your job).
- Physical security of an **Outposts** rack in your data centre is **your** responsibility.

📖 **Read more:** [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) · [Security pillar: shared responsibility](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/shared-responsibility.html)

## Compliance and governance

- **Compliance** = meeting laws and standards that apply to you (PCI DSS for card payments, HIPAA for health data, GDPR for EU personal data, SOC and ISO reports).
- Requirements vary by **country, industry, and AWS service**. Not every service is covered by every program; check **Services in Scope**.
- AWS is compliant for the **infrastructure**; **you** must make your workload compliant. Evidence of AWS's compliance is downloaded from **AWS Artifact**.
- **Governance** = setting rules and checking they are followed: policies (IAM, SCPs), monitoring (CloudWatch), auditing (CloudTrail, Config), and reports (Audit Manager, Artifact).

📖 **Read more:** [AWS compliance programs](https://aws.amazon.com/compliance/programs/) · [Services in scope by compliance program](https://aws.amazon.com/compliance/services-in-scope/)

## Encryption at rest and in transit

| Type | Protects | Typical AWS tools |
|---|---|---|
| **In transit** | Data moving across a network | HTTPS/TLS with **ACM** certificates, Site-to-Site/Client VPN |
| **At rest** | Data stored on disk or in a service | **KMS** keys with S3, EBS, RDS, DynamoDB; **CloudHSM** for dedicated hardware |

Many services now encrypt at rest **by default** (S3, DynamoDB, Storage Gateway); for others (EBS, RDS) you turn it on. Choosing and managing encryption remains a customer responsibility.

## Where to find security logs

| What you want to see | Where to look |
|---|---|
| **Who** called which AWS API, and when | **CloudTrail** |
| Metrics, alarms, application/OS logs | **CloudWatch** / CloudWatch Logs |
| Network traffic to and from the VPC | **VPC Flow Logs** |
| How a resource's **configuration** changed, and whether it is compliant | **AWS Config** |
| Detected threats | **GuardDuty** findings |
| All security findings in one place | **Security Hub** |

📖 **Read more:** [What is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) · [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)

---

## AWS Identity and Access Management (IAM)

**📘 AWS definition:** AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources: who is authenticated (signed in) and authorised (has permissions) to use them.

**💡 Simple definition:** **Staff badges and door permissions.** Identities are the badges; **policies** list the doors each badge can open. **IAM = "I Am who I say, and I can do only this."**

**🔑 Exam keywords:** `users` · `groups` · `roles` · `policies` · `least privilege` · `MFA` · `access keys` · `temporary credentials` · `global service`

**🎯 What they test**

1. **The four building blocks**

   | Item | What it is | Memory hook |
   |---|---|---|
   | **User** | A person or app with **long-term** credentials (password and/or access keys) | A named employee badge |
   | **Group** | A collection of users that share policies (groups **cannot** contain groups) | A department |
   | **Role** | An identity that is **assumed** and gives **temporary** credentials; used by AWS services (for example EC2 → S3), other accounts, or federated users | A visitor pass that expires |
   | **Policy** | A JSON document listing allowed/denied actions on resources | The rulebook |

2. **Policy logic:** everything is **denied by default** (implicit deny) → an **explicit allow** grants access → an **explicit deny always wins**.
3. **Policy types:** **AWS managed** (written by AWS, e.g. ReadOnlyAccess), **customer managed** (your reusable policies, best for least privilege), **inline** (embedded in one identity). **Identity-based** policies attach to users/groups/roles; **resource-based** policies attach to resources (for example an S3 bucket policy).
4. **Best practices:** **least privilege**; **MFA** everywhere; prefer **roles and temporary credentials** over long-term access keys; never share or hard-code access keys (store secrets in **Secrets Manager**); use **IAM Identity Center** for people; rotate credentials; use a **password policy**.
5. **Access types:** **console** (password + MFA) and **programmatic** (access key ID + secret access key for CLI/SDK/API).
6. **Federation:** users sign in with an existing identity (corporate directory via SAML 2.0, or OpenID Connect) and receive temporary credentials from **AWS STS**.
7. **Audit tools:** **IAM credential report** (all users and the status of their passwords, keys, and MFA) and **last accessed information** / **IAM Access Analyzer** (find unused permissions and resources shared outside your account).
8. IAM is **global** (not tied to a Region) and **free**.

**⚠️ Exam traps**

- "An EC2 instance needs to read from S3" → attach an **IAM role** to the instance; never store access keys on it.
- Groups hold **users only**, not other groups or roles.
- Explicit **deny** beats allow, every time.
- IAM is for **your AWS identities**. Customers signing in to **your app** → **Cognito**.

📖 **Read more:** [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) · [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) · [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) · [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) · [Temporary credentials (STS)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)

### Protecting the root user

The **root user** is the email address that created the account; it has **complete, unrestricted** access.

- Use a strong password and **MFA** (AWS now requires MFA for root users).
- **Do not create root access keys**; delete any that exist.
- Do not use root for daily work; create an administrative user (ideally in IAM Identity Center) and lock root away.

**Tasks only the root user can do** (common exam list):

- Change the account's root email address, root password, or root access keys.
- **Close** the AWS account.
- **Restore IAM user permissions** if the only IAM administrator locks themselves out.
- Activate IAM access to the **Billing and Cost Management** console; view certain tax invoices.
- Sign up for **AWS GovCloud (US)**.
- Register as a seller in the **EC2 Reserved Instance Marketplace**.
- Configure an S3 bucket for **MFA Delete**.
- Edit or delete an S3 bucket policy (or SQS policy) that **denies all principals**.

⚠️ **Exam trap:** older material says "change the support plan" is root-only. AWS now also allows an IAM user with the right permissions to do it.

📖 **Read more:** [Tasks that require root user credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) · [Root user best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)

---

## AWS IAM Identity Center

**📘 AWS definition:** AWS IAM Identity Center is the recommended AWS service for managing workforce access to AWS applications and multiple AWS accounts.

**💡 Simple definition:** **One login for employees to every AWS account and business app.** (Formerly **AWS Single Sign-On**.)

**🔑 Exam keywords:** `single sign-on (SSO)` · `workforce` · `multiple accounts` · `user portal` · `corporate directory` · `permission sets`

**🎯 What they test**

- Employees sign in once at a **user portal** and reach every assigned account (in AWS Organizations) and SAML apps (for example Microsoft 365, Salesforce).
- Identities can live in Identity Center itself or come from an existing directory (Microsoft AD, Okta, Microsoft Entra ID).
- **Permission sets** define what users can do in each account.

**⚠️ Exam traps**

- Identity Center = **employees**. Cognito = **your app's customers**.

📖 **Read more:** [What is IAM Identity Center?](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)

---

## AWS Organizations

**📘 AWS definition:** AWS Organizations is an account management service that lets you consolidate multiple AWS accounts into an organisation that you create and centrally manage.

**💡 Simple definition:** **Head office for all your AWS accounts.** One bill, folders (OUs) for grouping accounts, and guardrails (SCPs) that limit what any account can do.

**🔑 Exam keywords:** `multiple accounts` · `consolidated billing` · `organisational units (OUs)` · `service control policies (SCPs)` · `management account` · `volume discounts`

**🎯 What they test**

- **Management account** (the payer) + **member accounts**, grouped into **organisational units (OUs)**.
- **Consolidated billing:** one bill for all accounts; usage is **combined** for **volume pricing discounts**, and Reserved Instance / Savings Plans discounts can be **shared** across accounts. No extra charge.
- **Service control policies (SCPs):** set the **maximum** permissions for accounts or OUs (for example "no one may leave the eu-west-1 Region" or "no one may disable CloudTrail"). They apply even to the member account's root user.
- Create accounts programmatically; apply backup policies and tag policies centrally.
- **AWS Control Tower** builds on Organizations to set up a governed multi-account "landing zone" automatically (see [Management and governance](05-management-and-governance.md#aws-control-tower)).

**⚠️ Exam traps**

- An SCP **never grants** permissions. It only **limits** them; IAM policies must still allow the action.
- SCPs do **not** affect the **management account**.
- "One bill for many accounts" → Organizations (consolidated billing), not Cost Explorer.

📖 **Read more:** [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) · [Service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) · [Consolidated billing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html)

---

## AWS Resource Access Manager (AWS RAM)

**📘 AWS definition:** AWS Resource Access Manager (AWS RAM) helps you securely share your resources across AWS accounts, within your organisation or organisational units, and with IAM roles and users.

**💡 Simple definition:** **Lend one tool to several teams** instead of buying each team its own.

**🔑 Exam keywords:** `share resources across accounts` · `avoid duplication` · `shared subnets` · `Transit Gateway sharing`

**🎯 What they test**

- Share resources such as **VPC subnets, Transit Gateways, License Manager configurations, Route 53 Resolver rules**.
- No extra charge.

**⚠️ Exam traps**

- RAM **shares resources**; cross-account **permissions** come from IAM roles.

📖 **Read more:** [What is AWS RAM?](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html)

---

## Amazon Cognito

**📘 AWS definition:** Amazon Cognito is an identity platform for web and mobile apps: a user directory, an authentication server, and an authorisation service for OAuth 2.0 access tokens and AWS credentials.

**💡 Simple definition:** **The sign-up and sign-in screen for your app's customers**, run by AWS. "**Cog**nito" → recognise (identify) your app users.

**🔑 Exam keywords:** `app users` · `sign-up / sign-in` · `mobile and web apps` · `social login (Google, Facebook, Apple)` · `user pools` · `identity pools` · `millions of users`

**🎯 What they test**

- **User pools:** user directory + sign-in (including social and SAML identity providers, MFA).
- **Identity pools:** give signed-in (or guest) users **temporary AWS credentials** to call AWS services directly.
- Priced mainly by **monthly active users**.

**⚠️ Exam traps**

- Customers of **your application** → Cognito. **Employees** → IAM Identity Center. **AWS workloads** → IAM roles.

📖 **Read more:** [What is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)

---

## AWS Directory Service

**📘 AWS definition:** AWS Directory Service provides multiple ways to use Microsoft Active Directory (AD) with other AWS services.

**💡 Simple definition:** **Microsoft Active Directory, run by AWS** (or connected to the one you already have).

**🔑 Exam keywords:** `Microsoft Active Directory` · `domain join` · `Group Policy` · `AD Connector` · `Windows workloads`

**🎯 What they test**

- **AWS Managed Microsoft AD:** a real Microsoft AD run by AWS. **AD Connector:** a proxy to your **on-premises** AD (no directory data stored in AWS).
- Lets EC2 Windows instances, RDS for SQL Server, WorkSpaces, and FSx for Windows use existing AD users and groups.

**⚠️ Exam traps**

- "Active Directory" in the question → Directory Service, not Cognito.

📖 **Read more:** [What is AWS Directory Service?](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html)

---

## AWS Key Management Service (AWS KMS)

**📘 AWS definition:** AWS Key Management Service (AWS KMS) is a managed service that makes it easy for you to create and control the cryptographic keys that protect your data.

**💡 Simple definition:** **A guarded key cabinet built into AWS.** S3, EBS, RDS, and many others fetch keys from it to encrypt your data. Every use of a key is logged in CloudTrail.

**🔑 Exam keywords:** `encryption keys` · `encryption at rest` · `create and control keys` · `key rotation` · `key policy` · `integrated with AWS services` · `audit key usage`

**🎯 What they test**

- **Key types by owner:**

  | Key | Who manages it | Cost |
  |---|---|---|
  | **AWS owned keys** | AWS, used invisibly across accounts | Free |
  | **AWS managed keys** (`aws/s3`, `aws/ebs`…) | AWS creates them in your account; rotation automatic | No monthly fee |
  | **Customer managed keys** | **You** control policy, rotation, disabling, deletion | Monthly fee per key + requests |

- Keys never leave KMS's FIPS-validated hardware security modules unencrypted; KMS is **multi-tenant** (shared HSMs run by AWS).
- Control with **key policies** and IAM; all usage audited in **CloudTrail**.

**⚠️ Exam traps**

- KMS manages **keys**. Secrets Manager stores **secret values** (passwords). ACM manages **certificates**.
- "Single-tenant", "dedicated HSM", "you control the hardware module" → **CloudHSM**, not KMS.

📖 **Read more:** [What is AWS KMS?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) · [AWS KMS concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)

---

## AWS CloudHSM

**📘 AWS definition:** AWS CloudHSM offers secure cryptographic key storage by making hardware security modules (HSMs) available in the AWS Cloud.

**💡 Simple definition:** **Renting your own private safe** for keys, not a locker in a shared room. **HSM = Hardware Security Module.**

**🔑 Exam keywords:** `dedicated HSM` · `single-tenant` · `FIPS Level 3` · `customer-controlled keys` · `regulatory requirement for dedicated hardware`

**🎯 What they test**

- **Single-tenant**, FIPS-validated (Level 3) HSMs in your VPC; **you** manage the keys and users. AWS manages provisioning, patching, and backups of the hardware.
- Pay per HSM per hour.

**⚠️ Exam traps**

- AWS **cannot** recover your keys if you lose your CloudHSM credentials.

📖 **Read more:** [What is AWS CloudHSM?](https://docs.aws.amazon.com/cloudhsm/latest/userguide/introduction.html)

---

## AWS Secrets Manager

**📘 AWS definition:** AWS Secrets Manager helps you manage, retrieve, and rotate database credentials, application credentials, API keys, and other secrets throughout their lifecycles.

**💡 Simple definition:** **A password vault for applications** that can also change (rotate) the passwords automatically.

**🔑 Exam keywords:** `store credentials` · `automatic rotation` · `database passwords` · `API keys` · `do not hard-code secrets`

**🎯 What they test**

- Stores secrets encrypted with KMS; apps fetch them at run time with IAM permission.
- **Automatic rotation** (built-in for RDS, Aurora, Redshift, DocumentDB; Lambda for others).
- Pay per secret per month plus API calls.

**⚠️ Exam traps**

- **Rotation** is the key differentiator. **Systems Manager Parameter Store** can also store secrets (cheaper/free tier) but does not rotate them automatically.

📖 **Read more:** [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)

---

## AWS Certificate Manager (ACM)

**📘 AWS definition:** AWS Certificate Manager (ACM) handles the complexity of creating, storing, and renewing public and private SSL/TLS X.509 certificates and keys that protect your AWS websites and applications.

**💡 Simple definition:** **Free, auto-renewing padlocks (HTTPS certificates)** for your websites on AWS.

**🔑 Exam keywords:** `SSL/TLS certificates` · `HTTPS` · `encryption in transit` · `automatic renewal`

**🎯 What they test**

- **Public certificates are free** when used with integrated services (ELB, CloudFront, API Gateway) and **renew automatically**.
- **AWS Private CA** issues private certificates (paid).

**⚠️ Exam traps**

- ACM = encryption **in transit** (certificates). KMS = encryption **at rest** (keys).

📖 **Read more:** [What is AWS Certificate Manager?](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)

---

## AWS Shield

**📘 AWS definition:** AWS Shield is a managed service that provides protection against distributed denial of service (DDoS) attacks for applications running on AWS.

**💡 Simple definition:** **Crowd control** that stops a flood of fake visitors from knocking your site over. Picture a **shield** taking the hits.

**🔑 Exam keywords:** `DDoS` · `distributed denial of service` · `SYN flood / UDP reflection` · `always-on protection` · `cost protection` · `Shield Response Team`

**🎯 What they test**

| | **Shield Standard** | **Shield Advanced** |
|---|---|---|
| Cost | **Free, automatic** for all customers | Paid subscription (monthly fee, 1-year commitment) |
| Protects against | Common network/transport-layer (layer 3/4) attacks | Larger and more sophisticated attacks, including application layer with WAF |
| Extras | — | 24/7 access to the **Shield Response Team (SRT)**, real-time attack visibility, **cost protection** against scaling charges caused by DDoS, WAF included for protected resources |
| Resources | Everything, especially CloudFront and Route 53 | EC2 Elastic IPs, ELB, CloudFront, Global Accelerator, Route 53 |

**⚠️ Exam traps**

- **DDoS** → Shield. **SQL injection / cross-site scripting** → WAF.
- "Protection from **charges** caused by a DDoS spike" → **Shield Advanced** (cost protection).

📖 **Read more:** [How AWS Shield works](https://docs.aws.amazon.com/waf/latest/developerguide/ddos-overview.html) · [Shield Advanced overview](https://docs.aws.amazon.com/waf/latest/developerguide/ddos-advanced-summary.html)

---

## AWS WAF

**📘 AWS definition:** AWS WAF is a web application firewall that lets you monitor the HTTP(S) requests forwarded to your protected web application resources and allow, block, or count them based on rules you set.

**💡 Simple definition:** **A bouncer at the website door** reading each request and turning away the troublemakers. **WAF = Web Application Firewall.**

**🔑 Exam keywords:** `SQL injection` · `cross-site scripting (XSS)` · `layer 7` · `block IP addresses` · `rate limiting` · `bot control` · `web ACL` · `geo-blocking`

**🎯 What they test**

- Protects **CloudFront, Application Load Balancer, API Gateway**, AppSync, Cognito user pools (not EC2 directly, not NLB).
- **Web ACLs** contain rules: IP match, geo match, **rate-based rules** (limit requests per IP), managed rule groups (OWASP top 10, bots).
- Pay per web ACL, per rule, and per million requests.

**⚠️ Exam traps**

- WAF works at **layer 7 (HTTP)**. Security groups/NACLs filter **network** traffic. Shield handles **DDoS** volume.

📖 **Read more:** [What is AWS WAF?](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)

---

## AWS Firewall Manager

**📘 AWS definition:** AWS Firewall Manager simplifies your administration and maintenance tasks across multiple accounts and resources for a variety of protections, including AWS WAF, AWS Shield Advanced, and Amazon VPC security groups.

**💡 Simple definition:** **One security rulebook pushed to every account automatically.**

**🔑 Exam keywords:** `centrally manage firewall rules` · `across accounts` · `AWS Organizations` · `new accounts automatically protected`

**🎯 What they test**

- Requires **AWS Organizations**; new accounts and resources automatically get the policies.
- Manages WAF rules, Shield Advanced protections, security groups, and DNS Firewall rules.

**⚠️ Exam traps**

- Firewall Manager **does not filter traffic itself**; it deploys rules that WAF/Shield/security groups enforce.

📖 **Read more:** [What is AWS Firewall Manager?](https://docs.aws.amazon.com/waf/latest/developerguide/fms-chapter.html)

---

## The detection family at a glance

These five are the most confused services in the security domain. Learn **one verb** for each.

| Service | Verb | Looks at | Memory hook |
|---|---|---|---|
| **GuardDuty** | **Detects threats** | CloudTrail, VPC Flow Logs, DNS logs, S3/EKS/RDS activity | A **guard** on **duty** watching behaviour |
| **Inspector** | **Scans for vulnerabilities** | EC2, ECR container images, Lambda | A building **inspector** looking for weak spots (CVEs, open ports) |
| **Macie** | **Finds sensitive data** | **S3** buckets | Finds **PII** (names, card numbers) |
| **Detective** | **Investigates root cause** | Logs + findings from GuardDuty etc. | A **detective** solving the case after the alarm |
| **Security Hub** | **Collects and prioritises** findings, checks best practices | Findings from all of the above + partners | The **hub** / security dashboard |

## Amazon GuardDuty

**📘 AWS definition:** Amazon GuardDuty is a threat detection service that continuously monitors, analyses, and processes AWS data sources and logs in your environment to identify potential security threats.

**💡 Simple definition:** **A security guard on duty 24/7**, using machine learning and threat intelligence to spot suspicious behaviour.

**🔑 Exam keywords:** `threat detection` · `malicious activity` · `compromised credentials` · `cryptocurrency mining` · `unusual API calls` · `machine learning` · `continuous monitoring`

**🎯 What they test**

- Analyses **CloudTrail events, VPC Flow Logs, DNS logs** (no agents needed), plus optional S3, EKS, RDS login, Lambda, and malware protection.
- One click to enable; 30-day free trial; findings can trigger automation through EventBridge.

**⚠️ Exam traps**

- GuardDuty **detects**; it does not block traffic, patch servers, or fix findings for you.

📖 **Read more:** [What is Amazon GuardDuty?](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)

## Amazon Inspector

**📘 AWS definition:** Amazon Inspector is a vulnerability management service that automatically discovers workloads and continually scans them for software vulnerabilities and unintended network exposure.

**💡 Simple definition:** **A building inspector** checking your servers and containers for known weak spots.

**🔑 Exam keywords:** `vulnerability scanning` · `CVEs` · `software vulnerabilities` · `network exposure` · `EC2, ECR, Lambda`

**🎯 What they test**

- Continuous, automatic scans of **EC2 instances, ECR container images, and Lambda functions**; risk score prioritises findings.

**⚠️ Exam traps**

- Inspector = **vulnerabilities in software**. GuardDuty = **suspicious activity**.

📖 **Read more:** [What is Amazon Inspector?](https://docs.aws.amazon.com/inspector/latest/user/what-is-inspector.html)

## Amazon Macie

**📘 AWS definition:** Amazon Macie is a data security service that discovers sensitive data by using machine learning and pattern matching, provides visibility into data security risks, and enables automated protection against those risks.

**💡 Simple definition:** **A sniffer dog for personal data in S3.**

**🔑 Exam keywords:** `sensitive data` · `PII` · `S3` · `data privacy` · `GDPR / HIPAA data discovery`

**🎯 What they test**

- Finds PII, financial, and health data in **S3**; also flags buckets that are public, unencrypted, or shared outside the organisation.

**⚠️ Exam traps**

- Macie only looks at **S3**. It does not scan databases or EC2.

📖 **Read more:** [What is Amazon Macie?](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)

## Amazon Detective

**📘 AWS definition:** Amazon Detective makes it easy to analyse, investigate, and quickly identify the root cause of security findings or suspicious activities.

**💡 Simple definition:** **The detective who arrives after the alarm** to work out what happened and how.

**🔑 Exam keywords:** `investigate` · `root cause` · `security findings` · `visualise relationships` · `after the alert`

**🎯 What they test**

- Automatically collects log data and builds linked graphs of users, resources, and activity over time to speed up investigations.

**⚠️ Exam traps**

- GuardDuty/Macie/Inspector **raise** findings; Detective **investigates** them.

📖 **Read more:** [What is Amazon Detective?](https://docs.aws.amazon.com/detective/latest/userguide/what-is-detective.html)

## AWS Security Hub

**📘 AWS definition:** AWS Security Hub provides a comprehensive view of your security state in AWS and helps you assess your environment against security industry standards and best practices.

**💡 Simple definition:** **The single security dashboard** that gathers every finding in one place and scores you against best-practice checklists.

**🔑 Exam keywords:** `central view` · `aggregate findings` · `security posture` · `compliance standards (CIS, PCI DSS, AWS Foundational Security Best Practices)` · `multi-account`

**🎯 What they test**

- Aggregates findings from GuardDuty, Inspector, Macie, Firewall Manager, and partners; runs automated **best-practice checks**.
- AWS now calls the posture-checking part **Security Hub CSPM**, and **Security Hub** a broader unified service; you may see both names.

**⚠️ Exam traps**

- Security Hub **collects**; it does not detect threats itself.

📖 **Read more:** [Security Hub and Security Hub CSPM](https://docs.aws.amazon.com/securityhub/latest/userguide/what-are-securityhub-services.html)

---

## AWS Artifact

**📘 AWS definition:** AWS Artifact provides on-demand downloads of AWS security and compliance documents, such as AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports.

**💡 Simple definition:** **The document library for auditors.** If an auditor asks "prove AWS is compliant", you download it from Artifact.

**🔑 Exam keywords:** `compliance reports` · `SOC reports` · `PCI` · `ISO certifications` · `agreements (BAA)` · `auditor`

**🎯 What they test**

- **Artifact Reports:** AWS's third-party audit reports. **Artifact Agreements:** review and accept agreements such as the HIPAA **Business Associate Addendum (BAA)**.
- Free.

**⚠️ Exam traps**

- Artifact gives **documents**. Checking **your** resources against rules → **AWS Config** or Security Hub. Collecting **your** audit evidence → Audit Manager.

📖 **Read more:** [What is AWS Artifact?](https://docs.aws.amazon.com/artifact/latest/ug/what-is-aws-artifact.html)

## Security information and resources

| Resource | What it gives you |
|---|---|
| **AWS Knowledge Center** (on re:Post) | Official answers to common technical and security questions |
| **AWS Security Center** (aws.amazon.com/security) | Overview of security services, compliance, and best practices |
| **AWS Security Blog** | News and how-tos about AWS security |
| **AWS Trusted Advisor** | Checks for security issues such as open ports, MFA on root, public S3 buckets |
| **AWS Marketplace** | Third-party security products to buy and deploy on AWS |

📖 **Read more:** [Knowledge Center](https://repost.aws/knowledge-center) · [AWS Cloud Security](https://aws.amazon.com/security/) · [AWS Security Blog](https://aws.amazon.com/blogs/security/) · [Choosing AWS security services](https://docs.aws.amazon.com/decision-guides/latest/security-on-aws-how-to-choose/choosing-aws-security-services.html)

---

## Security service chooser

| Requirement | Service |
|---|---|
| Control who can do what in AWS | IAM |
| Give an AWS service (e.g. EC2) permissions | IAM role |
| Employees: one sign-in for many accounts/apps | IAM Identity Center |
| Customers sign in to my web/mobile app | Cognito |
| Microsoft Active Directory | Directory Service |
| Govern many accounts, one bill, guardrails | Organizations + SCPs |
| Share a resource with another account | RAM |
| Manage encryption keys | KMS |
| Dedicated, single-tenant key hardware | CloudHSM |
| Store and **rotate** passwords/API keys | Secrets Manager |
| HTTPS certificates | ACM |
| DDoS protection | Shield (Standard free, Advanced paid) |
| Block SQL injection / XSS / bad IPs at the web layer | WAF |
| Push firewall rules to all accounts | Firewall Manager |
| Detect suspicious activity / threats | GuardDuty |
| Find software vulnerabilities | Inspector |
| Find PII in S3 | Macie |
| Investigate root cause of a finding | Detective |
| One dashboard of all findings | Security Hub |
| Download AWS compliance reports | Artifact |

## Top traps on this page

1. **Data** is always the **customer's** responsibility.
2. EC2 guest OS patching = **customer**; RDS engine patching = **AWS**.
3. Explicit **deny** always wins in IAM.
4. EC2 needs AWS access → **IAM role**, never stored access keys.
5. **SCPs limit** permissions; they **never grant** them.
6. Employees → **Identity Center**; app customers → **Cognito**.
7. KMS (keys) vs Secrets Manager (passwords + **rotation**) vs ACM (certificates).
8. KMS (shared, managed) vs **CloudHSM** (dedicated hardware).
9. **Shield** = DDoS; **WAF** = SQL injection/XSS.
10. GuardDuty **detects**, Inspector **scans**, Macie **finds PII**, Detective **investigates**, Security Hub **collects**.
11. Artifact = **AWS's** compliance reports, not a scanner.

[← Storage and databases](03-storage-and-databases.md) | [Index](README.md) | [Management and governance →](05-management-and-governance.md)
