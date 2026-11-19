# 05 — Management, Governance, and Cost Tools

[← Security and compliance](04-security-and-compliance.md) | [Index](README.md) | [Analytics, integration, ML, and more →](06-analytics-integration-ml-and-more.md)

> **How to read each card:** 📘 how AWS describes it · 💡 how to remember it · 🔑 words that point to it in a question · 🎯 what the exam actually asks about it · ⚠️ the wrong answer you will be tempted by · 📖 where to read more.

## Contents

- **Monitor and audit:** [The big three](#the-big-three-cloudwatch-vs-cloudtrail-vs-config) · [CloudWatch](#amazon-cloudwatch) · [CloudTrail](#aws-cloudtrail) · [Config](#aws-config)
- **Advice and health:** [Trusted Advisor](#aws-trusted-advisor) · [Health Dashboard](#aws-health-dashboard) · [Compute Optimizer](#aws-compute-optimizer)
- **Build and operate:** [Ways to access AWS](#ways-to-access-aws-console-cli-sdks) · [CloudFormation](#aws-cloudformation) · [Systems Manager](#aws-systems-manager)
- **Govern many accounts:** [Control Tower](#aws-control-tower) · [Service Catalog](#aws-service-catalog) · [License Manager](#aws-license-manager) · [Service Quotas](#service-quotas) · Organizations → [04](04-security-and-compliance.md#aws-organizations) · Well-Architected Tool → [01](01-cloud-concepts.md#aws-well-architected-tool)
- **Cloud financial management:** [Cost tools at a glance](#cost-tools-at-a-glance) · [Pricing Calculator](#aws-pricing-calculator) · [Cost Explorer](#aws-cost-explorer) · [Budgets](#aws-budgets) · [Cost and Usage Reports](#aws-cost-and-usage-reports-cur) · [Cost allocation tags](#cost-allocation-tags)
- [Quick chooser](#quick-chooser) · [Top traps on this page](#top-traps-on-this-page)

---

## The big three: CloudWatch vs CloudTrail vs Config

These three appear together in many questions. One sentence each:

| Service | Answers the question… | Memory hook |
|---|---|---|
| **CloudWatch** | "How is it **performing**?" (metrics, logs, alarms) | **Watch** the health monitor |
| **CloudTrail** | "**Who did what**, when, from where?" (API calls) | A **trail** of footprints |
| **Config** | "What did this resource **look like**, and is it **compliant**?" | A **config**uration history book |

Example: someone opened port 22 on a security group. **CloudTrail** shows **who** called the API. **Config** shows the security group **before and after**, and flags it as non-compliant. **CloudWatch** would show the CPU rising if an attacker started mining crypto.

📖 **Read more:** [Management and governance services overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/management-governance.html)

## Amazon CloudWatch

**📘 AWS definition:** Amazon CloudWatch monitors your AWS resources and the applications you run on AWS in real time; you can collect and track metrics, create alarms, and automatically react to changes.

**💡 Simple definition:** **The dashboard, logbook, and alarm system** for everything running in your account.

**🔑 Exam keywords:** `metrics` · `alarms` · `logs` · `dashboards` · `monitor performance` · `CPU utilisation` · `billing alarm` · `observability`

**🎯 What they test**

- **Metrics:** numbers over time (EC2 CPU, network). EC2 **basic monitoring** = every 5 minutes (free); **detailed monitoring** = every 1 minute (paid). Memory and disk-space metrics need the **CloudWatch agent**.
- **Alarms:** watch a metric and act: notify through **SNS**, trigger **Auto Scaling**, or stop/reboot/recover an EC2 instance.
- **Billing alarm:** alert when estimated charges pass a threshold. Billing metric data is stored in **US East (N. Virginia), us-east-1**.
- **CloudWatch Logs:** collect and search logs from apps, EC2, Lambda, and VPC Flow Logs; **Logs Insights** queries them.
- **Dashboards:** custom views of metrics across Regions.
- (CloudWatch **Events** became **Amazon EventBridge**.)

**⚠️ Exam traps**

- "**Who** deleted the bucket?" → **CloudTrail**, not CloudWatch.
- CloudWatch **observes**; Systems Manager **acts** on servers; Auto Scaling **adds** servers.

📖 **Read more:** [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) · [Using CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) · [Create a billing alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html) · [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)

## AWS CloudTrail

**📘 AWS definition:** AWS CloudTrail is an AWS service that helps you enable operational and risk auditing, governance, and compliance of your AWS account by recording actions taken by users, roles, or AWS services as events.

**💡 Simple definition:** **CCTV for your AWS account.** Every console click, CLI command, and API call leaves a footprint: who, what, when, from which IP.

**🔑 Exam keywords:** `audit` · `API calls` · `who made the change` · `account activity` · `governance` · `compliance` · `forensics`

**🎯 What they test**

- **On by default:** **Event history** shows the last **90 days** of management events for free.
- Create a **trail** to keep events longer by delivering them to an **S3 bucket** (and optionally CloudWatch Logs). A trail can cover **all Regions** and, with Organizations, **all accounts**.
- **Management events** (create/delete/modify resources) vs **data events** (for example S3 object reads, Lambda invocations; extra cost).
- **CloudTrail Insights** spots unusual API activity (paid).

**⚠️ Exam traps**

- CloudTrail records **API activity**, not CPU or memory (CloudWatch) and not a resource's configuration state (Config).
- Need records **longer than 90 days** → create a **trail** to S3.

📖 **Read more:** [What is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) · [CloudTrail concepts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html)

## AWS Config

**📘 AWS definition:** AWS Config provides a detailed view of the configuration of AWS resources in your account, including how they are related and how their configurations changed over time.

**💡 Simple definition:** **A history book and rule checker for resource settings.** "Was this bucket public last Tuesday? Is every EBS volume encrypted?"

**🔑 Exam keywords:** `configuration history` · `configuration changes` · `compliance rules` · `conformance packs` · `resource inventory` · `drift from policy`

**🎯 What they test**

- Records configuration **snapshots and changes** over time (a timeline per resource).
- **Config rules** (AWS managed or custom) evaluate resources and mark them **compliant / non-compliant**; **conformance packs** bundle rules; **remediation** can fix issues automatically (with Systems Manager Automation).
- A **regional** service; aggregate across accounts and Regions with an aggregator.
- Pay per configuration item recorded and per rule evaluation.

**⚠️ Exam traps**

- Config **detects and reports** non-compliance; it does not **prevent** an action (that is an IAM policy or SCP).

📖 **Read more:** [What is AWS Config?](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) · [Config rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html)

---

## AWS Trusted Advisor

**📘 AWS definition:** AWS Trusted Advisor helps you optimise costs, increase performance, improve security and resilience, and operate at scale in the cloud by continuously evaluating your AWS environment against best practices.

**💡 Simple definition:** **An automated cloud consultant** who walks through your account and hands you a to-do list.

**🔑 Exam keywords:** `best-practice checks` · `recommendations` · `cost optimisation` · `security` · `fault tolerance` · `service limits` · `idle resources`

**🎯 What they test**

- **Check categories:** **Cost optimisation**, **Performance**, **Security**, **Fault tolerance** (newer pages say *resilience*), **Service limits** (quotas), and **Operational excellence**.
- Typical findings: idle EC2 instances or load balancers, underused EBS volumes, **open security group ports**, **no MFA on the root user**, **public S3 buckets**, missing backups, quotas above 80%.
- **Basic Support:** core security checks and service-limit checks. **Business Support+ and higher:** the **full set** of checks (and API access).

**⚠️ Exam traps**

- Trusted Advisor **recommends**; it does not apply changes for you.
- Trusted Advisor checks **your account** broadly. **Well-Architected Tool** reviews **one workload** through question sets. **Compute Optimizer** gives **rightsizing** from utilisation data.
- The full set of checks depends on the **support plan**.

📖 **Read more:** [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/trusted-advisor.html) · [Trusted Advisor check reference](https://docs.aws.amazon.com/awssupport/latest/user/trusted-advisor-check-reference.html)

## AWS Health Dashboard

**📘 AWS definition:** AWS Health provides ongoing visibility into your resource performance and the availability of your AWS services and accounts.

**💡 Simple definition:** **A weather report for your part of AWS.** "Is AWS having a problem that affects **my** resources, and is maintenance coming?"

**🔑 Exam keywords:** `AWS service events` · `affects my resources` · `scheduled maintenance` · `personalised` · `AWS Health API`

**🎯 What they test**

- **Service health:** public status of all AWS services in all Regions.
- **Your account health:** personalised events affecting **your** resources, with guidance and scheduled changes; can send alerts via **EventBridge**.
- The **AWS Health API** (programmatic access) needs **Business Support+ or higher**.

**⚠️ Exam traps**

- Health Dashboard = **AWS-side** problems and maintenance. CloudWatch = **your** metrics. Trusted Advisor = **your** configuration.

📖 **Read more:** [What is AWS Health?](https://docs.aws.amazon.com/health/latest/ug/what-is-aws-health.html) · [AWS Health API](https://docs.aws.amazon.com/health/latest/ug/health-api.html)

## AWS Compute Optimizer

**📘 AWS definition:** AWS Compute Optimizer analyses the configuration and utilisation metrics of your AWS resources and recommends optimal resources to reduce costs and improve performance.

**💡 Simple definition:** **A tailor for your servers**: "this instance is too big, that one is too small."

**🔑 Exam keywords:** `rightsizing` · `over-provisioned` · `under-provisioned` · `machine learning recommendations` · `EC2, EBS, Lambda`

**🎯 What they test**

- Recommendations for **EC2 instances, Auto Scaling groups, EBS volumes, Lambda functions, ECS on Fargate, and RDS**, based on historical CloudWatch metrics.
- Opt in once; no extra charge for the basic service.

**⚠️ Exam traps**

- Rightsizing → **Compute Optimizer**. Analyse historic **spend** → Cost Explorer. Broad best practices → Trusted Advisor.

📖 **Read more:** [What is AWS Compute Optimizer?](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html)

---

## Ways to access AWS: Console, CLI, SDKs

All three call the same **AWS APIs** underneath.

| Method | What it is | Pick when |
|---|---|---|
| **AWS Management Console** | The web interface (plus the **AWS Console Mobile App**) | Point-and-click, learning, one-off tasks, visual monitoring |
| **AWS CLI** | Command-line tool for Windows, macOS, Linux | Scripts and automating repeated tasks from a terminal |
| **AWS SDKs** | Libraries for languages such as Python (Boto3), Java, JavaScript, .NET, Go | Calling AWS from **your application code** |
| **Infrastructure as code** | CloudFormation (and the AWS CDK) | Repeatable, version-controlled environments |

- Console sign-in uses a **password (+ MFA)**. CLI and SDKs use **access keys** or, better, temporary credentials from roles / IAM Identity Center.

⚠️ **Exam trap:** "Automate from scripts" → **CLI**. "From inside the application's code" → **SDK**.

📖 **Read more:** [What is the AWS Management Console?](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/what-is.html) · [What is the AWS CLI?](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) · [Tools to build on AWS (SDKs)](https://aws.amazon.com/developer/tools/)

## AWS CloudFormation

**📘 AWS definition:** AWS CloudFormation is a service that helps you model and set up your AWS resources: you create a template that describes the resources you want, and CloudFormation provisions and configures them for you.

**💡 Simple definition:** **A blueprint that builds the house for you, the same way every time.** Write the infrastructure as a text file (code), and CloudFormation creates it, updates it, or deletes it as one unit.

**🔑 Exam keywords:** `infrastructure as code (IaC)` · `template` · `stack` · `repeatable deployments` · `YAML/JSON` · `same environment in many Regions/accounts` · `drift detection`

**🎯 What they test**

- **Infrastructure as code:** version-controlled, repeatable, consistent environments (dev/test/prod identical); fewer manual errors.
- No extra charge; you pay for the resources created.

| Term | Meaning |
|---|---|
| **Template** | YAML or JSON file describing the resources |
| **Stack** | The resources created from one template, managed as a single unit |
| **StackSets** | Deploy the same stack across many accounts and Regions |
| **Change set** | Preview what an update will change before applying it |
| **Drift detection** | Find resources changed manually outside CloudFormation |
| **Rollback** | If creation fails, CloudFormation undoes what it built |

### Template anatomy (the sections)

Sections appear in this order. **Only `Resources` is required.**

| Section | What it is | Easy way to remember |
|---|---|---|
| **AWSTemplateFormatVersion** | Format version; the only valid value is `2010-09-09` | Version stamp |
| **Description** | Text describing the template | Title on the blueprint |
| **Metadata** | Extra information about the template or resources | Notes in the margin |
| **Parameters** | Values you pass **at deploy time** (instance type, environment name) | Blanks to fill in |
| **Rules** | Validate parameters before anything is created | A form that rejects bad answers |
| **Mappings** | Fixed lookup tables (for example Region → AMI ID) | Phone book inside the template |
| **Conditions** | True/false tests deciding whether something is created (only in `prod`) | An "if" statement |
| **Transform** | Process the template with a macro, e.g. AWS SAM for serverless apps | Pre-processor |
| **Resources** (required) | The AWS resources to build (`Type` + `Properties`) | Building materials |
| **Outputs** | Values returned after the build (a URL, an ID); can be exported to other stacks | The receipt |

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Small web server
Parameters:                      # asked for at deploy time
  InstanceType:
    Type: String
    Default: t3.micro
    AllowedValues: [t3.micro, t3.small]
Mappings:                        # fixed lookup table (IDs are placeholders)
  RegionMap:
    us-east-1: { AMI: ami-11111111 }
    eu-west-1: { AMI: ami-22222222 }
Resources:                       # required: what to create
  WebServer:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", AMI]
Outputs:                         # returned after creation
  InstanceId:
    Value: !Ref WebServer
```

Quick test: values chosen **at deploy time** → **Parameters**; fixed table **by Region** → **Mappings**; create **only in production** → **Conditions**; return a URL/ID → **Outputs**.

**⚠️ Exam traps**

- CloudFormation builds **any infrastructure**. **Elastic Beanstalk** deploys **applications** (and uses CloudFormation behind the scenes).
- "Deploy the **same** environment quickly and consistently in another Region" → CloudFormation.
- Only **Resources** is mandatory; **Outputs** creates nothing.

📖 **Read more:** [What is AWS CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) · [Template anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html) · [StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) · [Change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) · [Drift detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html)

## AWS Systems Manager

**📘 AWS definition:** AWS Systems Manager is a secure, end-to-end management solution for resources on AWS and in multicloud and hybrid environments.

**💡 Simple definition:** **A remote control for your whole fleet of servers**: patch them, run commands on them, and log in to them, all from one place, without opening SSH ports.

**🔑 Exam keywords:** `patch management` · `run commands at scale` · `Session Manager` · `no SSH / bastion host` · `Parameter Store` · `inventory` · `hybrid (on-premises) servers`

**🎯 What they test**

| Capability | What it does |
|---|---|
| **Session Manager** | Secure shell/RDP access through the console or CLI **without opening inbound ports, bastion hosts, or SSH keys**; sessions are logged |
| **Patch Manager** | Automatically patch OS and applications on fleets of instances |
| **Run Command** | Run a script/command on many instances at once |
| **Parameter Store** | Store configuration values and secrets (no automatic rotation) |
| **Automation** | Runbooks for common tasks (for example, fix a non-compliant resource) |
| **Inventory / Fleet Manager** | See software and configuration on all managed nodes |

- Works for EC2 **and on-premises / other-cloud servers** (with the SSM Agent).
- Most core features have no extra charge.

**⚠️ Exam traps**

- "Securely connect to an EC2 instance **without** opening port 22" → **Session Manager**.
- Systems Manager **operates**; CloudWatch **monitors**; CloudTrail **audits**.

📖 **Read more:** [What is AWS Systems Manager?](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) · [Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html) · [Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html) · [Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)

---

## AWS Control Tower

**📘 AWS definition:** AWS Control Tower offers a straightforward way to set up and govern an AWS multi-account environment, following prescriptive best practices.

**💡 Simple definition:** **A ready-made, rule-checked campus for all your accounts**, built on top of Organizations in a few clicks.

**🔑 Exam keywords:** `landing zone` · `multi-account setup` · `guardrails / controls` · `account factory` · `best-practice governance`

**🎯 What they test**

- Sets up a **landing zone** (a well-architected multi-account environment) using Organizations, IAM Identity Center, CloudTrail, and Config.
- **Controls (guardrails):** **preventive** (stop actions, using SCPs), **detective** (find violations, using Config), and proactive.
- **Account Factory** creates new, pre-configured accounts; a dashboard shows compliance.
- No extra charge; pay for the services it sets up.

**⚠️ Exam traps**

- Organizations = the **engine** (accounts, SCPs, billing). Control Tower = the **automated, best-practice setup** on top of it.

📖 **Read more:** [What is AWS Control Tower?](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html)

## AWS Service Catalog

**📘 AWS definition:** AWS Service Catalog lets you centrally manage commonly deployed IT services, and helps you achieve consistent governance and meet your compliance requirements.

**💡 Simple definition:** **A company app store of approved AWS setups.** Users launch only what IT has pre-approved.

**🔑 Exam keywords:** `approved products` · `catalogue` · `self-service with governance` · `standardise deployments`

**🎯 What they test**

- Admins create **products** (usually CloudFormation templates) in **portfolios**; users launch them without needing broad IAM permissions.

**⚠️ Exam traps**

- Approved **internal** catalogue → Service Catalog. Buying **third-party** software → **AWS Marketplace**.

📖 **Read more:** [What is AWS Service Catalog?](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html)

## AWS License Manager

**📘 AWS definition:** AWS License Manager makes it easier to manage your software licences from vendors such as Microsoft, SAP, Oracle, and IBM across AWS and on-premises environments.

**💡 Simple definition:** **A licence counter** that stops you from using more copies than you paid for.

**🔑 Exam keywords:** `software licences` · `BYOL` · `licence compliance` · `prevent over-use`

**🎯 What they test**

- Define licence rules (per vCPU, socket, core), track usage, and **block launches** that would break them. Works with Dedicated Hosts for BYOL.

**⚠️ Exam traps**

- Licence tracking → License Manager; the **Dedicated Host** is the hardware that makes per-socket BYOL possible.

📖 **Read more:** [What is AWS License Manager?](https://docs.aws.amazon.com/license-manager/latest/userguide/license-manager.html)

## Service Quotas

**📘 AWS definition:** Service Quotas enables you to view and manage your quotas for AWS services from a central location.

**💡 Simple definition:** **The list of your account's limits**, and the button to ask for more.

**🔑 Exam keywords:** `service limits` · `quotas` · `request a quota increase` · `limit reached`

**🎯 What they test**

- Quotas (formerly "service limits") cap resources per account per Region (for example, number of VPCs). View them and **request increases** in one place; set CloudWatch alarms near limits.

**⚠️ Exam traps**

- Trusted Advisor **warns** about limits; Service Quotas is where you **view and raise** them.

📖 **Read more:** [What is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html)

---

## Cost tools at a glance

Domain 4 loves these. Learn the **time direction**:

| Tool | When | Question words | Memory hook |
|---|---|---|---|
| **Pricing Calculator** | **Before** you build | "Estimate the cost of a planned architecture" | Calculator = **future** |
| **Budgets** | **Ongoing / future** alerts | "Alert me when spend **will** exceed $X" (actual or **forecast**) | Budget = a **limit** with an alarm |
| **Cost Explorer** | **Past** (+ simple forecast) | "Visualise and analyse spend by service/account over months" | Explore the **history** |
| **Cost and Usage Reports** | **Past**, most detailed | "Most **granular** line-by-line data, export to S3, analyse with Athena/Quick" | The full **bank statement** |
| **Billing and Cost Management console / Bills page** | This month | "See current invoice and charges" | The bill |
| **Cost allocation tags** | Any | "Track cost **by project/department**" | Labels on spending |
| **Consolidated billing** (Organizations) | Any | "One bill for many accounts, volume discounts" | Family plan |

📖 **Read more:** [What is AWS Billing and Cost Management?](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html) · [AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html)

## AWS Pricing Calculator

**📘 AWS definition:** AWS Pricing Calculator lets you explore AWS services and create an estimate for the cost of your use cases on AWS.

**💡 Simple definition:** **A shopping-basket estimate before you buy.**

**🔑 Exam keywords:** `estimate` · `before deployment` · `plan costs` · `compare options`

**🎯 What they test**

- Free web tool; build estimates per service and Region, group them, and share or export them. Also helps compare on-premises costs (TCO-style reasoning).

**⚠️ Exam traps**

- Pricing Calculator estimates **new** workloads. Analysing **existing** spend → Cost Explorer.

📖 **Read more:** [What is AWS Pricing Calculator?](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/what-is-pricing-calculator.html) · [AWS Pricing Calculator](https://calculator.aws/)

## AWS Cost Explorer

**📘 AWS definition:** AWS Cost Explorer is a tool that enables you to view and analyse your costs and usage.

**💡 Simple definition:** **An interactive bank-statement analyser** with charts and a forecast.

**🔑 Exam keywords:** `visualise costs` · `historical spend` · `group by service / account / tag` · `forecast` · `Savings Plans / RI recommendations`

**🎯 What they test**

- Up to **13 months** of history (more with multi-year data enabled) and a **forecast**; filter and group by service, account, Region, and tags.
- Recommends **Savings Plans and Reserved Instance** purchases, and shows their utilisation and coverage.

**⚠️ Exam traps**

- Cost Explorer **analyses**; it does not **alert** (Budgets) or **estimate a new design** (Pricing Calculator).

📖 **Read more:** [Analysing costs with Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)

## AWS Budgets

**📘 AWS definition:** AWS Budgets lets you set custom budgets to track your cost and usage, and responds quickly to alerts received from email or Amazon SNS notifications if you exceed your threshold.

**💡 Simple definition:** **A spending limit with an alarm bell.**

**🔑 Exam keywords:** `alert` · `threshold` · `forecast to exceed` · `cost budget` · `usage budget` · `RI / Savings Plans utilisation` · `budget actions`

**🎯 What they test**

- Budget types: **cost**, **usage**, **Reserved Instance**, and **Savings Plans** (utilisation/coverage).
- Alerts on **actual** or **forecasted** amounts by email or **SNS**.
- **Budget actions** can respond automatically (for example apply an IAM policy or SCP, or stop EC2/RDS instances).

**⚠️ Exam traps**

- A budget alert does **not** automatically stop spending unless you configure **budget actions**.

📖 **Read more:** [Managing your costs with AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

## AWS Cost and Usage Reports (CUR)

**📘 AWS definition:** AWS Cost and Usage Reports contain the most comprehensive set of cost and usage data available, delivered to an Amazon S3 bucket.

**💡 Simple definition:** **The complete, line-by-line bank statement** dropped into S3 for deep analysis.

**🔑 Exam keywords:** `most detailed / granular` · `hourly line items` · `deliver to S3` · `analyse with Athena / Redshift / Quick`

**🎯 What they test**

- The **most granular** billing data (hourly/daily, per resource, with tags); delivered to **S3**; query with **Athena** or visualise with **Amazon Quick**.
- Newer accounts create it through **AWS Data Exports** (CUR 2.0).

**⚠️ Exam traps**

- "Most detailed" / "line item" → **CUR**. "Charts and quick analysis" → Cost Explorer.

📖 **Read more:** [What are AWS Cost and Usage Reports?](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html) · [AWS Data Exports](https://docs.aws.amazon.com/cur/latest/userguide/what-is-data-exports.html)

## Cost allocation tags

- **Tags** are key-value labels (for example `Project=Website`, `Department=Finance`) on resources.
- Once **activated as cost allocation tags** in the Billing console, they appear in Cost Explorer, Budgets, and CUR, so you can **charge costs back** to teams.
- Two kinds: **AWS-generated** (such as `aws:createdBy`) and **user-defined**.

⚠️ **Exam trap:** "Track costs by department/project" → **cost allocation tags** (plus Cost Explorer), not separate invoices.

📖 **Read more:** [Using cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)

---

## Quick chooser

| Question keyword | Answer |
|---|---|
| Metrics, alarms, dashboards, logs | CloudWatch |
| API history, who did what, audit | CloudTrail |
| Configuration history, compliance rule | Config |
| Best-practice recommendations across the account | Trusted Advisor |
| AWS outage/maintenance affecting my resources | Health Dashboard |
| Right-size EC2, EBS, Lambda | Compute Optimizer |
| Review one workload against the six pillars | [Well-Architected Tool](01-cloud-concepts.md#aws-well-architected-tool) |
| Repeatable infrastructure template | CloudFormation |
| Patch servers, run commands, no-SSH access | Systems Manager |
| Govern multiple accounts, SCPs, one bill | [Organizations](04-security-and-compliance.md#aws-organizations) |
| Secure multi-account landing zone | Control Tower |
| Approved internal product catalogue | Service Catalog |
| Track software licences | License Manager |
| View or raise service quotas | Service Quotas |
| Estimate a future architecture's cost | Pricing Calculator |
| Analyse past spend, forecast, RI/SP recommendations | Cost Explorer |
| Alert on actual or forecast overspend | Budgets |
| Most detailed billing data in S3 | Cost and Usage Reports |
| Costs by project/department | Cost allocation tags |

## Top traps on this page

1. CloudWatch = **performance**; CloudTrail = **who did it**; Config = **what it looked like / compliant?**
2. CloudTrail event history = **90 days**; keep longer with a **trail to S3**.
3. Billing metrics live in **us-east-1**.
4. Trusted Advisor's **full** checks need **Business Support+** or higher.
5. Health Dashboard = **AWS's** problems; CloudWatch = **your** metrics.
6. CloudFormation = infrastructure as code; Beanstalk = deploy an app.
7. Session Manager = access instances **without SSH/open ports**.
8. Organizations (engine) vs Control Tower (automated landing zone).
9. Pricing Calculator (**before**), Budgets (**alerts**), Cost Explorer (**analyse past**), CUR (**most detailed**).

[← Security and compliance](04-security-and-compliance.md) | [Index](README.md) | [Analytics, integration, ML, and more →](06-analytics-integration-ml-and-more.md)
