# EBS — Full Notes from A to Z

## What is EBS?

**EBS (Elastic Block Store)** is AWS block storage for EC2.  
It provides persistent storage volumes that you can attach to EC2 instances and use like a hard disk. EBS snapshots are point-in-time backups that persist independently of the volume. ([AWS Documentation][1])

---

## Notes / explanation

### 1. Volume

An **EBS volume** is a block storage device that can be attached to an EC2 instance. To the operating system, it looks like a disk drive. ([AWS Documentation][1])

### 2. Persistent storage

**Persistent** means the storage remains even if the EC2 instance is stopped. EBS is used when data must survive beyond the running life of a process or instance. ([AWS Documentation][2])

### 3. Block storage

**Block storage** means data is stored in fixed-size blocks and presented like a raw disk to the OS. This is different from file storage and object storage.

### 4. Snapshot

An **EBS snapshot** is a point-in-time backup of an EBS volume. You can use snapshots to restore or create new volumes later. ([AWS Documentation][2])

### 5. Volume types

AWS provides multiple EBS volume types:

* **gp2 / gp3** = general purpose SSD
* **io1 / io2** = provisioned IOPS SSD
* **st1** = throughput optimized HDD
* **sc1** = cold HDD
* **standard** = magnetic legacy type ([AWS Documentation][1])

### 6. gp3

**gp3** is the latest general purpose SSD option recommended for most workloads. AWS recommends gp3 for many workloads, and EKS best practices recommend starting with gp3 for host root volumes. ([AWS Documentation][3])

### 7. IOPS

**IOPS** means input/output operations per second. It represents how many read/write operations a storage volume can handle.

### 8. Throughput

**Throughput** means how much data can be transferred per second.

### 9. AZ scope

An EBS volume is tied to a specific **Availability Zone**. If an EC2 instance is in one AZ, the EBS volume attached to it must be in the same AZ.

### 10. Encryption

EBS volumes can be encrypted to protect data at rest.

### 11. Root volume

The **root volume** is the main disk from which the EC2 instance boots. Often this root disk is an EBS volume.

### 12. EBS in EKS

In EKS, EBS is commonly used for **persistent volumes** for stateful workloads. The **Amazon EBS CSI driver** is the Kubernetes CSI plugin that provides EBS storage for the cluster. It uses IAM permissions and supports service-account-based role association. ([AWS Documentation][4])

---

## How EBS was used in your project

In your project, EBS was important because:

* worker nodes likely used EBS-backed root volumes
* you enabled **persistent storage** for Kubernetes workloads
* you used the **EBS CSI driver**
* you used **OIDC / IRSA** so the CSI driver could get the AWS permissions it needed
* this allowed storage to remain separate from Pod lifecycle

**Interview line:**  
**“In my project, EBS was used to provide persistent block storage. I integrated the EBS CSI driver with IAM/OIDC so Kubernetes workloads could dynamically use EBS volumes instead of losing data when Pods restarted.”**

---

## Core concept Q&A

### What is EBS?

EBS is AWS block storage used mainly with EC2 instances. It provides persistent volumes that behave like disks. ([AWS Documentation][1])

### What is the difference between EBS and EC2?

EC2 is compute.  
EBS is storage attached to that compute.

### What is an EBS snapshot?

An EBS snapshot is a point-in-time backup of an EBS volume. ([AWS Documentation][2])

### Is EBS persistent or temporary?

EBS is persistent storage. It survives beyond normal runtime events like instance stop/start. ([AWS Documentation][2])

### What are the main EBS volume types?

The main EBS types are gp2, gp3, io1, io2, st1, sc1, and standard. ([AWS Documentation][1])

### Which EBS type is commonly used for general workloads?

gp3 is commonly recommended for general-purpose workloads. ([AWS Documentation][3])

### What is the EBS CSI driver?

It is the Kubernetes storage driver that lets EKS workloads use Amazon EBS volumes. ([AWS Documentation][4])

---

## Project-based Q&A

### Why did you need EBS in your EKS project?

Because containers and Pods can be ephemeral, but some workloads need persistent storage. EBS provided that persistent block storage.

### Why did you use the EBS CSI driver?

Because Kubernetes uses CSI drivers to provision and manage storage dynamically, and the EBS CSI driver is the AWS-supported way to use EBS with EKS. ([AWS Documentation][4])

### Why were OIDC and IRSA needed with EBS CSI?

Because the EBS CSI driver needs AWS permissions to create and manage EBS volumes, and AWS documents that the add-on uses service-account-based IAM integration. ([AWS Documentation][4])

### Why is EBS useful for stateful applications?

Because stateful applications need data to remain even if Pods restart, reschedule, or scale.

---

## Scenario-based Q&A

### A Pod restarts and its data is lost. What could be the storage reason?

The Pod may be using ephemeral storage instead of persistent storage backed by EBS.

### Your EKS workload cannot provision an EBS volume. What would you check?

I would check:

* whether the EBS CSI driver is installed
* whether the CSI driver has the required IAM permissions
* whether IRSA / service-account role mapping is correct
* whether the StorageClass and PVC are correct  
  AWS provides a runbook specifically for troubleshooting EBS CSI driver issues in EKS. ([AWS Documentation][5])

### An EC2 instance is running, but attached storage performance is low. What EBS concepts matter?

The main things are the EBS volume type, IOPS, and throughput.

### Can you attach an EBS volume from one AZ to an instance in another AZ?

No.  
EBS volumes are AZ-specific.

---

## Revision points

* **EBS = block storage for EC2**
* **Volume = disk**
* **Snapshot = backup**
* **Persistent = data survives**
* **gp3 = common general-purpose SSD**
* **IOPS = operations per second**
* **Throughput = data transfer rate**
* **EBS is AZ-specific**
* **EBS CSI driver = EKS storage integration**
* **IRSA/OIDC often used for EBS CSI permissions**

---

## Best interview-ready answer

**“EBS is Amazon Elastic Block Store, which is AWS block storage mainly used with EC2. It provides persistent volumes that behave like disks. In my EKS project, I used EBS through the EBS CSI driver so stateful workloads could use persistent storage, and I integrated it with OIDC and IRSA to give the driver the required AWS permissions.”** ([AWS Documentation][4])
