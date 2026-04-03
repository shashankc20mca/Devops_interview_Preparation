# EC2 — Full Notes from A to Z

## What is EC2?

**EC2 (Elastic Compute Cloud)** is AWS’s service for launching **virtual servers** in the cloud. You choose the OS, instance type, storage, networking, and security settings, then run your applications on it. ([AWS Documentation][1])

## Why is EC2 used?

EC2 is used when you need:

* compute power
* a Linux or Windows server
* full control over the machine
* scalable cloud-based virtual servers

From a DevOps point of view, EC2 is commonly used for:

* application hosting
* bastion host
* Jenkins server
* monitoring server
* test servers
* self-managed tools and workloads

---

## Core terms under EC2

### 1. Instance

An **EC2 instance** is a virtual server in AWS. ([AWS Documentation][2])

### 2. AMI

An **AMI (Amazon Machine Image)** is the template used to launch an EC2 instance. It contains the OS and software configuration required to boot the instance. ([AWS Documentation][3])

### 3. Instance Type

The **instance type** defines the hardware capacity of the instance, such as compute, memory, and storage characteristics. AWS groups them into families based on use case. ([AWS Documentation][4])

### 4. Key Pair

A **key pair** is used to securely connect to an EC2 instance. For Linux instances, the private key is used for SSH access. ([AWS Documentation][5])

### 5. Security Group

A **security group** acts as a virtual firewall for EC2 instances. It controls inbound and outbound traffic. ([AWS Documentation][6])

### 6. User Data

**User data** is a script or cloud-init configuration that runs when an EC2 instance launches. On Linux, you can pass shell scripts or cloud-init directives. ([AWS Documentation][7])

### 7. EBS-backed instance

If the root volume of the instance is **EBS**, the instance can be stopped and started later. ([AWS Documentation][8])

### 8. Public IP / Private IP

An EC2 instance can have a **private IP** for internal communication and may also have a **public IP** if it needs direct internet access, depending on subnet and routing design. This is part of VPC networking behavior. ([AWS Documentation][6])

### 9. Instance Metadata

**Instance metadata** is information about the running EC2 instance that can be accessed from inside the instance, such as instance ID, IP information, and attached security groups. AWS also warns not to store sensitive data in user data. ([AWS Documentation][9])

### 10. Instance States

An EC2 instance moves through lifecycle states such as **pending, running, stopping, stopped, shutting-down, and terminated**. ([AWS Documentation][10])

---

## Important EC2 concepts you must know

* EC2 is a **virtual server**
* AMI is the **machine image/template**
* Instance type is the **size/capacity**
* Security group controls **traffic**
* Key pair is used for **SSH access**
* User data is used for **bootstrapping**
* EBS-backed instances can be **stopped and started**
* Terminated instance is effectively gone
* EC2 runs inside a **VPC subnet**
* Public/private reachability depends on **subnet, route table, public IP, and security group**

---

## What EC2 did in your project

In your project, EC2 was used in two main ways:

### 1. Bastion Host

You created a **jump server / bastion host** in a public subnet. This EC2 instance acted as the secure entry point to access private resources like EKS worker nodes.

### 2. Test VMs in public subnet

You created EC2 instances in the public subnet and used **user data** to enable the `httpd` service automatically during launch.

So your EC2 knowledge will be tested around:

* bastion host purpose
* public vs private subnet placement
* SSH access
* security groups
* user data
* instance connectivity
* internet access

### Can we stop and start an EC2 instance?

Yes, if it is EBS-backed. AWS notes that when you start it again, it is typically placed on new underlying hardware and gets a new public IPv4 address unless you use an Elastic IP. ([AWS Documentation][8])

### What happens when an EC2 instance is terminated?

Its lifecycle ends. Termination deletes the instance itself; attached behavior depends on storage configuration, but the instance is no longer usable. ([AWS Documentation][10])

### What are the EC2 instance states?

Pending, running, stopping, stopped, shutting-down, and terminated. ([AWS Documentation][10])

---

## Project-based interview questions and answers

### Why did you use EC2 in your project?

