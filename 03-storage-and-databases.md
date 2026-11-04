# 03 — Storage and Databases

[← Compute and networking](02-compute-and-networking.md) | [Index](README.md) | [Security and compliance →](04-security-and-compliance.md)

## Storage decision map

| Need | Choose |
|---|---|
| Store objects, backups, media, or static website files | S3 |
| Persistent block disk for EC2 | EBS |
| Temporary, very fast disk on the EC2 host | Instance store |
| Shared file system for Linux workloads | EFS |
| Managed Windows, HPC, NetApp, or OpenZFS file systems | FSx |
| On-premises apps using cloud-backed storage with a local cache | Storage Gateway |
| Long-term S3 archive | S3 Glacier storage classes |
| Central backup policies | AWS Backup |
| Fast recovery of servers after a disaster | AWS Elastic Disaster Recovery |
| Move data between on-premises and AWS | DataSync online (Snow devices are legacy, see below) |

📖 **AWS docs:** [AWS storage services overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/storage-services.html)

## Amazon S3

📌 **What is it?** Highly durable object storage for virtually any amount of data. Think of a huge digital file cabinet accessed by a key rather than mounted as a normal disk.

🎯 **Need to Know for the Exam (Key Keywords)**

- Stores **objects** in globally unique-named **buckets**; supports versioning, encryption, access policies, replication, and static website hosting.
- **Strong read-after-write consistency**; designed for 11 nines of durability across multiple AZs.
- Pay for storage, requests, retrieval for some classes, and data transfer; lifecycle rules move or expire objects automatically.

⚠️ **Exam Trap / Common Distractor:** S3 is object storage, not an EC2 boot disk or shared POSIX file system.

