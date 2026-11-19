# 02 — Compute and Networking

[← Cloud concepts](01-cloud-concepts.md) | [Index](README.md) | [Storage and databases →](03-storage-and-databases.md)

> **How to read each card:** 📘 how AWS describes it · 💡 how to remember it · 🔑 words that point to it in a question · 🎯 what the exam actually asks about it · ⚠️ the wrong answer you will be tempted by · 📖 where to read more.

## Contents

- **Compute:** [Compute chooser](#compute-chooser) · [EC2](#amazon-ec2) · [EC2 purchase options](#ec2-purchase-options) · [Auto Scaling](#amazon-ec2-auto-scaling-and-aws-auto-scaling) · [Elastic Load Balancing](#elastic-load-balancing-elb) · [Lambda](#aws-lambda) · [Elastic Beanstalk](#aws-elastic-beanstalk) · [Lightsail](#amazon-lightsail) · [Batch](#aws-batch) · [Outposts](#aws-outposts)
- **Containers:** [Containers in one picture](#containers-in-one-picture) · [ECS](#amazon-ecs) · [EKS](#amazon-eks) · [Fargate](#aws-fargate) · [ECR](#amazon-ecr)
- **Networking:** [Amazon VPC](#amazon-vpc) · [Security groups vs NACLs](#security-groups-vs-network-acls) · [VPC peering](#vpc-peering) · [Transit Gateway](#aws-transit-gateway) · [PrivateLink and VPC endpoints](#aws-privatelink-and-vpc-endpoints)
- **Connecting to AWS:** [AWS VPN](#aws-vpn-site-to-site-vpn-and-client-vpn) · [Direct Connect](#aws-direct-connect)
- **Content delivery and DNS:** [Route 53](#amazon-route-53) · [CloudFront](#amazon-cloudfront) · [Global Accelerator](#aws-global-accelerator) · [API Gateway](#amazon-api-gateway)
- [Connectivity chooser](#connectivity-chooser) · [Top traps on this page](#top-traps-on-this-page)

---

## Compute chooser

| If the question says… | Choose |
|---|---|
| Full control of a virtual server, choose the OS, install anything | [EC2](#amazon-ec2) |
| Run code only when an event happens, no servers, pay per millisecond | [Lambda](#aws-lambda) |
| Upload my web app code and let AWS build the environment | [Elastic Beanstalk](#aws-elastic-beanstalk) |
| Simple website/blog for a fixed, low monthly price | [Lightsail](#amazon-lightsail) |
| Run containers, AWS-native | [ECS](#amazon-ecs) |
| Run containers with **Kubernetes** | [EKS](#amazon-eks) |
| Run containers **without managing servers** | [Fargate](#aws-fargate) (with ECS or EKS) |
| Store container images | [ECR](#amazon-ecr) |
| Thousands of queued batch jobs | [Batch](#aws-batch) |
| AWS hardware inside my own data centre | [Outposts](#aws-outposts) |

### What "serverless" means on the exam

**Serverless** = no servers for **you** to provision, patch, or scale; it scales automatically and you pay only while it runs. In-scope serverless examples: **Lambda, Fargate, DynamoDB, S3, API Gateway, SNS, SQS, EventBridge, Step Functions, Aurora Serverless**. **NOTE** That EC2, Elastic Beanstalk, and Lightsail are **not** serverless.

📖 **Read more:** [Choosing an AWS compute service](https://docs.aws.amazon.com/decision-guides/latest/compute-on-aws-how-to-choose/choosing-aws-compute-service.html) · [Compute services overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/compute-services.html)

---

## Amazon EC2

**📘 AWS definition:** Amazon Elastic Compute Cloud (Amazon EC2) provides on-demand, scalable computing capacity in the AWS Cloud.

**💡 Simple definition:** **Renting a computer in AWS** by the second. You pick the size, the operating system, and the software; AWS owns the building and the hardware. **EC2 = Elastic Compute Cloud = virtual servers.**

**🔑 Exam keywords:** `virtual server` · `instance` · `AMI` · `instance type` · `full control of the OS` · `resizable compute` · `IaaS`

**🎯 What they test**

1. **Building blocks of an instance**
   - **AMI (Amazon Machine Image):** the template (OS + software) used to launch an instance.
   - **Instance type:** the hardware size (CPU, memory, network). In `m7g.large`: `m` = family, `7` = generation, `g` = extra attribute (Graviton processor), `large` = size.
   - **Storage:** EBS volumes (persistent) and/or instance store (temporary). See [Storage](03-storage-and-databases.md#amazon-ebs).
   - **Security group:** the instance's firewall. **Key pair:** used to log in securely. **User data:** a script that runs at first boot.
2. **Instance families** (match the workload):

   | Family | Letters | Pick for |
   |---|---|---|
   | General purpose | M, T | Balanced: web servers, small databases, dev/test |
   | Compute optimised | C | CPU-heavy: batch processing, gaming servers, HPC, media encoding |
   | Memory optimised | R, X | Large in-memory datasets, in-memory databases, real-time analytics |
   | Storage optimised | I, D, H | High, sequential local disk I/O: data warehousing, big data |
   | Accelerated computing | P, G, Inf, Trn | GPUs/ML chips: machine learning, graphics |
   | HPC optimised | Hpc | Large, tightly coupled high-performance computing |

   Memory hook: **C**ompute = **C**PU, **R** = **R**AM.
3. **Shared Responsibility for EC2 (IaaS):** **you** patch the guest OS, install and secure applications, configure security groups, and manage data. AWS secures the physical hosts, hypervisor, and network.
4. **Billing basics:** Linux instances are billed **per second** (60-second minimum). A **stopped** instance has no compute charge, but its EBS volumes and Elastic IPs still cost money.
5. **Scaling words:** **vertical scaling / scale up** = bigger instance; **horizontal scaling / scale out** = more instances (Auto Scaling).

**⚠️ Exam traps**

- EC2 is **not serverless**. "Least operational overhead" usually points away from EC2 (to Lambda, Fargate, Beanstalk, or a managed service).
- Patching the guest OS on EC2 is the **customer's** job, not AWS's.
- Stopping an instance does **not** stop EBS storage charges.

📖 **Read more:** [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) · [EC2 instance types](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html) · [Amazon Machine Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) · [EC2 pricing](https://aws.amazon.com/ec2/pricing/)

### EC2 purchase options

The exam guide names these by name ("Reserved Instances, On-Demand Instances, Spot Instances"), so expect several questions.

| Option | How it works | Discount vs On-Demand | Pick when the question says… |
|---|---|---|---|
| **On-Demand** | Pay by the second, no commitment | None | Short-term, spiky, unpredictable, **cannot be interrupted**, testing |
| **Savings Plans** | Commit to a **$/hour spend** for 1 or 3 years | Up to 72% | Steady usage but want **flexibility** (instance family, Region, even Lambda/Fargate with Compute Savings Plans) |
| **Reserved Instances (RIs)** | Commit to a specific instance configuration for **1 or 3 years** | Up to 72% | Steady, predictable, always-on workload (for example a database server) |
| **Spot Instances** | Use spare EC2 capacity; AWS can reclaim it with a **2-minute warning** | Up to 90% | **Fault-tolerant, flexible, can be interrupted**: batch jobs, big data, CI, image rendering |
| **Dedicated Hosts** | A whole **physical server** for you, with visibility of sockets/cores | — (most expensive) | **Bring your own licence (BYOL)** tied to sockets/cores, strict compliance |
| **Dedicated Instances** | Your instances on hardware not shared with other customers | — | Compliance needs single-tenant hardware but no host visibility |
| **On-Demand Capacity Reservations** | Reserve capacity in a specific AZ, any duration | None by itself (combine with Savings Plans) | Must **guarantee capacity** for a known event |

**Payment choices for RIs/Savings Plans:** All Upfront (biggest discount) > Partial Upfront > No Upfront (smallest discount). **3-year** terms save more than **1-year**.

#### Types of Reserved Instances

An RI is described by **two separate choices**. Make each choice on its own, then combine them.

**Choice 1: offering class. "Can I change what I bought?"**

| | **Standard RI** | **Convertible RI** |
|---|---|---|
| Discount | **Biggest** (up to 72%) | Smaller (up to 66%) |
| Change the instance **family**, OS, or tenancy? | **No** | **Yes**: exchange it for another Convertible RI of equal or greater value |
| What you *can* change | AZ, instance size (within the same family), network type | Almost anything, through an exchange |
| Sell unused RIs on the **RI Marketplace**? | **Yes** | **No** |
| Pick when the question says… | "Workload is stable and will not change", "maximum savings" | "Needs may change over the term", "flexibility to change instance family" |
| Memory hook | **Standard = Stuck** with the family, **Saves the most** | **Convertible = Can Change** (like a convertible car roof) |

**Choice 2: scope. "Do I also need guaranteed capacity?"**

| | **Regional RI** (the default) | **Zonal RI** |
|---|---|---|
| Discount applies to | Matching usage in **any AZ** of the Region | Matching usage in **one specific AZ** |
| **Capacity reservation** | **No** | **Yes**: capacity is held for you in that AZ |
| Instance size flexibility | **Yes** (Linux/Unix, same family: e.g. one `m7g.xlarge` RI covers two `m7g.large`) | No (exact size only) |
| Pick when the question says… | "Discount, flexible AZ and size" | "Must be **guaranteed capacity** in a specific AZ for 1–3 years" |
| Memory hook | **Regional = Roams** across AZs | **Zonal = Zone-locked, holds a seat** |

**Putting it together:**

| Scenario | Answer |
|---|---|
| Steady database server for 3 years, will never change, cheapest possible | **Standard RI**, 3-year, **All Upfront** |
| Steady usage for 3 years, but may move to a newer instance family later | **Convertible RI** (or a **Compute Savings Plan**) |
| Must be sure EC2 capacity is available in `us-east-1a` all year | **Zonal RI** (or an On-Demand Capacity Reservation plus a Savings Plan) |
| Bought too many RIs and want to get money back | Sell **Standard** RIs on the **RI Marketplace** (Convertible RIs cannot be sold) |

**RIs vs Savings Plans (both up to 72%):**

| | Commit to… | Flexibility | Covers |
|---|---|---|---|
| **Standard RI** | One instance configuration | Lowest | EC2 (also RDS, ElastiCache, Redshift, OpenSearch have their own RIs) |
| **Convertible RI** | One configuration, exchangeable | Medium | EC2 |
| **EC2 Instance Savings Plan** (up to 72%) | $/hour on one instance **family** in one **Region** | Any size, OS, tenancy, AZ in that family | EC2 |
| **Compute Savings Plan** (up to 66%) | $/hour on any compute | **Highest**: any family, Region, OS | EC2, **Fargate, Lambda** |

AWS now **recommends Savings Plans** over RIs for most new EC2 commitments because they are simpler and more flexible. RIs still appear on the exam (and are the only commitment option for services such as **RDS**). *Scheduled RIs* are no longer offered, so ignore them if they appear in old questions.

**⚠️ Exam traps**

- **Spot** is cheapest, but only when the workload **can be interrupted**. "Critical", "cannot be interrupted" → not Spot.
- **Standard** RI = biggest discount but **cannot change family**. **Convertible** = can change but a smaller discount and **cannot be sold** on the RI Marketplace.
- Only a **zonal** RI reserves **capacity**. A **regional** RI gives only the **discount**.
- **Dedicated Host** vs **Dedicated Instance**: only the **Host** gives socket/core visibility for **licensing**.
- Reserved Instances and Savings Plans are **billing discounts**, not different kinds of servers.
- Savings Plans also cover **Lambda and Fargate** (Compute Savings Plans); RIs do not.

📖 **Read more:** [EC2 purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html) · [Reserved Instances overview](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-reserved-instances.html) · [Types of Reserved Instances (offering classes)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/reserved-instances-types.html) · [Regional and zonal RIs (scope)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/reserved-instances-scope.html) · [Selling on the RI Marketplace](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-market-general.html) · [Savings Plans vs RIs](https://docs.aws.amazon.com/savingsplans/latest/userguide/sp-ris.html) · [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html) · [What are Savings Plans?](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html) · [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html)

---

## Amazon EC2 Auto Scaling and AWS Auto Scaling

**📘 AWS definition:** Amazon EC2 Auto Scaling helps you ensure that you have the correct number of EC2 instances available to handle the load for your application.

**💡 Simple definition:** **Opening more checkout lanes when the shop gets busy, and closing them when it is quiet.** It also replaces broken instances automatically.

**🔑 Exam keywords:** `elasticity` · `scale out / scale in` · `match demand` · `Auto Scaling group` · `minimum / desired / maximum` · `replace unhealthy instances`

**🎯 What they test**

- An **Auto Scaling group (ASG)** has a **minimum**, **desired**, and **maximum** number of instances, usually spread across **multiple AZs** for high availability.
- **Scaling types:**

  | Type | Example |
  |---|---|
  | **Target tracking** | "Keep average CPU at 50%" |
  | **Step / simple** | "Add 2 instances when CPU > 80%" (uses CloudWatch alarms) |
  | **Scheduled** | "Add 10 instances every Monday 08:00" |
  | **Predictive** | Uses ML on past traffic to scale **before** the load arrives |

- **Self-healing:** health checks find unhealthy instances and replace them.
- **No extra charge** for Auto Scaling; you pay for the instances it launches.
- **AWS Auto Scaling** (the separate service on the exam guide's Management list) sets up scaling plans across **several services at once**: EC2 Auto Scaling groups, ECS services, DynamoDB tables, and Aurora replicas.

**⚠️ Exam traps**

- Auto Scaling changes **how many** instances there are; **ELB** spreads traffic **across** them. They are usually used together.
- Elasticity (scale automatically with demand) is a core **cloud benefit**; Auto Scaling is how EC2 achieves it.

📖 **Read more:** [What is Amazon EC2 Auto Scaling?](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) · [Scaling policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/scale-your-group.html) · [What is AWS Auto Scaling?](https://docs.aws.amazon.com/autoscaling/plans/userguide/what-is-a-scaling-plan.html)

---

## Elastic Load Balancing (ELB)

**📘 AWS definition:** Elastic Load Balancing automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones.

**💡 Simple definition:** A **receptionist** who sends each visitor to a free, healthy staff member, so nobody is overloaded and broken desks are skipped.

**🔑 Exam keywords:** `distribute traffic` · `health checks` · `high availability` · `multiple AZs` · `layer 7 / HTTP` · `layer 4 / TCP` · `single point of entry`

**🎯 What they test: the load balancer types**

| Type | OSI layer | Traffic | Pick when the question says… |
|---|---|---|---|
| **Application Load Balancer (ALB)** | 7 (application) | HTTP/HTTPS, gRPC | Web apps, microservices, route by **URL path** or **host name** |
| **Network Load Balancer (NLB)** | 4 (transport) | TCP/UDP/TLS | **Extreme performance**, millions of requests per second, ultra-low latency, **static IP** |
| **Gateway Load Balancer (GWLB)** | 3 (network) | IP packets | Deploy and scale **third-party virtual appliances** (firewalls, intrusion detection) |
| Classic Load Balancer | 4 and 7 | — | Legacy; recognise the name only |

- **Health checks** stop traffic going to unhealthy targets.
- Can terminate **HTTPS/TLS** using certificates from **AWS Certificate Manager (ACM)**.
- Billed per hour plus capacity units used.

**⚠️ Exam traps**

- A load balancer does **not** add servers; pair it with **Auto Scaling**.
- ELB balances traffic **within a Region**. To route users to the best **Region**, think **Route 53** (latency/geolocation routing) or **Global Accelerator**.

📖 **Read more:** [What is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) · [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) · [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) · [Gateway Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/introduction.html)

---

## AWS Lambda

**📘 AWS definition:** AWS Lambda is a compute service that runs code without the need to manage servers; your code runs and scales automatically, and you pay only for the compute time you use.

**💡 Simple definition:** **Code that wakes up when something happens, does its job, and goes back to sleep.** Like a light that switches on only when someone walks in. **No server, no idle bill.**

**🔑 Exam keywords:** `serverless` · `event-driven` · `function` · `run code without servers` · `pay per request / per millisecond` · `automatic scaling`

**🎯 What they test**

- **Triggers (events):** a file uploaded to S3, an API call through API Gateway, a message in SQS/SNS, a DynamoDB Streams change, an EventBridge rule or **schedule**.
- **Limits worth knowing:** maximum run time **15 minutes** per invocation; memory from 128 MB to 10,240 MB (CPU grows with memory).
- **Pricing:** number of **requests** + **duration** (rounded to the millisecond) × memory. A permanent **free tier** includes 1 million requests per month.
- **Scales automatically** from zero to thousands of concurrent runs.
- **Shared Responsibility:** AWS runs and patches the servers, OS, and runtime; **you** are responsible for your code, its IAM permissions, and data.

**⚠️ Exam traps**

- Lambda is **not** for long-running jobs (> 15 minutes) or always-on servers. Use EC2, ECS/Fargate, or Batch.
- "Run code on a schedule without a server" → **EventBridge schedule + Lambda**, not a cron job on EC2.
- Lambda has no charge while idle; EC2 bills while running even if idle.

📖 **Read more:** [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) · [Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) · [Lambda pricing](https://aws.amazon.com/lambda/pricing/)

---

## AWS Elastic Beanstalk

**📘 AWS definition:** AWS Elastic Beanstalk lets you deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications.

**💡 Simple definition:** **"Here is my code, please run it."** Beanstalk builds the EC2 instances, load balancer, Auto Scaling, and monitoring for you. Like the magic beanstalk that grows by itself once you plant the seed (your code).

**🔑 Exam keywords:** `PaaS` · `upload code` · `developers` · `quickly deploy web applications` · `capacity provisioning, load balancing, auto scaling handled for you`

**🎯 What they test**

- **Platform as a Service (PaaS):** supports Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker.
- You keep **full control** and can still see and change the underlying resources.
- **No extra charge** for Beanstalk; you pay for the resources it creates (EC2, ELB, and so on).

**⚠️ Exam traps**

- Beanstalk is **not serverless**; it runs EC2 instances for you.
- Beanstalk deploys **applications**. **CloudFormation** deploys **any infrastructure** from a template (infrastructure as code).

📖 **Read more:** [What is AWS Elastic Beanstalk?](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)

---

## Amazon Lightsail

**📘 AWS definition:** Amazon Lightsail offers easy-to-use virtual private server instances, containers, storage, databases, and more at a cost-effective monthly price.

**💡 Simple definition:** **The starter kit.** One click gives you a server, storage, and networking for a fixed monthly price. "**Light**" = simple and lightweight.

**🔑 Exam keywords:** `simple` · `low, predictable monthly price` · `small website / blog / WordPress` · `beginners` · `virtual private server`

**🎯 What they test**

- Pre-configured **bundles** (CPU, memory, SSD, data transfer allowance) for a fixed monthly price.
- Best for small websites, blogs, simple web apps, dev/test, and users with little cloud experience.
- Includes managed databases, load balancers, and containers in simplified form.

**⚠️ Exam traps**

- Enterprise-scale, highly customised architectures → **EC2** with VPC, ELB, and Auto Scaling.

📖 **Read more:** [What is Amazon Lightsail?](https://docs.aws.amazon.com/lightsail/latest/userguide/what-is-amazon-lightsail.html) · [Lightsail pricing](https://aws.amazon.com/lightsail/pricing/)

---

## AWS Batch

**📘 AWS definition:** AWS Batch helps you run batch computing workloads on the AWS Cloud, dynamically provisioning the optimal quantity and type of compute resources based on the volume and requirements of submitted jobs.

**💡 Simple definition:** **A job queue with automatic workers.** Submit thousands of jobs; Batch hires the right number of computers, runs the jobs, and lets the workers go.

**🔑 Exam keywords:** `batch jobs` · `job queue` · `hundreds of thousands of jobs` · `simulations` · `no need to install batch software`

**🎯 What they test**

- Runs containerised jobs on **EC2, Spot Instances, or Fargate** and scales compute automatically.
- **No extra charge** for Batch; pay for the compute used. Using **Spot** makes it very cheap.

**⚠️ Exam traps**

- Short, event-driven code → **Lambda**. Long, queued, compute-heavy jobs → **Batch**. Coordinating steps of a workflow → **Step Functions**.

📖 **Read more:** [What is AWS Batch?](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html)

---

## AWS Outposts

**📘 AWS definition:** AWS Outposts is a fully managed service that extends AWS infrastructure, services, APIs, and tools to customer premises.

**💡 Simple definition:** **An AWS branch office inside your own building.** AWS delivers and maintains real AWS racks/servers in your data centre so you can use the same AWS services locally.

**🔑 Exam keywords:** `hybrid` · `on-premises` · `low latency to local systems` · `data residency` · `same AWS APIs on premises`

**🎯 What they test**

- Use when data or processing **must stay on-premises** (latency, local data processing, data residency) but you want AWS services such as EC2, EBS, ECS, EKS, and RDS.
- AWS **installs, monitors, patches, and maintains** the hardware; you provide the space, power, and network.
- Comes as Outposts **racks** (42U) or **servers** (1U/2U).

**⚠️ Exam traps**

- Outposts **runs AWS services on-premises**. Direct Connect and VPN only **connect** networks. Storage Gateway only gives on-premises access to **storage**.
- Under Shared Responsibility, **you** are responsible for the physical security of the site where the Outpost sits.

📖 **Read more:** [What is AWS Outposts?](https://docs.aws.amazon.com/outposts/latest/userguide/what-is-outposts.html) · [Outposts FAQs](https://aws.amazon.com/outposts/rack/faqs/)

---

## Containers in one picture

```
 Build image ──► store it in  ECR  ──► run it with an orchestrator ──► on compute
                                        ├─ ECS (AWS-native)              ├─ EC2 instances (you manage servers)
                                        └─ EKS (Kubernetes)              └─ Fargate (serverless, no servers)
```

| Question says | Answer |
|---|---|
| "Kubernetes", open-source, portability | **EKS** |
| "Run Docker containers", AWS-native, simplest | **ECS** |
| "Containers **without** managing servers/clusters" | **Fargate** |
| "Store/share container images" | **ECR** |

📖 **Read more:** [Choosing an AWS container service](https://docs.aws.amazon.com/decision-guides/latest/containers-on-aws-how-to-choose/choosing-aws-container-service.html)

## Amazon ECS

**📘 AWS definition:** Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerised applications.

**💡 Simple definition:** **AWS's own dispatcher for containers.** It decides where each container runs, restarts failed ones, and connects them to load balancers.

**🔑 Exam keywords:** `container orchestration` · `Docker` · `AWS-native` · `tasks and services` · `microservices`

**🎯 What they test**

- Runs containers on **EC2** (you manage the instances) or **Fargate** (serverless).
- Integrates with ELB, IAM, CloudWatch, and ECR.
- **No charge** for ECS itself; pay for the EC2 or Fargate resources.

**⚠️ Exam traps**

- Requirement mentions **Kubernetes** → **EKS**, not ECS.

📖 **Read more:** [What is Amazon ECS?](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

## Amazon EKS

**📘 AWS definition:** Amazon Elastic Kubernetes Service (Amazon EKS) is a managed service that eliminates the need to install, operate, and maintain your own Kubernetes control plane on AWS.

**💡 Simple definition:** **Kubernetes, with AWS running the hard part (the control plane).** **K** in EKS = **K**ubernetes.

**🔑 Exam keywords:** `Kubernetes` · `open source` · `portability` · `hybrid containers` · `existing Kubernetes skills`

**🎯 What they test**

- Uses standard **Kubernetes** tools and APIs, so workloads can move between on-premises and other clouds.
- Worker nodes can be **EC2** or **Fargate**.
- Pricing: a fee per cluster per hour, plus the compute.

**⚠️ Exam traps**

- EKS is not the "simplest" container option; it is the **Kubernetes** option.

📖 **Read more:** [What is Amazon EKS?](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)

## AWS Fargate

**📘 AWS definition:** AWS Fargate is a serverless, pay-as-you-go compute engine that lets you focus on building applications without managing servers, and works with both Amazon ECS and Amazon EKS.

**💡 Simple definition:** **Containers without servers.** You say "this container needs 1 vCPU and 2 GB"; Fargate finds somewhere to run it. **Far-gate = far away from servers.**

**🔑 Exam keywords:** `serverless containers` · `no EC2 to manage` · `pay for vCPU and memory` · `with ECS or EKS`

**🎯 What they test**

- Removes the need to provision, patch, and scale EC2 instances for containers.
- Pay for the vCPU and memory your containers request, while they run. Covered by Compute Savings Plans; Fargate Spot gives extra discounts.

**⚠️ Exam traps**

- Fargate is **not** an orchestrator. It is the **compute engine** used **by** ECS or EKS.

📖 **Read more:** [AWS Fargate for Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html) · [Fargate product page](https://aws.amazon.com/fargate/)

## Amazon ECR

**📘 AWS definition:** Amazon Elastic Container Registry (Amazon ECR) is an AWS managed container image registry service that is secure, scalable, and reliable.

**💡 Simple definition:** **A warehouse for container images.** **R** in ECR = **R**egistry.

**🔑 Exam keywords:** `container images` · `Docker images` · `registry` · `image scanning`

**🎯 What they test**

- Private and public repositories; access controlled with IAM; images encrypted; **image scanning** finds vulnerabilities (with Amazon Inspector).
- Used by ECS, EKS, Fargate, and Lambda (container images).
- Pay for storage and data transferred out.

**⚠️ Exam traps**

- ECR **stores** images; ECS/EKS/Fargate **run** them.

📖 **Read more:** [What is Amazon ECR?](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

---

## Amazon VPC

**📘 AWS definition:** Amazon Virtual Private Cloud (Amazon VPC) lets you launch AWS resources in a logically isolated virtual network that you've defined.

**💡 Simple definition:** **Your own private, fenced-off network inside AWS.** You decide the IP ranges, which parts face the internet, and who can come in. **VPC = your plot of land in the AWS city.**

**🔑 Exam keywords:** `logically isolated network` · `subnet` · `route table` · `internet gateway` · `NAT gateway` · `security group` · `network ACL` · `CIDR`

**🎯 What they test: the parts of a VPC**

| Part | What it does | Memory hook |
|---|---|---|
| **VPC** | The private network; belongs to **one Region**, spans all its AZs | The fenced plot |
| **Subnet** | A slice of the VPC's IP range in **one AZ** | A room in the house |
| **Public subnet** | Has a route to an **internet gateway** | Room with a front door |
| **Private subnet** | No direct route to the internet (databases, app servers) | Room with no outside door |
| **Route table** | Rules saying where traffic goes | Signposts |
| **Internet gateway (IGW)** | Lets the VPC talk to the internet, both ways | Front gate |
| **NAT gateway** | Lets **private** subnets go **out** to the internet (updates), but blocks inbound connections | One-way exit door |
| **Security group** | Firewall on each **instance/resource** | Door guard |
| **Network ACL** | Firewall on each **subnet** | Checkpoint at the room entrance |
| **VPC Flow Logs** | Capture information about IP traffic in the VPC (for troubleshooting/security) | CCTV log |
| **Elastic IP** | A fixed public IPv4 address you own until you release it | Permanent phone number |

- Typical design: web servers in **public** subnets behind a load balancer; databases in **private** subnets; at least **two AZs** for high availability.
- The VPC itself is free; NAT gateways, VPC endpoints, VPN connections, public IPv4 addresses (charged per hour since February 2024), and data transfer cost money.

**⚠️ Exam traps**

- A subnet is "public" because of its **route to an IGW**, not just because an instance has a public IP.
- Private instances needing **outbound-only** internet (for example, software updates) → **NAT gateway**, not an IGW.
- A VPC is **regional**; a subnet is **one AZ**.

📖 **Read more:** [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) · [Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) · [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) · [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) · [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)

### Security groups vs network ACLs

**📘 AWS definition:** A security group controls the traffic allowed to reach and leave the resources it is associated with; a network ACL allows or denies specific inbound or outbound traffic at the subnet level.

**💡 Simple definition:** Security group = **guard at each apartment door** who remembers who went out (so lets them back in). NACL = **guard at the building entrance** with a numbered rule list who remembers nobody.

**🔑 Exam keywords:** `stateful` · `stateless` · `allow rules only` · `explicit deny` · `instance-level firewall` · `subnet-level firewall`

**🎯 What they test**

| | Security group | Network ACL |
|---|---|---|
| Applies to | Instance / network interface | **Subnet** |
| State | **Stateful** (return traffic allowed automatically) | **Stateless** (return traffic must be allowed too) |
| Rules | **Allow only** | **Allow and deny** |
| Order | All rules evaluated together | Rules evaluated **in number order**, first match wins |
| Default | New SG: deny all inbound, allow all outbound | Default NACL: allow all in and out |

Memory hook: **NACL** can say **N**o (deny); **S**ecurity group is **S**tateful.

**⚠️ Exam traps**

- "Block one specific IP address" → **NACL** (security groups cannot deny). At scale on the web layer, **AWS WAF** can also block IPs.
- Both are free; both are the **customer's** responsibility to configure.

📖 **Read more:** [Compare security groups and network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html) · [Security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) · [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)

---

## VPC peering

**📘 AWS definition:** A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IP addresses.

**💡 Simple definition:** **A private bridge between exactly two buildings.** Bridges do not chain: A↔B and B↔C does not give A↔C.

**🔑 Exam keywords:** `connect two VPCs` · `private IP` · `cross-account` · `inter-Region` · `not transitive`

**🎯 What they test**

- Works within a Region, **across Regions**, and **across accounts**.
- **Not transitive**; IP ranges (CIDR blocks) **must not overlap**; route tables must be updated on both sides.
- No charge to create; normal data transfer charges apply.

**⚠️ Exam traps**

- Many VPCs (and on-premises) to connect → **Transit Gateway**. Share **one service** only → **PrivateLink**.

📖 **Read more:** [What is VPC peering?](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html) · [Peering limitations](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-basics.html)

---

## AWS Transit Gateway

**📘 AWS definition:** AWS Transit Gateway is a network transit hub that you can use to interconnect your VPCs and on-premises networks.

**💡 Simple definition:** **An airport hub.** Every network flies to the hub once, instead of building a direct flight between every pair.

**🔑 Exam keywords:** `hub and spoke` · `connect many VPCs` · `central hub` · `simplify network` · `on-premises + VPCs`

**🎯 What they test**

- Attach VPCs, Site-to-Site VPNs, and Direct Connect gateways to one regional hub; routing is **transitive** through the hub.
- Pay per attachment per hour plus data processed.

**⚠️ Exam traps**

- Two VPCs only → peering is simpler. Dozens of VPCs → Transit Gateway.

📖 **Read more:** [What is a transit gateway?](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html)

---

## AWS PrivateLink and VPC endpoints

**📘 AWS definition:** AWS PrivateLink provides private connectivity between VPCs, supported AWS services, and your on-premises networks without exposing your traffic to the public internet.

**💡 Simple definition:** **A private side door** from your VPC straight into one service, so traffic never walks along the public street. A **VPC endpoint** is that door.

**🔑 Exam keywords:** `private connectivity` · `without the internet` · `VPC endpoint` · `interface endpoint` · `gateway endpoint` · `SaaS privately`

**🎯 What they test**

| Endpoint type | Reaches | Cost | Memory hook |
|---|---|---|---|
| **Gateway endpoint** | **S3 and DynamoDB** only (entry in the route table) | **Free** | "Gateway = the two big free ones" |
| **Interface endpoint** (powered by **PrivateLink**) | Most AWS services, your own services, SaaS partner services | Per hour + per GB | A private network card in your subnet |

- No IGW, NAT gateway, public IP, VPN, or Direct Connect needed to reach the service.

**⚠️ Exam traps**

- PrivateLink exposes **one service**, one way. To join **whole networks**, use peering or Transit Gateway.
- "Private instance must reach S3 without the internet" → **gateway VPC endpoint** for S3 (not a NAT gateway).

📖 **Read more:** [What is AWS PrivateLink?](https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html) · [Gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)

---

## AWS VPN: Site-to-Site VPN and Client VPN

**📘 AWS definition:** AWS Virtual Private Network (AWS VPN) lets you establish secure, private tunnels from your network or device to the AWS global network; it includes AWS Site-to-Site VPN and AWS Client VPN.

**💡 Simple definition:** **An armoured tunnel through the public internet.** Site-to-Site = a tunnel for a **whole office**. Client VPN = a tunnel for **one person's laptop**.

**🔑 Exam keywords:** `encrypted` · `over the internet` · `IPsec` · `quick to set up` · `remote workers` · `customer gateway` · `virtual private gateway`

**🎯 What they test**

| | **Site-to-Site VPN** | **Client VPN** |
|---|---|---|
| Connects | An on-premises **network** ↔ VPC/Transit Gateway | Individual **users/devices** ↔ AWS and on-premises |
| Uses | **Customer gateway** (your side) + **virtual private gateway** or transit gateway (AWS side), IPsec | OpenVPN-based client software |
| Typical question | "Quickly and securely connect the data centre to AWS" | "Remote employees need secure access" |

- Encrypted, but speed depends on the internet. Sets up in **minutes**. Pay per connection-hour plus data out.
- Common pattern: **VPN as a backup** for Direct Connect.

**⚠️ Exam traps**

- VPN = **encrypted + internet + fast to set up**. Direct Connect = **private + consistent + weeks to set up**.

📖 **Read more:** [What is AWS Site-to-Site VPN?](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html) · [What is AWS Client VPN?](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html)

---

## AWS Direct Connect

**📘 AWS definition:** AWS Direct Connect links your internal network to an AWS Direct Connect location over a standard Ethernet fibre-optic cable, bypassing the public internet.

**💡 Simple definition:** **A private road from your building straight to AWS.** No traffic jams from the public internet, but it takes time to build.

**🔑 Exam keywords:** `dedicated connection` · `private` · `bypass the internet` · `consistent network performance` · `large data transfers` · `reduce data transfer cost` · `hybrid`

**🎯 What they test**

- **Dedicated** connections (1, 10, 100 Gbps) or **hosted** connections from AWS partners (smaller speeds).
- **Consistent latency and bandwidth**; can **lower data-transfer-out costs** for large, steady volumes.
- Takes **weeks** to provision (physical cabling via a partner).
- **Not encrypted by default**; add a Site-to-Site VPN over it (or MACsec on supported links) when encryption is required.
- Pay per port-hour plus data transfer out.

**⚠️ Exam traps**

- "Quickly" or "within hours" → **VPN**, not Direct Connect.
- "Private and encrypted" → **Direct Connect + VPN**.

📖 **Read more:** [What is AWS Direct Connect?](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) · [Direct Connect FAQs](https://aws.amazon.com/directconnect/faqs/)

---

## Amazon Route 53

**📘 AWS definition:** Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service, used for domain registration, DNS routing, and health checking.

**💡 Simple definition:** **The internet's phone book for your app.** It turns `example.com` into an IP address and decides **which** address to give each user. (DNS runs on **port 53**, hence the name.)

**🔑 Exam keywords:** `DNS` · `domain registration` · `routing policy` · `health checks` · `failover` · `latency-based routing` · `global service`

**🎯 What they test: routing policies**

| Policy | Sends users to… | Use case |
|---|---|---|
| **Simple** | One resource | A single web server |
| **Weighted** | Resources by percentage (e.g. 80/20) | A/B testing, gradual rollouts |
| **Latency** | The Region with **lowest latency** for the user | Global apps, best performance |
| **Failover** | Primary; backup if the health check fails | Active-passive disaster recovery |
| **Geolocation** | Based on the user's **country/continent** | Local content, legal restrictions |
| **Geoproximity** | Based on distance, with a "bias" to shift traffic | Shift traffic between Regions |
| **IP-based** | Based on the user's IP range | Route by ISP/network |
| **Multivalue answer** | Several healthy records at random | Simple client-side load spreading |

- **Health checks** monitor endpoints and drive failover.
- A **global** service, with a **100% availability** service level agreement.
- Pay per hosted zone, per query, per health check, and per domain.

**⚠️ Exam traps**

- **Latency** routing ≠ **geolocation** routing. Speed → latency; the user's country/law → geolocation.
- Route 53 **answers DNS questions**; it does not cache content (CloudFront) or carry traffic (Global Accelerator).

📖 **Read more:** [What is Amazon Route 53?](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) · [Choosing a routing policy](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html) · [Route 53 health checks](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html)

---

## Amazon CloudFront

**📘 AWS definition:** Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content to your users by delivering it through a worldwide network of data centres called edge locations.

**💡 Simple definition:** **Local corner shops stocked from a faraway warehouse.** Popular content is **cached** close to users, so pages load faster and your origin server does less work. **CDN = content delivery network.**

**🔑 Exam keywords:** `CDN` · `edge locations` · `cache` · `low latency for global users` · `static and dynamic content` · `video streaming` · `reduce load on origin`

**🎯 What they test**

- **Edge locations** (hundreds worldwide, far more than Regions) cache copies; **regional edge caches** sit between them and the origin.
- **Origins:** S3 bucket, ALB, EC2, API Gateway, or any HTTP server.
- **Security:** HTTPS with ACM certificates, **AWS Shield Standard** included automatically, integrates with **AWS WAF**, **Origin Access Control (OAC)** keeps an S3 bucket private so users must come through CloudFront, signed URLs, and **geo-restriction** (block countries).
- Can run code at the edge (CloudFront Functions, Lambda@Edge).
- Pay mainly for data transferred out and requests; data transfer from AWS origins to CloudFront is free.

**⚠️ Exam traps**

- CloudFront **caches** HTTP content. **Global Accelerator** improves the network path for TCP/UDP apps but **does not cache**.
- Faster **downloads** to global users → CloudFront. Faster **uploads** to S3 → S3 Transfer Acceleration (which itself uses edge locations).

📖 **Read more:** [What is Amazon CloudFront?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) · [Restricting access to an S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) · [CloudFront pricing](https://aws.amazon.com/cloudfront/pricing/)

---

## AWS Global Accelerator

**📘 AWS definition:** AWS Global Accelerator is a networking service that improves the availability and performance of applications by directing user traffic over the AWS global network using static IP addresses.

**💡 Simple definition:** **A VIP fast lane.** Users join the AWS private motorway at the nearest edge location instead of crawling along the public internet, using **two fixed (static) IP addresses**.

**🔑 Exam keywords:** `static IP addresses` · `anycast` · `TCP/UDP` · `gaming` · `VoIP` · `IoT` · `fast regional failover` · `AWS global network`

**🎯 What they test**

- Provides **two static anycast IPv4 addresses** as fixed entry points; routes to the nearest **healthy** endpoint (ALB, NLB, EC2, Elastic IP) in one or more Regions.
- Failover between Regions in seconds without DNS caching delays.

**⚠️ Exam traps**

- Needs **caching** → CloudFront. Needs **static IPs + non-HTTP (UDP) traffic + fast failover** → Global Accelerator.

📖 **Read more:** [What is AWS Global Accelerator?](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)

---

## Amazon API Gateway

**📘 AWS definition:** Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale.

**💡 Simple definition:** **The front door for APIs.** It receives every API call, checks who is calling, limits abuse, and passes the request to the back end (often Lambda).

**🔑 Exam keywords:** `create and publish APIs` · `REST` · `WebSocket` · `throttling` · `serverless back end` · `API keys`

**🎯 What they test**

- Classic **serverless** pattern: **API Gateway → Lambda → DynamoDB**.
- Handles authorisation (IAM, Cognito, Lambda authorisers), **throttling**, caching, and monitoring.
- Pay per API call plus data transferred out.

**⚠️ Exam traps**

- API Gateway manages **APIs**. A load balancer spreads traffic across servers; Route 53 does DNS.

📖 **Read more:** [What is Amazon API Gateway?](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

---

## Connectivity chooser

| Requirement | Choose |
|---|---|
| Public subnet reaches the internet | Internet gateway |
| Private subnet reaches the internet **outbound only** | NAT gateway |
| Private access from a VPC to S3 or DynamoDB | Gateway VPC endpoint |
| Private access to another AWS/SaaS service without the internet | PrivateLink (interface endpoint) |
| Link **two** VPCs (same or different account/Region) | VPC peering |
| Connect **many** VPCs and on-premises networks | Transit Gateway |
| Quick, encrypted link from an office or data centre | Site-to-Site VPN |
| Individual remote users | Client VPN |
| Dedicated, private, consistent link for steady high volume | Direct Connect |
| Domain names and routing users to Regions | Route 53 |
| Cache content near users | CloudFront |
| Static IPs and faster global network path, no caching | Global Accelerator |
| Publish and secure APIs | API Gateway |

## Top traps on this page

1. **Spot** = cheapest but **interruptible**; never for critical workloads.
2. **Dedicated Host** (licensing, socket visibility) ≠ **Dedicated Instance**.
3. **Auto Scaling** adds/removes instances; **ELB** spreads traffic.
4. **ALB** = HTTP/path routing; **NLB** = extreme performance/static IP; **GWLB** = firewalls/appliances.
5. **Lambda** max 15 minutes; not for long-running servers.
6. **Fargate** is compute for ECS/EKS, not an orchestrator; **Kubernetes** → EKS.
7. **Security group** = stateful, allow only; **NACL** = stateless, can deny.
8. **NAT gateway** = outbound only for private subnets.
9. **VPN** = fast to set up, over the internet; **Direct Connect** = private, weeks to set up, not encrypted by default.
10. **CloudFront** caches; **Global Accelerator** does not.
11. **Latency** routing (speed) ≠ **geolocation** routing (country).
12. **VPC peering** is not transitive; use **Transit Gateway** for many networks.

[← Cloud concepts](01-cloud-concepts.md) | [Index](README.md) | [Storage and databases →](03-storage-and-databases.md)
