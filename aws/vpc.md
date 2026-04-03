# VPC — Full Notes from A to Z

## 1. What is a VPC?

A **VPC (Virtual Private Cloud)** is a logically isolated virtual network in AWS where you launch resources such as EC2 instances, EKS worker nodes, load balancers, and databases. A VPC belongs to **one AWS Region** and can contain subnets in multiple Availability Zones within that Region. ([AWS Documentation][1])

## 2. Why do we need a VPC?

We use a VPC to control the **networking layer** of our AWS infrastructure. It lets us define IP ranges, create subnets, decide which resources are public or private, control routing, and apply network security boundaries. In DevOps, VPC is the base on which the rest of the infrastructure is built. ([AWS Documentation][1])

---

## Core terms under VPC

### 3. CIDR block

A **CIDR block** defines the IP address range of the VPC, for example `10.0.0.0/16`. This is the private address space from which subnet IP ranges are carved out. When you create a VPC, you assign it an IPv4 CIDR block. ([AWS Documentation][1])

### 4. Subnet

A **subnet** is a smaller range of IP addresses created inside a VPC. A subnet must exist in **one Availability Zone only**. You place resources like EC2 instances and EKS worker nodes inside subnets. ([AWS Documentation][1])

### 5. Public subnet

A subnet is called **public** when its route table has a route to the **Internet Gateway**, and the instances that need internet communication also have a **public IPv4** or **Elastic IP**. In simple words, internet-facing resources are usually placed in a public subnet. ([AWS Documentation][2])

### 6. Private subnet

A subnet is called **private** when it does **not** have a direct route to the Internet Gateway. Resources in a private subnet are not directly reachable from the internet. They may still get **outbound-only** internet access through a NAT Gateway. ([AWS Documentation][3])

### 7. Route table

A **route table** is the traffic controller of a VPC. It contains rules that decide where traffic from a subnet is sent. Every VPC has a **main route table**, and you can create additional custom route tables. ([AWS Documentation][4])

### 8. Public route table

A **public route table** usually contains:

* local route for VPC communication
* `0.0.0.0/0` route pointing to the **Internet Gateway**

This route enables internet-bound traffic from associated public subnets. ([AWS Documentation][2])

### 9. Private route table

A **private route table** usually contains:

* local route for VPC communication
* `0.0.0.0/0` route pointing to the **NAT Gateway** for outbound internet access

This lets private resources reach the internet without being directly exposed. ([AWS Documentation][3])

### 10. Internet Gateway

An **Internet Gateway (IGW)** is attached to a VPC to enable communication between resources in the VPC and the internet. A public subnet uses a route to the IGW, and the instance also needs a public IP or Elastic IP for internet communication. ([AWS Documentation][2])

### 11. NAT Gateway

A **NAT Gateway** allows instances in a private subnet to initiate outbound connections to the internet or external services, while blocking unsolicited inbound internet connections to those instances. A **public NAT Gateway** is created in a **public subnet** and must be associated with an **Elastic IP**. ([AWS Documentation][3])

### 12. Elastic IP

An **Elastic IP** is a static public IPv4 address in AWS. In your VPC design, it is commonly used with a **public NAT Gateway** so private subnet traffic can go out to the internet. ([AWS Documentation][3])

### 13. Public IP

A **public IP** allows a resource to communicate directly with the internet, when routing also supports it. Public-facing EC2 instances in public subnets generally use a public IP or Elastic IP. ([AWS Documentation][2])

### 14. Private IP

A **private IP** is used for communication inside the VPC or connected private networks. Resources in both public and private subnets have private IPs. Private subnet resources normally rely only on private IPs internally. ([AWS Documentation][2])

### 15. Security Group

A **Security Group** is a virtual firewall attached to an instance or ENI level resource. It controls inbound and outbound traffic at the resource level. In interviews, remember: **Security Group is resource-level security**. AWS VPC docs distinguish this from subnet-level NACLs. ([AWS Documentation][1])

### 16. NACL

A **Network ACL (NACL)** is a subnet-level security layer. Each subnet must be associated with a NACL. If you do not explicitly associate one, the subnet uses the default NACL. In interviews, remember: **NACL is subnet-level security**. ([AWS Documentation][5])

### 17. Availability Zone in VPC design

A VPC is regional, but subnets are AZ-specific. That is why we create subnets in multiple AZs for high availability. In your project, you used **2 public and 2 private subnets across 2 AZs**, which is the correct production-style pattern. ([AWS Documentation][1])

### 18. Bastion Host

A **bastion host** is an EC2 instance placed in a public subnet and used as a controlled entry point to reach resources in private subnets. In your project, it was the only path used to access private EKS worker nodes. This is an architectural pattern built on VPC networking. The VPC docs show the public/private pattern and internet access rules that make this design possible. ([AWS Documentation][2])

---

## How all these terms connect together

### 19. Public subnet traffic flow

For a resource in a public subnet to access the internet:

* subnet route table must point `0.0.0.0/0` to the Internet Gateway
* the instance must have a public IP or Elastic IP

Then traffic goes:
**Instance → Route Table → Internet Gateway → Internet**. ([AWS Documentation][2])

### 20. Private subnet outbound traffic flow

For a private subnet resource to access the internet:

* private subnet route table points `0.0.0.0/0` to NAT Gateway
* NAT Gateway sits in a public subnet
* NAT Gateway uses the Internet Gateway for internet-bound traffic

Then traffic goes:
**Private Instance → Private Route Table → NAT Gateway → Internet Gateway → Internet**. External systems cannot initiate unsolicited inbound connections to the private instance. ([AWS Documentation][3])

### 21. Internal communication inside VPC

