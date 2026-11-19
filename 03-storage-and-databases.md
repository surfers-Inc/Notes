# 03 — Storage and Databases

[← Compute and networking](02-compute-and-networking.md) | [Index](README.md) | [Security and compliance →](04-security-and-compliance.md)

> **How to read each card:** 📘 how AWS describes it · 💡 how to remember it · 🔑 words that point to it in a question · 🎯 what the exam actually asks about it · ⚠️ the wrong answer you will be tempted by · 📖 where to read more.

## Contents

- **Storage:** [Storage types in 60 seconds](#storage-types-in-60-seconds) · [S3](#amazon-s3) · [S3 storage classes](#s3-storage-classes-and-s3-glacier) · [EBS](#amazon-ebs) · [Instance store](#ec2-instance-store) · [EFS](#amazon-efs) · [FSx](#amazon-fsx) · [Storage Gateway](#aws-storage-gateway)
- **Backup and recovery:** [AWS Backup](#aws-backup) · [Elastic Disaster Recovery](#aws-elastic-disaster-recovery) · [DR strategies](#disaster-recovery-strategies-the-big-picture)
- **Databases:** [Database chooser](#database-chooser) · [RDS](#amazon-rds) · [Aurora](#amazon-aurora) · [DynamoDB](#amazon-dynamodb) · [DocumentDB](#amazon-documentdb) · [Neptune](#amazon-neptune) · [ElastiCache](#amazon-elasticache) · [Redshift](#amazon-redshift)
- **Moving data and databases:** [DMS](#aws-database-migration-service-aws-dms) · [SCT](#aws-schema-conversion-tool-aws-sct) · [Moving large data sets](#moving-large-data-sets-into-aws)
- [Final comparison tables](#final-comparison-tables)

---

## Storage types in 60 seconds

Almost every storage question is really asking **"which of the three storage types is this?"** Decide that first, then pick the service.

| Type | How data is stored | Analogy | AWS services | Question words |
|---|---|---|---|---|
| **Object** | Whole files ("objects") with metadata, reached by a URL/key over the internet | A coat check: hand in a coat, get a ticket, ask for it back by ticket | **S3**, S3 Glacier classes | images, videos, backups, static website, data lake, "unlimited", "any amount" |
| **Block** | Raw disk split into blocks; the operating system formats it | The hard drive inside your laptop | **EBS**, instance store | boot volume, database disk, "attached to an EC2 instance", low latency |
| **File** | Folders and files shared over a network protocol (NFS/SMB) | A shared office network drive | **EFS** (Linux/NFS), **FSx** (Windows/SMB and others) | shared, many instances at once, mount, home directories, lift-and-shift file server |

| Need | Choose |
|---|---|
| Store objects, backups, media, or static website files | [S3](#amazon-s3) |
| Long-term, low-cost archive | [S3 Glacier storage classes](#s3-storage-classes-and-s3-glacier) |
| Persistent disk for one EC2 instance | [EBS](#amazon-ebs) |
| Very fast temporary scratch disk | [Instance store](#ec2-instance-store) |
| Shared file system for many **Linux** servers | [EFS](#amazon-efs) |
| Shared file system for **Windows** (SMB, Active Directory), NetApp, or OpenZFS | [FSx](#amazon-fsx) |
| On-premises apps using cloud storage through a local cache | [Storage Gateway](#aws-storage-gateway) |
| One place to schedule and govern backups for many services | [AWS Backup](#aws-backup) |
| Fail over whole servers into AWS within minutes | [Elastic Disaster Recovery](#aws-elastic-disaster-recovery) |

📖 **Read more:** [Choosing an AWS storage service](https://docs.aws.amazon.com/decision-guides/latest/storage-on-aws-how-to-choose/choosing-aws-storage-service.html) · [Storage services overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/storage-services.html)

---

## Amazon S3

**📘 AWS definition:** Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance for storing and retrieving any amount of data from anywhere.

**💡 Simple definition:** An unlimited online "bucket" where you drop files (objects) and get them back by name. Think **S3 = Simple Storage for Stuff**.

**🔑 Exam keywords:** `object storage` · `bucket` · `11 nines durability` · `static website` · `data lake` · `versioning` · `lifecycle policy` · `unlimited storage`

**🎯 What they test**

1. **Basic structure**
   - Data is stored as **objects** (file + metadata) inside **buckets**. A single object can be up to **5 TB**.
   - Bucket names are **globally unique** (across all AWS accounts), but each bucket lives in **one Region** that you choose.
   - Storage is effectively **unlimited**; you never provision capacity, you **pay for what you store**, plus requests, retrievals (some classes), and data transferred **out**. Data transferred **in** to S3 from the internet is free.
2. **Durability vs availability** (a favourite question)
   - **Durability** = will my data survive? S3 is designed for **99.999999999 % (11 nines)** for every storage class.
   - **Availability** = can I reach it right now? This **varies by class** (S3 Standard is designed for 99.99 %; One Zone-IA for 99.5 %).
   - Most classes store data across **at least three Availability Zones**; **One Zone** classes use just one.
3. **Protecting data**
   - **Versioning:** keeps every version of an object, so an overwrite or delete can be undone.
   - **MFA Delete:** requires MFA to permanently delete versions.
   - **S3 Object Lock:** write once, read many (**WORM**); stops objects being changed or deleted for a retention period, used for **compliance/legal hold**.
   - **Replication:** **Cross-Region Replication (CRR)** for disaster recovery or lower latency in another Region; **Same-Region Replication (SRR)** for copies in the same Region (for example, a separate account). Versioning must be turned on.
4. **Security**
   - New buckets are **private by default**: **Block Public Access** is on and all new objects are **encrypted by default** with SSE-S3.
   - Encryption options at rest: **SSE-S3** (AWS manages keys), **SSE-KMS** (keys in AWS KMS, with audit trail in CloudTrail), **SSE-C** (you supply the key), or client-side encryption.
   - Control access with **IAM policies** (for users/roles) and **bucket policies** (on the bucket itself). **Presigned URLs** give someone temporary access to one object.
5. **Features that show up as answers**
   - **Static website hosting:** host HTML, CSS, JS, and images with no server (dynamic code such as PHP needs EC2 or Lambda).
   - **Lifecycle rules:** automatically **transition** objects to cheaper classes after N days, or **expire** (delete) them.
   - **S3 Transfer Acceleration:** speeds up long-distance uploads by routing them through **CloudFront edge locations**.
   - **Multipart upload:** upload big files in parallel parts (recommended above 100 MB, needed above 5 GB per single upload).
   - **Event notifications:** trigger Lambda, SQS, SNS, or EventBridge when an object is created or deleted.

**⚠️ Exam traps**

- S3 is **object** storage. It is **not** a boot disk for EC2 (that is EBS) and **not** a mountable shared file system (that is EFS/FSx).
- "Bucket names are global" does **not** mean data is global. A bucket's data stays in its Region unless you replicate it.
- Durability is the **same** for all classes (11 nines). If a question compares classes, the difference is availability, number of AZs, retrieval time, and price.
- To stop accidental **deletion**, choose **Versioning** (plus MFA Delete). To stop **any** change for compliance, choose **Object Lock**.
- To speed up uploads from far away, choose **Transfer Acceleration**. To speed up **downloads** to many users worldwide, choose **CloudFront**.

📖 **Read more:** [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) · [Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) · [Replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html) · [Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) · [Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) · [Static website hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html) · [Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/transfer-acceleration.html) · [S3 FAQs](https://aws.amazon.com/s3/faqs/) · [S3 pricing](https://aws.amazon.com/s3/pricing/)

---

## S3 storage classes and S3 Glacier

**📘 AWS definition:** Amazon S3 offers a range of storage classes designed for different access patterns at corresponding costs; the S3 Glacier storage classes are purpose-built for data archiving at the lowest cost.

**💡 Simple definition:** Same S3, different "shelves". The **less often** you need the data and the **longer** you can wait for it, the **cheaper** the shelf. Glacier = the cold basement.

**🔑 Exam keywords:** `infrequent access` · `unknown access pattern` · `archive` · `retrieval time` · `minimum storage duration` · `re-creatable data` · `compliance retention 7–10 years`

**🎯 What they test: pick the class from three questions**

```
1. Do you know how often the data is accessed?
   └─ No / it changes ............................................ S3 Intelligent-Tiering
2. Is it accessed often (daily/weekly)? ............................ S3 Standard
3. Rarely accessed but must come back instantly (milliseconds)?
   ├─ Can be re-created if one AZ is lost ......................... S3 One Zone-IA
   ├─ Accessed about monthly ...................................... S3 Standard-IA
   └─ Accessed about once a quarter (archive) ..................... S3 Glacier Instant Retrieval
4. Archive that can wait to be restored?
   ├─ Minutes to hours ............................................ S3 Glacier Flexible Retrieval
   └─ Up to 12–48 hours, cheapest of all .......................... S3 Glacier Deep Archive
```

| Class | Best for | AZs | First-byte access | Minimum storage duration | Retrieval fee | Memory hook |
|---|---|---|---|---|---|---|
| **S3 Standard** | Frequently accessed data, websites, apps | ≥3 | Milliseconds | None | No | The default, "hot" shelf |
| **S3 Intelligent-Tiering** | Unknown or changing access patterns | ≥3 | Milliseconds (optional archive tiers are slower) | None | No (small monthly monitoring fee per object) | "S3 decides for me" |
| **S3 Standard-IA** | Infrequent but needs fast access (backups, DR copies) | ≥3 | Milliseconds | 30 days | Yes | IA = **I**nfrequent **A**ccess |
| **S3 One Zone-IA** | Infrequent, **re-creatable** data (secondary copies, thumbnails) | **1** | Milliseconds | 30 days | Yes | Cheaper because only **one** AZ |
| **S3 Glacier Instant Retrieval** | Archive that is rarely read but needed **immediately** (medical images, news archives) | ≥3 | Milliseconds | 90 days | Yes | "Archive price, Standard speed" |
| **S3 Glacier Flexible Retrieval** | Archive / backups restored occasionally | ≥3 | **Minutes to hours** (expedited 1–5 min, standard 3–5 h, bulk 5–12 h) | 90 days | Yes (bulk is free) | "Flexible" = you choose how fast |
| **S3 Glacier Deep Archive** | Long-term retention (7–10+ years, regulatory) | ≥3 | **Hours** (standard within 12 h, bulk within 48 h) | 180 days | Yes | Deepest, slowest, **cheapest** |

Also good to recognise: **S3 Express One Zone** is a high-performance class for single-digit-millisecond access in **one AZ** (for latency-sensitive workloads such as ML training). It is about speed, not saving money.

**How data moves between classes:** set an **S3 Lifecycle rule** (for example: Standard → Standard-IA after 30 days → Glacier Flexible Retrieval after 90 days → delete after 7 years), or use **Intelligent-Tiering** and let S3 move objects automatically.

**⚠️ Exam traps**

- "Archive" alone does **not** mean "slow". If the question says rarely accessed **but needs milliseconds**, the answer is **Glacier Instant Retrieval**.
- **One Zone-IA** is only right when the data can be **re-created** or is a second copy. If losing an AZ is unacceptable, use Standard-IA.
- **Unpredictable/unknown access** → **Intelligent-Tiering**, not Standard-IA.
- Cheaper storage classes charge **retrieval fees** and **minimum durations**. Deleting a Deep Archive object after 30 days still bills 180 days.
- Older material calls Glacier a separate service ("Amazon Glacier" with vaults). For CLF-C02, think of the Glacier tiers as **S3 storage classes**.

📖 **Read more:** [Amazon S3 storage classes (user guide)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html) · [Storage classes compared (product page)](https://aws.amazon.com/s3/storage-classes/) · [Archive retrieval options](https://docs.aws.amazon.com/AmazonS3/latest/userguide/restoring-objects-retrieval-options.html) · [S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) · [Intelligent-Tiering](https://docs.aws.amazon.com/AmazonS3/latest/userguide/intelligent-tiering.html)

---

## Amazon EBS

**📘 AWS definition:** Amazon Elastic Block Store (Amazon EBS) provides scalable, high-performance block storage volumes that you can attach to your EC2 instances.

**💡 Simple definition:** A virtual hard drive (USB-like disk) that you plug into **one** EC2 instance in the **same AZ**. **EBS = Elastic Block Store = the EC2's own disk.**

**🔑 Exam keywords:** `block storage` · `boot volume` · `attached to EC2` · `persistent` · `snapshot` · `single AZ` · `IOPS`

**🎯 What they test**

1. **It lives in one Availability Zone.** A volume can only attach to instances in the **same AZ**. To move it to another AZ or Region, take a **snapshot** and create a new volume from it.
2. **Persistence.** Data survives stopping or rebooting the instance. By default the **root** volume is deleted when the instance is **terminated** (the "Delete on termination" setting); extra data volumes are kept.
3. **Snapshots** are point-in-time, **incremental** backups stored in S3 (managed by AWS, you do not see the bucket). Snapshots can be copied across Regions and shared with other accounts. Use **AWS Backup** to schedule them.
4. **Volume types** (only the families, not the numbers):

   | Family | Types | Use for |
   |---|---|---|
   | SSD, general purpose | **gp3**, gp2 | Most workloads, boot volumes (the default) |
   | SSD, provisioned IOPS | **io2 Block Express**, io1 | Critical databases needing high, consistent IOPS |
   | HDD, throughput optimised | **st1** | Big data, log processing (large sequential reads) |
   | HDD, cold | **sc1** | Lowest-cost, infrequently accessed data |

   HDD volumes (st1/sc1) **cannot be boot volumes**.
5. **Encryption** uses AWS KMS keys and covers data at rest, data moving between the instance and the volume, and snapshots.
6. **Pricing:** you pay for the **provisioned** size (GB per month), whether you fill it or not, plus extra IOPS/throughput on some types and snapshot storage.

**⚠️ Exam traps**

- EBS is **not** shared storage for many instances in different AZs. Use **EFS** for that. (Multi-Attach exists only for io1/io2 in the **same AZ**; do not pick it for "shared file system".)
- Paying for EBS = paying for **provisioned** GB, not used GB (unlike S3/EFS).
- "Data must survive instance stop" → **EBS**, not instance store.
- Under the Shared Responsibility Model, **you** choose to encrypt volumes and take snapshots.

📖 **Read more:** [What is Amazon EBS?](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html) · [EBS volume types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html) · [EBS snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html) · [EBS pricing](https://aws.amazon.com/ebs/pricing/)

---

## EC2 instance store

**📘 AWS definition:** An instance store provides temporary block-level storage for an EC2 instance, located on disks that are physically attached to the host computer.

**💡 Simple definition:** Scratch paper on the desk. Very fast, free with the instance, but **thrown away** when the instance stops.

**🔑 Exam keywords:** `ephemeral` · `temporary` · `cache` · `buffer` · `scratch data` · `highest I/O`

**🎯 What they test**

- Data **survives a reboot** but is **lost** when the instance is **stopped, hibernated, or terminated**, or if the underlying disk fails.
- Included in the instance price (only on instance types that offer it); very low latency.
- Good for caches, buffers, temporary files, and data that is replicated elsewhere.

**⚠️ Exam traps**

- Never choose instance store for data you must keep. Choose **EBS** (or S3/EFS).
- Instance store cannot be detached and re-attached to another instance, and you cannot snapshot it like EBS.

📖 **Read more:** [EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)

---

## Amazon EFS

**📘 AWS definition:** Amazon Elastic File System (Amazon EFS) provides serverless, fully elastic file storage so that you can share file data without provisioning or managing storage capacity and performance.

**💡 Simple definition:** A shared **network drive for Linux** that grows and shrinks by itself. Many EC2 instances (even in different AZs) can open the same files at the same time.

**🔑 Exam keywords:** `shared file system` · `NFS` · `Linux` · `many instances` · `multi-AZ` · `elastic / grows automatically` · `serverless file storage`

**🎯 What they test**

- Uses the **NFS** protocol and works with **Linux** (not Windows) workloads: EC2, containers (ECS/EKS/Fargate), and Lambda.
- **Thousands** of instances across **multiple AZs** can mount the same file system concurrently. On-premises servers can mount it over Direct Connect or VPN.
- **Elastic:** no capacity to provision; you **pay only for what you store**.
- **Storage classes:** **EFS Standard** (frequent), **EFS Infrequent Access**, and **EFS Archive**. **Lifecycle management** moves files between them automatically to save money.
- **Regional** file systems store data across multiple AZs; **One Zone** file systems keep it in a single AZ at lower cost.

**⚠️ Exam traps**

- Windows file share (SMB, Active Directory) → **FSx for Windows File Server**, not EFS.
- One EC2 instance needs a disk → **EBS**, not EFS.
- EFS costs more per GB than EBS or S3; it is chosen because many servers **share** it.

📖 **Read more:** [What is Amazon EFS?](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) · [EFS storage classes](https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html) · [EFS pricing](https://aws.amazon.com/efs/pricing/)

---

## Amazon FSx

**📘 AWS definition:** Amazon FSx makes it cost-effective to launch, run, and scale feature-rich, high-performance file systems in the cloud, built on popular commercial and open-source file systems.

**💡 Simple definition:** "Pick a famous file system and AWS runs it for you." **FSx = File System eXchange**: you swap your own file server for a managed one.

**🔑 Exam keywords:** `Windows file server` · `SMB` · `Active Directory` · `NetApp ONTAP` · `OpenZFS` · `Lustre` · `high-performance computing`

**🎯 What they test: match the flavour**

| FSx option | Pick it when the question says… |
|---|---|
| **FSx for Windows File Server** | Windows applications, **SMB**, **Microsoft Active Directory**, lift-and-shift a Windows file share |
| **FSx for NetApp ONTAP** | Already use **NetApp**; need NFS **and** SMB (multi-protocol) |
| **FSx for OpenZFS** | Move a **ZFS** or Linux file server without changing apps |
| **FSx for Lustre** | **HPC**, machine learning, very fast parallel file system linked to S3 |

All are **fully managed** (AWS handles hardware, patching, and backups); you pay for storage capacity, throughput, and backups.

**⚠️ Exam traps**

- Plain Linux shared files → **EFS** is the simpler answer.
- "Windows" + "shared files" → **FSx for Windows File Server** (EFS does not support Windows).
- The CLF-C02 exam guide lists **Amazon FSx** as in scope but places **FSx for Lustre** on the **out-of-scope** list. Know that Lustre = HPC, but focus on the Windows option.

📖 **Read more:** [Amazon FSx documentation](https://docs.aws.amazon.com/fsx/) · [Choosing an FSx file system](https://aws.amazon.com/fsx/when-to-choose-fsx/) · [FSx for Windows File Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html)

---

## AWS Storage Gateway

**📘 AWS definition:** AWS Storage Gateway is a hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage.

**💡 Simple definition:** A **bridge** (gateway) in your own data centre. Your local apps think they are saving to a normal file share, disk, or tape, but the data actually goes to AWS, with a **local cache** for speed.

**🔑 Exam keywords:** `hybrid` · `on-premises` · `local cache` · `virtual tape library` · `iSCSI` · `extend data centre storage to the cloud`

**🎯 What they test: the gateway types**

| Gateway type | On-premises sees | Data lands in | Typical question |
|---|---|---|---|
| **S3 File Gateway** | NFS/SMB file share | **S3** objects | "Store on-premises files in S3 through a file share" |
| **Volume Gateway** | iSCSI block volumes | S3, with **EBS snapshots** | "Back up on-premises block volumes to AWS" (cached or stored mode) |
| **Tape Gateway** | A virtual **tape library** for existing backup software | S3 / **S3 Glacier** classes | "Replace physical backup tapes" |
| FSx File Gateway | SMB share | FSx for Windows File Server | No longer offered to new customers; recognise the name only |

Data is encrypted in transit and at rest by default.

**⚠️ Exam traps**

- Storage Gateway is for **ongoing hybrid** access (on-premises apps keep running). It is not a one-time migration tool; for that, think DataSync or Application Migration Service.
- "Replace tapes" → **Tape Gateway**.
- If the app is already in AWS, you do not need a gateway; use S3, EFS, or FSx directly.

📖 **Read more:** [What is AWS Storage Gateway?](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html) · [Storage Gateway FAQs](https://aws.amazon.com/storagegateway/faqs/)

---

## AWS Backup

**📘 AWS definition:** AWS Backup is a fully managed service that centralises and automates data protection across AWS services, in the cloud, and on premises.

**💡 Simple definition:** **One backup timetable for everything.** Instead of setting up backups service by service, write one **backup plan** and apply it to many resources and accounts.

**🔑 Exam keywords:** `centralised backup` · `backup plan` · `backup policy` · `retention` · `cross-Region / cross-account copy` · `backup compliance`

**🎯 What they test**

- Works with many services, including **EC2, EBS, RDS, Aurora, DynamoDB, EFS, FSx, S3, DocumentDB, Neptune**, and Storage Gateway volumes.
- A **backup plan** sets the schedule, retention period, and lifecycle (move older backups to cold storage).
- Can copy backups to **another Region** or **another account**, and apply backup policies across accounts with **AWS Organizations**.
- **Backup Vault Lock** makes backups WORM (cannot be deleted), for ransomware protection and compliance; **Backup Audit Manager** reports on compliance.

**⚠️ Exam traps**

- AWS Backup takes **periodic** backups (restore takes time). For **continuous replication and fast failover of servers**, choose **Elastic Disaster Recovery**.
- "Central place to manage backups across services/accounts" → AWS Backup, not individual EBS snapshots or S3 versioning.

📖 **Read more:** [What is AWS Backup?](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) · [Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html) · [AWS Backup pricing](https://aws.amazon.com/backup/pricing/)

---

## AWS Elastic Disaster Recovery

**📘 AWS definition:** AWS Elastic Disaster Recovery (AWS DRS) minimises downtime and data loss with fast, reliable recovery of on-premises and cloud-based applications using affordable storage, minimal compute, and point-in-time recovery.

**💡 Simple definition:** A **standby copy of your servers** that is always being updated in AWS. When disaster strikes, press a button and the servers launch in AWS within minutes.

**🔑 Exam keywords:** `disaster recovery` · `continuous replication` · `failover` · `RPO in seconds` · `RTO in minutes` · `on-premises servers`

**🎯 What they test**

- **Continuous block-level replication** of physical, virtual, or cloud servers into a low-cost **staging area** in your chosen AWS Region.
- **RPO** (how much data you may lose) of **seconds** and **RTO** (how long you are down) of **minutes**.
- You pay mainly for replicated servers and the small staging resources; full-size recovery instances cost money only when launched (drills or a real disaster).
- Supports **failback** to the original site once it is healthy again.

**⚠️ Exam traps**

- Elastic Disaster Recovery ≠ AWS Backup (periodic copies) ≠ Application Migration Service (moving servers **permanently**). Both DRS and Application Migration Service use replication; the difference is **recover** versus **migrate**.

📖 **Read more:** [What is AWS Elastic Disaster Recovery?](https://docs.aws.amazon.com/drs/latest/userguide/what-is-drs.html) · [Elastic Disaster Recovery product page](https://aws.amazon.com/disaster-recovery/)

### Disaster recovery strategies: the big picture

The exam may ask which approach gives the fastest recovery or the lowest cost. They form a ladder: **cheaper and slower at the top, faster and more expensive at the bottom.**

| Strategy | What is running in the recovery Region | RTO / RPO | Cost |
|---|---|---|---|
| **Backup and restore** | Nothing; only backups | Hours | $ |
| **Pilot light** | Core data replicated; servers off until needed | Tens of minutes | $$ |
| **Warm standby** | A scaled-down but working copy | Minutes | $$$ |
| **Multi-site active/active** | A full copy serving traffic | Near zero | $$$$ |

📖 **Read more:** [Disaster recovery options in the cloud (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html)

---

## Database chooser

First identify the **data model**, then the service.

| If the question says… | Data model | Choose |
|---|---|---|
| SQL, tables, joins, transactions, existing MySQL/PostgreSQL/Oracle/SQL Server | Relational (OLTP) | [RDS](#amazon-rds) |
| Relational but needs higher performance and availability, cloud-native, MySQL/PostgreSQL-compatible | Relational (OLTP) | [Aurora](#amazon-aurora) |
| Key-value, serverless, millisecond latency at any scale, gaming, shopping carts, IoT | NoSQL key-value/document | [DynamoDB](#amazon-dynamodb) |
| JSON documents, **MongoDB-compatible** | NoSQL document | [DocumentDB](#amazon-documentdb) |
| Relationships, social network, fraud detection, recommendations, knowledge graph | Graph | [Neptune](#amazon-neptune) |
| Cache, microseconds, reduce database load, session store, leaderboard | In-memory | [ElastiCache](#amazon-elasticache) |
| Data warehouse, analytics, BI, OLAP, petabytes of historical data | Columnar analytics | [Redshift](#amazon-redshift) |

**OLTP vs OLAP (one-line memory):** **OLTP** = **T**ransactions (many small reads/writes, such as orders) → RDS/Aurora/DynamoDB. **OLAP** = **A**nalytics (few big queries over lots of history) → Redshift.

### Managed database vs installing a database on EC2

| | Managed (RDS, Aurora, DynamoDB…) | Self-managed on EC2 |
|---|---|---|
| OS and database patching | **AWS** | **You** |
| Backups, failover, scaling | Built in (you configure them) | You build them |
| Access to the operating system | No (except RDS Custom) | Full control |
| Choose when | You want less admin work | You need OS-level access or an unsupported engine/version |

Under the **Shared Responsibility Model**, for RDS AWS manages the hardware, OS, and database engine patching; **you** still manage database users and permissions, security groups, encryption settings, and your data.

📖 **Read more:** [Choosing an AWS database service](https://docs.aws.amazon.com/decision-guides/latest/databases-on-aws-how-to-choose/databases-on-aws-how-to-choose.html) · [Database services overview (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/database.html)

---

## Amazon RDS

**📘 AWS definition:** Amazon Relational Database Service (Amazon RDS) is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud.

**💡 Simple definition:** **Your favourite SQL database, but AWS does the boring jobs** (hardware, installation, patching, backups, failover). You just look after the tables and data.

**🔑 Exam keywords:** `managed relational database` · `Multi-AZ` · `read replica` · `automated backups` · `point-in-time recovery` · `MySQL / PostgreSQL / Oracle / SQL Server`

**🎯 What they test**

1. **Engines:** MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, IBM Db2, and Amazon Aurora (Aurora has its own card below).
2. **Multi-AZ vs read replicas.** This is the single most tested RDS fact.

   | | **Multi-AZ deployment** | **Read replica** |
   |---|---|---|
   | Main purpose | **High availability / failover** (disaster recovery inside a Region) | **Read scaling / performance** |
   | Copy method | **Synchronous** replication to a standby in another AZ | **Asynchronous** replication |
   | Can apps read from the copy? | No (classic Multi-AZ instance: standby is passive) | **Yes**, that is the point |
   | Where | Different AZ, **same Region** | Same AZ, other AZ, or **another Region** |
   | On failure | **Automatic failover**; the database endpoint (DNS name) stays the same | No automatic failover; you can **promote** a replica to a standalone database |
   | Memory hook | **M**ulti-AZ = **M**aintain uptime | **R**ead replica = **R**eads |

   > Newer option to recognise: a **Multi-AZ DB cluster** has one writer and **two readable standbys** in three AZs, so it combines failover with some read capacity.
3. **Backups**
   - **Automated backups:** daily snapshot plus transaction logs, kept for **1 to 35 days**; allow **point-in-time recovery** to any second in that window.
   - **Manual snapshots:** you take them; they are kept **until you delete them** (even after the database is deleted).
4. **Security:** runs inside your **VPC**; access controlled with **security groups**; encryption at rest with **KMS** (turn it on when you **create** the database) and in transit with SSL/TLS.
5. **Scaling:** **vertical** (bigger instance class), **storage auto scaling**, and **read replicas** for read traffic.
6. **Pricing:** pay for instance hours (On-Demand or Reserved Instances), storage, I/O (some types), backup storage beyond the free allowance, and data transfer.

**⚠️ Exam traps**

- "Improve **availability** / survive an AZ failure" → **Multi-AZ**. "Improve **read performance** / offload reporting queries" → **Read replicas**. Swapping these is the classic wrong answer.
- Read replicas in **another Region** help with disaster recovery and local reads, but they do not fail over automatically.
- You **cannot SSH** into the RDS host or install OS software. If you need that, use RDS Custom or a database on EC2.
- RDS is **not serverless** for most engines; you choose an instance size. For a serverless relational option, think **Aurora Serverless**; for serverless NoSQL, **DynamoDB**.

📖 **Read more:** [What is Amazon RDS?](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) · [Multi-AZ deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) · [Read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) · [Automated backups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html) · [RDS FAQs](https://aws.amazon.com/rds/faqs/) · [RDS pricing](https://aws.amazon.com/rds/pricing/)

---

## Amazon Aurora

**📘 AWS definition:** Amazon Aurora is a fully managed relational database engine that is compatible with MySQL and PostgreSQL, combining the performance and availability of commercial databases with the simplicity and cost-effectiveness of open-source databases.

**💡 Simple definition:** AWS's **own, faster, tougher MySQL/PostgreSQL**, built for the cloud. **Aurora = RDS with a turbo and six spare tyres.**

**🔑 Exam keywords:** `MySQL- and PostgreSQL-compatible` · `high performance` · `six copies across three AZs` · `Aurora Serverless` · `Global Database` · `cloud-native relational`

**🎯 What they test**

- **Compatible** with MySQL and PostgreSQL, so existing apps and tools work. AWS states up to **5× the throughput of MySQL** and **3× of PostgreSQL**.
- **Storage:** data is automatically kept as **six copies across three Availability Zones**, and the storage volume **grows automatically**; you do not provision it.
- **Aurora Replicas:** up to **15** low-lag read replicas that can also act as **automatic failover targets**.
- **Aurora Serverless (v2):** capacity scales automatically with demand; good for variable or unpredictable workloads.
- **Aurora Global Database:** one primary Region plus read-only **secondary Regions** with typically sub-second replication; a secondary can be promoted for **cross-Region disaster recovery**.
- Managed through the **RDS console**; backups, patching, and monitoring work like RDS.

**⚠️ Exam traps**

- Aurora is **relational**, not NoSQL.
- Aurora is **not** compatible with Oracle or SQL Server. For those engines use RDS (and for converting them to Aurora, use SCT + DMS).
- "Relational + serverless/auto-scaling capacity" → **Aurora Serverless**, not DynamoDB.

📖 **Read more:** [What is Amazon Aurora?](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html) · [Aurora Serverless v2](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2.html) · [Aurora Global Database](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html) · [Aurora FAQs](https://aws.amazon.com/rds/aurora/faqs/)

---

## Amazon DynamoDB

**📘 AWS definition:** Amazon DynamoDB is a serverless, fully managed, distributed NoSQL database with single-digit millisecond performance at any scale.

**💡 Simple definition:** A **giant, never-full lookup table**. Give it a key, get the item back in milliseconds, whether you have 10 users or 10 million. **Dynamo = dynamic scale, no servers.**

**🔑 Exam keywords:** `NoSQL` · `key-value` · `serverless` · `single-digit millisecond` · `any scale` · `global tables` · `DAX` · `schemaless`

**🎯 What they test**

- **Serverless:** no instances to choose or patch; data is automatically replicated across **three AZs** in a Region and encrypted at rest by default.
- **Capacity modes:**

  | Mode | You pay | Choose when |
  |---|---|---|
  | **On-demand** | Per read/write request | Unpredictable or new workloads; zero planning |
  | **Provisioned** (optionally with auto scaling) | For the read/write capacity you set | Steady, predictable traffic; lower cost |

- **Global tables:** **multi-Region, multi-active** (read and write in every Region) replication for global apps and DR.
- **DAX (DynamoDB Accelerator):** an in-memory cache built for DynamoDB that cuts reads from milliseconds to **microseconds**.
- **Backups:** on-demand backups and **point-in-time recovery** (up to 35 days).
- **Other features you may see:** **DynamoDB Streams** (react to item changes, often with Lambda), **TTL** (auto-delete expired items), Standard-IA table class for rarely accessed tables.
- Items are limited to **400 KB**, so large files go in S3 with a pointer in DynamoDB.

**⚠️ Exam traps**

- Complex SQL **joins** and relational schemas → **RDS/Aurora**, not DynamoDB.
- "Cache in front of DynamoDB" → **DAX**. "Cache in front of RDS" → **ElastiCache**.
- "MongoDB-compatible" → **DocumentDB**, even though DynamoDB also stores documents.
- DynamoDB global tables (multi-active) are different from Aurora Global Database (one writer Region).

📖 **Read more:** [What is Amazon DynamoDB?](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) · [Capacity modes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/capacity-mode.html) · [Global tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html) · [DAX](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html) · [DynamoDB FAQs](https://aws.amazon.com/dynamodb/faqs/)

---

## Amazon DocumentDB

**📘 AWS definition:** Amazon DocumentDB (with MongoDB compatibility) is a fast, reliable, fully managed database service that makes it easy to set up, operate, and scale MongoDB-compatible databases in the cloud.

**💡 Simple definition:** **Managed MongoDB-style database** for JSON documents. "Document" in the name → MongoDB in the question.

**🔑 Exam keywords:** `MongoDB-compatible` · `document database` · `JSON` · `content management` · `catalogues` · `user profiles`

**🎯 What they test**

- Existing **MongoDB drivers, tools, and code** work with little or no change.
- Fully managed: storage grows automatically and is replicated six ways across three AZs (same style as Aurora); up to 15 read replicas.
- Migrate existing MongoDB data with **AWS DMS**.

**⚠️ Exam traps**

- "MongoDB" → DocumentDB. "Key-value at massive scale, serverless" → DynamoDB. "Relationships/graph" → Neptune.

📖 **Read more:** [What is Amazon DocumentDB?](https://docs.aws.amazon.com/documentdb/latest/developerguide/what-is.html) · [DocumentDB FAQs](https://aws.amazon.com/documentdb/faqs/)

---

## Amazon Neptune

**📘 AWS definition:** Amazon Neptune is a fast, reliable, fully managed graph database service that makes it easy to build and run applications that work with highly connected datasets.

**💡 Simple definition:** A database for **who-knows-whom / what-links-to-what**. Picture a spider web of dots (people, products) and lines (relationships).

**🔑 Exam keywords:** `graph database` · `highly connected data` · `relationships` · `social network` · `fraud detection` · `recommendation engine` · `knowledge graph`

**🎯 What they test**

- Recognising the use case is enough: when the question stresses **relationships between items**, the answer is Neptune.
- Supports graph query languages (Gremlin, openCypher, SPARQL); fully managed with replicas across AZs and continuous backup to S3.

**⚠️ Exam traps**

- Do not pick RDS (tables) or DynamoDB (key lookups) when the question is about traversing relationships.

📖 **Read more:** [What is Amazon Neptune?](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html) · [Neptune FAQs](https://aws.amazon.com/neptune/faqs/)

---

## Amazon ElastiCache

**📘 AWS definition:** Amazon ElastiCache is a fully managed, in-memory caching service compatible with Valkey, Memcached, and Redis OSS, delivering real-time, cost-optimised performance for modern applications.

**💡 Simple definition:** Keep the most-asked answers **on the desk (in memory)** instead of walking to the filing room (the database) every time. **Cache = speed + less database load.**

**🔑 Exam keywords:** `in-memory` · `cache` · `microsecond latency` · `reduce database load` · `session store` · `leaderboard` · `Redis` · `Memcached` · `Valkey`

**🎯 What they test**

- Sits **in front of** a database (often RDS) to speed up repeated reads and reduce load and cost on the database.
- Engines: **Valkey** and **Redis OSS** (replication, Multi-AZ failover, backups, rich data types) and **Memcached** (simple, multithreaded cache, no persistence).
- Can run as **ElastiCache Serverless** (no nodes to size) or on nodes you choose.

**⚠️ Exam traps**

- A cache is **not** the main, durable system of record. Keep the real data in a database.
- Cache for DynamoDB specifically → **DAX**.

📖 **Read more:** [What is Amazon ElastiCache?](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/WhatIs.html) · [ElastiCache FAQs](https://aws.amazon.com/elasticache/faqs/)

---

## Amazon Redshift

> The exam guide lists Redshift under **Analytics**; it is kept here so you can compare it with the other databases.

**📘 AWS definition:** Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud.

**💡 Simple definition:** A **warehouse for reports**, not a shop till. It stores huge amounts of historical data in columns so big analytical questions ("sales by region for 5 years") run fast. **Redshift = shift data to insights.**

**🔑 Exam keywords:** `data warehouse` · `OLAP` · `business intelligence` · `petabyte scale` · `columnar` · `complex analytical queries`

**🎯 What they test**

- **Columnar storage** + **massively parallel processing (MPP)** = fast SQL over very large datasets.
- **Redshift Serverless** runs analytics without managing clusters.
- **Redshift Spectrum** queries data directly in **S3** without loading it.
- Works with BI tools such as **Amazon Quick** (formerly QuickSight) for dashboards.

**⚠️ Exam traps**

- Redshift is for **analytics (OLAP)**, not for an app's day-to-day transactions (OLTP); that is RDS/Aurora/DynamoDB.
- Quick, serverless, pay-per-query SQL on files **already in S3** with no warehouse → **Athena**. A permanent warehouse for heavy, repeated BI → **Redshift**.

📖 **Read more:** [What is Amazon Redshift?](https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html) · [Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-whatis.html) · [Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-using-spectrum.html)

---

## AWS Database Migration Service (AWS DMS)

**📘 AWS definition:** AWS Database Migration Service (AWS DMS) is a managed migration and replication service that helps move your database and analytics workloads to AWS quickly and securely, with minimal downtime and zero data loss.

**💡 Simple definition:** **Moving house while still living in it.** The source database keeps working while DMS copies the data and then keeps syncing changes until you switch over.

**🔑 Exam keywords:** `migrate database` · `minimal downtime` · `source remains operational` · `homogeneous` · `heterogeneous` · `continuous replication`

**🎯 What they test**

- **Homogeneous** migration = same engine (Oracle → RDS for Oracle). **Heterogeneous** = different engines (Oracle → Aurora PostgreSQL); the schema must be converted first with **SCT** (or DMS Schema Conversion).
- **Change data capture (CDC)** keeps the target in sync, so downtime is minimal.
- Sources and targets can be on-premises, on EC2, or AWS managed databases (RDS, Aurora, DynamoDB, Redshift, S3, DocumentDB…).

**⚠️ Exam traps**

- **DMS moves data. SCT converts the schema.** Moving servers (not just databases) → **Application Migration Service** (see [Cloud concepts](01-cloud-concepts.md#aws-migration-tools)).

📖 **Read more:** [What is AWS DMS?](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) · [DMS FAQs](https://aws.amazon.com/dms/faqs/)

---

## AWS Schema Conversion Tool (AWS SCT)

**📘 AWS definition:** The AWS Schema Conversion Tool (AWS SCT) converts your existing database schema from one database engine to another, along with most database code objects such as views, stored procedures, and functions.

**💡 Simple definition:** A **translator** that rewrites the database's blueprint in a new "language" (engine) before DMS moves the furniture (data).

**🔑 Exam keywords:** `convert schema` · `heterogeneous migration` · `different database engine` · `Oracle to PostgreSQL`

**🎯 What they test**

- Needed only for **heterogeneous** migrations (different source and target engines).
- Highlights the code it cannot convert automatically so you can fix it by hand.

**⚠️ Exam traps**

- Same engine to same engine → no SCT needed, just DMS.

📖 **Read more:** [What is AWS SCT?](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html)

---

## Moving large data sets into AWS

Not separate in-scope services for CLF-C02, but older questions still ask "how do I get lots of data into AWS?"

| Situation | Answer |
|---|---|
| Good network; move or sync files online | **AWS DataSync** |
| Ongoing hybrid access from on-premises apps | **Storage Gateway** |
| Dedicated private network link for regular large transfers | **Direct Connect** (see [Compute and networking](02-compute-and-networking.md#aws-direct-connect)) |
| Faster long-distance uploads to S3 | **S3 Transfer Acceleration** |
| Huge data, little or no network (offline) | Historically the **Snow Family** (Snowcone, Snowball). Snowcone is discontinued, Snowball Edge is closed to new customers, and support in commercial Regions ends on **31 December 2026**. AWS now points to DataSync (online) and **AWS Data Transfer Terminal** (physical). |

📖 **Read more:** [What is AWS DataSync?](https://docs.aws.amazon.com/datasync/latest/userguide/what-is-datasync.html) · [Snowball Edge availability change](https://docs.aws.amazon.com/snowball/latest/developer-guide/snowball-edge-availability-change.html)

---

## Final comparison tables

### Storage

| Service | Type | Scope | Shared by many instances? | Memory hook |
|---|---|---|---|---|
| S3 | Object | Regional (multi-AZ) | Yes, over HTTPS | Unlimited bucket |
| EBS | Block | **One AZ** | No (one instance) | EC2's own hard drive |
| Instance store | Block | The physical host | No | Scratch paper, lost on stop |
| EFS | File (NFS, Linux) | Regional (multi-AZ) | **Yes** | Linux network drive |
| FSx | File (SMB, NFS, Lustre…) | AZ or multi-AZ | **Yes** | Windows (and friends) network drive |
| Storage Gateway | Hybrid bridge | On-premises ↔ AWS | — | Local door into cloud storage |

### Databases

| Service | Data model | Serverless? | Main exam phrase |
|---|---|---|---|
| RDS | Relational | No (pick instance size) | Managed SQL database; Multi-AZ vs read replicas |
| Aurora | Relational | Optional (Aurora Serverless) | AWS-built, MySQL/PostgreSQL-compatible, 6 copies in 3 AZs |
| DynamoDB | Key-value / document | **Yes** | NoSQL, millisecond latency at any scale |
| DocumentDB | Document | Optional | MongoDB-compatible |
| Neptune | Graph | Optional | Relationships / highly connected data |
| ElastiCache | In-memory | Optional | Cache, microseconds, reduce DB load |
| Redshift | Columnar analytics | Optional (Redshift Serverless) | Data warehouse / OLAP |

### Top traps on this page

1. Multi-AZ = **availability**; read replica = **read performance**.
2. One Zone-IA only for **re-creatable** data.
3. "Rarely accessed but needed in milliseconds" = **Glacier Instant Retrieval**.
4. EBS = **one AZ, one instance**; EFS = **many instances, many AZs**.
5. Instance store data is **lost when the instance stops**.
6. Windows file share = **FSx for Windows**, not EFS.
7. **DMS moves** data; **SCT converts** schemas.
8. Backup (periodic copies) ≠ Elastic Disaster Recovery (continuous replication, fast failover).

[← Compute and networking](02-compute-and-networking.md) | [Index](README.md) | [Security and compliance →](04-security-and-compliance.md)
