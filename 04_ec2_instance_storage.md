# Amazon EC2 – Instance Storage

## What's an EBS Volume?

- An **EBS (Elastic Block Store) Volume** is a **network drive** you can attach to your instances while they run.
- It allows your instances to **persist data**, even after their termination.
- **They can only be mounted to one instance at a time** (at the CCP level).
- They are bound to a **specific Availability Zone (AZ)**.

### Analogy
- Think of them as a **"network USB stick"**.

### Free Tier
- 30 GB of free EBS storage per month (General Purpose SSD or Magnetic).

## EBS Volume

### It's a network drive (i.e. not a physical drive)
- It uses the **network** to communicate with the instance, so **latency** may be present.
- It can be **detached** from one EC2 instance and **re-attached** to another one quickly.

### It's locked to an Availability Zone (AZ)
- An EBS Volume in `us-east-1a` **cannot** be attached to `us-east-1b`.
- To move a volume across AZs, you must first **snapshot** it.

### Have a provisioned capacity (size in GBs and IOPS)
- You are **billed** for all provisioned capacity.
- Capacity can be **increased over time**.

## EBS – Delete on Termination attribute

- Controls the EBS behaviour when an EC2 instance terminates:
  - By default, the **root EBS volume is deleted** (attribute enabled).
  - By default, **any other attached EBS volume is not deleted** (attribute disabled).

- This attribute can be controlled via:
  - **AWS Console**
  - **AWS CLI**

### Use case:
- Preserve the root volume **after an EC2 instance is terminated**, e.g. for data recovery or forensic purposes.

## EBS Snapshots

- You can make a **backup (snapshot)** of your EBS volume at a specific point in time.
- **Detaching the volume is not necessary** to perform a snapshot, but it is recommended for data consistency.
- **Snapshots can be copied** across:
  - **Availability Zones (AZ)**
  - **Regions**

### Example
An EBS volume in `us-east-1a` (50 GB) is snapshotted and the snapshot can be used to **restore** the volume in another AZ or Region (e.g., `us-east-1b`).

## EBS Snapshots Features

### EBS Snapshot Archive
- Move a snapshot to an **archive tier** that is up to **75% cheaper**.
- **Restore time** from archive: **24 to 72 hours**.

### Recycle Bin for EBS Snapshots
- You can configure **retention rules** to keep deleted snapshots, allowing recovery after accidental deletion.
- Retention period can be set from **1 day to 1 year**.

### Fast Snapshot Restore (FSR)
- Forces **full initialization** of a snapshot to ensure **zero latency** on first use.
- This feature has an **additional cost**.

### Example
- You can **archive** a snapshot to reduce costs (but with slower restore).
- Deleted snapshots can be sent to a **recycle bin** for recovery within the configured retention period.

## AMI Overview

- **AMI = Amazon Machine Image**

- AMIs are a **customization** of an EC2 instance:
  - You add your own software, configuration, operating system, monitoring, etc.
  - Faster boot/configuration time since all your software is pre-packaged.

- AMIs are built for a **specific region** (but can be copied across regions).

- You can launch EC2 instances from:
  - **A Public AMI**: provided by AWS
  - **Your own AMI**: created and maintained by you
  - **An AWS Marketplace AMI**: created by someone else and possibly sold

## AMI Process (from an EC2 instance)

- **Start** an EC2 instance and customize it
- **Stop** the instance (for data integrity)
- **Build an AMI** – this will also create EBS snapshots
- **Launch** instances from other AMIs

## EC2 Instance Store

- EBS volumes are **network drives** with good but *limited* performance
- If you need a **high-performance hardware disk**, use EC2 Instance Store

### Characteristics:
- Better I/O performance
- EC2 Instance Store loses its storage if the instance is stopped (ephemeral)
- Good for buffer / cache / scratch data / temporary content
- Risk of data loss if hardware fails
- **Backups and Replication are your responsibility**

## Local EC2 Instance Store – Very High IOPS

### Performance by Instance Size

