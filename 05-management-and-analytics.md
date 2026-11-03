# 05 — Management and Analytics

[← Security and compliance](04-security-and-compliance.md) | [Index](README.md) | [Cheat sheet →](06-cheat-sheet-exam-highlights.md)

## Amazon CloudWatch

📌 **What is it?** Monitoring and observability service for AWS resources and applications. Think of a dashboard, logbook, and alarm system for cloud workloads.

🎯 **Need to Know for the Exam (Key Keywords)**

- Collects **metrics**, **logs**, and events; dashboards visualise health and **alarms** trigger actions or notifications.
- Common uses: CPU monitoring, application logs, operational dashboards, and Auto Scaling alarms.
- Basic monitoring for some services is included; custom/detailed metrics, logs, alarms, and dashboards can incur charges.

⚠️ **Exam Trap / Common Distractor:** CloudWatch monitors performance and logs; CloudTrail records AWS API activity for auditing.

📖 **AWS docs:** [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)

## AWS CloudTrail

📌 **What is it?** Records account activity and AWS API calls. Think of a security camera log showing who did what, when, and from where.

🎯 **Need to Know for the Exam (Key Keywords)**

- Provides **governance, compliance, operational auditing, and risk auditing**.
- Event history includes recent management events; trails can deliver events to S3 and CloudWatch Logs.
- Event history is available at no charge; trails, data events, Insights, and storage/analysis can add cost.

⚠️ **Exam Trap / Common Distractor:** “Who deleted or changed this resource?” points to CloudTrail, not CloudWatch.

