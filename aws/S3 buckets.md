# S3 Buckets — Full Notes from A to Z

## What is S3?

**Amazon S3 (Simple Storage Service)** is AWS’s **object storage** service. It stores data as **objects** inside **buckets**. A bucket is the top-level container, and an object is the actual file plus its metadata. ([AWS Documentation][1])

---

## Notes / explanation

### 1. Bucket

An **S3 bucket** is a container that stores objects. Before storing anything in S3, you create a bucket. ([AWS Documentation][1])

### 2. Object

An **object** is the actual data stored in S3, such as a file, plus metadata and a key. S3 stores data as objects, not blocks or filesystems. ([AWS Documentation][1])

### 3. Key

A **key** is the name/path of the object inside the bucket. It is used to uniquely identify and retrieve the object. ([AWS Documentation][2])

### 4. Object storage

S3 is **object storage**.  
That means:

* **S3** = objects in buckets
* **EBS** = block storage
* **EFS** = file storage

### 5. Regional service

You create an S3 bucket in a chosen AWS Region. AWS documents S3 general purpose buckets as created in an AWS Region. ([AWS Documentation][3])

### 6. Bucket name

Bucket names must be globally unique for general purpose buckets in the shared global namespace. ([AWS Documentation][4])

### 7. Versioning

**Versioning** keeps multiple versions of the same object in a bucket so deleted or overwritten objects can be recovered more easily. ([AWS Documentation][5])

### 8. Bucket policy

A **bucket policy** is a resource-based policy attached to the bucket to control access to the bucket and its objects. ([AWS Documentation][6])

### 9. Block Public Access

S3 provides **Block Public Access** settings to help prevent buckets or objects from being exposed publicly. AWS documentation notes that if you want public website-style access, you must adjust block public access and bucket policy settings. ([AWS Documentation][4])

### 10. Encryption

S3 supports server-side encryption for protecting data at rest. AWS security best practices for S3 discuss encryption options and defaults. ([AWS Documentation][7])

### 11. Object Ownership

**Object Ownership** is a bucket-level setting used to control object ownership and ACL behavior. AWS notes that **Bucket owner enforced** disables ACLs and lets the bucket owner manage access using policies. ([AWS Documentation][4])

### 12. Static website hosting

S3 can be used to host a **static website** for content like HTML, CSS, and JavaScript, with proper bucket policy and public access configuration. ([AWS Documentation][8])

---

## How S3 fits DevOps work

S3 is commonly used for:

* storing application files
* storing logs and backups
* Terraform state files
* CI/CD artifacts
* static website hosting
* snapshots, exports, and reports

Even if your main project centered on EKS, S3 is still a very common DevOps service because it is used everywhere for storage and automation workflows. ([AWS Documentation][1])

---

## Core concept Q&A

### What is an S3 bucket?

An S3 bucket is a container used to store objects in Amazon S3. ([AWS Documentation][1])

### What type of storage is S3?

S3 is object storage. ([AWS Documentation][1])

### What is stored inside an S3 bucket?

Objects are stored inside an S3 bucket. Each object contains the file data, metadata, and key. ([AWS Documentation][2])

### What is versioning in S3?

Versioning keeps multiple versions of an object in the same bucket so you can recover from accidental deletion or overwrite. ([AWS Documentation][5])

### What is a bucket policy?

A bucket policy is a resource-based policy used to control access to an S3 bucket and its objects. ([AWS Documentation][6])

### Why is S3 called durable storage?

Because it is designed for highly durable object storage and is commonly used for long-term data storage and backups. ([AWS Documentation][1])

### What is the difference between S3 and EBS?

S3 is object storage.  
EBS is block storage attached to EC2.

### Can bucket names repeat across AWS?

For general purpose buckets in the shared global namespace, bucket names must be globally unique. ([AWS Documentation][4])

---

## Project-based Q&A

### Where would S3 typically be used in a DevOps project?

It is commonly used for Terraform state files, backups, logs, CI/CD artifacts, and static content storage.

### Why is S3 useful for storing Terraform state?

Because it provides durable centralized storage that multiple team members or pipelines can access.

### Why would you enable versioning on an S3 bucket?

To recover older versions of objects if they are overwritten or deleted by mistake. ([AWS Documentation][5])

### If you had to store build artifacts from Jenkins, would S3 be a good choice?

Yes.  
S3 is commonly used for storing build artifacts and generated files.

---

## Scenario-based Q&A

### A file in S3 was accidentally overwritten. How can you recover it more easily?

