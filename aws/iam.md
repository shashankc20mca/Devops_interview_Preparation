# IAM — Full Notes from A to Z

## What is IAM?

**IAM (Identity and Access Management)** is the AWS service used to securely control **who can access what** in your AWS account and **what actions they can perform**. It handles both **authentication** and **authorization**. ([AWS Documentation][1])

---

## Core terms under IAM

### Identity

An **identity** is who is making the request in AWS. Common IAM identities are **users** and **roles**. ([AWS Documentation][2])

### Authentication

**Authentication** means verifying **who you are**. Example: signing in with username/password, access keys, or temporary credentials. ([AWS Documentation][1])

### Authorization

**Authorization** means deciding **what you are allowed to do** after authentication. This is controlled by IAM permissions and policies. ([AWS Documentation][1])

### IAM User

An **IAM user** is an identity created in an AWS account for a person or workload that needs permissions. AWS recommends preferring roles and temporary credentials where possible. ([AWS Documentation][2])

### IAM Group

An **IAM group** is a collection of IAM users. Permissions can be assigned to the group so all users in that group get the same access. ([AWS Documentation][3])

### IAM Role

An **IAM role** is an identity with permissions that is **assumed** when needed. It is not tied to one permanent person and does not have long-term credentials like a password or access keys. ([AWS Documentation][4])

### Policy

A **policy** is a JSON document that defines permissions. Policies decide whether a user or role can perform specific actions on AWS resources. ([AWS Documentation][5])

### Permissions

**Permissions** are the allowed or denied actions defined by policies. Example: permission to create an EC2 instance or read an S3 bucket. ([AWS Documentation][5])

### Least Privilege

**Least privilege** means giving only the minimum permissions required to do a task, and nothing extra. AWS recommends this as a core IAM best practice. ([Amazon Web Services, Inc.][6])

### Root User

The **root user** is the account identity created when the AWS account is first opened. It has full access to all resources in the account. AWS strongly recommends using it only for tasks that require root and protecting it with MFA. ([AWS Documentation][7])

### MFA

**MFA (Multi-Factor Authentication)** adds an extra verification step during sign-in. AWS supports MFA for root users and IAM users, and recommends using it for stronger account security. ([AWS Documentation][8])

### Temporary Credentials

**Temporary credentials** are short-lived AWS credentials. They are the basis for IAM roles and federation and are preferred over long-term static credentials in many cases. ([AWS Documentation][9])

### Access Keys

**Access keys** are long-term programmatic credentials used by IAM users for CLI or API access. In practice, roles and temporary credentials are preferred where possible. ([AWS Documentation][10])

### Trust Policy

A **trust policy** defines **who is allowed to assume a role**. For example, EC2 or EKS can be trusted to assume a role. ([AWS Documentation][4])

### Instance Profile

An **instance profile** is the container used to attach an IAM role to an EC2 instance. EC2 uses the instance profile to provide the role’s credentials to the instance. ([AWS Documentation][11])

### IRSA

**IRSA (IAM Roles for Service Accounts)** lets EKS Pods use IAM roles through Kubernetes service accounts, so applications in Pods can access AWS services without relying only on the node’s role. ([AWS Documentation][12])

---

## How IAM was used in your project

In your project, IAM was used mainly in three places:

### 1. EKS cluster IAM role

You created an IAM role for the **EKS control plane**. EKS requires a cluster IAM role for each cluster. ([AWS Documentation][13])

### 2. EC2 / worker node IAM role

You created an IAM role for the **worker nodes** so the EC2 instances in the node group could interact with required AWS services. EC2 uses an instance profile to attach the role. ([AWS Documentation][11])

### 3. OIDC / IRSA-related permissions

For components like the **EBS CSI driver** or other Pod-level AWS access, IAM and OIDC-based role association are used so Pods can assume dedicated roles instead of using broad node permissions. ([AWS Documentation][12])

---

## Core concept questions and answers

### What is IAM?

IAM is the AWS service used to control access to AWS resources securely. It manages authentication and authorization. ([AWS Documentation][1])

### What is the difference between authentication and authorization?

Authentication verifies who you are. Authorization decides what you are allowed to do. ([AWS Documentation][1])

### What is the difference between IAM user and IAM role?

An IAM user is a long-term identity created in the account. An IAM role is an assumable identity with permissions and typically uses temporary credentials. ([AWS Documentation][4])

### What is a policy in IAM?

A policy is a JSON document that defines permissions for identities or resources. ([AWS Documentation][5])

### What is least privilege?

Least privilege means granting only the permissions required for a task and nothing more. ([Amazon Web Services, Inc.][6])