📖 **AWS docs:** [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) · [S3 pricing](https://aws.amazon.com/s3/pricing/)

### S3 storage classes

| Class | Best fit | Retrieval / availability idea |
|---|---|---|
| S3 Standard | Frequent access | Millisecond access, multi-AZ |
| S3 Intelligent-Tiering | Unknown/changing access | Automatically moves objects between access tiers |
| S3 Standard-IA | Infrequent but rapid access | Retrieval fee, multi-AZ |
| S3 One Zone-IA | Re-creatable infrequent data | One AZ, lower cost |
| S3 Glacier Instant Retrieval | Rare archive needing milliseconds | Retrieval fee and minimum duration |
| S3 Glacier Flexible Retrieval | Archive restored in minutes to hours | Lower storage cost |
| S3 Glacier Deep Archive | Lowest-cost long retention | Restore in hours |

📖 **AWS docs:** [Amazon S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html) · [S3 lifecycle management](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)

## S3 Glacier storage classes

📌 **What is it?** Low-cost archival tiers within S3 for rarely accessed data. Think of off-site boxes that take longer or cost more to retrieve.

🎯 **Need to Know for the Exam (Key Keywords)**

- Instant Retrieval, Flexible Retrieval, and Deep Archive trade retrieval speed for storage cost.
- Use S3 lifecycle policies to archive data automatically; useful for compliance and long retention.
- Retrieval charges and minimum storage durations apply (90 days for Instant and Flexible Retrieval, 180 days for Deep Archive); Deep Archive is generally the lowest-cost and slowest.

⚠️ **Exam Trap / Common Distractor:** Glacier tiers are S3 storage classes, not a block or file storage service.

📖 **AWS docs:** [Amazon S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)

## Amazon EBS

📌 **What is it?** Persistent block storage volumes for EC2. Think of a detachable virtual hard drive.

🎯 **Need to Know for the Exam (Key Keywords)**

- An EBS volume exists in **one AZ** and normally attaches to EC2 in that AZ; snapshots are stored durably using S3 infrastructure.
- Data **persists** when the instance stops; volumes can be encrypted and backed up with snapshots.
- Good for boot volumes, databases, and workloads needing low-latency block storage.
- Pay for provisioned storage and, depending on type, performance; snapshots incur storage charges.

⚠️ **Exam Trap / Common Distractor:** EBS is not a multi-AZ shared file system. Use EFS when many Linux instances need the same files.

- Also used as a storage for AWS RDS databases.
- **Amazon Data Lifecycle Manager** - is a service designed to automate the creation, retention and deletion of Amazon EBS snapshots and EBS-backed AMI's.

📖 **AWS docs:** [What is Amazon EBS?](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html) **Amazon Data Lifecycle Manager** [what is AWS DlM?](https://docs.aws.amazon.com/ebs/latest/userguide/snapshot-lifecycle.html)

## EC2 instance store

📌 **What is it?** Temporary block storage on disks physically attached to the host computer running your EC2 instance. Think of scratch paper on the desk, not a filing cabinet.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Ephemeral / temporary:** data lasts only for the life of the instance. It is lost if the instance is stopped, hibernated, or terminated (or the disk fails), but survives a reboot.
- Very fast, low latency; included in the price of supported instance types.
- Best for caches, buffers, and scratch data that can be recreated.

⚠️ **Exam Trap / Common Distractor:** Instance store is not persistent and cannot be detached and reattached like EBS. Use EBS for data you must keep.

📖 **AWS docs:** [EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) · [Instance store vs EBS (Knowledge Center)](https://www.repost.aws/knowledge-center/instance-store-vs-ebs)

## Amazon EFS

📌 **What is it?** Managed, elastic shared file storage for Linux workloads. Think of a network drive that many computers can open together.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses **NFS**, grows and shrinks automatically, and can mount to multiple compute instances.
- Regional EFS stores data across multiple AZs; One Zone storage classes trade resilience for cost.
- Pay for storage used, with storage classes and throughput options affecting price.

⚠️ **Exam Trap / Common Distractor:** EFS is file storage; EBS is block storage and S3 is object storage.

📖 **AWS docs:** [What is Amazon EFS?](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)

## Amazon FSx

📌 **What is it?** Fully managed file systems built on popular third-party technologies. Think of choosing a ready-made file server instead of building one.

🎯 **Need to Know for the Exam (Key Keywords)**

- Four options: **FSx for Windows File Server** (Windows apps, SMB), **FSx for Lustre** (high-performance computing and machine learning), **FSx for NetApp ONTAP**, and **FSx for OpenZFS**.
- Fully managed: AWS handles hardware, patching, and backups.
- Pay for storage capacity, throughput, and backups.

⚠️ **Exam Trap / Common Distractor:** For plain Linux shared files, EFS is the simpler answer. Choose FSx when the question says Windows file shares, SMB, NetApp, or HPC.

📖 **AWS docs:** [Amazon FSx](https://aws.amazon.com/fsx) · [FSx documentation](https://docs.aws.amazon.com/fsx/)

## AWS Storage Gateway

📌 **What is it?** A hybrid cloud storage service that gives on-premises applications access to cloud storage, with a local cache for speed. Think of a local counter that quietly stores your files in the cloud warehouse.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Hybrid:** on-premises applications use standard protocols while data lives in AWS.
- Four gateway types: **S3 File Gateway** (NFS/SMB to S3), **FSx File Gateway** (SMB to FSx for Windows), **Volume Gateway** (iSCSI block volumes backed by S3, with EBS snapshots), **Tape Gateway** (virtual tape library for backup apps).
- Caches frequently used data locally for low latency.

⚠️ **Exam Trap / Common Distractor:** Storage Gateway bridges on-premises and AWS storage. It is not a general file system in AWS (EFS/FSx) and not a bulk migration tool (DataSync).

📖 **AWS docs:** [AWS Storage Gateway details](https://aws.amazon.com/storagegateway/details) · [Storage Gateway FAQs](https://aws.amazon.com/storagegateway/faqs/)

## AWS Backup

📌 **What is it?** Central service for automating and governing backups across supported AWS services. Think of one backup timetable and vault for many systems.

🎯 **Need to Know for the Exam (Key Keywords)**

- Backup plans define schedules, retention, lifecycle, and supported cross-Region or cross-account copies.
- Centralises compliance monitoring and backup vault access policies.
- Pay for backup storage, restores, copies, and data transfer where applicable.

⚠️ **Exam Trap / Common Distractor:** AWS Backup coordinates backups; it is not the same as disaster-recovery replication or S3 versioning.

📖 **AWS docs:** [What is AWS Backup?](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html)

## AWS Elastic Disaster Recovery

📌 **What is it?** Recovers on-premises and cloud-based servers quickly after a failure, using low-cost continuous replication into AWS. Think of a standby office that is ready to switch on.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Disaster recovery (DR)** with minimal downtime and data loss; supports point-in-time recovery.
- Replicates your source servers to a low-cost **staging area** in your chosen AWS Region, then launches full recovery instances when needed.
- Pay for replicating servers and the small staging resources; recovery resources cost money only when launched.

⚠️ **Exam Trap / Common Distractor:** AWS Backup stores periodic backups. Elastic Disaster Recovery keeps servers continuously replicated for fast failover.

📖 **AWS docs:** [AWS Elastic Disaster Recovery](https://aws.amazon.com/disaster-recovery/) · [Storage services overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/storage-services.html)

## AWS Snow Family (legacy)

📌 **What is it?** Rugged physical devices that were used to move large amounts of data to AWS offline, and sometimes run edge compute. Think of shipping a tamper-resistant storage box to AWS.

🎯 **Need to Know for the Exam (Key Keywords)**

- The idea to remember: **offline/physical** data transfer for huge datasets, poor connectivity, or remote sites.
- **Snowcone** is discontinued. **Snowball Edge** is no longer available to new customers, and AWS support for Snowball devices in commercial Regions ends on **31 December 2026**.
- AWS now recommends **AWS DataSync** for online transfers, **AWS Data Transfer Terminal** for secure physical transfers, and **AWS Outposts** for edge computing.

⚠️ **Exam Trap / Common Distractor:** Snow Family is not on the current CLF-C02 in-scope service list. Older study material still features it, so know the concept but do not spend long on it. DataSync moves data online over a network.

📖 **AWS docs:** [Snowball Edge availability change](https://docs.aws.amazon.com/snowball/latest/developer-guide/snowball-edge-availability-change.html) · [AWS Snow end-of-support notice](https://aws.amazon.com/snow/)

## Database decision map

| Need | Choose |
|---|---|
| Managed relational engine | RDS |
| AWS-built high-performance relational database | Aurora |
| Serverless key-value/document database | DynamoDB |
| MongoDB-compatible document database | DocumentDB |
| Graph database for highly connected data | Neptune |
| Petabyte-scale analytics warehouse | Redshift |
| In-memory cache | ElastiCache |
| Migrate databases with minimal downtime | DMS |
| Convert a database schema to another engine | SCT |

### Managed database vs database on EC2

| | Managed service (RDS, Aurora, DynamoDB) | Database you install on EC2 |
|---|---|---|
| Patching, backups, failover | AWS handles much of it | You handle it |
| Operating system access | No | Yes (full control) |
| Best when | You want less administration | You need OS-level control or unsupported features |

📖 **AWS docs:** [Choosing an AWS NoSQL database](https://docs.aws.amazon.com/whitepapers/latest/choosing-an-aws-nosql-database/amazon-documentdb.html)

## Amazon RDS

📌 **What is it?** Managed relational database service for familiar SQL engines. Think of AWS maintaining the database machinery while you manage schemas and data.

🎯 **Need to Know for the Exam (Key Keywords)**

- Supports engines including PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, and Db2; automates backups, patching, and infrastructure management.
- **Multi-AZ** improves availability; **read replicas** improve read scaling.
- Pay for database instance/serverless capacity, storage, backups beyond allowances, and transfer.

⚠️ **Exam Trap / Common Distractor:** Multi-AZ is primarily disaster recovery/high availability, not read scaling. Read replicas are for reads.

📖 **AWS docs:** [What is Amazon RDS?](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)

## Amazon Aurora

📌 **What is it?** AWS-built relational database compatible with MySQL and PostgreSQL. Think of a cloud-designed performance engine that still speaks familiar SQL.

🎯 **Need to Know for the Exam (Key Keywords)**

- Managed through RDS; distributed, fault-tolerant storage across multiple AZs.
- Higher performance and availability than standard MySQL/PostgreSQL options for many workloads; Aurora Serverless adjusts capacity.
- Pay for instances or capacity, storage, I/O under applicable configurations, backups, and transfer.

⚠️ **Exam Trap / Common Distractor:** Aurora is relational and MySQL/PostgreSQL-compatible; it is not a NoSQL database.

📖 **AWS docs:** [What is Amazon Aurora?](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

## Amazon DynamoDB

📌 **What is it?** Fully managed serverless key-value and document database with single-digit millisecond performance at scale. Think of a giant, instantly scalable lookup table.

🎯 **Need to Know for the Exam (Key Keywords)**

- **NoSQL**, automatic scaling, multi-AZ resilience, global tables, streams, backup, and point-in-time recovery.
- Best for high-scale applications needing predictable low latency and no server management.
- On-demand or provisioned capacity pricing; also pay for storage and optional features.

⚠️ **Exam Trap / Common Distractor:** Choose RDS/Aurora for relational joins and traditional SQL schemas; choose DynamoDB for key-based NoSQL access.

📖 **AWS docs:** [What is Amazon DynamoDB?](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

## Amazon DocumentDB

📌 **What is it?** A fully managed document database that is compatible with the MongoDB API. Think of a managed home for JSON-style documents that MongoDB apps already understand.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Document database**, **MongoDB-compatible**: existing MongoDB drivers and tools work with little or no change.
- Fully managed, scalable, and highly available; AWS DMS can migrate MongoDB data with minimal downtime.
- Pay for instances, storage, I/O, and backups.

⚠️ **Exam Trap / Common Distractor:** DocumentDB stores documents and is MongoDB-compatible. DynamoDB is key-value/document with its own API, and Neptune is for graphs.

📖 **AWS docs:** [Amazon DocumentDB Developer Guide](https://docs.aws.amazon.com/documentdb/latest/developerguide/) · [DocumentDB overview](https://docs.aws.amazon.com/whitepapers/latest/choosing-an-aws-nosql-database/amazon-documentdb.html)

## Amazon Neptune

📌 **What is it?** A fast, fully managed **graph database** for data where the relationships matter. Think of a map of people, places, and links between them.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Graph**: highly connected data such as social networks, fraud detection, recommendations, and knowledge graphs.
- Supports graph query languages (such as Gremlin, openCypher, and SPARQL).
- Managed, highly available, with backups and replicas; pay for instances, storage, and I/O.

⚠️ **Exam Trap / Common Distractor:** If the question stresses relationships between items, choose Neptune, not RDS (tables) or DynamoDB (key lookups).

📖 **AWS docs:** [What is Amazon Neptune?](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html)

## Amazon Redshift

📌 **What is it?** Managed cloud data warehouse for large-scale SQL analytics. Think of a warehouse built to analyse many pallets of historical data, not serve one item at a time.

🎯 **Need to Know for the Exam (Key Keywords)**

- **OLAP**, columnar storage, business intelligence, complex analytics, and petabyte-scale datasets.
- Redshift Serverless removes cluster management; integrates with analytics and data-lake tools.
- Pay for provisioned nodes or serverless processing plus storage and transfer.

⚠️ **Exam Trap / Common Distractor:** Redshift is for analytics, not a transactional OLTP replacement for RDS.

📖 **AWS docs:** [What is Amazon Redshift?](https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html)

## Amazon ElastiCache

📌 **What is it?** Managed in-memory cache using Valkey, Redis OSS, or Memcached. Think of keeping frequently used answers on the desk instead of fetching them from a filing room.

🎯 **Need to Know for the Exam (Key Keywords)**

- Reduces database load and provides microsecond-to-millisecond access for caching and session data.
- Data is held in memory; architecture must account for cache loss or replication as appropriate.
- Pay for cache nodes or serverless data/compute usage and related transfer.

⚠️ **Exam Trap / Common Distractor:** A cache speeds access but is not normally the durable system of record.

📖 **AWS docs:** [What is Amazon ElastiCache?](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/WhatIs.html)

## AWS Database Migration Service (AWS DMS)

📌 **What is it?** Moves databases to AWS, between AWS services, or between supported engines with minimal downtime. Think of moving house while daily life continues.

🎯 **Need to Know for the Exam (Key Keywords)**

- Supports **homogeneous** migrations (same engine) and **heterogeneous** migrations (different engines).
- Uses ongoing change replication to reduce downtime; AWS Schema Conversion Tool can help convert unlike schemas.
- Pay for replication capacity, storage, and data transfer as applicable.

⚠️ **Exam Trap / Common Distractor:** DMS moves data; schema conversion may require AWS SCT or other conversion work.

📖 **AWS docs:** [What is AWS DMS?](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html)

## AWS Schema Conversion Tool (AWS SCT)

📌 **What is it?** A tool that converts a source database schema and code to work with a different target engine. Think of translating a book into another language before moving it to a new library.

🎯 **Need to Know for the Exam (Key Keywords)**

- Used for **heterogeneous** migrations (for example Oracle to PostgreSQL), usually together with DMS.
- Converts schema and database code objects and flags items that need manual work.

⚠️ **Exam Trap / Common Distractor:** SCT converts the schema; DMS moves the data.

📖 **AWS docs:** [What is AWS SCT?](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html)

## Fast comparison

| Service | Data model | Main exam phrase |
|---|---|---|
| RDS | Relational | Managed SQL database |
| Aurora | Relational | AWS-built MySQL/PostgreSQL-compatible |
| DynamoDB | Key-value/document | Serverless NoSQL at scale |
| DocumentDB | Document | MongoDB-compatible |
| Neptune | Graph | Highly connected data / relationships |
| Redshift | Columnar relational analytics | Data warehouse / OLAP |
| ElastiCache | In-memory key-value/cache | Reduce latency and database load |


[← Compute and networking](02-compute-and-networking.md) | [Index](README.md) | [Security and compliance →](04-security-and-compliance.md)
