```md
## CodePipeline and CodeBuild

Since you used **Jenkins for CI** and **Argo CD for CD**, the easiest way to understand these is:

* **CodeBuild ≈ Jenkins build job**
* **CodePipeline ≈ overall CI/CD workflow/orchestrator**
* **Argo CD ≈ GitOps CD tool for Kubernetes**

AWS says **CodeBuild** is a fully managed build service that compiles source code, runs tests, and produces deployable artifacts, while **CodePipeline** is a continuous delivery service that models, visualizes, and automates release steps through stages. ([AWS Documentation][1])

---

## 1. What is CodeBuild?

**AWS CodeBuild** is AWS’s managed build service.

It is used to:

* compile code
* run tests
* build Docker images
* produce artifacts ready for deployment

AWS says CodeBuild eliminates the need to provision, manage, and scale your own build servers. ([AWS Documentation][1])

### Simple understanding

If in your project **Jenkins** did:

* checkout code
* run Trivy scan
* run SonarQube
* build image
* push image
* update manifest

then **CodeBuild** is the AWS service that can do the **build/test job part** of that flow. Its commands are usually defined in a **buildspec.yml** file. AWS documents buildspec as a YAML file containing build commands and related settings. ([AWS Documentation][2])

### Important term: buildspec

A **buildspec** is the YAML file where you define the build commands for CodeBuild. It can be stored in source code or specified when creating the project. ([AWS Documentation][2])

---

## 2. What is CodePipeline?

**AWS CodePipeline** is the AWS service that orchestrates the whole release workflow.

It organizes the workflow into **stages**, and each stage has **actions**. AWS says a pipeline is made of a series of stages, and the first stage must be a **source** stage, with at least one additional **build or deploy** stage. ([AWS Documentation][3])

### Simple understanding

If in your project the full flow was:

GitHub push → Jenkins build/test/image push → Argo CD deploy

then **CodePipeline** is the AWS service that can automate that end-to-end release flow as stages. AWS explicitly says CodePipeline can automate build, test, and deploy steps and can integrate with other tools and deployment targets. ([AWS Documentation][4])

---

## 3. Core difference between CodeBuild and CodePipeline

### CodeBuild

Build service only.
It runs the commands needed to build/test/package software. ([AWS Documentation][1])

### CodePipeline

Pipeline orchestration service.
It connects source, build, test, and deployment stages together. ([AWS Documentation][4])

### Easy memory

* **CodeBuild = build engine**
* **CodePipeline = workflow/orchestrator**

---

## 4. Mapping with your project

### Your project

* **Jenkins** = CI
* **Argo CD** = CD

### AWS equivalent view

* **CodeBuild** = closest to your Jenkins build job
* **CodePipeline** = orchestrates source → build → deploy flow
* **Argo CD** does not have an exact direct AWS-native equivalent in these two services because Argo CD is specifically a **GitOps Kubernetes CD tool**, while CodePipeline is a broader release orchestrator. AWS documents CodePipeline as a release automation service, not a GitOps Kubernetes controller. ([AWS Documentation][4])

Best interview line:

**“In my project, Jenkins handled CI and Argo CD handled CD. In AWS terms, CodeBuild is similar to the build part of Jenkins, while CodePipeline is the service that orchestrates the overall release workflow.”** ([AWS Documentation][1])

---

## 5. Most expected fresher interview questions

### What is AWS CodeBuild?

CodeBuild is AWS’s fully managed build service used to compile source code, run tests, and produce deployable artifacts. ([AWS Documentation][1])

### What is AWS CodePipeline?

CodePipeline is AWS’s continuous delivery service used to model and automate software release workflows through stages. ([AWS Documentation][4])

### What is the difference between CodeBuild and CodePipeline?

CodeBuild performs the build/test/package job. CodePipeline connects source, build, test, and deploy stages into one automated release flow. ([AWS Documentation][1])

### What is buildspec in CodeBuild?

A buildspec is a YAML file containing the build commands and settings CodeBuild uses to run a build. ([AWS Documentation][2])

### What is a stage in CodePipeline?

A stage is a step in the pipeline workflow, such as source, build, test, or deploy. A pipeline is made of a series of stages. ([AWS Documentation][3])

### What are artifacts in CodePipeline?

Artifacts are the files that actions in the pipeline use and produce, such as source code packages or build outputs. ([AWS Documentation][5])

### Can CodeBuild be used with CodePipeline?

Yes. AWS documents CodeBuild as a build service that can be used inside CodePipeline for automated release processes. ([AWS Documentation][6])

### Does CodeBuild remove the need for build servers?

Yes. AWS says CodeBuild eliminates the need to provision, manage, and scale build servers. ([AWS Documentation][1])

---

## 6. Important differences interviewers may test

### Jenkins vs CodeBuild

* **Jenkins**: you manage the Jenkins server/agents yourself
* **CodeBuild**: AWS manages the build infrastructure for you
  AWS explicitly says CodeBuild removes the need to manage build servers. ([AWS Documentation][1])

### Jenkins pipeline vs CodePipeline

* **Jenkins pipeline**: CI/CD workflow inside Jenkins
* **CodePipeline**: AWS-managed pipeline service with stages and actions
  AWS documents CodePipeline as the service that models and automates release steps. ([AWS Documentation][4])

### CodeBuild vs Argo CD

* **CodeBuild**: build/test/package
* **Argo CD**: deploy/sync Kubernetes from Git
  This second part is from general product behavior, not AWS docs. Your safe interview framing is that CodeBuild is not a Kubernetes GitOps CD controller, while Argo CD is. This is an inference based on what CodeBuild is documented to do. ([AWS Documentation][1])

---

## 7. Project-based questions they may ask you

### Why did you use Jenkins instead of CodeBuild?

A good answer:

**“I used Jenkins because it was already suitable for my CI workflow and gave me flexibility for my custom stages like Trivy scan, SonarQube, Docker build, push, and manifest updates. If I wanted an AWS-managed build service without maintaining Jenkins infrastructure, CodeBuild would be a strong alternative.”** AWS says CodeBuild is managed and eliminates the need to manage build servers. ([AWS Documentation][1])

### Why did you use Argo CD instead of CodePipeline for CD?

A good answer:

**“My deployment model was GitOps-based Kubernetes deployment, so Argo CD was the better fit because it continuously syncs Kubernetes manifests from Git. CodePipeline is more of a general AWS release orchestrator.”** The second sentence is an inference from CodePipeline’s documented role as a release automation service. ([AWS Documentation][4])

### If you redesigned your project using AWS-native services, what could you use?

**“I could use CodeBuild for the CI/build part and CodePipeline to orchestrate the end-to-end release stages.”** AWS explicitly documents this combination. ([AWS Documentation][6])

---

## 8. Scenario-based answers

### If interviewer asks: “Which AWS service is similar to Jenkins?”

**“CodeBuild is the closest AWS-managed service for the build part of Jenkins, because it runs build commands, tests, and produces artifacts.”** ([AWS Documentation][1])

### If interviewer asks: “Which AWS service is similar to the overall CI/CD pipeline?”

**“CodePipeline is the AWS-managed service for the overall release workflow, because it organizes source, build, test, and deploy steps into stages.”** ([AWS Documentation][4])

### If interviewer asks: “Can CodePipeline work with services outside AWS?”

Yes. AWS says CodePipeline can integrate with other tools or services through integration points. ([AWS Documentation][7])
```
