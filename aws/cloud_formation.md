In my first project, I created the EKS cluster using the eksctl CLI command. Since I used command-based provisioning and did not maintain a YAML config file in Git, I would describe that as AWS-native cluster automation, not a full Infrastructure as Code setup. eksctl is an AWS/EKS tool that can work through CLI commands or declarative YAML config, but in my case I used the CLI approach.

## Then compare it with your Terraform project like this:

> “In my second project, I used Terraform, which is a proper IaC approach because the infrastructure was explicitly defined in Terraform configuration files, versionable as code, and managed through the Terraform workflow.

## So the clean distinction is:

- eksctl command only = AWS-native provisioning / automation
- eksctl with YAML config in Git = closer to declarative IaC
- Terraform .tf files = clear, full IaC approach

eksctl is mainly an EKS-focused tool not a general-purpose AWS provisioning tool like Terraform or CloudFormation. resources are mostly the ones needed for EKS and tightly related to EKS, such as:

- VPCs and networking needed for the cluster,
- node groups,
- IAM OIDC provider,
- IAM roles/service accounts,
- and EKS add-ons.

## There are 2 methods to create the cluster using eksctl

### 1.eksctl command only = AWS-native provisioning / automation

```bash
eksctl create cluster --name mycluster --region us-east-1
```

= imperative style, because you are issuing a command to create the cluster now.

### 2.eksctl with YAML config in Git = closer to declarative IaC

```bash
eksctl create cluster -f cluster.yaml
```

= declarative style, because you first define the cluster in a YAML file and then apply that definition.

Terraform is a general-purpose Infrastructure as Code tool. In AWS, it can create and manage a very wide range of resources such as VPC, subnets, EC2, IAM, EKS, S3, RDS, Route 53, load balancers, and many more, as long as those resource types are supported by the AWS provide

Terraform being declarative means you write what end result you want, not the step-by-step procedure to build it.

CloudFormation is AWS’s Infrastructure as Code service. It is the service that creates, updates, and deletes AWS resources as stacks from a template.  
In my first project, I did not write CloudFormation templates directly. I used eksctl, which is an EKS-focused tool, and it used CloudFormation-backed stacks underneath to provision the EKS-related resources. CloudFormation itself is general-purpose, while eksctl is only for EKS workflows.

## Ok lets see how cloudformation works

1. You write a template in YAML or JSON.
2. In that template, the Resources section defines what AWS resources should be created, such as EC2, VPC, S3, IAM, load balancers, etc.
3. You deploy that template as a stack. A stack is the actual deployed collection of AWS resources managed together as one unit.
4. CloudFormation then handles the provisioning, dependency order, updates, and deletion of those resources in the form of stack.

## Very simple:

- Template = blueprint / design
- Stack = real infrastructure created from that blueprint on AWS

## Most expected fresher interview questions with answers

### 1. What is CloudFormation?

CloudFormation is an AWS service used to define and provision infrastructure as code using templates. It creates and manages AWS resources as a stack.

### 2. What is a CloudFormation template?

A template is a JSON or YAML text file that describes the AWS resources, properties, and configuration CloudFormation should create.

### 3. What is a stack?

A stack is a collection of AWS resources created and managed together from one CloudFormation template.

### 4. What are Parameters in CloudFormation?

Parameters are input values passed to the template at runtime, such as environment name, instance type, or CIDR block. They help make templates reusable.

### 5. What are Outputs in CloudFormation?

Outputs are values returned after stack creation or update, such as a bucket name, VPC ID, or load balancer DNS name. They become available after the stack operation completes.

### 6. What is a change set? similar to terraform plan

A change set is a preview of the modifications CloudFormation will make to a stack. It shows which resources will be added, modified, or deleted before execution.

### 7. What is drift detection?

Drift detection checks whether the actual resource configuration has changed outside CloudFormation and no longer matches the template definition.

Drift detection alone = report only.  
It does not automatically bring resources back to the template state we need to manually take care of it

### 8. What is StackSets?

StackSets are used to deploy the same CloudFormation stack across multiple AWS accounts and Regions from one central definition.

#### Very simple example

If you create:

- 1 VPC
- 2 subnets
- 1 EC2 instance

in one account and one Region, that is a stack.

If you want the same setup in:

- account A and account B
- us-east-1 and ap-south-1

then you use a StackSet. It will create separate stack instances in those target accounts and Regions from the same template.

### 9. What happens if stack creation fails?

CloudFormation generally rolls back the stack changes so the environment returns to the earlier stable state, depending on the operation settings and rollback behavior.

### 10. Why is CloudFormation better than manual resource creation?

It gives consistency, repeatability, version control, easier recovery, and reduces manual mistakes. It also makes infra reviewable just like source code. This is the main interview angle.

### 11. Which format can templates use?

CloudFormation templates are commonly written in YAML or JSON.

### 12. Can CloudFormation manage existing resources?

Yes.  
It means CloudFormation can “adopt” resources that already exist in your AWS account, even if you created them earlier outside CloudFormation, such as from the console, CLI, or some other tool. CloudFormation supports this through resource import, and AWS also has IaC generator to help create a template from existing resources.

## Project-based questions they may ask you

Since you used Terraform in your project, these are very likely:

### 1. Why did you use Terraform instead of CloudFormation?

Good interview answer:

> “CloudFormation is AWS-native and very strong for AWS environments. I used Terraform because it is widely used, easier for multi-cloud or mixed environments, and has strong module reusability. But the IaC idea in both is the same: define infra in code and provision it in a repeatable way.”

### 2. If your company wants only AWS-native IaC, would CloudFormation be a good choice?

Yes. Because it is directly integrated with AWS services and is designed specifically to provision and manage AWS resources as code.

### 3. In an EKS project, what could CloudFormation create?

It can create supporting AWS infrastructure such as VPC, subnets, security groups, IAM roles, load balancers, and even EKS-related resources defined in a stack template.

### 4. How would you safely update production infrastructure in CloudFormation?

Use change sets first to review the impact, then execute only after validation.

### 5. If someone manually changes a security group created by CloudFormation, how would you detect it?

Use drift detection.
