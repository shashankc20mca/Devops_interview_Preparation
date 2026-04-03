# EKS — Full Notes from A to Z

## What is EKS?

**EKS (Elastic Kubernetes Service)** is AWS’s managed Kubernetes service. It helps you run Kubernetes on AWS without managing the Kubernetes control plane yourself. AWS manages the control plane, and you manage the worker nodes and workloads. ([AWS Documentation][1])

---

## Core terms under EKS

### Kubernetes Cluster

A **Kubernetes cluster** is a set of machines used to run containerized applications. In EKS, AWS provides the managed Kubernetes environment. ([AWS Documentation][1])

### Control Plane

The **control plane** is the management part of Kubernetes. It manages the cluster and exposes the Kubernetes API. In EKS, the control plane is managed by AWS. ([AWS Documentation][2])

### Worker Nodes / Data Plane

**Worker nodes** are the machines where actual applications run. In EKS, these are typically EC2 instances. AWS docs also refer to the set of worker nodes as the **data plane**. ([AWS Documentation][2])

### Node Group

A **node group** is a group of worker nodes in EKS. It helps you manage multiple nodes together. ([AWS Documentation][3])

### Managed Node Group

A **managed node group** is a node group where EKS automates provisioning and lifecycle management of the EC2 worker nodes. ([AWS Documentation][3])

### Self-Managed Nodes

**Self-managed nodes** are worker nodes that you manage yourself instead of letting EKS manage their lifecycle. ([AWS Documentation][4])

### Cluster Endpoint

The **cluster endpoint** is the Kubernetes API server endpoint used to communicate with the EKS cluster. Endpoint access can be public, private, or both. ([AWS Documentation][5])

### VPC CNI

The **Amazon VPC CNI plugin** provides Pod networking for nodes running on AWS infrastructure. ([AWS Documentation][6])

### OIDC

**OIDC** is the trust mechanism used between EKS and AWS IAM for service-account-based IAM access. EKS clusters have an OIDC issuer URL, and an IAM OIDC provider must exist to use IAM roles for service accounts. ([AWS Documentation][7])

### Service Account

A **service account** is a Kubernetes identity used by Pods inside the cluster.

### IRSA

**IRSA (IAM Roles for Service Accounts)** lets you assign an IAM role to a Kubernetes service account, so Pods can access AWS services with fine-grained permissions. ([AWS Documentation][8])

### Add-on

An **add-on** is a component that adds extra functionality to the cluster, such as VPC CNI, CoreDNS, kube-proxy, or EBS CSI driver. AWS discusses add-on configuration during cluster creation. ([AWS Documentation][9])

---

## Important EKS concepts you must know

* **EKS = managed Kubernetes service**
* **AWS manages the control plane**
* **You manage worker nodes, workloads, and many cluster-level configurations**
* **Worker nodes are commonly EC2 instances**
* **EKS cluster runs inside a VPC**
* **Cluster subnets must be in at least two AZs**
* **Node groups help manage worker nodes**
* **Managed node groups reduce operational work**
* **Cluster endpoint is used to access the cluster**
* **IRSA is used for Pod-level AWS permissions**
* **Pods run on nodes, not on the control plane**

---

## How EKS was used in your project

In your project:

* you created a **custom EKS cluster using Terraform**
* AWS managed the **control plane**
* you created **4 worker nodes**
* worker nodes were placed in **private subnets**
* you used **IAM roles** for the cluster and nodes
* you used **OIDC / IRSA** for components like EBS CSI and autoscaler-related access
* you used **HPA** for application scaling
* you used **Cluster Autoscaler** for node scaling
* your cluster followed a **multi-AZ VPC design**

This is a strong project because it shows:

* EKS architecture understanding
* VPC + subnet integration
* IAM integration
* production-style private node design
* scaling understanding

---

## Core concept questions and answers

### What is EKS?

EKS is AWS’s managed Kubernetes service used to run Kubernetes clusters on AWS. ([AWS Documentation][1])

### What does AWS manage in EKS?

AWS manages the Kubernetes control plane in EKS. ([AWS Documentation][2])

### What do we manage in EKS?

We manage worker nodes, applications, Kubernetes objects, networking choices, IAM integration, and many cluster configurations.

