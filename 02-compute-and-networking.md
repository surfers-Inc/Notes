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
| Store container images | ECR |
| Run batch jobs at scale | Batch |
| Run AWS infrastructure in your own data centre | Outposts |
| Simple bundled virtual server | Lightsail |

📖 **AWS docs:** [Choosing an AWS compute service](https://docs.aws.amazon.com/decision-guides/latest/compute-on-aws-how-to-choose/choosing-aws-compute-service.html)

## Amazon EC2

📌 **What is it?** Resizable virtual computers in AWS. Think of EC2 as renting a computer by the second or hour instead of buying one.

🎯 **Need to Know for the Exam (Key Keywords)**

- **General purpose:** balanced; **compute optimized:** CPU-heavy; **memory optimized:** large memory; **storage optimized:** high local I/O; **accelerated computing:** GPU or specialised hardware; **HPC optimized:** high-performance computing.
- Choose the AMI, instance type, storage, network, and security group; use Auto Scaling for elasticity.
- **On-Demand** for flexibility, **Savings Plans/Reserved Instances** for steady use, **Spot** for interruptible work, and **Dedicated Hosts** for licensing/compliance. See [Cloud concepts](01-cloud-concepts.md#ec2-purchasing-models-at-a-glance).
- Charges depend on instance, runtime, purchase option, storage, and data transfer.

⚠️ **Exam Trap / Common Distractor:** EC2 is not serverless. The customer patches the guest OS and applications.

📖 **AWS docs:** [EC2 instance types](https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-type-specifications.html) · [EC2 purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html) · [EC2 pricing](https://aws.amazon.com/ec2/pricing/)

## EC2 Auto Scaling

📌 **What is it?** Automatically changes the number of EC2 instances to match demand. It is like opening more checkout lanes when a shop gets busy.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses minimum, desired, and maximum capacity; scaling can follow metrics or schedules.
- Improves **elasticity**, availability, and cost efficiency.
- No additional Auto Scaling fee; pay for the resources it creates.

⚠️ **Exam Trap / Common Distractor:** Auto Scaling changes capacity; Elastic Load Balancing distributes traffic.

📖 **AWS docs:** [What is Amazon EC2 Auto Scaling?](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)

## Elastic Load Balancing (ELB)

📌 **What is it?** Distributes incoming traffic across healthy targets. Think of a receptionist directing visitors to available staff.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Application Load Balancer (ALB):** layer 7, HTTP/HTTPS, path/host routing. **Network Load Balancer (NLB):** layer 4, very high performance TCP/UDP/TLS. **Gateway Load Balancer (GWLB):** layer 3, virtual network appliances such as firewalls.
- Health checks avoid sending traffic to unhealthy targets; works across multiple AZs.
- Billed for load balancer running time and capacity/traffic processed.

⚠️ **Exam Trap / Common Distractor:** A load balancer does not add instances; pair it with Auto Scaling.

📖 **AWS docs:** [Elastic Load Balancing User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/) · [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html)

## AWS Lambda

📌 **What is it?** Runs code in response to events without managing servers. It is like calling a worker only when a job arrives.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Serverless**, event-driven, automatic scaling, short-lived functions (up to 15 minutes per run).
- Good for file processing, APIs, automation, and scheduled tasks.
- Pay for requests and execution duration; no charge while code is idle.

⚠️ **Exam Trap / Common Distractor:** Lambda has runtime limits and is not for a continuously running traditional server.

📖 **AWS docs:** [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

## AWS Elastic Beanstalk

📌 **What is it?** Deploys and manages web applications while AWS provisions the underlying environment. Think of handing over your code and receiving a ready-to-run stage.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Platform as a service**, easy deployment, capacity provisioning, load balancing, scaling, and health monitoring.
- Customer controls the application and can access underlying resources.
- No additional Beanstalk charge; pay for resources such as EC2 and load balancers.

⚠️ **Exam Trap / Common Distractor:** Beanstalk is not serverless and does not remove access to the infrastructure it creates.

📖 **AWS docs:** [What is Elastic Beanstalk?](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)

## Amazon ECS

📌 **What is it?** AWS-native service for orchestrating Docker containers. Think of a dispatcher deciding where containerised jobs run.

🎯 **Need to Know for the Exam (Key Keywords)**

- Runs containers on EC2 instances or serverlessly with **AWS Fargate**.
- Best when AWS-native container orchestration is wanted without Kubernetes.
- ECS control plane has no extra charge; pay for EC2 or Fargate resources.

⚠️ **Exam Trap / Common Distractor:** Choose EKS, not ECS, when the requirement explicitly says Kubernetes.

📖 **AWS docs:** [What is Amazon ECS?](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

## Amazon EKS

📌 **What is it?** Managed Kubernetes control plane on AWS. Think of AWS maintaining the Kubernetes management office while you run container workloads.

🎯 **Need to Know for the Exam (Key Keywords)**

- Standard **Kubernetes** APIs and ecosystem; worker compute can be EC2 or Fargate.
- Useful for Kubernetes portability and existing Kubernetes skills.
- Pay for each EKS cluster plus worker compute and related resources.

⚠️ **Exam Trap / Common Distractor:** EKS manages Kubernetes; it is not the simplest answer for generic AWS container orchestration.

📖 **AWS docs:** [What is Amazon EKS?](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)

## AWS Fargate

📌 **What is it?** Serverless compute engine for containers used with ECS or EKS. It is like renting container space without managing the host machines.

🎯 **Need to Know for the Exam (Key Keywords)**

- Removes EC2 server provisioning and cluster capacity management.
- Choose CPU and memory for each task or pod; it scales with workload scheduling.
- Pay for requested compute resources while workloads run.

⚠️ **Exam Trap / Common Distractor:** Fargate does not replace ECS or EKS; it supplies their serverless compute capacity.

📖 **AWS docs:** [AWS Fargate for Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)

## Amazon ECR

📌 **What is it?** A fully managed registry for storing, managing, and deploying container images. Think of a secure warehouse for container "boxes".

🎯 **Need to Know for the Exam (Key Keywords)**

- Stores **Docker/container images** used by ECS, EKS, Fargate, and Lambda.
- Access is controlled with IAM; images can be scanned and encrypted.
- Pay for image storage and data transferred out.

⚠️ **Exam Trap / Common Distractor:** ECR only stores images; ECS, EKS, or Fargate actually run the containers.

📖 **AWS docs:** [What is Amazon ECR?](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

## AWS Batch

📌 **What is it?** Fully managed service that plans, schedules, and runs batch computing jobs at any scale. Think of a job scheduler that automatically hires the right number of workers.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Batch jobs**: many similar jobs run without manual intervention (simulations, analytics, ML, media processing).
- Runs containerised jobs on EC2, Spot, or Fargate and scales the compute automatically.
- No extra charge for Batch itself; pay for the compute and storage the jobs use.

⚠️ **Exam Trap / Common Distractor:** Lambda is for short, event-driven functions; Batch is for larger, queued jobs. Step Functions coordinates workflow steps.

📖 **AWS docs:** [AWS Batch User Guide](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html)

## AWS Outposts

📌 **What is it?** Fully managed AWS infrastructure installed in your own data centre or site. It extends AWS services, APIs, and tools to your premises, like an AWS branch office on your site.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Hybrid** cloud, **low latency**, local data processing, or data that must stay on-premises.
- AWS delivers, installs, and maintains the hardware; you use the same AWS APIs and tools.
- Pay for the Outposts capacity you choose.

⚠️ **Exam Trap / Common Distractor:** Outposts runs AWS services on-premises. Direct Connect and VPN only connect networks, and Snow Family moves data offline.

📖 **AWS docs:** [What is AWS Outposts?](https://docs.aws.amazon.com/outposts/latest/userguide/what-is-outposts.html)

## Amazon Lightsail

📌 **What is it?** Simplified virtual servers, storage, databases, and networking in predictable bundles. Think of a starter kit for a small website.

🎯 **Need to Know for the Exam (Key Keywords)**

- Good for small websites, blogs, prototypes, and simple business applications.
- Easier and less flexible than assembling advanced EC2 architectures.
- Fixed monthly bundles include an allowance of resources and data transfer.

⚠️ **Exam Trap / Common Distractor:** Choose EC2 and other services when fine-grained enterprise architecture or scaling is required.

📖 **AWS docs:** [What is Amazon Lightsail?](https://docs.aws.amazon.com/lightsail/latest/userguide/what-is-amazon-lightsail.html)

## Networking foundations

### Amazon VPC

📌 **What is it?** A logically isolated virtual network for AWS resources. Think of it as your private fenced area inside AWS.

🎯 **Need to Know for the Exam (Key Keywords)**

- A VPC is **regional**; subnets belong to one AZ and are called public or private based on routing.
- **Route tables** decide where traffic goes; an **internet gateway (IGW)** connects public resources to the internet.
- VPC itself has no charge, but components such as NAT gateways, endpoints, and data transfer can cost money.

⚠️ **Exam Trap / Common Distractor:** A subnet is not public merely because it has public IPs; it needs a route to an IGW.

📖 **AWS docs:** [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

### Internet Gateway

📌 **What is it?** A horizontally scaled gateway that lets a VPC communicate with the internet.

🎯 **Need to Know for the Exam (Key Keywords)**

- Attach one IGW to a VPC and add a route from a public subnet.
- Resources also need public IPv4 addresses or IPv6 addresses and permitted security rules.
- No hourly IGW charge, but **public IPv4 addresses are charged per hour** (since February 2024), plus normal data-transfer charges.

⚠️ **Exam Trap / Common Distractor:** An IGW does not give private-subnet instances outbound-only IPv4 access; use a NAT gateway.

📖 **AWS docs:** [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) · [Public IPv4 address charge (AWS News Blog)](https://aws.amazon.com/blogs/aws/new-aws-public-ipv4-address-charge-public-ip-insights/)

### NAT Gateway

📌 **What is it?** Lets resources in private subnets start outbound IPv4 connections without accepting unsolicited inbound internet connections.

🎯 **Need to Know for the Exam (Key Keywords)**

- Place a public NAT gateway in a public subnet and route private-subnet internet traffic to it.
- Managed and scalable; deploy per AZ for resilient designs.
- Charged per hour and per amount of data processed.

⚠️ **Exam Trap / Common Distractor:** NAT gateway is for outbound access from private subnets; an IGW supports public internet connectivity.

📖 **AWS docs:** [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)

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

📖 **AWS docs:** [Compare security groups and network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html) · [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)

## Amazon Route 53

📌 **What is it?** Scalable Domain Name System (DNS), domain registration, and health-check service. It is the internet's address book for your application.

🎯 **Need to Know for the Exam (Key Keywords)**

- Routes names to resources using policies: **simple, weighted, latency, failover, geolocation, geoproximity, IP-based, and multivalue answer**.
- Health checks can support DNS failover; Route 53 is a global service.
- Pay for hosted zones, DNS queries, health checks, and registered domains.

⚠️ **Exam Trap / Common Distractor:** Route 53 resolves names; it does not cache application content like CloudFront.

📖 **AWS docs:** [What is Route 53?](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) · [Choosing a routing policy](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html)

## Amazon CloudFront

📌 **What is it?** A content delivery network (CDN) that caches content at edge locations near users. Think of local shops stocking popular items from a distant warehouse.

🎯 **Need to Know for the Exam (Key Keywords)**

- Low latency, global delivery, edge caching; origins can include S3, load balancers, and web servers.
- Supports HTTPS and integrates with Shield and WAF.
- Pay mainly for data transfer out and requests; caching can reduce origin load.

⚠️ **Exam Trap / Common Distractor:** CloudFront accelerates and caches web content; Global Accelerator improves network paths for applications without caching.

📖 **AWS docs:** [What is Amazon CloudFront?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

## AWS Direct Connect

📌 **What is it?** A dedicated private network connection from a site to AWS. Think of a leased private road to AWS instead of the public internet.

🎯 **Need to Know for the Exam (Key Keywords)**

- Consistent network performance, private connectivity, and potentially lower high-volume transfer cost.
- Useful for hybrid environments and steady large data flows; setup is not instant.
- Pay for connection port hours and data transfer out; provider charges may apply.

⚠️ **Exam Trap / Common Distractor:** Direct Connect is not encrypted by default and is not the quick setup choice; a Site-to-Site VPN uses the internet and is faster to establish.

📖 **AWS docs:** [What is AWS Direct Connect?](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)

## AWS Global Accelerator

📌 **What is it?** Sends application traffic through the AWS global network to healthy regional endpoints using static anycast IP addresses.

🎯 **Need to Know for the Exam (Key Keywords)**

- Improves availability and performance for TCP and UDP applications.
- Gives you **two static IP addresses** as fixed entry points and routes to healthy endpoints in one or more Regions.
- Charged per accelerator-hour and data transfer through the service.

⚠️ **Exam Trap / Common Distractor:** Global Accelerator does not cache content. Choose CloudFront for CDN caching and HTTP edge features.

📖 **AWS docs:** [What is AWS Global Accelerator?](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)

## Amazon API Gateway

📌 **What is it?** A fully managed service to create, publish, maintain, monitor, and secure APIs at any scale. Think of a front door that receives API calls and passes them to your back end.

🎯 **Need to Know for the Exam (Key Keywords)**

- Supports **REST**, **HTTP**, and **WebSocket** APIs; handles authorisation, throttling, and monitoring.
- Common serverless pair: API Gateway in front of **Lambda**.
- Pay for API calls received and data transferred out.

⚠️ **Exam Trap / Common Distractor:** API Gateway manages APIs. Route 53 handles DNS, and a load balancer spreads traffic across servers.

📖 **AWS docs:** [What is Amazon API Gateway?](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

## AWS PrivateLink

📌 **What is it?** Private connectivity from your VPC to services (AWS services, services in other VPCs, or SaaS) without using the internet. Think of a private door to one specific room, not the whole building.

🎯 **Need to Know for the Exam (Key Keywords)**

- No internet gateway, NAT device, public IP, Direct Connect, or VPN is needed to reach the service.
- Uses **interface endpoints**; traffic stays on the AWS network. Gateway endpoints (S3, DynamoDB) are a separate, free type.
- Interface endpoints are charged per hour and per GB processed.

⚠️ **Exam Trap / Common Distractor:** PrivateLink exposes one service one-way. VPC peering or Transit Gateway connect whole networks.

📖 **AWS docs:** [What is AWS PrivateLink?](https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html)

## AWS Transit Gateway

📌 **What is it?** A central hub that connects many VPCs and on-premises networks. Think of a cloud router or airport hub that everything connects through.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Hub-and-spoke**: each network connects once to the hub, avoiding a tangle of one-to-one connections.
- Attachments can be VPCs, VPNs, or Direct Connect.
- Pay per attachment per hour plus data processed.

⚠️ **Exam Trap / Common Distractor:** Transit Gateway links many networks. PrivateLink shares a single service.

📖 **AWS docs:** [What is AWS Transit Gateway?](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html)



## AWS Site-to-Site VPN

📌 **What is it?** An encrypted connection over the public internet between your network and your VPC. Think of an armoured tunnel through a public road.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses a **customer gateway** (your side) and a **virtual private gateway** or transit gateway (AWS side).
- Fast to set up; traffic is encrypted but performance depends on the internet.
- Pay per VPN connection-hour plus data transfer out.

⚠️ **Exam Trap / Common Distractor:** Site-to-Site VPN is encrypted but uses the internet. Direct Connect is dedicated and private but slower to set up.

📖 **AWS docs:** [What is AWS Site-to-Site VPN?](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)

## AWS Client VPN

📌 **What is it?** A managed client-based VPN that lets individual users securely reach AWS and on-premises resources from anywhere. Think of a personal tunnel for each remote worker.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Remote users** connect with an OpenVPN-based client; traffic is encrypted.
- Managed and scalable; no VPN hardware to run.
- Pay for endpoint association and active client connections.

⚠️ **Exam Trap / Common Distractor:** Client VPN is for individual users. Site-to-Site VPN connects whole networks.

📖 **AWS docs:** [What is AWS Client VPN?](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html)

## Connectivity chooser

| Requirement | Choose |
|---|---|
| Public subnet reaches the internet | Internet gateway |
| Private subnet reaches the internet outbound only | NAT gateway |
| Private access to one service without the internet | PrivateLink |
| Connect many VPCs and on-premises networks | Transit Gateway |
| Quick encrypted link from an office or data centre | Site-to-Site VPN |
| Dedicated private link, steady high volume | Direct Connect |
| Individual remote users | Client VPN |
| Name-to-address routing | Route 53 |
| Cache content near users | CloudFront |
| Static IPs and faster network path | Global Accelerator |


[← Cloud concepts](01-cloud-concepts.md) | [Index](README.md) | [Storage and databases →](03-storage-and-databases.md)