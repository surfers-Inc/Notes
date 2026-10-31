# 03 — Storage and Databases

[← Compute and networking](02-compute-and-networking.md) | [Index](README.md) | [Security and compliance →](04-security-and-compliance.md)

## Storage decision map

| Need | Choose |
|---|---|
| Store objects, backups, media, or static website files | S3 |
| Persistent block disk for EC2 | EBS |
| Shared file system for Linux workloads | EFS |
| Long-term S3 archive | S3 Glacier storage classes |
| Central backup policies | AWS Backup |
| Move data where networks are slow or unavailable | Snow Family |

## Amazon S3

📌 **What is it?** Highly durable object storage for virtually any amount of data. Think of a huge digital file cabinet accessed by a key rather than mounted as a normal disk.

🎯 **Need to Know for the Exam (Key Keywords)**

- Stores **objects** in globally unique-named **buckets**; supports versioning, encryption, access policies, replication, and static website hosting.
- **Strong read-after-write consistency**; designed for 11 nines of durability across multiple AZs.
- Pay for storage, requests, retrieval for some classes, and data transfer; lifecycle rules move or expire objects automatically.

⚠️ **Exam Trap / Common Distractor:** S3 is object storage, not an EC2 boot disk or shared POSIX file system.

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

## S3 Glacier storage classes

📌 **What is it?** Low-cost archival tiers within S3 for rarely accessed data. Think of off-site boxes that take longer or cost more to retrieve.

🎯 **Need to Know for the Exam (Key Keywords)**

- Instant Retrieval, Flexible Retrieval, and Deep Archive trade retrieval speed for storage cost.
- Use S3 lifecycle policies to archive data automatically; useful for compliance and long retention.
- Retrieval charges and minimum storage durations apply; Deep Archive is generally the lowest-cost and slowest.

⚠️ **Exam Trap / Common Distractor:** Glacier tiers are S3 storage classes, not a block or file storage service.

## Amazon EBS

📌 **What is it?** Persistent block storage volumes for EC2. Think of a detachable virtual hard drive.

🎯 **Need to Know for the Exam (Key Keywords)**

- An EBS volume exists in **one AZ** and normally attaches to EC2 in that AZ; snapshots are stored durably using S3 infrastructure.
- Good for boot volumes, databases, and workloads needing low-latency block storage.
- Pay for provisioned storage and, depending on type, performance; snapshots incur storage charges.

⚠️ **Exam Trap / Common Distractor:** EBS is not a multi-AZ shared file system. Use EFS when many Linux instances need the same files.

## Amazon EFS

📌 **What is it?** Managed, elastic shared file storage for Linux workloads. Think of a network drive that many computers can open together.

🎯 **Need to Know for the Exam (Key Keywords)**

- Uses **NFS**, grows and shrinks automatically, and can mount to multiple compute instances.
- Regional EFS stores data across multiple AZs; One Zone storage classes trade resilience for cost.
- Pay for storage used, with storage classes and throughput options affecting price.

⚠️ **Exam Trap / Common Distractor:** EFS is file storage; EBS is block storage and S3 is object storage.

## AWS Backup

📌 **What is it?** Central service for automating and governing backups across supported AWS services. Think of one backup timetable and vault for many systems.

🎯 **Need to Know for the Exam (Key Keywords)**

- Backup plans define schedules, retention, lifecycle, and supported cross-Region or cross-account copies.
- Centralises compliance monitoring and backup vault access policies.
- Pay for backup storage, restores, copies, and data transfer where applicable.

⚠️ **Exam Trap / Common Distractor:** AWS Backup coordinates backups; it is not the same as disaster-recovery replication or S3 versioning.

## AWS Snow Family: Snowcone and Snowball Edge

📌 **What is it?** Secure physical devices for moving data and sometimes running edge compute where network transfer is impractical. Think of shipping a tamper-resistant storage box to AWS.

🎯 **Need to Know for the Exam (Key Keywords)**

- **Snowcone:** smallest, portable edge/data-transfer device; **Snowball Edge:** larger capacity with storage and compute options.
- Use for offline migration, limited connectivity, remote locations, and large datasets.
- Job/device service fees, shipping, and extra-day charges can apply.

⚠️ **Exam Trap / Common Distractor:** Snow devices move or process data physically; DataSync transfers data online over a network.

## Database decision map