| Instance Size     | 100% Random Read IOPS | Write IOPS   |
|-------------------|------------------------|--------------|
| i3.large*         | 100,125                | 35,000       |
| i3.xlarge*        | 206,250                | 70,000       |
| i3.2xlarge        | 412,500                | 180,000      |
| i3.4xlarge        | 825,000                | 360,000      |
| i3.8xlarge        | 1.65 million           | 720,000      |
| i3.16xlarge       | 3.3 million            | 1.4 million  |
| i3.metal          | 3.3 million            | 1.4 million  |
| i3en.large*       | 42,500                 | 32,500       |
| i3en.xlarge*      | 85,000                 | 65,000       |
| i3en.2xlarge*     | 170,000                | 130,000      |
| i3en.3xlarge      | 250,000                | 200,000      |
| i3en.6xlarge      | 500,000                | 400,000      |
| i3en.12xlarge     | 1 million              | 800,000      |
| i3en.24xlarge     | 2 million              | 1.6 million  |
| i3en.metal        | 2 million              | 1.6 million  |

## EBS Volume Types

- EBS Volumes come in 6 types:
  - **gp2 / gp3 (SSD)**: General purpose SSD volume that balances price and performance for a wide variety of workloads
  - **io1 / io2 Block Express (SSD)**: Highest-performance SSD volume for mission-critical, low-latency or high-throughput workloads
  - **st1 (HDD)**: Low cost HDD volume designed for frequently accessed, throughput-intensive workloads
  - **sc1 (HDD)**: Lowest cost HDD volume designed for less frequently accessed workloads

- EBS Volumes are characterized in:
  - **Size**
  - **Throughput**
  - **IOPS (I/O Operations Per Second)**

- When in doubt, always consult the AWS documentation – it's good!

- **Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes**

## EBS Volume Types Use Cases

### General Purpose SSD

- Cost effective storage, low-latency
- Suitable for:
  - System boot volumes
  - Virtual desktops
  - Development and test environments
- Volume size range: **1 GiB – 16 TiB**

### gp3:
- Baseline: **3,000 IOPS** and **125 MiB/s** throughput
- Can increase:
  - IOPS up to **16,000**
  - Throughput up to **1,000 MiB/s**
- IOPS and throughput can be configured **independently**

### gp2:
- Small volumes can **burst IOPS to 3,000**
- Volume size and IOPS are **linked**:
  - **3 IOPS per GiB**
  - Max IOPS: **16,000**
- At **5,334 GiB**, you reach the **maximum IOPS** (5,334 × 3 = 16,002)

## EBS Volume Types Use Cases

### Provisioned IOPS (PIOPS) SSD

- Designed for **critical business applications** with sustained IOPS performance
- Suitable for applications that need **more than 16,000 IOPS**
- Ideal for **database workloads**, where storage performance and consistency are critical

### io1 (4 GiB – 16 TiB):
- Max PIOPS:
  - **64,000** for **Nitro EC2 instances**
  - **32,000** for other instances
- PIOPS can be **increased independently** from storage size

### io2 Block Express (4 GiB – 64 TiB):
- **Sub-millisecond latency**
- Max PIOPS: **256,000**
  - IOPS:GiB ratio up to **1,000:1**
- Supports **EBS Multi-Attach**

## EBS Volume Types Use Cases

### Hard Disk Drives (HDD)

- **Cannot be used as boot volume**
- Volume size range: **125 GiB – 16 TiB**

### Throughput Optimized HDD (st1)
- Use cases:
  - Big Data
  - Data Warehouses
  - Log Processing
- **Max throughput**: 500 MiB/s
- **Max IOPS**: 500

### Cold HDD (sc1)
- Use cases:
  - Infrequently accessed data
  - Cost-sensitive scenarios
- **Max throughput**: 250 MiB/s
- **Max IOPS**: 250

## EBS – Volume Types Summary