Resources within the same VPC can communicate using the **local route** that is automatically present in route tables. This is why resources in different subnets inside the same VPC can still talk to each other, subject to security rules. ([AWS Documentation][4])

---

## VPC in your project

### 22. How you used VPC

In your project, VPC was the complete network foundation:

* one custom VPC
* two public subnets
* two private subnets
* subnets spread across two AZs
* Internet Gateway for public subnets
* NAT Gateway in public subnet for private subnet outbound internet
* bastion host in public subnet
* EKS worker nodes in private subnets
* no public IP for private nodes

This is a production-style VPC design because it separates internet-facing administration from internal compute resources. ([AWS Documentation][1])

### 23. Best project explanation

“In my project, I created a custom VPC as the base network layer. Inside it, I created two public and two private subnets across two Availability Zones. I attached an Internet Gateway for public internet access and used a NAT Gateway in the public subnet so private resources could get outbound internet securely. I placed the bastion host in the public subnet and kept EKS worker nodes in private subnets without public IPs for better security.”

---

## Core concept questions and answers

### 24. What is a VPC?

A VPC is a logically isolated virtual network in AWS where we launch resources securely.

### 25. Is VPC regional or AZ-specific?

VPC is **regional**.
Subnets inside it are **AZ-specific**. ([AWS Documentation][1])

### 26. What is a subnet?

A subnet is a range of IP addresses inside a VPC, and it exists in one Availability Zone only. ([AWS Documentation][1])

### 27. What makes a subnet public?

A subnet is public when its route table has a route to the Internet Gateway, and the resource has a public IP or Elastic IP if it needs internet communication. ([AWS Documentation][2])

### 28. What makes a subnet private?

A subnet is private when it does not have a direct route to the Internet Gateway. ([AWS Documentation][2])

### 29. What is the use of a route table?

A route table decides where network traffic from a subnet or gateway is directed. ([AWS Documentation][4])

### 30. Why is NAT Gateway needed?

NAT Gateway allows private subnet resources to access the internet outbound without allowing direct inbound internet access to them. ([AWS Documentation][3])

### 31. Why is NAT Gateway placed in a public subnet?

Because a public NAT Gateway must use an Elastic IP and send traffic through the Internet Gateway, so it must be in a public subnet. ([AWS Documentation][3])

### 32. What is the difference between Security Group and NACL?

Security Group is **resource-level** security.
NACL is **subnet-level** security. ([AWS Documentation][5])

### 33. Why do we create subnets in multiple AZs?

To improve availability and resilience if one Availability Zone fails. ([AWS Documentation][1])

---

## Project-based interview questions and answers

### 34. Why did you use a custom VPC instead of default VPC?

Because custom VPC gives full control over subnet design, routing, NAT, security, and production-style architecture.

### 35. Why did you put worker nodes in private subnets?

To reduce attack surface and avoid exposing the worker nodes directly to the internet.

### 36. Why did you keep bastion host in a public subnet?

Because bastion host must be reachable for admin access, and then it can be used to connect to private resources.

### 37. Why did private nodes still need internet access?

They may need outbound access for package downloads, image pulls, updates, or communication with external services, so NAT Gateway was used.

### 38. Why did you create subnets across two AZs?

For high availability and better resilience of the EKS environment.

### 39. How did your private EKS nodes reach the internet?

Through the private route table to the NAT Gateway, then through the Internet Gateway. ([AWS Documentation][3])

---

## Scenario-based questions and answers

### 40. If I name a subnet “public-subnet,” does it become public automatically?

No.
It becomes public only when routing and internet access are configured correctly.

### 41. If a private subnet instance has a public IP, can it access the internet without an IGW route?

No.
Without a route to the Internet Gateway, internet communication will not work. AWS specifically notes that even public IPs are not enough if there is no route to the IGW. ([AWS Documentation][2])

### 42. Can resources in private subnet receive direct inbound internet traffic through NAT Gateway?

No.
NAT Gateway supports outbound initiation from private subnet resources; unsolicited inbound internet connections are not allowed. ([AWS Documentation][3])

### 43. What happens if you place NAT Gateway in a private subnet?

That design is wrong for public internet access. A public NAT Gateway must be created in a public subnet and associated with an Elastic IP. ([AWS Documentation][3])

### 44. If one AZ goes down, does the VPC go down?

No.
VPC is regional. A multi-AZ subnet design helps the architecture continue from the remaining AZ. ([AWS Documentation][1])

---

## Crisp revision notes

* **VPC = private network in AWS**
* **VPC is regional**
* **Subnet is AZ-specific**
* **CIDR = IP range**
* **Public subnet = route to IGW**
* **Private subnet = no direct route to IGW**
* **Route table controls traffic**
* **IGW gives internet path**
* **NAT Gateway gives outbound internet to private subnets**
* **Public NAT Gateway stays in public subnet**
* **Security Group = resource-level**
* **NACL = subnet-level**
* **Bastion host = public entry point to private resources**
* **My project used custom VPC with 2 public + 2 private subnets across 2 AZs**

---

## Mistakes to avoid in interview

Do not say:

* “VPC is inside subnet”
* “VPC is AZ-specific”
* “Subnet becomes public by name”
* “NAT Gateway allows inbound internet to private instances”
* “Private subnet can never use internet”
* “Route table is optional for subnet behavior”

---

## One final interview-ready answer

“A VPC is a logically isolated virtual network in AWS and it is the base networking layer for cloud infrastructure. Inside a VPC, we create subnets, define IP ranges using CIDR, control traffic using route tables, and enable internet communication using Internet Gateway and NAT Gateway. In my project, I created a custom VPC with two public and two private subnets across two Availability Zones. I placed the bastion host in the public subnet and the EKS worker nodes in private subnets, so the nodes were secure and still had outbound internet access through the NAT Gateway.”