If versioning was enabled, you can retrieve the older version of the object. ([AWS Documentation][5])

### Your S3 bucket became publicly accessible by mistake. What settings would you check first?

I would check:

* Block Public Access
* bucket policy
* object/public access settings ([AWS Documentation][4])

### You want to host a simple static frontend. Can S3 do that?

Yes, S3 can host static website content with the required access settings. ([AWS Documentation][8])

### A user has IAM permission to S3 in general, but still cannot access one bucket. Why?

A bucket policy or public access configuration may be restricting access to that specific bucket. ([AWS Documentation][6])

---

## Revision points

* **S3 = object storage**
* **Bucket = container**
* **Object = file + metadata**
* **Key = object name/path**
* **Versioning = keep old versions**
* **Bucket policy = access control**
* **Block Public Access = prevent public exposure**
* **Bucket names are globally unique**
* **S3 is commonly used for logs, backups, artifacts, and Terraform state**

---

## Best interview-ready answer

**“S3 buckets are containers in Amazon S3 used to store objects. S3 is AWS’s object storage service, so it stores files as objects inside buckets instead of using block or file storage. In DevOps, S3 is commonly used for logs, backups, Terraform state, CI/CD artifacts, and static website hosting. Important concepts in S3 are bucket, object, key, versioning, bucket policy, and block public access.”**

---

## All important S3 storage classes and lifecycle of storage

### 1. S3 Standard

Used for frequently accessed data.  
 This is the default class when you upload an object unless you choose another one. It gives low latency and high throughput. In AWS pricing, the first 50 TB/month is $0.023 per GB-month in the pricing example/search snippet.

Use for:

* active application data
* current logs
* frequently downloaded files
* websites/assets used often

### 2. S3 Intelligent-Tiering

Used when access pattern is unknown or changes over time.  
 AWS automatically moves objects between access tiers to reduce cost. AWS says it has a monitoring and automation charge per object, and there are no retrieval charges for objects moved between its internal frequent/infrequent tiers. Objects under 128 KB are not auto-tiered.

Use for:

* data where you are not sure how often it will be accessed
* mixed workloads
* unpredictable access patterns

### 5. S3 Glacier Instant Retrieval

Used for rarely accessed archive data that still needs instant retrieval in milliseconds. AWS says it is suited for data accessed about once a quarter. AWS pricing/search snippets show $0.004 per GB-month. It has:

* 90-day minimum storage duration
* 128 KB minimum billable object size
* retrieval charges when accessed.

Use for:

* long-term records
* medical/legal files
* rarely used documents that still need immediate opening

### 6. S3 Glacier Flexible Retrieval

Used for archive data that is accessed once or twice a year and does not need immediate real-time access. AWS says retrieval can take minutes to hours, and bulk retrievals are free. AWS pricing/search snippet shows $0.0036 per GB-month. It has:

* 90-day minimum storage duration
* restore required before access
* extra archived-object metadata charges.

Use for:

* backup archives
* disaster recovery archives
* old compliance data

**Important:**  
 Objects in Glacier Flexible Retrieval are archived, so you do not simply GET them like Standard or IA. First, you request a restore, then AWS creates a temporary accessible copy.

### 7. S3 Glacier Deep Archive

Used for data accessed less than once a year. AWS says it is the lowest-cost S3 storage option and is intended for long-term retention and compliance-style archiving. AWS pricing/search snippet shows $0.00099 per GB-month. It has:

* 180-day minimum storage duration
* restore time typically 9 to 48 hours
* restore required before access.

Use for:

* compliance archives
* very old backups
* records kept for years but almost never read

### 8. S3 Express One Zone

This is for very low-latency, high-performance object access in a single AZ. AWS says it is built for the most latency-sensitive workloads and is up to 10x faster than regular S3 classes, but it is not the normal lifecycle cost-saving choice. Also, directory buckets only support this class.

Use for:

* ultra-low-latency workloads
* performance-sensitive data processing

For your interview, this is not the main lifecycle/cost-optimization class. It is more about performance than archival savings.

---

## Notes

Objects are not moved automatically by default; they move automatically only if you use S3 Lifecycle rules or S3 Intelligent-Tiering.

Manual classes like Standard, Standard-IA, One Zone-IA, Glacier Instant Retrieval, Glacier Flexible Retrieval, Deep Archive → you must configure Lifecycle rules if you want automatic movement. The duration is whatever number of days you define in the lifecycle rule, such as 30 days, 90 days, or 365

Intelligent-Tiering = AWS decides automatically based on usage
