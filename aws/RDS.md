

## 1. Relational database in simple words

Suppose you have an ecommerce app.  
You may have separate tables like:

**Users table**

```text
user_id
name
email
1
Ram
ram@gmail.com
```

**Orders table**

```text
order_id
user_id
product
101
1
Laptop
```

Here:

- Users and Orders are related
- user_id connects both tables

That is why it is called relational database.  

**Main idea**

Data is stored in structured tables, and relationships are maintained between tables.

---

## 2. Non-relational database in simple words

In a non-relational database like MongoDB, the same data may look like this in one document:

```json
{
 "name": "Ram",
 "email": "ram@gmail.com",
 "orders": [
   {
     "order_id": 101,
     "product": "Laptop"
   }
 ]
}
```

Here:

- data is stored like a JSON-like document
- no strict table relationship is required
- related data can be stored together inside one document

That is why MongoDB is called non-relational.

---

# RDS

## 1. What is RDS?

Amazon RDS (Relational Database Service) is AWS’s managed relational database service. It helps you set up, operate, and scale relational databases in the cloud without manually managing many routine database tasks. (AWS Documentation)

## 2. What problem does it solve?

If you install a database directly on EC2, you manage:

- OS maintenance
- database installation
- backups
- patching
- failover
- scaling
- monitoring

RDS reduces that operational work by managing common database administration tasks for you. (AWS Documentation)

## 3. Why do we use it?

We use RDS when we want a relational database but do not want to manage everything manually.  
Typical reasons:

- managed service
- easier backups
- easier scaling
- high availability options
- easier recovery
- easier maintenance (AWS Documentation)

## 4. Databases supported in RDS

RDS supports multiple relational engines such as:

- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server
- Db2

Aurora is also part of the Amazon RDS family, though it is usually discussed separately in interviews. (AWS Documentation)

## 5. Important concepts / terms

### DB instance

A DB instance is the actual RDS database server you create and run. It is the main compute resource for your database. This is the unit you launch when using standard RDS deployments. (AWS Documentation)

### Automated backups

RDS automatically takes backups during the backup window and stores them for the retention period you configure. It also supports point-in-time recovery during that retention window. (AWS Documentation)

### Snapshot

A snapshot is a manual backup that you keep until you delete it. Automated backups follow retention rules, but snapshots are manually controlled. This is the practical difference interviewers often expect. (AWS Documentation)

### Multi-AZ

Multi-AZ is for high availability. RDS maintains a standby copy in another Availability Zone provided this stand by copy is only used as the failover if primary fails and have issue .

**Example:**  
Your main DB is in AZ-1. RDS keeps a standby in AZ-2. If AZ-1 has a problem, RDS switches to AZ-2.

### Read replica

A read replica is a read-only copy of your primary database used mainly for read scaling. RDS creates it from the source and updates it using asynchronous replication.

**Simple meaning:**

- Main DB handles writes
- Replica helps with extra reads

### Parameter group

A DB parameter group is a container for database engine settings that apply to one or more DB instances that you associate with that parameter group 

**Simple meaning:**

- It is like a settings file for the database.

**Example settings:**

- max connections
- timeout values
- logging behavior

So if interviewer asks “where do you change DB engine config in RDS?”  
Answer: parameter group

For Multi-AZ DB clusters, a DB cluster parameter group applies settings to all DB instances in the cluster. (AWS Documentation)

**Simple meaning:**  
If you have a cluster with multiple DB instances, and you want some settings common across the cluster, that is where cluster-level parameters come in. 

---

## What is the difference between RDS and database on EC2?

RDS is managed by AWS for common admin tasks, while on EC2 we install and manage the database ourselves. (AWS Documentation)

## What is point-in-time recovery?

It means restoring the database to any specific point within the backup retention period using automated backups and transaction logs. (AWS Documentation)

---

## 8. Practical fresher questions

### If your application has too many read requests, what will you use?

Use a read replica to offload read traffic from the primary database. (AWS Documentation)

### If you want high availability for your database, what will you use?

Use Multi-AZ deployment. (AWS Documentation)