### What is the root user?

The root user is the original account identity with full access to everything in the AWS account. ([AWS Documentation][7])

### Why is MFA important in IAM?

MFA adds an extra layer of security and helps protect root and IAM user sign-ins. ([AWS Documentation][8])

### What are temporary credentials?

Temporary credentials are short-lived AWS credentials commonly used with roles and federation. ([AWS Documentation][9])

### What is an instance profile?

An instance profile is how an IAM role is attached to an EC2 instance. ([AWS Documentation][11])

---

## Project-based interview questions and answers

### Why did you create separate IAM roles for EKS and EC2 worker nodes?

Because the EKS control plane and worker nodes perform different functions and should have separate permissions. This follows least privilege and keeps access more controlled. The EKS cluster requires a cluster IAM role, while EC2 nodes use their own role through an instance profile. ([AWS Documentation][13])

### Why not use one admin role for everything in the project?

Because giving broad permissions to all components is not secure. Separate roles reduce unnecessary access and follow the principle of least privilege. ([Amazon Web Services, Inc.][6])

### How do EC2 worker nodes get AWS permissions?

By attaching an IAM role to the EC2 instances through an instance profile. ([AWS Documentation][11])

### Why is IAM important in EKS projects?

Because EKS needs IAM for cluster creation, node permissions, and Pod-to-AWS service access through mechanisms like IRSA. ([AWS Documentation][13])

### Why is OIDC/IRSA useful in EKS?

It lets a specific Kubernetes service account assume a specific IAM role, so Pod permissions can be controlled separately from node permissions. ([AWS Documentation][12])

---

## Scenario-based questions and answers

### A developer needs CLI access to AWS. Would you prefer access keys or a role?

Prefer a role and temporary credentials where possible, because AWS recommends using roles and temporary credentials over long-term credentials in many scenarios. ([AWS Documentation][14])

### Your EC2 instance must access AWS services securely. What should you do?

Attach an IAM role to the instance through an instance profile instead of storing access keys on the server. ([AWS Documentation][11])

### A Pod in EKS needs access to S3. Should you give that permission to the whole node role?

Prefer using IRSA so only that Pod’s service account gets the required AWS permissions. ([AWS Documentation][12])

### Someone regularly uses the root user for daily AWS work. Is that good practice?

No. AWS strongly recommends avoiding routine use of the root user and protecting it with MFA. ([AWS Documentation][7])

### A user can sign in but cannot create an EC2 instance. What is the likely IAM reason?

They are authenticated, but their IAM permissions or policies do not authorize that action. ([AWS Documentation][1])

---

## Crisp revision notes

* **IAM = who can access what in AWS**
* **Authentication = who you are**
* **Authorization = what you can do**
* **User = long-term identity**
* **Role = assumable identity with temporary credentials**
* **Policy = JSON permissions document**
* **Least privilege = minimum required access**
* **Root user = full account access, avoid daily use**
* **MFA = extra security layer**
* **Instance profile = attach role to EC2**
* **IRSA = attach role to EKS service account**

---

## Best interview-ready answer

**“IAM is AWS Identity and Access Management. It is used to control who can access AWS resources and what actions they can perform. It works through users, roles, and policies. In my project, I used separate IAM roles for the EKS control plane and EC2 worker nodes, following least privilege. I also used IAM concepts related to OIDC and IRSA for service-level access in EKS.”** ([AWS Documentation][1])

Yes. Very simply:

### Service Account

A **service account** in Kubernetes is an identity used by a **Pod/application inside the cluster**.
It is how the Pod identifies itself to Kubernetes.

### OIDC

**OIDC (OpenID Connect)** is an identity standard.
In EKS, it is used to create a **trust link between the Kubernetes cluster and AWS IAM** so AWS can trust tokens issued for Kubernetes service accounts. ([AWS Documentation][1])

### IRSA

**IRSA (IAM Roles for Service Accounts)** means attaching an **AWS IAM role** to a **Kubernetes service account**.
Then any Pod using that service account gets only that IAM role’s AWS permissions, instead of using broad node-level permissions. ([AWS Documentation][2])

### Easy flow

* **Service account** = Kubernetes identity for Pod
* **OIDC** = trust mechanism between EKS and AWS IAM
* **IRSA** = use that trust to give a Pod its own IAM role

### One-line interview answer

**“A service account is a Kubernetes identity for Pods, OIDC is the trust mechanism between EKS and AWS IAM, and IRSA is the method of assigning an IAM role to a Kubernetes service account so Pods get fine-grained AWS permissions.”**