| Volume Type                    | Durability                         | Use Cases                                                                 | Volume Size         | Max IOPS per Volume       | Max Throughput per Volume | EBS Multi-Attach | NVMe Reservations | Boot Volume |
|-------------------------------|-------------------------------------|---------------------------------------------------------------------------|----------------------|----------------------------|----------------------------|-------------------|--------------------|-------------|
| **gp3** (General Purpose SSD) | 99.8% - 99.9% (0.1% - 0.2% AFR)     | - Transactional workloads<br>- Virtual desktops<br>- Medium-sized DBs<br>- Low-latency apps<br>- Boot volumes<br>- Dev & test | 1 GiB – 16 TiB       | 16,000                     | 1,000 MiB/s                | Not supported     | Not supported       | Supported   |
| **gp2** (General Purpose SSD) | 99.8% - 99.9% (0.1% - 0.2% AFR)     | Same as gp3                                                               | 1 GiB – 16 TiB       | 16,000 (3 IOPS per GiB)    | 250 MiB/s                  | Not supported     | Not supported       | Supported   |
| **io2 Block Express (PIOPS)** | 99.999% (0.001% AFR)                | - Sub-millisecond latency<br>- Sustained IOPS<br>- >64,000 IOPS or 1,000 MiB/s | 4 GiB – 64 TiB       | 256,000                    | 4,000 MiB/s                | Supported         | Supported           | Supported   |
| **io1 (PIOPS)**               | 99.8% - 99.9% (0.1% - 0.2% AFR)     | - Sustained IOPS<br>- >16,000 IOPS<br>- I/O-intensive DB workloads        | 4 GiB – 16 TiB       | 64,000 (Nitro) / 32,000    | 1,000 MiB/s                | Supported         | Not supported       | Supported   |
| **st1** (Throughput HDD)      | 99.8% - 99.9% (0.1% - 0.2% AFR)     | - Big Data<br>- Data warehouses<br>- Log processing                       | 125 GiB – 16 TiB     | 500                        | 500 MiB/s                  | Not supported     | -                  | Not supported |
| **sc1** (Cold HDD)            | 99.8% - 99.9% (0.1% - 0.2% AFR)     | - Infrequently accessed data<br>- Lowest cost scenarios                  | 125 GiB – 16 TiB     | 250                        | 250 MiB/s                  | Not supported     | -                  | Not supported |

> 📎 [AWS Docs Reference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives)

## EBS Multi-Attach – io1/io2 Family

- Allows attaching the **same EBS volume** to **multiple EC2 instances** within the **same Availability Zone**
- Each instance has **full read & write permissions** to the high-performance volume

### Use Cases
- Achieve **higher application availability** in clustered Linux applications (e.g. Teradata)
- Applications must **handle concurrent write operations**

### Key Characteristics
- Supports up to **16 EC2 instances simultaneously**
- Requires a **cluster-aware file system** (e.g. not XFS, EXT4)

> Example: A single io2 volume is shared by up to 16 EC2 instances within the same AZ

## EBS Encryption

### When you create an encrypted EBS volume, you get:
- **Data at rest** is encrypted inside the volume
- **Data in flight** (between the instance and the volume) is encrypted
- **All snapshots** of the volume are encrypted
- **All volumes** created from encrypted snapshots are also encrypted

### Key Points
- Encryption and decryption are handled **transparently** (no user action needed)
- **Minimal impact on latency**
- Uses **AWS KMS (AES-256)** for encryption keys
- **Copying an unencrypted snapshot** allows encryption
- **Snapshots of encrypted volumes are always encrypted**

## Encryption: Encrypt an Unencrypted EBS Volume

### Steps to encrypt an existing unencrypted EBS volume:

1. **Create an EBS snapshot** of the unencrypted volume
2. **Encrypt the snapshot** using the **Copy** action
3. **Create a new EBS volume** from the encrypted snapshot  
   - The new volume will be **encrypted**
4. **Attach the encrypted volume** to the original EC2 instance (if needed)

## Amazon EFS – Elastic File System

