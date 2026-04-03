## EFS — Full Notes from A to Z

### What is EFS?

**EFS (Elastic File System)** is AWS’s managed **file storage** service.
It provides a shared file system that multiple EC2 instances or Kubernetes Pods can mount and use at the same time. Amazon EFS uses NFS and is designed to grow and shrink automatically as you add or remove files. ([AWS Documentation][1])

---

## Notes / explanation

### 1. File storage

EFS is **file storage**, not block storage.
It behaves like a shared network file system.

### 2. Shared storage

Multiple servers can mount the same EFS file system at the same time, which makes it useful for shared content, common application files, and workloads needing concurrent access. EFS uses NFSv4.0 and NFSv4.1. ([AWS Documentation][1])

### 3. Regional service

EFS is designed as a **regional** file system and uses **mount targets** in your VPC subnets so resources in different AZs can access it. AWS recommends creating mount targets in the AZs where clients access the file system. ([AWS Documentation][2])

### 4. Mount target

A **mount target** is the network endpoint in a subnet through which EC2 instances or Pods connect to the EFS file system. Mount targets are associated with security groups. ([AWS Documentation][3])

### 5. Access point

An **EFS access point** is an application-specific entry point into an EFS file system. It helps control access and directory permissions for different applications. ([AWS Documentation][4])

### 6. Performance mode

EFS supports performance modes such as **General Purpose**, which is ideal for latency-sensitive workloads. AWS documentation also discusses performance behavior and optimization separately. ([AWS Documentation][5])

### 7. Throughput mode

EFS supports throughput modes such as **Elastic**, which automatically scales throughput up or down based on workload activity. ([AWS Documentation][5])

### 8. Security

Access to EFS is controlled using:

* VPC security groups
* NFS networking rules
* IAM/file system policies in some setups
* POSIX permissions inside the file system ([AWS Documentation][3])

### 9. EFS in EKS

In EKS, EFS is commonly used through the **Amazon EFS CSI driver**, which is the Kubernetes CSI plugin for EFS storage. ([AWS Documentation][6])

### 10. EFS vs EBS

* **EFS** = file storage, shared across multiple clients
* **EBS** = block storage, typically attached to one EC2 instance at a time in one AZ

This is one of the most important interview differences.

---

## How EFS fits projects

EFS is useful when:

* multiple EC2 instances need the same files
* multiple Pods need shared storage
* you need file-system-style shared access
* you want storage that grows automatically

In Kubernetes, EFS is more suitable than EBS when multiple Pods on different nodes need to read/write shared files.

---

## Core concept Q&A

### What is EFS?

EFS is AWS’s managed file storage service that provides a shared file system for AWS resources. ([AWS Documentation][1])

### What type of storage is EFS?

EFS is **file storage**.

### Can multiple EC2 instances use the same EFS file system?

Yes.
That is one of the key benefits of EFS.

### What protocol does EFS use?

EFS uses **NFS**. AWS documentation describes mounting through NFSv4.0 and NFSv4.1. ([AWS Documentation][1])

### What is a mount target in EFS?

A mount target is the network endpoint in a subnet used to access the EFS file system. ([AWS Documentation][3])

### What is an access point in EFS?

An access point is an application-specific entry point into the EFS file system with its own access settings. ([AWS Documentation][4])

### What is the difference between EFS and EBS?

EFS is shared file storage.
EBS is block storage usually attached to EC2 in a single AZ.

### What CSI driver is used for EFS in EKS?

The **Amazon EFS CSI driver** is used. ([AWS Documentation][6])

---

## Project-based Q&A

### When would you choose EFS instead of EBS in EKS?

I would choose EFS when multiple Pods across different nodes need shared access to the same files.

### Why is EFS useful for shared application data?

Because the same file system can be mounted by multiple compute instances or Pods at the same time.

### How is EFS used in Kubernetes?

It is used through the Amazon EFS CSI driver so Pods can mount EFS-backed storage. ([AWS Documentation][6])

### If your project had shared files across multiple application replicas, which would be better: EBS or EFS?

EFS would be better because it is shared file storage.

---

## Scenario-based Q&A

### Multiple Pods on different nodes need to read and write the same files. What storage would you choose?

EFS.

### Your application needs a common shared uploads directory for all replicas. What AWS storage fits better?

EFS.

### Your EC2 instance cannot mount EFS. What are the main things you would check?

I would check:

* mount target availability
* security group rules
* NFS access
* mount helper / mount command
* file system and IAM policy conditions if IAM-based mounting is used ([AWS Documentation][3])

### Your workload needs a disk-like volume attached to one instance, not shared storage. Should you use EFS?

No.
That is a better fit for EBS.

---

## Revision points

* **EFS = managed file storage**
* **Shared storage**
* **Uses NFS**
* **Can be mounted by multiple clients**
* **Mount target = network endpoint in subnet**
* **Access point = controlled entry point**
* **EFS CSI driver = EKS integration**
* **EFS for shared storage**
* **EBS for block storage**

---

## Best interview-ready answer

**“EFS is Amazon Elastic File System, which is AWS’s managed shared file storage service. It provides a file system that multiple EC2 instances or Kubernetes Pods can mount at the same time. It is useful when applications need common shared storage. In EKS, EFS is used through the EFS CSI driver.”** ([AWS Documentation][1])

[1]: https://docs.aws.amazon.com/efs/latest/ug/how-it-works.html?utm_source=chatgpt.com "How Amazon EFS works - Amazon Elastic File System"
[2]: https://docs.aws.amazon.com/efs/latest/ug/getting-started.html?utm_source=chatgpt.com "Getting started with Amazon EFS - Amazon Elastic File System"
[3]: https://docs.aws.amazon.com/efs/latest/ug/network-access.html?utm_source=chatgpt.com "Using VPC security groups - Amazon Elastic File System"
[4]: https://docs.aws.amazon.com/efs/latest/ug/mounting-access-points.html?utm_source=chatgpt.com "Mounting with EFS access points"
[5]: https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html?utm_source=chatgpt.com "What is Amazon Elastic File System?"
[6]: https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html?utm_source=chatgpt.com "Use elastic file system storage with Amazon EFS - Amazon EKS"
