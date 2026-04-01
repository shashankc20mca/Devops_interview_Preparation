```md
# Fundamentals of devops

## 1. What is DevOps?
DevOps is the combination of development and operations to deliver software quickly, reliably, and efficiently through collaboration and automation.

## 2. Why was DevOps introduced or main goals of DevOps?
DevOps was introduced to remove the gap between development and operations teams. Earlier, both teams worked separately, which caused slow releases, manual errors, and deployment issues. DevOps helped solve this by improving collaboration, automation, faster delivery, and system reliability.

## 3. What problems does DevOps solve?
DevOps solves problems like slow software delivery, poor communication between teams, manual deployment errors, environment mismatch issues, and slow bug fixing. It improves collaboration, automates repetitive tasks, makes deployments faster and more reliable, and helps teams monitor and fix issues quickly

## 4. What is the difference between traditional software development and DevOps
“In traditional software development, development and operations teams work separately. Developers write the code, and later the operations team deploys and manages it. This often causes delays, communication gaps, and deployment issues.

In DevOps, both teams work together throughout the software lifecycle. The process is more automated, releases are faster, testing and deployment are continuous, and issues are resolved more quickly.

## 5. What are the different phases in the DevOps lifecycle? explain in one line each phase
DevOps lifecycle is the continuous process following below steps

- Planning – decide what feature or change needs to be developed.  
- Development – developers write the application code.  
- Build – the code is compiled and packaged into a usable form.  
- Testing – the application is checked for bugs and quality issues.  
- Release – the tested build is prepared for deployment.  
- Deployment – the application is moved to the production environment.  
- Operations – the application is managed and kept running smoothly.  
- Monitoring – performance, errors, and system health are continuously tracked.

## 6. what is sdlc and different phases of it ?
“SDLC stands for Software Development Life Cycle. It is a step-by-step process used to develop software in a structured way.”

Phases of SDLC:

- Requirement gathering – collect and understand what the customer needs.  
- Planning – decide timeline, cost, resources, and approach.  
- Design – prepare the system design, architecture, and flow.  
- Development – write the actual code.  
- Testing – check the software for bugs and verify requirements.  
- Deployment – release the software to users or production.  
- Maintenance – fix issues, update features, and support the software after release.

## 7. Different types of sdlc models
Different types of SDLC models are different ways to develop software depending on project need.

- Waterfall model – work is done step by step, one phase after another.  
Example: building payroll software where requirements are fixed from the beginning.  

- V-Model – every development phase has a related testing phase.  
Example: in a banking app, after design, proper testing is planned for each module.  

- Iterative model – first build a basic version, then improve it again in cycles.  
Example: first create a simple shopping app, then add wishlist, offers, and tracking later.  

- Incremental model – product is delivered in small parts or modules.  
Example: first release login module, then payment module, then order tracking.  

- Spiral model – development is done in cycles with focus on risk analysis.  
Example: aerospace or defense software where risk checking is very important.  

- Agile model – software is developed in small sprints with continuous feedback.  
Example: in a startup app, features are added every 2 weeks based on user feedback.  

- DevOps model – development, testing, deployment, and monitoring happen continuously with automation.  
Example: in an e-commerce app, after code is written, it is automatically tested and deployed using CI/CD.  

## 8. what problem of sdlc does the devops solves
“For example, in traditional SDLC, if a team develops a new login feature, the developer first writes the code and then hands it over to the testing team. After testing is completed, it is passed to the operations team for deployment. Since these teams work separately, there can be delays in handover, miscommunication, environment issues, and late bug fixing. If a bug is found at the deployment stage, the work may again go back to the development team, which increases time.

In DevOps, the process is more connected and automated. Once the developer writes the code and pushes it to the repository, an automated pipeline can start. The code is built, tested, and prepared for deployment automatically. If there is any issue, it is found early, and the team can fix it quickly. After deployment, monitoring tools also check whether the feature is working properly in production.

So in traditional SDLC, the process is slower because teams work in separate stages, but in DevOps, the same process becomes faster, continuous, and more reliable because of collaboration and automation.”

## 9. What is DevSecOps?
“DevSecOps means integrating security into every stage of the DevOps lifecycle, instead of treating security as a separate final step. In normal development, code is developed first, tested, deployed, and only later security may be checked. That is risky because vulnerabilities may be found very late. In DevSecOps, security checks start from the development and CI/CD stages itself.

For example, when a developer pushes code to Git, the pipeline can automatically run security tools. SonarQube can check the code quality and also identify security issues like bad coding practices, possible bugs, code smells, and some vulnerability patterns. Trivy can scan dependencies, file systems, and Docker images to detect known vulnerabilities. So before the application is deployed, these tools help identify security risks early.

This makes the software more secure, reduces risk, and avoids fixing major security issues in production.

## 10. What is the difference between Continuous Integration, Continuous Delivery, and Continuous Deployment?
CI stands for Continuous Integration :It means developers regularly push code to a shared repository, and that code is automatically built and tested so issues are found early.

CD stands for Continuous Delivery  
It is the process after CI,it is deployed to a staging/pre-production environment needs manual approval to deploy in production environment

or Continuous Deployment.  
It is the process after CI,it is directly deployed to the production environment without manual approval.”

---

## What happens in a CI pipeline?
“In a CI pipeline, when code is pushed to the repository, it is automatically fetched, built, and tested. It helps find errors early before moving to the next stage.”

### Example:
“For example, if a developer pushes login code, the CI pipeline checks whether the code builds properly and whether test cases pass.”

## What happens in a CD pipeline?
“In a CD pipeline, after CI is successful, the application is packaged and deployed to staging or production. It makes release faster and more reliable.”

### Example:
“For example, after the login feature passes CI, the CD pipeline deploys it to staging, or directly to production if it is continuous deployment.”

## How does CI/CD reduce manual errors?
CI/CD reduces manual errors by automating build, testing, and deployment steps. So there is less chance of mistakes like wrong file, missed step, or incorrect deployment.

## What is rollback in deployment?
Rollback in deployment means going back to the previous stable version of the application if the new deployment causes an issue

## What is a pipeline in DevOps?
“A pipeline in DevOps is an automated sequence of steps used to move code from development to deployment. It usually includes stages like build, test, and deploy

## What is the difference between build and release?
“Build means converting the source code into a runnable package or artifact.  
Release means making that built package ready for deployment

### Example:
“If a Docker image is built for a login feature, release means tagging it like v1.2.0, pushing it to Docker Hub or ECR, and marking it as the version to deploy to staging or production.”

## What is the difference between manual deployment and automated deployment?
“In manual deployment, a person may log in to the server and deploy the app themselves. In automated deployment, we use tools like  Argo CD is a GitOps continuous delivery tool for Kubernetes

Argo CD watches the Git repository, and when it detects a change in manifests, it automatically syncs the cluster and deploys the new version. So instead of deploying manually, we just update Git, and Argo CD handles the deployment.

## What is infrastructure in DevOps?
“Infrastructure means the resources needed to run an application, like servers, virtual machines, storage, networking, load balancers, and databases.”

## What is Infrastructure as Code and its importance?
“Infrastructure as Code means managing and creating infrastructure using code instead of doing it manually. We write configuration files, and tools like Terraform create the resources automatically.”

Infrastructure as Code is important because it makes infrastructure creation fast, consistent, and repeatable. It reduces manual errors, saves time, and helps teams manage the same setup across different environments

## What is configuration management and Why do we need configuration management tools?
Configuration management is the process of maintaining systems in a desired and consistent state. It helps manage things like software installation, package updates, service configuration, users, and permissions.”

For example, if 10 servers all need Nginx installed and configured the same way instead of manually logging into each server and installing Nginx, Ansible can do it on all servers using one playbook

## What is the difference between Infrastructure as Code and configuration management?
“IaC creates the infrastructure, and configuration management configures the infrastructure.”

Example:“Terraform can create an EC2 instance, and Ansible can go inside that EC2 instance and install Nginx.”

## What is an environment in software delivery? What is the difference between development, testing, staging, and production environments?
 “An environment is a setup where the application is run and tested for a specific purpose

Difference between environments:

- Development – where developers build and test their code changes.  
- Testing – where QA or team checks whether the application works correctly.  
- Staging – a production-like environment used for final validation before release.  
- Production – the live environment used by real users.  

Simple example:  
“A new login feature is first built in development, checked in testing, verified in staging, and finally released in production.”

## What is environment drift?
“Environment drift means environments slowly become different over time because of manual changes, missing updates.

### Example:
“For example, one server may have a newer software version, while another server still has the old one.”

## Why is consistency across environments important?
because it reduces deployment issues, avoids surprises in production, and makes testing more reliable.

## What is scaling in cloud and DevOps?
Scaling means increasing or decreasing resources based on application demand, so the application can handle traffic properly.  
Vertical scaling means scaling up one machine, and horizontal scaling means scaling out by adding more machines.”

### Example:
“If one server is slow, giving it more RAM is vertical scaling. Adding two more servers behind a load balancer is horizontal scaling.”

## What is the difference between mutable and immutable infrastructure?
In mutable infrastructure, if I want to update Java from version 11 to 17, I log in to the same server and update Java there.  
In immutable infrastructure, I do not touch the old server. I create a new server image with Java 17 already installed, launch a new server from it, move the application to that new server, and then remove the old server.”

## What is idempotency in DevOps tools?
“Idempotency means if we run the same command or configuration multiple times, the result stays the same and no unwanted extra changes happen.”

### Example
For example, if Ansible is told to install Nginx, running the playbook again will not install it again and again. It will just check and keep the system in the required state.

# Cloud

## What is cloud computing?
“Cloud computing means getting IT resources like servers, storage, databases, and networking from a cloud provider over the internet, whenever needed, instead of buying and managing physical hardware yourself.”

### Example:
“For example, instead of purchasing a server and keeping it in your office, you can create a server in AWS in a few minutes and use it online.”

## What is the difference between traditional infrastructure and cloud infrastructure or on-premises and cloud?
Traditional infrastructure uses physical servers and manual setup. Cloud infrastructure provides virtual resources on demand

In traditional infrastructure, setting up a new server may take days or weeks. in cloud we can create it in minutes using tools like Terraform.  
Cloud gives advantages like fast provisioning, easy scaling, pay-as-you-go pricing, high availability, and easy automation.  
Traditional infrastructure has disadvantages like slow setup, high upfront cost, difficult scaling, manual maintenance, and hardware dependency. Because of this, it is less flexible and slower compared to cloud.”

## What are the disadvantages of cloud computing?
internet dependency, less direct control over physical infrastructure, vendor lock-in, and sometimes higher cost if resources are not managed properly.

## What are the main types of cloud computing?
In public cloud, the physical servers are shared between many customers,only there will be logical layer separation not at hardware level.  
In private cloud, the physical servers are used only by one organization.”

- Public cloud (AWS normal EC2)  
→ your EC2 runs on hardware also used by other customers (but isolated)  

- Private cloud  
→ your company gets its own dedicated hardware (no sharing)  

why private cloud used ?” → “for security, compliance, and full control.”

Hybrid cloud means using both public cloud and private infrastructure together. Some workloads run in private cloud, and some run in public cloud.”

### Example:
“A company keeps sensitive data in private cloud, but runs its application or website on AWS public cloud.

Multi-cloud means using services from more than one cloud provider, like AWS, Azure, and GCP.

### Example:
“For example, a company may run its application on AWS, store backups on GCP, and use Azure for analytics.

Note: Private cloud can be either on-premises infrastructure or dedicated infrastructure provided by a cloud provider like AWS. The key point is that it is used by only one organization and not shared.

## What are cloud service models?
“Cloud service models define what level of control we get over infrastructure and what the provider manages.”

### IaaS (Infrastructure as a Service)
“In IaaS, I create a server and manage everything.” “I manage everything from OS to application.”

### PaaS (Platform as a Service) (Elastic Beanstalk)
“In PaaS, I just give my code, and AWS handles infrastructure.”

What I do:

- upload code  
- AWS creates server, load balancer, scaling  

AWS Elastic Beanstalk is a PaaS service that helps deploy and manage applications without worrying about infrastructure. It automatically handles server provisioning, load balancing, scaling, and monitoring.”

Interview line:  
“I focus only on code, AWS manages infrastructure.”

### SaaS (Software as a Service) (Gmail / Shopify)
“In SaaS, I don’t build anything, I just use the software.”

What I do:

- just login and use  

Simple understanding

- If you build the app → you are a provider  
- If you use someone else’s app → that is SaaS  

________________________________________

### Example
“If I build a CRM application and users log in and use it, for them it is SaaS, but for me it is my application running on IaaS or PaaS.”

## What is virtualization?
“Virtualization means splitting one physical server into multiple virtual servers/machines.”

### Example
“When you create an EC2 instance in AWS,  
you are not getting a full physical server,  
you are getting a virtual machine created using virtualization on actual physical server, say part of entire physical server.”

Hypervisor is the software that enables virtualization by creating and managing virtual machines on physical hardware.

Note: physical server runs a hypervisor, and on top of that hypervisor, multiple virtual machines are created. Each VM behaves like a separate system.”

## What is a virtual machine (VM)?
“A virtual machine is a software-based computer that runs on a physical server and behaves like a real system with its own OS.”

### Example:
“An AWS EC2 instance is a virtual machine.”

## What is the difference between a physical server and a virtual machine?
Physical server is real hardware, VM is a virtual system created on that hardware.”

## What is the difference between a virtual machine and a container?
VMs run full OS and are heavier,

“Container images do not have a full OS. They include only the required libraries, dependencies, and application code, and they use the host OS kernel.

## What is scalability in cloud?
“Scalability means the ability to increase or decrease resources to handle workload.”

### Example:
“If more users come to a website, we can add more servers.”

## What is elasticity in cloud?
“Elasticity means automatically increasing or decreasing resources based on demand.”

### Example:
“If traffic increases, instances are added automatically, and when traffic decreases, they are removed.”

## What is high availability?
“High availability means keeping the application available with very less downtime.”

## What is fault tolerance?
“Fault tolerance means the system continues working even if one component fails.”

### Example:
“For example, if one instance crashes, another instance takes over without stopping the application.”

## Why is automation easier in cloud environments?
“Automation is easier in cloud because resources can be created, configured, and managed through APIs and tools like Terraform, instead of manual hardware setup.”

“By API, I mean AWS, Azure, or GCP provide interfaces through which tools and scripts can create and manage resources automatically.

### Example:
“When Terraform creates an EC2 instance, it is actually sending a request to the AWS API to launch that instance.”

## What is disaster recovery in cloud
“Disaster recovery means bringing the application back to working state after a major failure. That failure can be server loss, database corruption, data loss, or even full region failure.”

Simple example:  
“If my application is running in one AWS region and that region goes down, disaster recovery means I restore the app and data from backup or start it in another region so users can use it again.”

## What is pay-as-you-go in cloud?
“Pay-as-you-go means we only pay for the cloud resources we use, not for unused capacity.”

## What is a region in cloud?
“A region is a geographical location where cloud data centers are located.”

### Example:
“AWS has regions like Mumbai, Singapore, and US-East.”

________________________________________

## What is an availability zone (AZ)?
“An availability zone is a separate data center within a region.”

### Example:
“One region can have multiple AZs like AZ1, AZ2, AZ3.”

## Why multiple AZs?
“Cloud providers have multiple AZs to ensure high availability and fault tolerance. If one AZ fails, another can continue running the application.”

### Example
“If my app runs in two AZs and one data center fails, users are still served from the other

## What is a data center in cloud computing?
“A data center is a physical facility that contains servers, storage, and networking equipment used to run cloud services.”

## What is a cloud provider?
“A cloud provider is a company that offers cloud services like servers, storage, and databases over the internet.”

### Example:
“AWS provides services like EC2, S3, and RDS.”

## Major cloud providers

- Amazon Web Services (AWS)  
- Microsoft Azure  
- Google Cloud Platform  

## What are the common services provided by cloud platforms?
Common services

- Compute  
→ virtual servers to run applications  
Example: EC2  

- Storage  
→ store files and data  
Example: S3  

- Networking  
→ manage communication between resources  
Example: VPC  

- Database  
→ managed databases  
Example: RDS  

- Security & IAM  
→ manage users and permissions  

- Monitoring & Logging  
→ track performance and logs  
Example: CloudWatch  

## What are the different types of cloud storage?
Different types of cloud storage

- Object storage  
“Stores data as objects (files with metadata). Used for large unstructured data.”  

- “Used when we need to store large files like images, videos, logs, backups.”  
Example: AWS S3  

________________________________________

- Block storage  
“Provides storage as blocks, like a disk attached to a server/instance.”  

- “Used when a server needs a disk to run OS, database, or applications.”  
Example: AWS EBS  

________________________________________

- File storage  
“Provides shared file system that multiple systems/instance can access.”  

- “Used when multiple servers need shared access to the same files.”  
Example: AWS EFS  

Note: Block storage like EBS is typically attached to one EC2 instance at a time, while file storage like EFS can be accessed by multiple instances at the same time.  
S3 cannot be attached to EC2 like a disk; it is accessed via APIs or URLs.

Simple idea

- S3 → cheap storage  
- EBS → medium cost (performance disk)  
- EFS → higher cost (shared + scalable)  

## Difference between manual scaling and auto scaling

- Manual scaling  
“Resources are increased or decreased manually by a person.”  

- Auto scaling  
“Resources are automatically adjusted based on demand or metrics.”

## Why is auto scaling important?
“Auto scaling is important to handle changing traffic automatically. It ensures the application performs well during high load and saves cost during low usage.

## Types of Auto Scaling

- Vertical Scaling  
“Increase or decrease resources of a single instance like CPU or RAM.”  

________________________________________

- Horizontal Scaling  
“Add or remove multiple instances to handle load.”

## What is load balancing in cloud?
“Load balancing distributes incoming traffic across multiple servers so no single server is overloaded.”

## What is serverless computing?
“Serverless computing means running applications without managing servers. The cloud provider handles infrastructure, scaling, and maintenance.”

### Example
“For example, AWS Lambda runs code automatically when triggered, without creating or managing EC2 instances.”

Food delivery app (like Swiggy)

- EC2  
→ runs the main website/app continuously  
→ users are always using it  

- Lambda  
→ runs small tasks when needed, like:  
o	send OTP  
o	process payment  
o	resize image  
o	send email  

________________________________________

Key difference

- EC2 → always ON  
- Lambda → runs only when triggered  

## What are the advantages of serverless?

- no server management  
- automatic scaling  
- pay only when used  
- faster development  

### Example:
“Lambda runs code only when triggered, so no cost when idle.”

________________________________________

## What are the limitations of serverless?

- not suitable for long-running applications  
- execution time limits  
- less control over environment  
- cold start delays  

## What is the principle of least privilege?
“Principle of least privilege means giving only the minimum permissions required to perform a task, nothing extra.”

________________________________________

## Why should it be followed in cloud?
It reduces security risk. If an account or service is compromised, limited permissions prevent major damage.”

## What is the cloud provider responsible for in cloud?
Cloud provider manages:

- hardware (servers, storage)  
- data centers  
- networking infrastructure  
- basic security of cloud  

## What is a security group / firewall rule in cloud?
“A security group is a virtual firewall that controls incoming and outgoing traffic to resources like EC2.”

### Example:
“We can allow only port 22 for SSH or port 80 for HTTP.”

________________________________________

## What is a VPC / virtual network in cloud?
“A VPC is a logically isolated network in the cloud where we launch and manage our resources.”

### Example:
“We create subnets, IP ranges, and control networking inside a VPC.”

## What is a public subnet?
“A public subnet is a subnet that has access to the internet through an internet gateway.”

### Example:
“Instances in public subnet can have public IP and be accessed from the internet.”

________________________________________

## What is a private subnet?
“A private subnet is a subnet that does not have direct access to the internet.”

### Example:
“Instances in private subnet do not have public IP and are not directly accessible from the internet.”

## What is an Internet Gateway?
“An Internet Gateway is a component that allows communication between a VPC and the internet.”

### Example:
“It enables instances in a public subnet to access the internet.”

________________________________________

## What is NAT in cloud networking?
“NAT (Network Address Translation) allows private instances to access the internet without exposing them to incoming traffic.”

________________________________________

## Why do private instances need NAT?
“Private instances need NAT to download updates or access external services while remaining secure and not directly reachable from the internet.”

________________________________________

## What is a public IP?
“A public IP is an IP address that is accessible over the internet.”

### Example:
“Used to access a server from anywhere.”

________________________________________

## What is a private IP?
“A private IP is used within a private network and is not accessible from the internet.”

## What is DNS in cloud?
“DNS (Domain Name System) converts domain names into IP addresses.”

### Example:
“google.com → IP address”

## What is backup in cloud?
“Backup means creating a copy of data to prevent loss.”

________________________________________

## What is snapshot in cloud?
“A snapshot is a point-in-time copy of a storage volume.”

## What is CDN in cloud?
“CDN (Content Delivery Network) is a network of servers that deliver content from locations closer to users.”

________________________________________

## Why is CDN used?
“To reduce latency and improve performance by serving content from nearby locations.”

### Example:
“Images load faster because they come from nearest server.”

## How CDN works?
“CDN stores copies of content in multiple locations (edge servers) and serves the content from the nearest location to the user.”

________________________________________

### Step-by-step

1. User requests a website (image/video)  
2. Request goes to CDN  
3. CDN checks nearest server (edge location)  
4. If content is available → serves immediately  
5. If not → fetches from origin server, caches it, and then serves  

Simple understanding  
First request → slight delay (fetch from origin)  
Next requests → fast (served from CDN cache)  

________________________________________

### Example
“If a user in India opens a website, CDN serves content from a nearby India server instead of a US server.”

## What is replication in cloud?
“Replication means creating and maintaining copies of data in multiple locations.”

________________________________________

## Why is replication important?
“It ensures high availability and data safety. If one location fails, data is still available from another location.”

## What is the difference between self-hosted database and managed database?

-  Self-hosted database  
→ we install and manage database on our server  
→ handle backups, updates, scaling  

-  Managed database  
→ cloud provider manages everything  
→ we just use the database  

### Example
“If I install MySQL on EC2 → self-hosted  
If I use AWS RDS → managed database”

Note: “AWS provides managed databases like RDS, Aurora, DynamoDB, Redshift, ElastiCache, Neptune, DocumentDB, and Keyspaces.”

## What is monitoring in cloud?
“Monitoring means tracking the performance and health of resources like CPU, memory, and traffic.”

### Example:
“Checking CPU usage of EC2.”

## Why is observability important in cloud systems?
observability uses metrics, logs, and traces together to deeply analyze and find the root cause of that problem  
Metrics, Logs, and Traces mean:

________________________________________

### 1. Metrics (numbers)
“Metrics are numerical values that show system performance.”

Examples:

- CPU usage  
- memory usage  
- request count  

👉 tells something is wrong

________________________________________

### 2. Logs (records)
“Logs are detailed records of events happening in the system.”

Examples:

- error messages  
- user actions  
- system logs  

👉 tells what happened

________________________________________

### 3. Traces (request flow)
“Traces show the path of a request as it moves through different services.”

### Example:
“User request → API → service → database → response”

👉 tells where it failed

## What is logging in cloud?
“Logging means storing records of events or activities happening in the system.”

### Example:
“Application logs or error logs.”

## What is alerting in cloud?
“Alerting means sending notifications when something goes wrong or crosses a threshold.”

### Example:
“Alert when CPU usage is above 80%.”

## What is cloud migration?
“Cloud migration means moving applications, data, and infrastructure from on-premises or another environment to the cloud.”

## What are the common challenges in cloud migration?
Moving large data safely without loss or downtime is difficult.  
Applications may face downtime during migration.  
Old applications may not work properly in cloud.  
Old applications may not work properly in cloud.

## What is vendor lock-in in cloud?
“Vendor lock-in means becoming dependent on a single cloud provider’s services, making it difficult to switch to another provider.”

________________________________________

### Example
“If we use many AWS-specific services, moving the application to Azure or GCP becomes difficult.”

## What is encryption in cloud?
“Encryption means converting data into a secure format so only authorized users can read it.”

________________________________________

## Why is encryption important?
“It protects sensitive data from unauthorized access and ensures data security.”

________________________________________

## What is encryption at rest?
“Encryption at rest means data is encrypted when stored in storage like disks, databases, or S3.”

### Example:
“Data stored in S3 is encrypted.”

________________________________________

## What is encryption in transit?
“Encryption in transit means data is encrypted while being transferred over a network.”

### Example:
“Data sent over HTTPS is encrypted.”

## What is the difference between traditional applications and cloud-native applications?
Traditional apps are monolithic and less scalable, while cloud-native apps are built for cloud with microservices, containers, and easy scalability

## Difference between Monolithic and Microservices

- Monolithic architecture  
“Entire application is built as a single unit. All components are tightly coupled.”  

- Microservices architecture  
“Application is divided into small independent services, each handling a specific function.”  

________________________________________

### Example
“E-commerce app:

- Monolithic → login, payment, orders all in one codebase  
- Microservices → separate services for login, payment, orders”  

________________________________________

### Key differences

- Monolithic → hard to scale, harder to update  
- Microservices → easy to scale, independent deployment  

## Why are containers important in cloud?
“Containers are important because they package the application with its dependencies, making it portable, lightweight, and consistent across environments.”

### Example:
“The same container can run in dev, test, and production without issues.”

________________________________________

## Why is Kubernetes used in cloud?
“Kubernetes is used to manage and orchestrate containers. It handles deployment, scaling, load balancing, and ensures containers are running properly.”

### Example:
“If a container fails, Kubernetes automatically restarts it.”

## What is cost optimization in cloud?
“Cost optimization means using cloud resources efficiently to reduce unnecessary spending.”

________________________________________

## How can companies reduce cloud costs?

- stop unused resources  
- use auto scaling  
- choose right instance size  
- use reserved/spot instances  
- monitor usage regularly  

________________________________________

## What causes unexpected cloud bills?

- unused running instances  
- over-provisioned resources  
- high data transfer  
- lack of monitoring  
- not shutting down services  

________________________________________

### Example
“Leaving an EC2 instance running without use can increase cost.”

## What is the difference between on-demand, reserved, and spot instances?

### On-Demand Instances
“Pay for instances as you use them, with no long-term commitment.”

Used when:  
flexibility is needed

________________________________________

### Reserved Instances
“Commit to using instances for 1 or 3 years to get lower cost.”

Used when:  
workload is predictable

________________________________________

### Spot Instances
“Use unused AWS capacity at very low cost, but instances can be terminated anytime.”

Used when:  
workload is flexible or non-critical

________________________________________

### Example
“Production app → reserved  
testing → on-demand  
batch jobs → spot”

Batch job = background work on large data (not urgent)

Real-world example: Amazon / Flipkart orders  
At the end of the day:

- company needs to  
o	calculate total sales  
o	generate reports  
o	analyze user data  

👉 This does not need to happen instantly for users

## Why Spot instances?
Because:

- if instance stops → job can restart  
- not urgent  
- saves cost  

## What is cloud availability?
“Cloud availability means how consistently a system or service is accessible and running without interruption.”

________________________________________

## What does uptime mean?
“Uptime is the time when the system is working and available to users.”

________________________________________

## What does downtime mean?
“Downtime is the time when the system is not available or not working.”

## Why do DevOps engineers need cloud knowledge?
“DevOps engineers need cloud knowledge because modern applications are mostly deployed on cloud, and DevOps work includes managing infrastructure, CI/CD, scaling, security, and monitoring in cloud.”

## What is the role of DevOps in cloud environments?
DevOps in cloud helps automate infrastructure, deployment, scaling, monitoring, and application delivery. It makes software release faster and more reliable.”

### Example:
“For example, using Terraform to create AWS infrastructure and Argocd to deploy the application automatically.”
```
