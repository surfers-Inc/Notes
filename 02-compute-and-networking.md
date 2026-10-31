# 02 — Compute and Networking

[← Cloud concepts](01-cloud-concepts.md) | [Index](README.md) | [Storage and databases →](03-storage-and-databases.md)

## Compute decision map

| Need | Best starting answer |
|---|---|
| Full control of a virtual server | EC2 |
| Run code when an event occurs | Lambda |
| Deploy an application without managing platform setup | Elastic Beanstalk |
| Run AWS-managed containers | ECS |
| Run Kubernetes | EKS |
| Run containers without managing servers | Fargate with ECS or EKS |
| Simple bundled virtual server | Lightsail |

## Amazon EC2

📌 **What is it?** Resizable virtual computers in AWS. Think of EC2 as renting a computer by the second or hour instead of buying one.

🎯 **Need to Know for the Exam (Key Keywords)**

- **General purpose:** balanced; **compute optimized:** CPU-heavy; **memory optimized:** large memory; **storage optimized:** high local I/O; **accelerated computing:** GPU or specialised hardware.
- Choose the AMI, instance type, storage, network, and security group; use Auto Scaling for elasticity.
- **On-Demand** for flexibility, **Savings Plans/Reserved Instances** for steady use, **Spot** for interruptible work, and **Dedicated Hosts** for licensing/compliance.
- Charges depend on instance, runtime, purchase option, storage, and data transfer.

⚠️ **Exam Trap / Common Distractor:** EC2 is not serverless. The customer patches the guest OS and applications.

## EC2 Auto Scaling

📌 **What is it?** Automatically changes the number of EC2 instances to match demand. It is like opening more checkout lanes when a shop gets busy.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses minimum, desired, and maximum capacity; scaling can follow metrics or schedules.
- Improves **elasticity**, availability, and cost efficiency.
- No additional Auto Scaling fee; pay for the resources it creates.

⚠️ **Exam Trap / Common Distractor:** Auto Scaling changes capacity; Elastic Load Balancing distributes traffic.

## Elastic Load Balancing (ELB)

📌 **What is it?** Distributes incoming traffic across healthy targets. Think of a receptionist directing visitors to available staff.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Application Load Balancer:** HTTP/HTTPS and path/host routing; **Network Load Balancer:** very high performance TCP/UDP/TLS; **Gateway Load Balancer:** virtual network appliances.
- Health checks avoid sending traffic to unhealthy targets; works across multiple AZs.
- Billed for load balancer running time and capacity/traffic processed.

⚠️ **Exam Trap / Common Distractor:** A load balancer does not add instances; pair it with Auto Scaling.

## AWS Lambda

📌 **What is it?** Runs code in response to events without managing servers. It is like calling a worker only when a job arrives.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Serverless**, event-driven, automatic scaling, short-lived functions.
- Good for file processing, APIs, automation, and scheduled tasks.
- Pay for requests and execution duration; no charge while code is idle.

⚠️ **Exam Trap / Common Distractor:** Lambda has runtime limits and is not for a continuously running traditional server.

## AWS Elastic Beanstalk

📌 **What is it?** Deploys and manages web applications while AWS provisions the underlying environment. Think of handing over your code and receiving a ready-to-run stage.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Platform as a service**, easy deployment, capacity provisioning, load balancing, scaling, and health monitoring.
- Customer controls the application and can access underlying resources.
- No additional Beanstalk charge; pay for resources such as EC2 and load balancers.

⚠️ **Exam Trap / Common Distractor:** Beanstalk is not serverless and does not remove access to the infrastructure it creates.

## Amazon ECS

📌 **What is it?** AWS-native service for orchestrating Docker containers. Think of a dispatcher deciding where containerised jobs run.

🎯 **Need to Know for the Exam (Key Keywords)**

- Runs containers on EC2 instances or serverlessly with **AWS Fargate**.
- Best when AWS-native container orchestration is wanted without Kubernetes.
- ECS control plane has no extra charge; pay for EC2 or Fargate resources.

⚠️ **Exam Trap / Common Distractor:** Choose EKS—not ECS—when the requirement explicitly says Kubernetes.

## Amazon EKS

📌 **What is it?** Managed Kubernetes control plane on AWS. Think of AWS maintaining the Kubernetes management office while you run container workloads.

🎯 **Need to Know for the Exam (Key Keywords)**

- Standard **Kubernetes** APIs and ecosystem; worker compute can be EC2 or Fargate.
- Useful for Kubernetes portability and existing Kubernetes skills.
- Pay for each EKS cluster plus worker compute and related resources.

⚠️ **Exam Trap / Common Distractor:** EKS manages Kubernetes; it is not the simplest answer for generic AWS container orchestration.

## AWS Fargate

📌 **What is it?** Serverless compute engine for containers used with ECS or EKS. It is like renting container space without managing the host machines.

🎯 **Need to Know for the Exam (Key Keywords)**

- Removes EC2 server provisioning and cluster capacity management.
- Choose CPU and memory for each task or pod; it scales with workload scheduling.
- Pay for requested compute resources while workloads run.

⚠️ **Exam Trap / Common Distractor:** Fargate does not replace ECS or EKS; it supplies their serverless compute capacity.

## Amazon Lightsail

📌 **What is it?** Simplified virtual servers, storage, databases, and networking in predictable bundles. Think of a starter kit for a small website.

