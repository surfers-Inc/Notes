# 06 — Analytics, Integration, Machine Learning, and Other Categories

[← Management and governance](05-management-and-governance.md) | [Index](README.md) | [Cheat sheet →](07-cheat-sheet-exam-highlights.md)

> **How to read each card:** 📘 how AWS describes it · 💡 how to remember it · 🔑 words that point to it in a question · 🎯 what the exam actually asks about it · ⚠️ the wrong answer you will be tempted by · 📖 where to read more.
>
> Questions on these services are almost always **"match the task to the service"**. Learn the one-line job and the keyword for each; you do not need deep configuration knowledge.

## Contents

- **Analytics:** [Analytics chooser](#analytics-chooser) · [Athena](#amazon-athena) · [Glue](#aws-glue) · [EMR](#amazon-emr) · [Kinesis](#amazon-kinesis) · [OpenSearch Service](#amazon-opensearch-service) · [Amazon Quick](#amazon-quick) · Redshift → [03](03-storage-and-databases.md#amazon-redshift)
- **Application integration:** [Integration chooser](#integration-chooser) · [SNS](#amazon-sns) · [SQS](#amazon-sqs) · [EventBridge](#amazon-eventbridge) · [Step Functions](#aws-step-functions)
- **Machine learning and AI:** [AI/ML chooser](#aiml-chooser) · [SageMaker AI](#amazon-sagemaker-ai) · [Amazon Q](#amazon-q) · [Lex](#amazon-lex) · [Polly](#amazon-polly) · [Transcribe](#amazon-transcribe) · [Translate](#amazon-translate) · [Comprehend](#amazon-comprehend) · [Rekognition](#amazon-rekognition) · [Textract](#amazon-textract)
- **Developer tools:** [CodeBuild](#aws-codebuild) · [CodePipeline](#aws-codepipeline) · [X-Ray](#aws-x-ray) · AWS CLI → [05](05-management-and-governance.md#ways-to-access-aws-console-cli-sdks)
- **Business applications:** [Amazon Connect](#amazon-connect) · [Amazon SES](#amazon-ses)
- **End-user computing:** [WorkSpaces](#amazon-workspaces) · [AppStream 2.0](#amazon-appstream-20) · [WorkSpaces Secure Browser](#amazon-workspaces-secure-browser)
- **Frontend and IoT:** [Amplify](#aws-amplify) · [IoT Core](#aws-iot-core)
- [Top traps on this page](#top-traps-on-this-page)

---

## Analytics chooser

| If the question says… | Choose |
|---|---|
| Run SQL directly on files in **S3**, serverless, pay per query | [Athena](#amazon-athena) |
| **ETL** (extract, transform, load), discover schemas, **data catalogue** | [Glue](#aws-glue) |
| **Big data** frameworks: Hadoop, **Spark**, Hive | [EMR](#amazon-emr) |
| **Real-time streaming** data (clickstreams, IoT, logs) | [Kinesis](#amazon-kinesis) |
| **Search**, log analytics, full-text search | [OpenSearch Service](#amazon-opensearch-service) |
| **Dashboards**, business intelligence (BI), visualisations | [Amazon Quick](#amazon-quick) |
| **Data warehouse**, OLAP on petabytes | [Redshift](03-storage-and-databases.md#amazon-redshift) |

A typical **data lake** flow to remember: **Kinesis** (ingest stream) → **S3** (store) → **Glue** (catalogue + transform) → **Athena** / **Redshift** (query) → **Quick** (dashboards).

📖 **Read more:** [Choosing an AWS analytics service](https://docs.aws.amazon.com/decision-guides/latest/analytics-on-aws-how-to-choose/analytics-on-aws-how-to-choose.html) · [Analytics overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/analytics.html)

## Amazon Athena

**📘 AWS definition:** Amazon Athena is an interactive query service that makes it easy to analyse data directly in Amazon S3 using standard SQL.

**💡 Simple definition:** **Ask SQL questions of files sitting in S3**, with no database to load and no servers. Athena, goddess of wisdom → instant answers.

**🔑 Exam keywords:** `query S3 with SQL` · `serverless` · `ad hoc` · `pay per data scanned` · `log analysis` · `data lake`

**🎯 What they test**

- **Serverless**; supports CSV, JSON, Parquet, ORC; uses the **Glue Data Catalog** for table definitions.
- Pay per **TB scanned**; compressing, partitioning, and columnar formats (Parquet) cut cost.
- Common use: query CloudTrail, ALB, or VPC Flow Logs stored in S3, or the Cost and Usage Report.

**⚠️ Exam traps**

- One-off queries on S3 → **Athena**. Heavy, repeated BI on a permanent warehouse → **Redshift**.

📖 **Read more:** [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)

## AWS Glue

**📘 AWS definition:** AWS Glue is a serverless data integration service that makes it easy for analytics users to discover, prepare, move, and integrate data from multiple sources.

**💡 Simple definition:** **The glue between data sources**: it finds your data, catalogues it, and cleans/transforms it so other tools can use it.

**🔑 Exam keywords:** `ETL` · `extract, transform, load` · `Data Catalog` · `crawler` · `prepare data` · `serverless data integration`

**🎯 What they test**

- **Crawlers** scan data stores and fill the **Glue Data Catalog** (a central metadata store used by Athena, EMR, Redshift Spectrum).
- **Glue jobs** run serverless ETL (Spark-based); **Glue DataBrew** prepares data visually without code.

**⚠️ Exam traps**

- Glue **prepares and catalogues**; Athena **queries**; Quick **visualises**.

📖 **Read more:** [What is AWS Glue?](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)

## Amazon EMR

**📘 AWS definition:** Amazon EMR is a managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark, on AWS to process and analyse vast amounts of data.

**💡 Simple definition:** **A ready-made Hadoop/Spark cluster** for crunching huge datasets. (EMR used to stand for Elastic MapReduce.)

**🔑 Exam keywords:** `big data` · `Hadoop` · `Spark` · `Hive` · `Presto` · `petabyte-scale processing` · `managed clusters`

**🎯 What they test**

- Runs on EC2 clusters (often with **Spot** for savings), on EKS, or **EMR Serverless**.

**⚠️ Exam traps**

- The words **Hadoop / Spark** in a question point to **EMR**.

📖 **Read more:** [What is Amazon EMR?](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html)

## Amazon Kinesis

**📘 AWS definition:** Amazon Kinesis makes it easy to collect, process, and analyse real-time streaming data so you can get timely insights and react quickly to new information.

**💡 Simple definition:** **A conveyor belt for data that never stops arriving.** Kinesis → **kinetic** = in motion.

**🔑 Exam keywords:** `real-time` · `streaming data` · `clickstream` · `IoT telemetry` · `log streams` · `video streams` · `ingest`

**🎯 What they test: the family**

| Service | Job |
|---|---|
| **Kinesis Data Streams** | Capture and store streams for **real-time** processing by your own apps |
| **Amazon Data Firehose** (formerly Kinesis Data Firehose) | Easiest way to **load** streams into **S3, Redshift, OpenSearch**, Splunk; fully managed, near real time |
| **Managed Service for Apache Flink** | Analyse streams in real time with SQL/Java/Python |
| **Kinesis Video Streams** | Stream **video** from devices for analytics/ML |

**⚠️ Exam traps**

- Continuous **real-time** data → Kinesis. A **queue** of messages between app components → SQS. SQL on data **already at rest** in S3 → Athena.

📖 **Read more:** [Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) · [What is Amazon Data Firehose?](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)

## Amazon OpenSearch Service

**📘 AWS definition:** Amazon OpenSearch Service is a managed service that makes it easy to deploy, operate, and scale OpenSearch clusters in the AWS Cloud for use cases such as log analytics, real-time application monitoring, and search.

**💡 Simple definition:** **A managed search engine** for your own data and logs (the open-source successor to Elasticsearch).

**🔑 Exam keywords:** `search` · `full-text search` · `log analytics` · `Elasticsearch` · `OpenSearch Dashboards`

**🎯 What they test**

- Formerly **Amazon Elasticsearch Service**; supports OpenSearch and legacy Elasticsearch; an **OpenSearch Serverless** option exists.

**⚠️ Exam traps**

- "Search" or "Elasticsearch" → OpenSearch Service, not Athena or Redshift.

📖 **Read more:** [What is Amazon OpenSearch Service?](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/what-is.html)

## Amazon Quick

**📘 AWS definition:** Amazon Quick (which includes the business intelligence capability formerly called Amazon QuickSight) is a cloud-scale BI and analytics service for building interactive dashboards and getting insights from your data.

**💡 Simple definition:** **Charts and dashboards for business users**, in a browser, with no servers. "Quick" insight.

**🔑 Exam keywords:** `business intelligence (BI)` · `dashboards` · `visualisation` · `reports` · `ML insights` · `natural-language questions`

**🎯 What they test**

- Connects to S3 (via Athena), Redshift, RDS, and many other sources; pay per user/session; serverless.
- Older questions use the name **QuickSight**; treat it as the same answer.

**⚠️ Exam traps**

- **Visualise** → Quick. **Monitor infrastructure metrics** → CloudWatch dashboards.

📖 **Read more:** [Amazon Quick product page](https://aws.amazon.com/quicksight/) · [Quick documentation](https://docs.aws.amazon.com/quicksight/)

---

## Integration chooser

Application integration services **decouple** parts of an application so one part can fail or slow down without breaking the others. That is a core **Well-Architected reliability** idea.

| If you need… | Choose | Memory hook |
|---|---|---|
| **Push** one message to **many** subscribers (email, SMS, Lambda, queues) | [SNS](#amazon-sns) | **N**otification = loudspeaker |
| A **queue** where messages wait until a worker **pulls** them | [SQS](#amazon-sqs) | **Q**ueue = ticket line |
| **Route events** by rules from AWS services, apps, or SaaS; or run on a **schedule** | [EventBridge](#amazon-eventbridge) | Event **bridge** = smart switchboard |
| **Coordinate steps** in order with retries and branching | [Step Functions](#aws-step-functions) | A flowchart that runs itself |

**Fan-out pattern:** SNS topic → several SQS queues, so one event is processed by several independent systems.

📖 **Read more:** [SQS, SNS, or EventBridge?](https://docs.aws.amazon.com/decision-guides/latest/sns-or-sqs-or-eventbridge/sns-or-sqs-or-eventbridge.html)

## Amazon SNS

**📘 AWS definition:** Amazon Simple Notification Service (Amazon SNS) is a managed service that provides message delivery from publishers to subscribers (also known as producers and consumers).

**💡 Simple definition:** **A loudspeaker.** Publish once to a **topic**; everyone subscribed hears it immediately.

**🔑 Exam keywords:** `publish/subscribe (pub/sub)` · `topic` · `push` · `fan-out` · `email / SMS / mobile push notifications` · `alerts`

**🎯 What they test**

- **Push**-based, one-to-many. Subscribers: **SQS, Lambda, HTTP/S, email, SMS, mobile push**.
- Common pairing: a **CloudWatch alarm** or **Budgets alert** → SNS → email to the team.

**⚠️ Exam traps**

- SNS **pushes** and does not store messages for later; SQS **holds** them until pulled.

📖 **Read more:** [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

## Amazon SQS

**📘 AWS definition:** Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components.

**💡 Simple definition:** **A ticket line.** Producers drop messages in; workers take them out when ready, so nothing is lost if workers are busy.

**🔑 Exam keywords:** `queue` · `decouple` · `buffer` · `pull / poll` · `asynchronous` · `FIFO` · `at-least-once`

**🎯 What they test**

| Queue type | Ordering | Delivery | Throughput |
|---|---|---|---|
| **Standard** | Best-effort | **At least once** (occasional duplicates) | Nearly unlimited |
| **FIFO** | **Strict order** (first in, first out) | **Exactly once** processing | Lower (very high with batching) |

- Messages are kept for up to **14 days** (default 4); consumers **poll** (pull).
- Often used with Auto Scaling: scale workers based on **queue length**.

**⚠️ Exam traps**

- "Decouple" or "buffer requests so they are not lost" → SQS.
- Order must be preserved → **FIFO** queue.

📖 **Read more:** [What is Amazon SQS?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

## Amazon EventBridge

**📘 AWS definition:** Amazon EventBridge is a serverless service that uses events to connect application components together, making it easier to build scalable event-driven applications.

**💡 Simple definition:** **A smart switchboard.** "When **this** happens, send it **there**." Also a cloud alarm clock (schedules). Formerly **CloudWatch Events**.

**🔑 Exam keywords:** `event-driven` · `event bus` · `rules` · `SaaS events` · `schedule / cron` · `react to AWS service events`

**🎯 What they test**

- **Event buses** receive events from AWS services, your apps, and **SaaS partners**; **rules** match patterns and send events to targets (Lambda, SQS, SNS, Step Functions…).
- **EventBridge Scheduler** runs tasks on a schedule (for example, stop dev instances every night).

**⚠️ Exam traps**

- **Rules-based routing** of events → EventBridge. **Broadcast** a message → SNS. **Queue** work → SQS.

📖 **Read more:** [What is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)

## AWS Step Functions

**📘 AWS definition:** AWS Step Functions is a visual workflow service that helps developers use AWS services to build distributed applications, automate processes, orchestrate microservices, and create data and machine learning pipelines.

**💡 Simple definition:** **A flowchart that runs itself**: step 1, then step 2, if it fails retry, if approved go left, else go right.

**🔑 Exam keywords:** `workflow` · `orchestrate` · `state machine` · `visual workflow` · `multi-step process` · `retries and error handling`

**🎯 What they test**

- Coordinates Lambda functions and 200+ AWS services as **state machines**; built-in retries, branching, parallel steps, and waits (including human approval).

**⚠️ Exam traps**

- **Coordinating steps in order** → Step Functions. Just passing messages → SQS/SNS.

📖 **Read more:** [What is AWS Step Functions?](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)

---

## AI/ML chooser

AWS AI services come in layers. The exam mostly tests the **pre-trained AI services** (no ML skills needed) versus **SageMaker AI** (build your own models).

| The task in the question | Service | Memory hook |
|---|---|---|
| Build, train, and deploy **your own** ML models | **SageMaker AI** | The ML **workshop** |
| Generative AI **assistant** for work or coding | **Amazon Q** | **Q**uestions answered |
| **Chatbot** / voice bot | **Lex** | Lex = **lex**icon, the engine behind Alexa |
| **Text → speech** | **Polly** | Polly the parrot **talks** |
| **Speech → text** (subtitles, call transcripts) | **Transcribe** | Transcribe = write down what is said |
| **Translate** languages | **Translate** | Does what it says |
| Text **sentiment**, key phrases, entities | **Comprehend** | **Comprehend** what the text means |
| Faces, objects, scenes in **images/video** | **Rekognition** | **Recognise** what is in the picture |
| Text, forms, tables from **scanned documents** | **Textract** | **Extract text** |

📖 **Read more:** [Machine learning and AI overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/machine-learning.html) · [Choosing an AWS ML service](https://docs.aws.amazon.com/decision-guides/latest/machine-learning-on-aws-how-to-choose/guide.html)

## Amazon SageMaker AI

**📘 AWS definition:** Amazon SageMaker AI is a fully managed machine learning service that lets data scientists and developers build, train, and deploy ML models at scale.

**💡 Simple definition:** **The workshop for building your own ML models** from raw data to a live prediction endpoint.

**🔑 Exam keywords:** `build, train, deploy` · `custom ML models` · `data scientists` · `Jupyter notebooks` · `managed ML infrastructure`

**🎯 What they test**

- Covers the whole ML lifecycle: label data, build (notebooks), train, tune, deploy, and monitor. (Previously named Amazon SageMaker.)

**⚠️ Exam traps**

- If a **pre-trained** service already does the task (translate, transcribe, detect faces), choose that service, not SageMaker AI.

📖 **Read more:** [What is Amazon SageMaker AI?](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)

## Amazon Q

**📘 AWS definition:** Amazon Q is a generative AI–powered assistant that can be tailored to your business, helping you answer questions, summarise content, generate content, and complete tasks, and helping developers write, test, and debug code.

**💡 Simple definition:** **AWS's AI assistant.** Ask it **Q**uestions about your company data or your code.

**🔑 Exam keywords:** `generative AI assistant` · `answer questions from company data` · `coding assistant` · `Amazon Q Business` · `Amazon Q Developer`

**🎯 What they test**

- **Amazon Q Business:** answers questions using your company's documents and apps. **Amazon Q Developer** (formerly CodeWhisperer): code suggestions, debugging, AWS console help.

**⚠️ Exam traps**

- Amazon Q is a ready-made **assistant**. Building custom models → SageMaker AI. (Amazon Bedrock, for building gen-AI apps on foundation models, is not on the current CLF-C02 in-scope list.)

📖 **Read more:** [Amazon Q](https://aws.amazon.com/q/) · [What is Amazon Q Business?](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/what-is.html) · [What is Amazon Q Developer?](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/what-is.html)

## Amazon Lex

**📘 AWS definition:** Amazon Lex is an AWS service for building conversational interfaces into applications using voice and text.

**💡 Simple definition:** **Build a chatbot** that understands what people type or say. The same technology as Alexa.

**🔑 Exam keywords:** `chatbot` · `conversational interface` · `voice bot` · `natural language understanding`

**🎯 What they test:** chatbots for websites and contact centres (often with **Amazon Connect**).

**⚠️ Exam traps:** Lex **understands and replies**; Polly only **speaks**; Transcribe only **writes down** speech.

📖 **Read more:** [What is Amazon Lex?](https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html)

## Amazon Polly

**📘 AWS definition:** Amazon Polly is a cloud service that converts text into lifelike speech.

**💡 Simple definition:** **Polly the parrot reads text out loud.**

**🔑 Exam keywords:** `text-to-speech` · `lifelike voices` · `read articles aloud` · `accessibility`

**🎯 What they test:** turn written content into audio (many languages and voices).

**⚠️ Exam traps:** text → speech = **Polly**; speech → text = **Transcribe**.

📖 **Read more:** [What is Amazon Polly?](https://docs.aws.amazon.com/polly/latest/dg/what-is.html)

## Amazon Transcribe

**📘 AWS definition:** Amazon Transcribe is an automatic speech recognition service that uses machine learning models to convert audio to text.

**💡 Simple definition:** **A stenographer**: writes down what was said.

**🔑 Exam keywords:** `speech-to-text` · `subtitles / captions` · `call transcripts` · `medical transcription`

**🎯 What they test:** captions for videos, transcripts of customer calls.

**⚠️ Exam traps:** do not confuse with **Translate** (language to language).

📖 **Read more:** [What is Amazon Transcribe?](https://docs.aws.amazon.com/transcribe/latest/dg/what-is.html)

## Amazon Translate

**📘 AWS definition:** Amazon Translate is a text translation service that uses advanced machine learning technologies to provide high-quality translation on demand.

**💡 Simple definition:** **A neural translator** for text.

**🔑 Exam keywords:** `translate` · `localise content` · `multilingual` · `real-time translation`

**🎯 What they test:** localise websites/apps, translate user content.

**⚠️ Exam traps:** spoken audio first needs **Transcribe**, then **Translate**, then (optionally) **Polly**.

📖 **Read more:** [What is Amazon Translate?](https://docs.aws.amazon.com/translate/latest/dg/what-is.html)

## Amazon Comprehend

**📘 AWS definition:** Amazon Comprehend uses natural language processing (NLP) to extract insights about the content of documents, such as entities, key phrases, language, and sentiment.

**💡 Simple definition:** **Reads text and tells you what it means and how people feel.**

**🔑 Exam keywords:** `natural language processing (NLP)` · `sentiment analysis` · `key phrases` · `entities` · `customer reviews / social media`

**🎯 What they test:** "Is this review positive or negative?" → Comprehend.

**⚠️ Exam traps:** Comprehend **analyses** text; Textract **extracts** text from images/scans.

📖 **Read more:** [What is Amazon Comprehend?](https://docs.aws.amazon.com/comprehend/latest/dg/what-is.html)

## Amazon Rekognition

**📘 AWS definition:** Amazon Rekognition offers pre-trained and customisable computer vision capabilities to extract information and insights from your images and videos.

**💡 Simple definition:** **Eyes for your app**: recognises faces, objects, text, and unsafe content in pictures and video.

**🔑 Exam keywords:** `image analysis` · `video analysis` · `facial recognition / comparison` · `object detection` · `content moderation` · `celebrity recognition`

**🎯 What they test:** identity verification by face, moderating uploaded images, finding objects in video.

**⚠️ Exam traps:** extracting **form fields/tables from documents** → Textract, not Rekognition.

📖 **Read more:** [What is Amazon Rekognition?](https://docs.aws.amazon.com/rekognition/latest/dg/what-is.html)

## Amazon Textract

**📘 AWS definition:** Amazon Textract is a machine learning service that automatically extracts text, handwriting, and data from scanned documents, going beyond simple optical character recognition (OCR) to identify forms and tables.

**💡 Simple definition:** **Turns a scanned form into structured data.**

**🔑 Exam keywords:** `extract text from documents` · `forms` · `tables` · `OCR` · `invoices / receipts / ID documents`

**🎯 What they test:** process invoices, tax forms, and IDs automatically.

**⚠️ Exam traps:** Textract **extracts** text; Comprehend **understands** text.

📖 **Read more:** [What is Amazon Textract?](https://docs.aws.amazon.com/textract/latest/dg/what-is.html)

---

## Developer tools

A CI/CD pipeline in one line: **code** → **build and test (CodeBuild)** → **deploy**, all orchestrated by **CodePipeline**; **X-Ray** then traces the running app.

## AWS CodeBuild

**📘 AWS definition:** AWS CodeBuild is a fully managed build service in the cloud that compiles your source code, runs unit tests, and produces artifacts that are ready to deploy.

**💡 Simple definition:** **The kitchen that cooks your code**: compile, test, package. No build servers to run.

**🔑 Exam keywords:** `compile` · `build` · `run tests` · `continuous integration` · `pay per build minute`

**🎯 What they test:** fully managed and scales automatically; pay per minute of build time.

**⚠️ Exam traps:** CodeBuild **builds**; CodePipeline **orchestrates** the whole release.

📖 **Read more:** [What is AWS CodeBuild?](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html)

## AWS CodePipeline

**📘 AWS definition:** AWS CodePipeline is a continuous delivery service you can use to model, visualise, and automate the steps required to release your software.

**💡 Simple definition:** **The assembly line** that moves code automatically from commit to production.

**🔑 Exam keywords:** `CI/CD` · `continuous delivery` · `release pipeline` · `automate releases`

**🎯 What they test:** stages (source → build → test → deploy) run automatically on every change; integrates with GitHub, CodeBuild, CloudFormation, Elastic Beanstalk, and others.

**⚠️ Exam traps:** "Automate the **whole** release process" → CodePipeline, not CodeBuild alone.

📖 **Read more:** [What is AWS CodePipeline?](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)

## AWS X-Ray

**📘 AWS definition:** AWS X-Ray is a service that collects data about requests that your application serves and provides tools to view, filter, and gain insights into that data to identify issues and opportunities for optimisation.

**💡 Simple definition:** **An X-ray of a request** as it travels through every microservice, showing where it slows down or breaks.

**🔑 Exam keywords:** `distributed tracing` · `debug microservices` · `trace requests` · `bottlenecks` · `service map`

**🎯 What they test:** find which component in a distributed/serverless app is slow or failing.

**⚠️ Exam traps:** X-Ray **traces requests**; CloudWatch shows **metrics/logs**; CloudTrail logs **API calls**.

📖 **Read more:** [What is AWS X-Ray?](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html)

---

## Amazon Connect

**📘 AWS definition:** Amazon Connect is an AI-powered cloud contact centre that helps companies of any size deliver customer service at lower cost.

**💡 Simple definition:** **A call centre in the cloud**: phone, chat, and tasks, set up in minutes.

**🔑 Exam keywords:** `contact centre` · `call centre` · `customer service` · `omnichannel` · `IVR`

**🎯 What they test:** pay-per-use contact centre, often paired with **Lex** (chatbots) and **Transcribe/Comprehend** (call analytics). Newer AWS pages may call it *Connect Customer*.

**⚠️ Exam traps:** sending **email** campaigns → SES, not Connect.

📖 **Read more:** [What is Amazon Connect?](https://docs.aws.amazon.com/connect/latest/adminguide/what-is-amazon-connect.html)

## Amazon SES

**📘 AWS definition:** Amazon Simple Email Service (Amazon SES) is an email platform that provides an easy, cost-effective way for you to send and receive email using your own email addresses and domains.

**💡 Simple definition:** **A bulk post office for email.**

**🔑 Exam keywords:** `send email` · `transactional email` · `marketing / bulk email` · `email at scale`

**🎯 What they test:** order confirmations, password resets, newsletters.

**⚠️ Exam traps:** **SES** = email to customers at scale. **SNS** = notifications to subscribers (can include email, but for alerts, not marketing).

📖 **Read more:** [What is Amazon SES?](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html)

---

## Amazon WorkSpaces

**📘 AWS definition:** Amazon WorkSpaces is a fully managed virtual desktop infrastructure (VDI) service that lets you provision Windows or Linux desktops for your users.

**💡 Simple definition:** **A whole desktop computer in the cloud**, reachable from any device.

**🔑 Exam keywords:** `virtual desktop` · `VDI` · `remote workers` · `desktop as a service` · `BYOD`

**🎯 What they test:** replace office PCs for remote/contract staff; data stays in AWS, not on the device; monthly or hourly billing.

**⚠️ Exam traps:** full **desktop** → WorkSpaces; a single **application** → AppStream 2.0.

📖 **Read more:** [What is Amazon WorkSpaces?](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces.html)

## Amazon AppStream 2.0

**📘 AWS definition:** Amazon AppStream 2.0 is a fully managed application streaming service that provides users with instant access to their desktop applications from anywhere.

**💡 Simple definition:** **Stream one application** to a browser, like streaming a film instead of downloading it. (Newer AWS pages call it **Amazon WorkSpaces Applications**.)

**🔑 Exam keywords:** `stream applications` · `no installation` · `browser access to desktop apps` · `software trials / training labs`

**🎯 What they test:** deliver a specific desktop app (CAD, analytics tool) to users without installing it on their devices.

**⚠️ Exam traps:** app → AppStream; desktop → WorkSpaces; browser → Secure Browser.

📖 **Read more:** [What is Amazon AppStream 2.0?](https://docs.aws.amazon.com/appstream2/latest/developerguide/what-is-appstream.html)

## Amazon WorkSpaces Secure Browser

**📘 AWS definition:** Amazon WorkSpaces Secure Browser is a low-cost, fully managed service that provides secure access to internal websites and SaaS applications from existing web browsers, without the cost and complexity of dedicated client software.

**💡 Simple definition:** **A safe, disposable web browser in the cloud** for reaching internal sites; nothing is stored on the user's device.

**🔑 Exam keywords:** `secure browser` · `internal websites / SaaS` · `no VPN client` · `contractors` · `data stays off the device`

**🎯 What they test:** give contractors or remote staff access to internal web apps without a VPN or managed laptop.

**⚠️ Exam traps:** only **web** apps; for a full desktop use WorkSpaces.

📖 **Read more:** [What is Amazon WorkSpaces Secure Browser?](https://docs.aws.amazon.com/workspaces-web/latest/adminguide/what-is-workspaces-web.html)

---

## AWS Amplify

**📘 AWS definition:** AWS Amplify is a set of purpose-built tools and features that lets frontend web and mobile developers quickly and easily build full-stack applications on AWS.

**💡 Simple definition:** **The fast lane for web and mobile developers**: back end, authentication, and hosting with a few commands.

**🔑 Exam keywords:** `frontend web and mobile` · `full-stack app` · `host a web app` · `React / Angular / Vue / iOS / Android`

**🎯 What they test:** build and host web/mobile apps quickly with built-in auth (Cognito), data, storage, and CI/CD hosting.

**⚠️ Exam traps:** a simple static site can also be **S3 + CloudFront**; "developers want a complete frontend/mobile toolkit" → Amplify.

📖 **Read more:** [What is AWS Amplify Hosting?](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html)

## AWS IoT Core

**📘 AWS definition:** AWS IoT Core provides the services that connect your IoT devices to the AWS Cloud so that other cloud services and applications can interact with your internet-connected devices.

**💡 Simple definition:** **The meeting point where billions of sensors and gadgets talk to the cloud.**

**🔑 Exam keywords:** `IoT devices` · `connect devices securely` · `MQTT` · `sensors` · `device messages`

**🎯 What they test:** securely connect devices, receive their messages, and route them to services (Lambda, Kinesis, S3, DynamoDB) with rules.

**⚠️ Exam traps:** IoT Greengrass and IoT Device Defender are on the **out-of-scope** list; IoT Core is the one to know.

📖 **Read more:** [What is AWS IoT Core?](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html)

---

## Top traps on this page

1. Athena = SQL on **S3**; Redshift = **warehouse**; EMR = **Hadoop/Spark**; Glue = **ETL + catalogue**; Quick = **dashboards**.
2. Kinesis = **real-time streams**; SQS = **queue**.
3. SNS **pushes** to many; SQS is **pulled** by workers; EventBridge **routes by rules**; Step Functions **orders steps**.
4. Need strict order in a queue → **SQS FIFO**.
5. Pre-trained AI service first; **SageMaker AI** only for **custom** models.
6. Polly (text→speech) vs Transcribe (speech→text) vs Translate (language→language).
7. Comprehend (**understands** text) vs Textract (**extracts** text) vs Rekognition (**images/video**).
8. CodeBuild **builds**, CodePipeline **releases**, X-Ray **traces**.
9. WorkSpaces = **desktop**; AppStream = **app**; Secure Browser = **browser**.
10. SES = **email at scale**; Connect = **contact centre**.

[← Management and governance](05-management-and-governance.md) | [Index](README.md) | [Cheat sheet →](07-cheat-sheet-exam-highlights.md)
