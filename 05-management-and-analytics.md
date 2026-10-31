# 05 — Management and Analytics

[← Security and compliance](04-security-and-compliance.md) | [Index](README.md) | [Cheat sheet →](06-cheat-sheet-exam-highlights.md)

## Amazon CloudWatch

📌 **What is it?** Monitoring and observability service for AWS resources and applications. Think of a dashboard, logbook, and alarm system for cloud workloads.

🎯 **Need to Know for the Exam (Key Keywords)**

- Collects **metrics**, **logs**, and events; dashboards visualise health and **alarms** trigger actions or notifications.
- Common uses: CPU monitoring, application logs, operational dashboards, and Auto Scaling alarms.
- Basic monitoring for some services is included; custom/detailed metrics, logs, alarms, and dashboards can incur charges.

⚠️ **Exam Trap / Common Distractor:** CloudWatch monitors performance and logs; CloudTrail records AWS API activity for auditing.

## AWS CloudTrail

📌 **What is it?** Records account activity and AWS API calls. Think of a security camera log showing who did what, when, and from where.

🎯 **Need to Know for the Exam (Key Keywords)**

- Provides **governance, compliance, operational auditing, and risk auditing**.
- Event history includes recent management events; trails can deliver events to S3 and CloudWatch Logs.
- Event history is available at no charge; trails, data events, Insights, and storage/analysis can add cost.

⚠️ **Exam Trap / Common Distractor:** “Who deleted or changed this resource?” points to CloudTrail, not CloudWatch.

## AWS Config

📌 **What is it?** Records resource configurations and evaluates them against desired rules. Think of a historian that notices when equipment no longer matches policy.

🎯 **Need to Know for the Exam (Key Keywords)**

- Configuration history, change tracking, relationships, compliance rules, and conformance packs.
- Answers “what did this resource's configuration look like?” and “is it compliant?”
- Pay for configuration items recorded and rule/conformance evaluations.

⚠️ **Exam Trap / Common Distractor:** Config records resource configuration and compliance; CloudTrail records API calls and identity activity.

## AWS Trusted Advisor

📌 **What is it?** Reviews an AWS environment and recommends improvements. Think of an automated cloud consultant checking for common problems.

🎯 **Need to Know for the Exam (Key Keywords)**

- Recommendation categories include **cost optimization, performance, security, fault tolerance, service limits, and operational excellence**.
- Examples include idle resources, security exposure, resilience, and quota risks.
- Available checks and features depend on the AWS Support plan.

⚠️ **Exam Trap / Common Distractor:** Trusted Advisor recommends; it does not automatically apply every change or replace Well-Architected workload reviews.

## AWS Cost Explorer

📌 **What is it?** Visualises, analyses, and forecasts AWS cost and usage over time. Think of an interactive bank-statement analyser.

🎯 **Need to Know for the Exam (Key Keywords)**

- Filter and group spend by service, account, Region, and activated tags.
- Shows historical trends, forecasts, and Savings Plans/Reserved Instance recommendations.
- Core Cost Explorer access is generally available without upfront cost; some API and feature usage can be charged.

⚠️ **Exam Trap / Common Distractor:** Cost Explorer analyses spend; AWS Budgets alerts against thresholds; Pricing Calculator estimates before deployment.

## AWS Budgets

📌 **What is it?** Tracks cost, usage, reservations, or Savings Plans against limits and sends alerts. Think of a spending limit alarm.

🎯 **Need to Know for the Exam (Key Keywords)**

- Alerts on **actual or forecast** values through email/SNS; budget actions can apply selected responses.
- Supports cost, usage, reservation, and Savings Plans budgets.
- A limited number of simple budgets may be included; action-enabled and additional budgets can cost money.

⚠️ **Exam Trap / Common Distractor:** A budget alert does not by itself stop all AWS spending or close resources.

## AWS Organizations

📌 **What is it?** Central governance and consolidated billing for multiple AWS accounts. Think of one parent account organising departments into governed groups.

🎯 **Need to Know for the Exam (Key Keywords)**

- Organisational units group accounts; SCPs set permission guardrails; consolidated billing combines usage.
- Central account management can support separation by workload, team, or environment.
- Organizations has no additional charge; deployed resources and integrated services may cost money.

⚠️ **Exam Trap / Common Distractor:** SCPs limit the maximum permissions available; they do not grant IAM permissions.

See [Security and compliance](04-security-and-compliance.md#aws-organizations) for the security-focused explanation.

## AWS Well-Architected Tool

📌 **What is it?** Reviews workloads using questions based on the six Well-Architected pillars. Think of a guided architecture checklist.

🎯 **Need to Know for the Exam (Key Keywords)**

- Identifies high-risk issues and improvement guidance across all six pillars.
- Records workloads and review milestones; useful throughout the architecture lifecycle.
- The tool has no charge; changes made after a review may cost money.

⚠️ **Exam Trap / Common Distractor:** It reviews individual workloads; Trusted Advisor checks an AWS environment for service-level recommendations.

## Amazon Athena

📌 **What is it?** Serverless interactive query service that uses SQL to analyse data in S3. Think of asking spreadsheet-like questions directly against files in a warehouse.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Serverless**, no infrastructure to manage; queries formats such as CSV, JSON, and Parquet in S3.
- Good for ad hoc log analysis and data-lake queries; integrates with AWS Glue Data Catalog.
- Pay mainly for data scanned; compressed columnar formats and partitioning reduce cost.

⚠️ **Exam Trap / Common Distractor:** Athena queries data in S3; Redshift is a managed data warehouse for sustained analytics.

## AWS CloudFormation

📌 **What is it?** Provisions AWS resources from declarative templates. Think of a reusable blueprint for building the same environment repeatedly.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Infrastructure as code (IaC)**, repeatable deployments, stacks, change sets, and drift detection.
- Templates can be YAML or JSON and describe desired resources and dependencies.
- CloudFormation normally has no extra charge for AWS resource provisioning; pay for created resources and certain extensions.

⚠️ **Exam Trap / Common Distractor:** CloudFormation creates infrastructure; Elastic Beanstalk focuses on deploying and operating applications.

## AWS Systems Manager

📌 **What is it?** Centrally views and operates supported AWS, on-premises, and multicloud nodes. Think of a remote operations console for a fleet of machines.

🎯 **Need to Know for the Exam (Key Keywords)**

- Capabilities include Session Manager, automation, patching, inventory, run commands, and Parameter Store.
- Helps operate at scale without direct inbound SSH/RDP when Session Manager is used.
- Many core capabilities have no additional charge; advanced features and related resources can cost money.

⚠️ **Exam Trap / Common Distractor:** Systems Manager operates and automates resources; CloudWatch observes health and CloudTrail audits API activity.

## Quick chooser

| Question keyword | Answer |
|---|---|
| Metrics, alarms, dashboards, logs | CloudWatch |
| API history, identity, audit | CloudTrail |
| Configuration history, compliance rule | Config |
| Best-practice recommendations | Trusted Advisor |
| Workload pillar review | Well-Architected Tool |
| Analyse historical spend | Cost Explorer |
| Alert on forecast overspend | Budgets |
| Govern multiple accounts | Organizations |
| Query S3 with SQL | Athena |
| Repeatable infrastructure template | CloudFormation |
| Fleet operations and patching | Systems Manager |


[← Security and compliance](04-security-and-compliance.md) | [Index](README.md) | [Cheat sheet →](06-cheat-sheet-exam-highlights.md)