📖 **AWS docs:** [What is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

## AWS Config

📌 **What is it?** Records resource configurations and evaluates them against desired rules. Think of a historian that notices when equipment no longer matches policy.

🎯 **Need to Know for the Exam (Key Keywords)**

- Configuration history, change tracking, relationships, compliance rules, and conformance packs.
- Answers “what did this resource's configuration look like?” and “is it compliant?”
- Pay for configuration items recorded and rule/conformance evaluations.

⚠️ **Exam Trap / Common Distractor:** Config records resource configuration and compliance; CloudTrail records API calls and identity activity.

📖 **AWS docs:** [What is AWS Config?](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html)

## AWS Trusted Advisor

📌 **What is it?** Reviews an AWS environment and recommends improvements. Think of an automated cloud consultant checking for common problems.

🎯 **Need to Know for the Exam (Key Keywords)**

- Recommendation categories include **cost optimization, performance, security, fault tolerance (called *resilience* in newer AWS pages), service limits, and operational excellence**.
- Examples include idle resources, security exposure (such as open ports), resilience, and quota risks.
- **Core checks come with Basic Support; the full set of checks needs Business Support+ or higher.**

⚠️ **Exam Trap / Common Distractor:** Trusted Advisor recommends; it does not automatically apply every change or replace Well-Architected workload reviews.

📖 **AWS docs:** [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/trusted-advisor.html) · [Compare support plans](https://aws.amazon.com/premiumsupport/plans/)

## AWS Cost Explorer

📌 **What is it?** Visualises, analyses, and forecasts AWS cost and usage over time. Think of an interactive bank-statement analyser.

🎯 **Need to Know for the Exam (Key Keywords)**

- Filter and group spend by service, account, Region, and activated tags.
- Shows historical trends, forecasts, and Savings Plans/Reserved Instance recommendations.
- Core Cost Explorer access is generally available without upfront cost; some API and feature usage can be charged.

⚠️ **Exam Trap / Common Distractor:** Cost Explorer analyses spend; AWS Budgets alerts against thresholds; Pricing Calculator estimates before deployment.

📖 **AWS docs:** [Getting started with Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-getting-started.html)

## AWS Budgets

📌 **What is it?** Tracks cost, usage, reservations, or Savings Plans against limits and sends alerts. Think of a spending limit alarm.

🎯 **Need to Know for the Exam (Key Keywords)**

- Alerts on **actual or forecast** values through email/SNS; budget actions can apply selected responses.
- Supports cost, usage, reservation, and Savings Plans budgets.
- A limited number of simple budgets may be included; action-enabled and additional budgets can cost money.

⚠️ **Exam Trap / Common Distractor:** A budget alert does not by itself stop all AWS spending or close resources.

📖 **AWS docs:** [Managing your costs with AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

## AWS Organizations

📌 **What is it?** Central governance and consolidated billing for multiple AWS accounts. Think of one parent account organising departments into governed groups.

🎯 **Need to Know for the Exam (Key Keywords)**

- Organisational units group accounts; SCPs set permission guardrails; consolidated billing combines usage.
- Central account management can support separation by workload, team, or environment.
- Organizations has no additional charge; deployed resources and integrated services may cost money.

⚠️ **Exam Trap / Common Distractor:** SCPs limit the maximum permissions available; they do not grant IAM permissions.

See [Security and compliance](04-security-and-compliance.md#aws-organizations) for the security-focused explanation.

📖 **AWS docs:** [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html)

## AWS Well-Architected Tool

📌 **What is it?** Reviews workloads using questions based on the six Well-Architected pillars. Think of a guided architecture checklist.

🎯 **Need to Know for the Exam (Key Keywords)**

- Identifies high-risk issues and improvement guidance across all six pillars.
- Records workloads and review milestones; useful throughout the architecture lifecycle.
- The tool has no charge; changes made after a review may cost money.

⚠️ **Exam Trap / Common Distractor:** It reviews individual workloads; Trusted Advisor checks an AWS environment for service-level recommendations.

📖 **AWS docs:** [Well-Architected Tool User Guide](https://docs.aws.amazon.com/wellarchitected/latest/userguide/well-architected.html)

## Amazon Athena

📌 **What is it?** Serverless interactive query service that uses SQL to analyse data in S3. Think of asking spreadsheet-like questions directly against files in a warehouse.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Serverless**, no infrastructure to manage; queries formats such as CSV, JSON, and Parquet in S3.
- Good for ad hoc log analysis and data-lake queries; integrates with AWS Glue Data Catalog.
- Pay mainly for data scanned (or queries run); compressed columnar formats and partitioning reduce cost.

⚠️ **Exam Trap / Common Distractor:** Athena queries data in S3; Redshift is a managed data warehouse for sustained analytics.

📖 **AWS docs:** [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) · [Analytics services overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/analytics.html)

## AWS CloudFormation

📌 **What is it?** Provisions AWS resources from declarative templates. Think of a reusable blueprint for building the same environment repeatedly.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Infrastructure as code (IaC)**, repeatable deployments, stacks, change sets, and drift detection.
- Templates can be YAML or JSON and describe desired resources and dependencies.
- CloudFormation normally has no extra charge for AWS resource provisioning; pay for created resources and certain extensions.

⚠️ **Exam Trap / Common Distractor:** CloudFormation creates infrastructure; Elastic Beanstalk focuses on deploying and operating applications.

📖 **AWS docs:** [What is AWS CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

### Key terms

| Term | Meaning |
|---|---|
| **Template** | The YAML or JSON file that describes the resources you want |
| **Stack** | The group of resources CloudFormation creates, updates, and deletes together from one template |
| **Change set** | A preview of what an update will change, before you apply it |
| **Drift detection** | Finds resources that were changed manually outside CloudFormation |
| **Rollback** | If creation fails, CloudFormation undoes what it built so you are not left with a half-built stack |

### Template anatomy (the sections)

Sections appear in this order. **Only `Resources` is required.** Everything else is optional.

| Section | What it is | Easy way to remember | Exam keyword |
|---|---|---|---|
| **AWSTemplateFormatVersion** | The template format version. The only valid value is `2010-09-09` | The template's version stamp | Format version |
| **Description** | A text note about what the template does. It must come right after the format version | The title on the blueprint | Description |
| **Metadata** | Extra information about the template or a resource, for example how the console groups and orders parameters, or configuration for instance setup | Notes in the margin | Additional information |
| **Parameters** | Values you pass in **when you create or update a stack** (instance type, key pair, environment name). Can have defaults and allowed values | Blanks to fill in at deploy time | Input, customise, reuse the same template |
| **Rules** | Checks that validate parameters, or combinations of them, before anything is created | A form that rejects bad answers | Validate parameters |
| **Mappings** | Fixed lookup tables written into the template, such as Region to AMI ID | A phone book inside the template | Lookup, per-Region values |
| **Conditions** | True/false tests that decide whether a resource is created (for example only in `prod`) | An "if" statement | Create only if, environment-aware |
| **Transform** | Tells CloudFormation to process the template with a macro, for example AWS SAM for serverless apps or snippets stored in S3 | A pre-processor | SAM, macros |
| **Resources** (required) | The AWS resources to build, each with a name, a `Type` such as `AWS::EC2::Instance`, and `Properties` | The actual building materials | What to create |
| **Outputs** | Values returned after the stack is built (a website URL, a bucket name, an instance ID). They can be **exported** so other stacks can import them | The receipt after the build | Return values, cross-stack reference |

### Mini example

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
    Description: ID of the web server
    Value: !Ref WebServer
```

- **Intrinsic functions** such as `!Ref`, `!GetAtt`, `!FindInMap`, `!If`, and `!ImportValue` connect the sections together.
- Quick test: "values chosen at deploy time" means **Parameters**; "fixed table by Region" means **Mappings**; "create only in production" means **Conditions**; "return the URL or ID" means **Outputs**.

⚠️ **Exam Trap / Common Distractor:** **Parameters** change per deployment (you supply them). **Mappings** are fixed inside the template. Only **Resources** is mandatory, and **Outputs** does not create anything; it only reports values.

📖 **AWS docs:** [Template anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html) · [Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) · [Mappings](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html) · [Conditions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html) · [Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resources-section-structure.html) · [Outputs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html) · [Metadata](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/metadata-section-structure.html) · [Change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) · [Drift detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html)

## AWS Systems Manager

📌 **What is it?** Centrally views and operates supported AWS, on-premises, and multicloud nodes. Think of a remote operations console for a fleet of machines.

🎯 **Need to Know for the Exam (Key Keywords)**

- Capabilities include Session Manager, automation, patching, inventory, run commands, and Parameter Store.
- Helps operate at scale without direct inbound SSH/RDP when Session Manager is used.
- Many core capabilities have no additional charge; advanced features and related resources can cost money.

⚠️ **Exam Trap / Common Distractor:** Systems Manager operates and automates resources; CloudWatch observes health and CloudTrail audits API activity.

📖 **AWS docs:** [What is AWS Systems Manager?](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)

## More management and governance services

### AWS Compute Optimizer

📌 **What is it?** Uses machine learning on your usage data to recommend the best-sized resources. Think of a tailor who tells you your instances are too big or too small.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Rightsizing** recommendations for **EC2 instances, EBS volumes, and Lambda functions**; spots over-provisioned (wasting money) and under-provisioned (slow) resources.
- Opt in once; no additional charge for the service.

⚠️ **Exam Trap / Common Distractor:** Compute Optimizer recommends sizes from utilisation. Trusted Advisor gives broader best-practice checks, and Cost Explorer analyses spend.

📖 **AWS docs:** [What is AWS Compute Optimizer?](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html)

### AWS Control Tower

📌 **What is it?** Sets up and governs a secure, well-architected **multi-account** AWS environment (a *landing zone*) following best practices. Think of a ready-made, rule-checked campus for all your accounts.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Landing zone**, account factory, and **controls (guardrails)** that prevent or detect rule-breaking, built on Organizations and other services.
- Dashboard shows compliance status across accounts.
- No extra charge for Control Tower itself; you pay for the services it sets up.

⚠️ **Exam Trap / Common Distractor:** Organizations manages accounts and SCPs. Control Tower automates the whole secure setup on top of it.

📖 **AWS docs:** [What is AWS Control Tower?](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html)

### AWS Health Dashboard

📌 **What is it?** Shows the health of AWS services and alerts you to events that affect **your** resources. Think of a weather report for your own cloud neighbourhood.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Service health:** the general status of AWS services. **Your account health:** a personalised view of events affecting your resources, plus scheduled maintenance.
- Proactive notifications (for example through EventBridge or SNS); the AWS Health API needs Business Support+ or higher.
- No charge for the dashboard.

⚠️ **Exam Trap / Common Distractor:** Health Dashboard reports AWS-side events and scheduled changes. CloudWatch monitors your own metrics, and Trusted Advisor checks your configuration.

📖 **AWS docs:** [What is AWS Health?](https://docs.aws.amazon.com/health/latest/ug/what-is-aws-health.html) · [AWS Health API](https://docs.aws.amazon.com/health/latest/ug/health-api.html)

### Other management services (quick reference)

| Service | What it does | Exam keyword | AWS docs |
|---|---|---|---|
| **AWS Service Catalog** | Create and share catalogues of **approved** IT products so users deploy only what policy allows | Approved products, governance | [Docs](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) |
| **AWS License Manager** | Track and manage software licences (for example Microsoft, Oracle, SAP, IBM) across AWS and on-premises; can block over-use | Licence compliance, BYOL | [Docs](https://docs.aws.amazon.com/license-manager/latest/userguide/license-manager.html) |
| **Service Quotas** | View and request increases for your service quotas (limits) in one place | Quota, limit increase | [Docs](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) |
| **AWS Auto Scaling** | Scaling plans that keep capacity right across several services (for example EC2, ECS, DynamoDB) | Elasticity, scaling plan | [Docs](https://docs.aws.amazon.com/autoscaling/plans/userguide/what-is-aws-auto-scaling.html) |
| **AWS Management Console** | The web interface for managing AWS (also a mobile app) | Point-and-click | [Overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/management-governance.html) |

📖 **AWS docs:** [Management and governance services overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/management-governance.html)

## More analytics services

### Amazon Kinesis

📌 **What is it?** A family of services to collect, process, and analyse **real-time streaming data** such as clickstreams, logs, IoT telemetry, and video. Think of a conveyor belt that carries data as it is created.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Kinesis Data Streams:** capture and store streaming data for real-time processing by your own applications.
- **Amazon Data Firehose:** the easiest way to **load** streaming data into S3, Redshift, OpenSearch Service, and Splunk, with no administration.
- **Managed Service for Apache Flink:** analyse streams with SQL or Java in real time. **Kinesis Video Streams:** stream video from devices.
- Pay for what you use (for example data ingested and shard or throughput usage).

⚠️ **Exam Trap / Common Distractor:** Kinesis handles continuous streams in real time. Athena queries data already stored in S3, and SQS is a message queue between components.

📖 **AWS docs:** [Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) · [Amazon Kinesis](https://aws.amazon.com/kinesis/) · [Amazon Data Firehose](https://aws.amazon.com/firehose/)

### Other analytics services (quick reference)

| Service | What it does | Exam keyword | AWS docs |
|---|---|---|---|
| **AWS Glue** | Fully managed **ETL** (extract, transform, load) service with a **Data Catalog** that stores table definitions and schemas | ETL, data catalogue, prepare data | [Glue](https://aws.amazon.com/glue/) |
| **Amazon EMR** | Big data platform that runs frameworks such as **Apache Spark, Hive, HBase, and Flink** on managed clusters | Big data, Spark, Hadoop | [EMR](https://aws.amazon.com/emr/) |
| **Amazon OpenSearch Service** | Deploy and run OpenSearch for **log analytics, full-text search**, and application monitoring | Search, log analytics | [OpenSearch Service](https://aws.amazon.com/opensearch-service/) |
| **Amazon Quick** (the BI service, formerly QuickSight) | **Business intelligence**: interactive dashboards viewed in a browser or mobile device, with no servers to manage | Dashboards, BI, visualisation | [Quick](https://aws.amazon.com/quicksight/) |
| **Amazon Redshift** | Cloud data warehouse for large-scale analytics | Data warehouse | See [Storage and databases](03-storage-and-databases.md#amazon-redshift) |

📖 **AWS docs:** [Choosing an AWS analytics service](https://docs.aws.amazon.com/decision-guides/latest/analytics-on-aws-how-to-choose/analytics-on-aws-how-to-choose.html)

## Application integration

📌 **What is it?** Services that let separate parts of an application (microservices, serverless apps) talk to each other without being tightly tied together. Choose by how the message should travel.

| If you need… | Choose |
|---|---|
| **Push** a message or alert to many subscribers (email, SMS, mobile, Lambda, queues) | **SNS** |
| A **queue** where messages wait until a consumer processes them | **SQS** |
| **Route events** from AWS services, your apps, or SaaS to targets using rules or schedules | **EventBridge** |
| **Coordinate steps** of a workflow with retries and error handling | **Step Functions** |

📖 **AWS docs:** [Amazon SQS, SNS, or EventBridge?](https://docs.aws.amazon.com/decision-guides/latest/sns-or-sqs-or-eventbridge/sns-or-sqs-or-eventbridge.html) · [Choosing an application integration service](https://docs.aws.amazon.com/decision-guides/latest/application-integration-on-aws-how-to-choose/application-integration-on-aws-how-to-choose.html)

### Amazon SNS (Simple Notification Service)

📌 **What is it?** Fully managed **publish/subscribe (pub/sub)** messaging. A publisher sends one message to a **topic**, and SNS pushes it to every subscriber. Think of a loudspeaker announcing to everyone who is listening.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Push**-based, many-to-many; **fan-out** to SQS queues, Lambda functions, HTTP/S endpoints, and also **email, SMS, and mobile push** to end users.
- Typical use: send alerts and notifications (for example from CloudWatch alarms or Budgets).
- Pay per request and per delivery.

⚠️ **Exam Trap / Common Distractor:** SNS pushes messages to subscribers immediately. SQS holds messages until a consumer pulls them.

📖 **AWS docs:** [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

### Amazon SQS (Simple Queue Service)

📌 **What is it?** Fully managed **message queue** that decouples components. One part puts messages in the queue, another processes them when ready. Think of a ticket line at a counter.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Decouple** and scale components; messages are stored until processed, so nothing is lost if the consumer is busy or down.
- **Standard queues:** maximum throughput, best-effort ordering, at-least-once delivery. **FIFO queues:** messages processed exactly once, in the order sent.
- Consumers **poll (pull)** messages; pay per request.

⚠️ **Exam Trap / Common Distractor:** SQS is one-to-one queuing that you pull from. SNS is one-to-many push. Use SNS plus SQS together for fan-out.

📖 **AWS docs:** [What is Amazon SQS?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

### Amazon EventBridge

📌 **What is it?** A **serverless event bus** that routes events from AWS services, your own applications, and SaaS partners to targets such as Lambda, using rules. It was formerly called CloudWatch Events. Think of a smart switchboard that reacts to things happening.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Event-driven** architecture; **rules** decide where each event goes; supports **schedules** (cron-style) too.
- Event publisher and consumer are fully decoupled; integrates with many AWS services and SaaS apps.
- Pay per event published.

⚠️ **Exam Trap / Common Distractor:** EventBridge routes events by rules. SNS broadcasts a message to subscribers, and SQS queues messages.

📖 **AWS docs:** [What is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)

### AWS Step Functions

📌 **What is it?** A fully managed service that coordinates the steps of an application as a **visual workflow**. Think of a flowchart that runs itself.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Workflow / state machine** of steps (Lambda functions and other AWS services); handles **retries and error handling** and logs each step.
- Good for multi-step, ordered processes such as order handling or data pipelines.
- Pay based on usage (for example per state transition).

⚠️ **Exam Trap / Common Distractor:** Step Functions orchestrates a sequence of steps. SQS and SNS move messages and do not manage workflow logic.

📖 **AWS docs:** [What is AWS Step Functions?](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)

## AI and machine learning services

Most CLF-C02 questions describe a task and ask for the matching service.

| Service | What it does | Exam keyword |
|---|---|---|
| **Amazon SageMaker AI** | Build, **train**, and **deploy** your own ML models with managed infrastructure | Custom ML models, data scientists |
| **Amazon Lex** | Build **chatbots** and voice bots (speech recognition plus language understanding) | Chatbot, conversational interface |
| **Amazon Polly** | Turns **text into lifelike speech** | Text-to-speech |
| **Amazon Transcribe** | Converts **speech to text** | Speech-to-text, subtitles, call transcripts |
| **Amazon Translate** | Neural **language translation** | Translate, localise content |
| **Amazon Comprehend** | Natural language processing: finds language, key phrases, entities, and **sentiment** in text | Sentiment, text analysis |
| **Amazon Rekognition** | **Image and video analysis**: objects, people, text, faces, inappropriate content | Detect faces or objects in images |
| **Amazon Textract** | Extracts **text, forms, and tables** from scanned documents (beyond simple OCR) | Extract data from documents |
| **Amazon Q** | Generative AI assistant. **Q Business** answers questions from your company data. **Q Developer** (formerly CodeWhisperer) helps developers code, test, and troubleshoot | Generative AI assistant |

⚠️ **Exam Trap / Common Distractor:** Pre-trained AI services (Polly, Transcribe, Rekognition and others) need no ML skills. SageMaker AI is for teams building **custom** models. Amazon Bedrock appears in AWS's AI overview but is not named on the current CLF-C02 in-scope list.

📖 **AWS docs:** [ML and AI services overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/machine-learning.html) · [Choosing an AWS machine learning service](https://docs.aws.amazon.com/decision-guides/latest/machine-learning-on-aws-how-to-choose/guide.html) · [Amazon SageMaker AI](https://aws.amazon.com/sagemaker/) · [Amazon Q](https://aws.amazon.com/q/)

## Developer tools

| Service | What it does | Exam keyword | AWS docs |
|---|---|---|---|
| **AWS CodeBuild** | Fully managed **build** service: compiles code, runs tests, produces deployable packages | Build, compile, test | [Docs](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html) |
| **AWS CodePipeline** | Fully managed **continuous delivery (CI/CD)** service that automates release pipelines; integrates with GitHub and other tools | CI/CD, release pipeline | [Docs](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html) |
| **AWS X-Ray** | Helps analyse and **debug distributed applications** by tracing requests end to end | Trace, troubleshoot, microservices | [Docs](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) |

- Also remember the **AWS CLI**, **SDKs**, and **APIs** for programmatic access (see [Cloud concepts](01-cloud-concepts.md#ways-to-access-aws)), and **Amazon Q Developer** for AI coding help.

⚠️ **Exam Trap / Common Distractor:** CodeBuild builds, CodePipeline orchestrates the whole pipeline, and X-Ray traces and troubleshoots running applications.

📖 **AWS docs:** [Developer tools overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/developer-tools)

## Business applications

| Service | What it does | Exam keyword | AWS docs |
|---|---|---|---|
| **Amazon Connect** | Cloud **contact centre** with omnichannel customer service (AWS's newest docs may call it *Connect Customer*) | Contact centre, customer service calls | [Overview](https://aws.amazon.com/connect/) |
| **Amazon SES** (Simple Email Service) | Send **email** securely and at scale: transactional, marketing, and bulk | Send email | [Docs](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html) |

## End-user computing

| Service | What it does | Exam keyword |
|---|---|---|
| **Amazon WorkSpaces** | Managed **virtual desktop** (a full desktop in the cloud) | Virtual desktop, VDI |
| **Amazon AppStream 2.0** (newer docs: *WorkSpaces Applications*) | **Streams desktop applications** to a user's browser or device without installing them | Stream applications |
| **Amazon WorkSpaces Secure Browser** | Managed, isolated browser for secure access to internal websites and SaaS apps | Secure browser access, no VPN client |

Memory aid: WorkSpaces = a **desktop** in the cloud, AppStream = an **application** in the cloud, Secure Browser = a **browser** in the cloud.

📖 **AWS docs:** [End user computing overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/end-user-computing.html) · [Amazon WorkSpaces](https://aws.amazon.com/workspaces/)

## Frontend web and mobile

| Service | What it does | Exam keyword | AWS docs |
|---|---|---|---|
| **AWS Amplify** | Set of tools and services for frontend web and mobile developers to **build, host, and deploy** full-stack apps quickly | Web and mobile app, frontend hosting | [Docs](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html) |

## Internet of Things (IoT)

| Service | What it does | Exam keyword | AWS docs |
|---|---|---|---|
| **AWS IoT Core** | Lets connected devices **connect securely to the cloud** and interact with AWS services and each other; manages messaging and device communication | IoT devices, device connectivity | [Docs](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) |

## Quick chooser

| Question keyword | Answer |
|---|---|
| Metrics, alarms, dashboards, logs | CloudWatch |
| API history, identity, audit | CloudTrail |
| Configuration history, compliance rule | Config |
| Best-practice recommendations | Trusted Advisor |
| Right-size EC2, EBS, Lambda | Compute Optimizer |
| Workload pillar review | Well-Architected Tool |
| Analyse historical spend | Cost Explorer |
| Alert on forecast overspend | Budgets |
| Govern multiple accounts | Organizations |
| Secure multi-account landing zone | Control Tower |
| AWS events affecting my resources | Health Dashboard |
| Approved product catalogue | Service Catalog |
| Track software licences | License Manager |
| View or raise service quotas | Service Quotas |
| Query S3 with SQL | Athena |
| Real-time streaming data | Kinesis |
| ETL and data catalogue | Glue |
| Big data (Spark, Hadoop) | EMR |
| Search and log analytics | OpenSearch Service |
| Dashboards and BI | Amazon Quick |
| Push notifications to many subscribers | SNS |
| Message queue / decouple components | SQS |
| Route events or schedules | EventBridge |
| Coordinate workflow steps | Step Functions |
| Build, train, deploy custom ML models | SageMaker AI |
| Chatbot | Lex |
| Text to speech / speech to text / translation | Polly / Transcribe / Translate |
| Sentiment / image analysis / document extraction | Comprehend / Rekognition / Textract |
| Generative AI assistant | Amazon Q |
| Build code / CI/CD / trace requests | CodeBuild / CodePipeline / X-Ray |
| Contact centre / send email | Amazon Connect / SES |
| Virtual desktops / app streaming / secure browser | WorkSpaces / AppStream 2.0 / Secure Browser |
| Web and mobile frontend | Amplify |
| IoT device connectivity | IoT Core |
| Repeatable infrastructure template | CloudFormation |
| Fleet operations and patching | Systems Manager |


[← Security and compliance](04-security-and-compliance.md) | [Index](README.md) | [Cheat sheet →](06-cheat-sheet-exam-highlights.md)