- **Managed NFS** (Network File System) that can be mounted on **multiple EC2 instances**
- Works with EC2 instances in **multi-AZ**
- **Highly available**, **scalable**, and **expensive** (approximately **3× gp2**)
- **Pay-per-use** pricing model

### Key Characteristics
- Ideal for **shared access** across AZs
- Automatically scales with usage
- EC2 instances access EFS through a **Security Group**

> Example: EC2 instances in us-east-1a, 1b, and 1c are connected to a shared EFS filesystem via a Security Group, supporting multi-AZ shared storage.

## Amazon EFS – Elastic File System

### Use Cases
- Content management
- Web serving
- Data sharing
- Wordpress hosting

### Key Features
- Uses **NFSv4.1 protocol**
- Access is controlled via **Security Groups**
- **Compatible only with Linux-based AMIs** (not Windows)
- Encryption at rest using **AWS KMS**

### File System Characteristics
- **POSIX-compliant** (standard Linux file API)
- Automatically scales
- **Pay-per-use**, no need for capacity planning

## EFS – Performance & Storage Classes

### EFS Scale
- Supports **thousands of concurrent NFS clients**
- Up to **10 GB/s throughput**
- **Automatically scales** to petabyte-scale file systems

### Performance Mode *(set at EFS creation time)*
- **General Purpose (default)**:  
  - Best for **latency-sensitive** use cases (e.g. web server, CMS)
- **Max I/O**:  
  - Higher latency, but optimized for **throughput and parallelism**  
  - Suitable for **big data, media processing**

### Throughput Mode
- **Bursting**:  
  - Baseline: **1 TB = 50 MiB/s**  
  - Burst capacity: **up to 100 MiB/s**

- **Provisioned**:  
  - Set fixed throughput, e.g. **1 GiB/s for 1 TB storage**, regardless of actual size

- **Elastic**:  
  - Automatically adjusts throughput based on usage  
  - **Up to 3 GiB/s for reads** and **1 GiB/s for writes**
  - Ideal for **unpredictable workloads**

## EFS – Storage Classes

### Storage Tiers (Managed via Lifecycle Policies)
- **Standard**: For frequently accessed files
- **Infrequent Access (EFS-IA)**:  
  - Lower storage cost  
  - Higher retrieval cost  
- **Archive**:  
  - For rarely accessed files (a few times per year)  
  - 50% cheaper than EFS-IA
- You can **automatically move** files between tiers using **lifecycle policies** (e.g., after 60 days of no access)

### Availability and Durability
- **Standard**:
  - **Multi-AZ** (high availability)
  - Recommended for production workloads
- **One Zone**:
  - Stored in a **single AZ**
  - Better for development/test
  - Backup enabled by default
  - Compatible with IA → **EFS One Zone-IA**

### Key Benefit
- **Over 90% cost savings** with EFS lifecycle tiering

## EBS vs EFS – Elastic Block Storage

### EBS Volumes
- Can be attached to **one instance only** (except for **multi-attach io1/io2**)
- Are **AZ-bound** (locked to the same Availability Zone)
- **gp2**: IOPS increase with volume size
- **gp3 / io1**: IOPS and throughput can be configured **independently**

### To Migrate an EBS Volume Across AZ
1. **Take a snapshot** of the volume
2. **Restore the snapshot** to a volume in a different AZ
3. Note: EBS backups consume **I/O**, so avoid running during peak traffic

### Additional Notes
- **Root EBS volumes** are deleted by default when the EC2 instance is terminated  
  - This setting **can be disabled**

## EBS vs EFS – Elastic File System

### EFS Key Features
- Can be mounted to **hundreds of instances across multiple AZs**
- Commonly used to **share website files** (e.g. WordPress)
- **POSIX-compliant**, so it works **only with Linux instances**
- Typically **more expensive than EBS**
- Can reduce cost using **Storage Tiers**

### Reminder
- Know when to use:
  - **EFS**: Shared, multi-AZ, scalable file system
  - **EBS**: Single-AZ block storage, low-latency
  - **Instance Store**: Ephemeral storage tied to instance lifecycle