I used EC2 to create the bastion host and also launched test VMs in the public subnet to validate networking and automate web server setup using user data.

### Why did you use a bastion host EC2 instance?

Because my worker nodes were in private subnets and not directly accessible from the internet. The bastion host in the public subnet acted as the secure jump server to access private resources.

### Why was the bastion host placed in a public subnet?

Because it needed controlled external access for administration, while private resources remained non-public.

### How did you automatically install or enable software on EC2 at launch?

Using **user data**. In my project, I used user data to enable the `httpd` service automatically when the instance launched. ([AWS Documentation][7])

### Why not place all EC2 instances in public subnet?

Because not all instances should be internet-facing. Private placement improves security and reduces attack surface.

### Why were private EKS worker nodes not directly accessible?

Because they were in private subnets without public IPs, and access was controlled through the bastion host and VPC networking.

---

## General interview questions and answers

### What is the difference between EC2 and EKS?

EC2 gives you virtual machines. EKS gives you a managed Kubernetes control plane, while worker nodes may still run on EC2.

### What is the difference between AMI and instance type?

AMI is the software image.
Instance type is the hardware capacity.

### What is the difference between public IP and private IP in EC2?

Private IP is used for internal VPC communication. Public IP is used for direct internet communication when networking allows it.

### Can an EC2 instance exist outside a VPC?

In modern AWS usage, EC2 instances are launched in a VPC.

### What is the use of user data in DevOps?

It helps automate bootstrapping steps like package installation, service startup, or base configuration during instance launch. ([AWS Documentation][7])

### What is instance metadata?

It is information about the running instance, accessible from inside the instance, used for configuration and management. ([AWS Documentation][9])

---

## Scenario-based questions and answers

### Your EC2 instance is in a public subnet but you still cannot SSH into it. What will you check?

I will check:

* does it have a public IP
* does the subnet route table have route to Internet Gateway
* does the security group allow inbound SSH on port 22
* is the correct key pair being used
* is the instance actually running

These checks align with how EC2 connectivity depends on networking and security group rules. ([AWS Documentation][6])

### Your EC2 instance in private subnet needs internet to download packages. What will you do?

I will ensure the private subnet routes outbound traffic to a NAT Gateway placed in a public subnet.

### You lost SSH access to your bastion host. What could be the reasons?

Possible reasons:

* wrong key pair
* security group not allowing port 22
* no public IP
* wrong route configuration
* instance not running

### You stop and start an EC2 instance and public IP changes. Why?

AWS states that an EBS-backed instance typically gets a new public IPv4 address when restarted unless an Elastic IP is associated. ([AWS Documentation][8])

### Your user data script did not configure the server as expected. What will you check?

I will check:

* whether user data script syntax is correct
* whether it ran at launch
* cloud-init or boot logs on the instance
* whether the package/service commands were valid

AWS documents user data as launch-time initialization input for Linux instances. ([AWS Documentation][7])

---

## Crisp revision notes

* **EC2 = virtual server in AWS**
* **AMI = image/template**
* **Instance type = CPU/RAM capacity**
* **Key pair = SSH access**
* **Security group = firewall**
* **User data = launch-time automation**
* **EC2 runs inside VPC subnet**
* **Public access depends on subnet + route + public IP + SG**
* **EBS-backed instance can be stopped and started**
* **My project used EC2 as bastion host and public test VM**

---

## Mistakes to avoid

Do not say:

* “EC2 is a container service”
* “AMI and instance type are same”
* “Security group is attached to subnet”
* “User data is same as metadata”
* “Any instance in public subnet is automatically accessible”
* “Private subnet instance can never access internet”

---

## Best interview-ready answer

**“EC2 is AWS’s virtual server service. It lets us launch cloud-based machines with a selected AMI, instance type, storage, and networking. In my project, I used EC2 mainly for the bastion host in the public subnet and also launched public subnet test instances where I used user data to automatically enable the `httpd` service. From a DevOps point of view, EC2 is commonly used for servers, administration hosts, and tool deployments.”** ([AWS Documentation][1])