### What is the control plane in EKS?

The control plane is the management layer of Kubernetes that manages the cluster and provides access to the Kubernetes API. In EKS, AWS manages it. ([AWS Documentation][2])

### What are worker nodes in EKS?

Worker nodes are the machines where the actual application Pods run. In EKS, these are usually EC2 instances. ([AWS Documentation][2])

### What is a managed node group?

A managed node group is a group of EC2 worker nodes whose provisioning and lifecycle management are automated by EKS. ([AWS Documentation][3])

### Why does EKS need at least two subnets in different AZs?

EKS requires cluster subnets in at least two different Availability Zones. ([AWS Documentation][10])

### What is the EKS cluster endpoint?

The cluster endpoint is the Kubernetes API server endpoint used to communicate with the cluster. Endpoint access can be public, private, or both. ([AWS Documentation][5])

### What is IRSA in EKS?

IRSA allows Kubernetes service accounts to assume IAM roles so Pods can access AWS services with specific permissions. ([AWS Documentation][8])

---

## Project-based interview questions and answers

### Why did you choose EKS for your project?

Because I wanted a managed Kubernetes service where AWS handles the control plane, while I can focus on nodes, workloads, networking, IAM, and deployment design.

### What part of your EKS cluster was managed by AWS?

The Kubernetes control plane was managed by AWS.

### What part did you manage yourself?

I managed the VPC design, subnets, worker nodes, IAM roles, storage integration, autoscaling setup, and application deployment.

### Why did you place worker nodes in private subnets?

For better security. Private nodes are not directly exposed to the internet, which reduces attack surface.

### Why was your EKS design called production-style?

Because it used a custom VPC, multiple AZs, private worker nodes, bastion-based access, IAM role separation, persistent storage support, and autoscaling.

### Why did you use Terraform for EKS?

Terraform made the cluster infrastructure reusable, repeatable, and easier to recreate in a consistent way.

### Why were IAM roles important in your EKS project?

Because EKS needs IAM for the cluster role, node role, and Pod-level AWS access through IRSA.

---

## Scenario-based questions and answers

### Your Pod is not getting scheduled in EKS. What will you check?

I will check:

* whether worker nodes are ready
* whether there are enough resources on nodes
* whether the Pod has scheduling constraints like taints, tolerations, or node selectors
* whether the node group is healthy

### Your worker nodes are running but Pods still cannot access AWS services. What will you check?

I will check whether the correct IAM permissions are configured, and if Pod-level access is needed, whether IRSA and the service account role mapping are set properly. ([AWS Documentation][8])

### Your EKS cluster creation fails due to subnet issue. What could be the reason?

One likely reason is that the subnets do not meet EKS requirements, such as not being in at least two different AZs or not having enough IP addresses. ([AWS Documentation][10])

### Your private worker nodes need outbound internet access. How will they get it?

Through the VPC design, usually by routing private subnet traffic through a NAT Gateway in a public subnet. AWS EKS best practices note that nodes are commonly created in private subnets. ([AWS Documentation][5])

### A specific Pod needs access only to S3. What is the better approach?

Use IRSA with a dedicated Kubernetes service account and IAM role, instead of giving broad permissions to the whole node role. ([AWS Documentation][8])

---

## Crisp revision notes

* **EKS = managed Kubernetes on AWS**
* **AWS manages control plane**
* **We manage worker nodes and workloads**
* **Worker nodes usually run on EC2**
* **Cluster needs subnets in at least 2 AZs**
* **Node group = group of worker nodes**
* **Managed node group = EKS manages node lifecycle**
* **Cluster endpoint = API access point**
* **IRSA = Pod-level IAM access**
* **Private worker nodes = better security**

---

## Best interview-ready answer

**“EKS is AWS’s managed Kubernetes service. It helps us run Kubernetes without managing the control plane ourselves. AWS manages the control plane, while we manage worker nodes, workloads, networking, IAM, and storage integration. In my project, I built a custom EKS cluster using Terraform inside a custom VPC, placed worker nodes in private subnets across two AZs, and used IAM roles, IRSA, persistent storage, and autoscaling to make it production-like.”** ([AWS Documentation][1])