### If the database is accidentally corrupted and you want to restore it to an earlier time, what feature helps?

Automated backups with point-in-time recovery. (AWS Documentation)

### If you need to change database engine settings like configuration values, where do you do it?

In a parameter group. (AWS Documentation)

---

## 9. Quick revision

- RDS = managed relational database
- DB instance = actual database server
- Multi-AZ = HA / failover
- Read replica = read scaling
- Automated backup = retention + PITR
- Snapshot = manual backup
- Parameter group = DB config values 

---

## In my current project I am using mongodb as the statefulset on eks cluster which store the data on ebs volumes how is it different from rds and also explain how rds is connected to ec2 instance?

Yes — your current setup and RDS are very different.

### First, the biggest difference

Your current database is MongoDB running inside Kubernetes as a StatefulSet with persistent storage backed by EBS volumes through the EBS CSI driver. In that model, Kubernetes manages the MongoDB pods and persistent volumes, while the EBS CSI driver manages the lifecycle of EBS volumes used as persistent volumes. Amazon RDS, on the other hand, is a managed relational database service for engines like MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Db2, and Aurora. So the first difference is not just managed vs self-managed — it is also NoSQL document DB vs managed relational DB.

### How your MongoDB-on-EKS setup differs from RDS

In your setup, you are operating the database: the DB runs in your cluster, you manage the StatefulSet behavior, storage attachment, DB upgrades, backups, restore strategy, replica configuration, and Kubernetes-level troubleshooting. With RDS, the database runs as an AWS-managed DB instance outside Kubernetes, and AWS handles much of the heavy operational work such as provisioning, patching, backups, and scaling-related management. RDS gives your application a DB endpoint and port, and your app connects to that network endpoint instead of mounting a volume and running the DB engine inside the cluster.

### Practical difference in one line

Your current design is:

```text
App pod in EKS -> MongoDB pod in EKS -> EBS volume
```

RDS design is:

```text
App on EC2 or EKS -> RDS endpoint over network -> AWS-managed database storage/backend
```

---

### How RDS is practically used with an EC2 instance

Practical flow with EC2 is simple. You create an RDS DB instance in a VPC, and that DB instance gets an endpoint. You control access using a security group on the database. Then you allow inbound DB traffic from the EC2 instance’s security group to the RDS security group on the database port, such as 3306 for MySQL or 5432 for PostgreSQL. Your application running on EC2 stores the DB endpoint, port, username, password, and database name in config or environment variables, and the DB driver connects to the RDS endpoint. AWS has an EC2-to-RDS tutorial specifically around securely connecting an EC2 instance to an RDS database, and RDS documentation states that clients connect using the DB instance endpoint and port.

### How RDS is practically used from an EKS cluster

With EKS, the database is still outside the cluster. Your pods do not run the DB engine. Instead, your application pods connect to the RDS endpoint over VPC networking. The common setup is: create the RDS instance in private subnets, allow DB access from the EKS worker node security group or from pod-level security groups if you use security groups for pods, store the DB endpoint/credentials in a Kubernetes Secret or other secret manager integration, and configure the application deployment to connect to the RDS endpoint. Amazon EKS supports assigning security groups to individual pods, which can be useful for DB access control.

### Why many teams prefer RDS with EKS for production apps

For many production apps, teams prefer to keep the database outside Kubernetes because the app layer and DB layer are separated. Kubernetes is then used for running stateless or lightly stateful application workloads, while RDS handles the database as a managed service. If the app scales up or pods restart frequently, the database remains stable behind its endpoint. If connection count becomes a problem, AWS also provides RDS Proxy,

### RDS Proxy

RDS Proxy is a managed database proxy that sits between your application and your RDS/Aurora database. 

### Why it is used 

When many app instances, pods, or Lambda invocations open lots of DB connections, the database can get overloaded just handling connections. RDS Proxy reduces that by pooling and reusing connections instead of making the database create a brand-new connection for every client request 

---

**Note :.** If you want an AWS-managed database for MongoDB-style workloads, the closer AWS service is Amazon DocumentDB (with MongoDB compatibility)