🎯 **Need to Know for the Exam (Key Keywords)**

- Good for small websites, blogs, prototypes, and simple business applications.
- Easier and less flexible than assembling advanced EC2 architectures.
- Fixed monthly bundles include an allowance of resources and data transfer.

⚠️ **Exam Trap / Common Distractor:** Choose EC2 and other services when fine-grained enterprise architecture or scaling is required.

## Networking foundations

### Amazon VPC

📌 **What is it?** A logically isolated virtual network for AWS resources. Think of it as your private fenced area inside AWS.

🎯 **Need to Know for the Exam (Key Keywords)**

- A VPC is **regional**; subnets belong to one AZ and are called public or private based on routing.
- **Route tables** decide where traffic goes; an **internet gateway (IGW)** connects public resources to the internet.
- VPC itself has no charge, but components such as NAT gateways, endpoints, and data transfer can cost money.

⚠️ **Exam Trap / Common Distractor:** A subnet is not public merely because it has public IPs; it needs a route to an IGW.

### Internet Gateway

📌 **What is it?** A horizontally scaled gateway that lets a VPC communicate with the internet.

🎯 **Need to Know for the Exam (Key Keywords)**

- Attach one IGW to a VPC and add a route from a public subnet.
- Resources also need public IPv4 addresses or IPv6 addresses and permitted security rules.
- No hourly IGW charge; normal data-transfer charges can apply.

⚠️ **Exam Trap / Common Distractor:** An IGW does not give private-subnet instances outbound-only IPv4 access; use a NAT gateway.

### NAT Gateway

📌 **What is it?** Lets resources in private subnets start outbound IPv4 connections without accepting unsolicited inbound internet connections.

🎯 **Need to Know for the Exam (Key Keywords)**

- Place a public NAT gateway in a public subnet and route private-subnet internet traffic to it.
- Managed and scalable; deploy per AZ for resilient designs.
- Charged per hour and per amount of data processed.

⚠️ **Exam Trap / Common Distractor:** NAT gateway is for outbound access from private subnets; an IGW supports public internet connectivity.

### Security groups and network ACLs

| Feature | Security group | Network ACL (NACL) |
|---|---|---|
| Applied to | Network interface / resource | Subnet |
| State | **Stateful** | **Stateless** |
| Rules | Allow only | Allow and deny |
| Evaluation | All rules together | Numbered rules in order |
| Return traffic | Automatically allowed | Must be explicitly allowed |

📌 **What are they?** VPC traffic filters. A security group is a resource's door guard; a NACL is the checkpoint at the subnet boundary.

🎯 **Need to Know for the Exam (Key Keywords)**

- Security groups are the primary resource-level firewall; NACLs add subnet-level defence.
- New security groups deny inbound and allow outbound by default; default/custom NACL defaults differ.
- No separate charge for security groups or NACLs.

⚠️ **Exam Trap / Common Distractor:** Security groups are stateful and cannot write explicit deny rules; NACLs are stateless and can deny.

## Amazon Route 53

📌 **What is it?** Scalable Domain Name System (DNS), domain registration, and health-check service. It is the internet's address book for your application.

🎯 **Need to Know for the Exam (Key Keywords)**

- Routes names to resources using policies such as simple, weighted, latency, failover, geolocation, and multivalue.
- Health checks can support DNS failover; Route 53 is a global service.
- Pay for hosted zones, DNS queries, health checks, and registered domains.

⚠️ **Exam Trap / Common Distractor:** Route 53 resolves names; it does not cache application content like CloudFront.

## Amazon CloudFront

📌 **What is it?** A content delivery network (CDN) that caches content at edge locations near users. Think of local shops stocking popular items from a distant warehouse.

🎯 **Need to Know for the Exam (Key Keywords)**

- Low latency, global delivery, edge caching; origins can include S3, load balancers, and web servers.
- Supports HTTPS and integrates with Shield and WAF.
- Pay mainly for data transfer out and requests; caching can reduce origin load.

⚠️ **Exam Trap / Common Distractor:** CloudFront accelerates and caches web content; Global Accelerator improves network paths for applications without caching.

## AWS Direct Connect

📌 **What is it?** A dedicated private network connection from a site to AWS. Think of a leased private road to AWS instead of the public internet.

🎯 **Need to Know for the Exam (Key Keywords)**

- Consistent network performance, private connectivity, and potentially lower high-volume transfer cost.
- Useful for hybrid environments and steady large data flows; setup is not instant.
- Pay for connection port hours and data transfer out; provider charges may apply.

⚠️ **Exam Trap / Common Distractor:** Direct Connect is not encrypted by default and is not the quick setup choice; a Site-to-Site VPN uses the internet and is faster to establish.

## AWS Global Accelerator

📌 **What is it?** Sends application traffic through the AWS global network to healthy regional endpoints using static anycast IP addresses.

🎯 **Need to Know for the Exam (Key Keywords)**

- Improves availability and performance for TCP and UDP applications.
- Routes to healthy endpoints in one or more Regions; provides fixed entry IPs.
- Charged per accelerator-hour and data transfer through the service.

⚠️ **Exam Trap / Common Distractor:** Global Accelerator does not cache content. Choose CloudFront for CDN caching and HTTP edge features.


[← Cloud concepts](01-cloud-concepts.md) | [Index](README.md) | [Storage and databases →](03-storage-and-databases.md)