| Need | Choose |
|---|---|
| Managed relational engine | RDS |
| AWS-built high-performance relational database | Aurora |
| Serverless key-value/document database | DynamoDB |
| Petabyte-scale analytics warehouse | Redshift |
| In-memory cache | ElastiCache |
| Migrate databases with minimal downtime | DMS |

## Amazon RDS

📌 **What is it?** Managed relational database service for familiar SQL engines. Think of AWS maintaining the database machinery while you manage schemas and data.

🎯 **Need to Know for the Exam (Key Keywords)**

- Supports engines including PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, and Db2; automates backups, patching, and infrastructure management.
- **Multi-AZ** improves availability; **read replicas** improve read scaling.
- Pay for database instance/serverless capacity, storage, backups beyond allowances, and transfer.

⚠️ **Exam Trap / Common Distractor:** Multi-AZ is primarily disaster recovery/high availability, not read scaling. Read replicas are for reads.

## Amazon Aurora

📌 **What is it?** AWS-built relational database compatible with MySQL and PostgreSQL. Think of a cloud-designed performance engine that still speaks familiar SQL.

🎯 **Need to Know for the Exam (Key Keywords)**

- Managed through RDS; distributed, fault-tolerant storage across multiple AZs.
- Higher performance and availability than standard MySQL/PostgreSQL options for many workloads; Aurora Serverless adjusts capacity.
- Pay for instances or capacity, storage, I/O under applicable configurations, backups, and transfer.

⚠️ **Exam Trap / Common Distractor:** Aurora is relational and MySQL/PostgreSQL-compatible; it is not a NoSQL database.

## Amazon DynamoDB

📌 **What is it?** Fully managed serverless key-value and document database with single-digit millisecond performance at scale. Think of a giant, instantly scalable lookup table.

🎯 **Need to Know for the Exam (Key Keywords)**

- **NoSQL**, automatic scaling, multi-AZ resilience, global tables, streams, backup, and point-in-time recovery.
- Best for high-scale applications needing predictable low latency and no server management.
- On-demand or provisioned capacity pricing; also pay for storage and optional features.

⚠️ **Exam Trap / Common Distractor:** Choose RDS/Aurora for relational joins and traditional SQL schemas; choose DynamoDB for key-based NoSQL access.

## Amazon Redshift

📌 **What is it?** Managed cloud data warehouse for large-scale SQL analytics. Think of a warehouse built to analyse many pallets of historical data, not serve one item at a time.

🎯 **Need to Know for the Exam (Key Keywords)**

- **OLAP**, columnar storage, business intelligence, complex analytics, and petabyte-scale datasets.
- Redshift Serverless removes cluster management; integrates with analytics and data-lake tools.
- Pay for provisioned nodes or serverless processing plus storage and transfer.

⚠️ **Exam Trap / Common Distractor:** Redshift is for analytics, not a transactional OLTP replacement for RDS.

## Amazon ElastiCache

📌 **What is it?** Managed in-memory cache using Valkey, Redis OSS, or Memcached. Think of keeping frequently used answers on the desk instead of fetching them from a filing room.

🎯 **Need to Know for the Exam (Key Keywords)**

- Reduces database load and provides microsecond-to-millisecond access for caching and session data.
- Data is held in memory; architecture must account for cache loss or replication as appropriate.
- Pay for cache nodes or serverless data/compute usage and related transfer.

⚠️ **Exam Trap / Common Distractor:** A cache speeds access but is not normally the durable system of record.

## AWS Database Migration Service (AWS DMS)

📌 **What is it?** Moves databases to AWS, between AWS services, or between supported engines with minimal downtime. Think of moving house while daily life continues.

🎯 **Need to Know for the Exam (Key Keywords)**

- Supports **homogeneous** migrations and **heterogeneous** migrations.
- Uses ongoing change replication to reduce downtime; AWS Schema Conversion Tool can help convert unlike schemas.
- Pay for replication capacity, storage, and data transfer as applicable.

⚠️ **Exam Trap / Common Distractor:** DMS moves data; schema conversion may require AWS SCT or other conversion work.

## Fast comparison

| Service | Data model | Main exam phrase |
|---|---|---|
| RDS | Relational | Managed SQL database |
| Aurora | Relational | AWS-built MySQL/PostgreSQL-compatible |
| DynamoDB | Key-value/document | Serverless NoSQL at scale |
| Redshift | Columnar relational analytics | Data warehouse / OLAP |
| ElastiCache | In-memory key-value/cache | Reduce latency and database load |


[← Compute and networking](02-compute-and-networking.md) | [Index](README.md) | [Security and compliance →](04-security-and-compliance.md)