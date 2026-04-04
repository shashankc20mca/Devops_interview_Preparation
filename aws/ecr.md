Amazon ECR, or Amazon Elastic Container Registry, is AWS’s managed container image registry. It stores Docker images and OCI-compatible images and artifacts. Every AWS account gets a default private ECR registry, and AWS also has ECR Public for publicly shareable images

### 1. What is a registry?

A registry is the top-level ECR space for your AWS account in a Region. AWS says each account gets a default private ECR registry.

### 2. What is a repository?

A repository is where specific container images are stored inside the registry, such as frontend, backend, or payments-service. ECR private repositories can store Docker and OCI images and artifacts.

### 3. What is an image tag?

An image tag is the label added to an image version, like latest, v1, or build-25. In practice, you push images to ECR as repository:tag

### 4. What is private ECR vs public ECR?

Private ECR is for images you control access to in your AWS account. ECR Public is for images intended to be publicly available through the ECR Public Gallery.

### 5. What is authentication in ECR?

Before using docker push or docker pull with private ECR, you must authenticate the Docker client to that registry. AWS says the authorization token is valid for 12 hours.

### 6. What is image scanning?

ECR can scan images for software vulnerabilities. AWS documents basic scanning and enhanced scanning, and notes that scanning can be configured at the private registry level

### 7. What is a lifecycle policy?

It is a rule-based mechanism to automatically clean up old or unneeded images from a repository. AWS recommends previewing the lifecycle policy before applying it.

### 8. What is the difference between IAM policy and repository policy in ECR?

IAM policy is attached to users, groups, or roles. Repository policy is attached directly to the ECR repository and is often used for repository-specific or cross-account access or push permissions

## What is the difference between ECR and Docker Hub?

Both store container images, but ECR is AWS-native and integrates directly with AWS IAM, repository policies, EKS, ECS, scanning, and lifecycle management.

## Why you used docker hub instead of ecr?

In that project, I used Docker Hub because it was simpler and faster for my setup. I already had the images and workflow working there, so for a fresher-level project it was enough to build, push, and pull images. If the project were more AWS-focused or production-oriented, I would prefer ECR because it is AWS-native and integrates better with IAM and EKS
