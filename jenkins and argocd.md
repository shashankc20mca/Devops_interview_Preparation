# Jenkins and argocd

## What is Jenkins? / Why is Jenkins used? / What is the role of Jenkins in DevOps?

Jenkins is an open-source automation tool mainly used for Continuous Integration and Continuous Delivery/Deployment. It helps automate steps like code checkout, build, test, image creation, scanning, and deployment.

---

## Is Jenkins a CI tool or CD tool? / How does Jenkins support CI/CD?

Jenkins supports both CI and CD.
For CI, it automatically builds and tests code whenever changes are pushed.
For CD, it can automate the next steps such as image build, push to registry, update deployment files, and trigger deployment.

---

## What is Continuous Integration? / What is Continuous Delivery?

Continuous Integration means developers frequently merge code, and each change is automatically built and tested.
Continuous Delivery means the application is always ready for release after passing the pipeline.
Continuous Deployment means the application is automatically deployed to production without manual approval after pipeline success.

---

## What is a Jenkins pipeline? / Why is pipeline important?

A Jenkins pipeline is a sequence of automated stages written as code that defines the software delivery workflow. It is important because it standardizes the build and deployment process, reduces manual errors, and makes the workflow repeatable and version-controlled.

---

## What is Jenkinsfile?

A Jenkinsfile is a file that defines the pipeline stages and steps in code. It is usually stored in the same Git repository as the application code

---

## What is the difference between Declarative Pipeline and Scripted Pipeline?

My pipeline is written in Declarative style because it uses pipeline, agent, environment, stages, and steps blocks. If the same pipeline is written in Scripted style, it uses node and Groovy scripting syntax instead of those predefined structured blocks. Declarative is easier to read and maintain, while Scripted gives more flexibility.

---

## Main visual difference

### Declarative

Uses:

```groovy
pipeline {
    agent any
    environment { }
    stages {
        stage('...') {
            steps { }
        }
    }
}
```

### Scripted

Uses:

```groovy
node {
    stage('...') {
        sh '...'
    }
}
```

---

## What changed in Scripted version

Your logic is the same, but syntax changes:
•	pipeline {} becomes node {} 
•	environment {} becomes normal Groovy variables like def IMAGE_NAME 
•	steps {} block is not needed 
•	script {} block is also usually not needed, because Scripted is already Groovy-based

---

## What are jobs in Jenkins?

A job in Jenkins is an automation task that Jenkins is configured to run.

---

## What is the difference between Freestyle job and Pipeline job?

A Freestyle job is configured through the Jenkins UI and is good for simple tasks.
A Pipeline job is defined as code using a Jenkinsfile and is better for real CI/CD workflows because it is reusable, scalable, and version-controlled.

---

## Ways to trigger the Jenkins job or pipeline

Manual trigger user starts the Jenkins job directly by clicking Build Now or Build with Parameters in the Jenkins UI.
Webhook trigger ->external tool like GitHub sends an automatic notification to Jenkins when an event happens, usually a code push.
Poll SCM=> Poll SCM means Jenkins checks the source code repository at regular intervals to see whether any change has happened.
Scheduled trigger -> Scheduled trigger means Jenkins starts the job automatically at a fixed time or interval using cron syntax.
Trigger from another job ->Trigger from another job is used when one stage of the process depends on the successful completion of another Jenkins job.

---

## What happens when code is pushed to GitHub in Jenkins-based CI/CD?

When code is pushed to GitHub, a webhook or poll trigger notifies Jenkins. Jenkins starts the pipeline, checks out the latest code, runs the defined stages if successful, continues with deployment-related steps.

---

## What is a webhook in Jenkins? Why is webhook used?

A webhook is an automatic notification sent from GitHub or another SCM tool to Jenkins when a code change happens. It is used to trigger the pipeline immediately

---

## What is the difference between controller and agent/nodes?

The controller is the main Jenkins server.
The agent is the machine that executes the actual job steps.
Controller does:
•	manage Jenkins UI 
•	store job and pipeline configuration 
•	schedule builds 
•	manage plugins 
•	manage credentials 
•	assign jobs to agents 
•	store build history and logs 
Agent does:
•	receive work from controller 
•	create workspace 
•	run the pipeline steps 
•	execute commands and tools 
•	send result and logs back to controller

---

## In my project setup

Installed Jenkins on one VM, accessed the Jenkins UI there, created jobs there, and ran the pipelines there.
So:
•	the controller is on that VM 
•	the build execution is also happening on that same VM 
That means the same machine is acting as:
•	Jenkins controller 
•	built-in agent

---

## What are agents or nodes in Jenkins?

In Jenkins, agents or nodes are the machines where the actual job or pipeline steps run.

---

## Important Manage Jenkins options you should know

Tools configure software installations.
Plugins add functionality and integrations.
Credentials store secrets securely.
System manages global and integration settings.
Nodes/Agents handle job execution.
Global Security controls access and permissions.
System Log is used for troubleshooting.
Safe Restart restarts Jenkins safely after running jobs finish.

---

## What are plugins in Jenkins? / Why are plugins important?

Plugins are extensions that add extra functionality to Jenkins. They are important because Jenkins becomes useful through plugins such as Git, Docker, Pipeline, SonarQube, Kubernetes, Slack, and many others.

---

## What are credentials manager in Jenkins? / How do you store secrets in Jenkins?

Credentials in Jenkins are used to securely store sensitive information such as Git tokens, Docker Hub passwords, SSH keys, and API keys. Instead of hardcoding secrets in the pipeline, Jenkins credentials are referenced securely inside the job or Jenkinsfile.

---

## Why should secrets not be hardcoded in Jenkinsfile?

Secrets should not be hardcoded because the Jenkinsfile is usually stored in Git, and that creates a security risk

---

## What are environment variables in Jenkins? / Why are they used?

Environment variables are values used inside the pipeline such as image name, branch name, build number, or deployment environment. They help make the pipeline reusable and configurable.

---

## What are tools in Jenkins

In Jenkins, the Tools section is used to configure external software installations that the pipeline may need during exection.
In our case:
•	SonarQube Scanner was configured in Jenkins Tools 

---

## what are system in Jenkins

System is for configuring the server connection details that Jenkins should use while talking to an external service.
important difference
Tools = SonarQube Scanner installation 
System = SonarQube server connection details such as: server name like sonar-server , SonarQube URL ,token

In your pipeline:
•	tool 'sonarqube' → gets the scanner installation 
•	withSonarQubeEnv('sonar-server') → gets the SonarQube server connection details

---

## What is an artifact in Jenkins? / What is artifact archiving?

An artifact is the output generated by the build process, such as a JAR file, WAR file, binary, or reports. Artifact archiving means storing those outputs in Jenkins or another repository so they can be reused later.

---

## What is the difference between build, test, and deploy in Jenkins pipeline?

Build means compiling or packaging the application.
Test means validating the code using automated checks.
Deploy means releasing the built application to a target environment such as staging or production.

---

## How does Jenkins integrate with GitHub? / How does Jenkins integrate with Docker?

Jenkins integrates with GitHub through Git plugins and webhooks for code checkout and automatic triggering.
It integrates with Docker by building images, tagging them, and pushing them to a registry as part of the pipeline.

---

## How does Jenkins integrate with SonarQube and Trivy?

Jenkins can call SonarQube during the pipeline for code quality analysis and can run Trivy for filesystem or container image vulnerability scanning. This helps include quality and security checks in CI.

---

## What is the use of agent any in Jenkins?

Answer:
agent any means the pipeline can run on any available Jenkins agent. It tells Jenkins not to restrict the job to a specific node.

---

## What are post actions in Jenkins pipeline?

Post actions are steps that run after the main pipeline stages complete. For example, cleanup, sending notifications, or actions that should happen on success or failure.

---

## What happens if one stage fails in Jenkins pipeline?

If one stage fails, the pipeline usually stops, and the job is marked as failed 

---

## How do you debug a failed Jenkins pipeline?

Answer:
I first check the console output of the failed stage. Then I verify the exact error, such as Git issue, build issue, test issue, Docker login problem, missing dependency, wrong path, or credentials problem. After identifying the stage and error message, I fix the root cause and rerun the pipeline.

---

## If Jenkins job is not getting triggered after GitHub push, what will you check?

Answer:
I will check whether webhook is configured correctly, whether Jenkins is reachable from GitHub, whether the repository URL and branch are correct, whether the trigger is enabled in Jenkins, and whether there are any authentication or permission issues.

---

## If Jenkins pipeline fails at Git checkout stage, what could be the reason?

Answer:
Possible reasons are wrong repository URL, wrong branch name, authentication issue, expired token, SSH key issue, or network connectivity problem between Jenkins and the Git repository.

---

## If Docker image build fails in Jenkins, what will you check?

Answer:
I will check the Dockerfile, file paths, required build files, syntax errors, missing dependencies, Docker daemon availability, and whether the agent has permission to run Docker commands.

---

## If Docker push fails in Jenkins, what could be the reason?

Answer:
Possible reasons are invalid Docker credentials, incorrect image name or tag, registry login failure, insufficient permission, or network issues while connecting to Docker Hub or the private registry.

---

## If Jenkins cannot deploy after build success, what will you check?

Answer:
I will check whether deployment configuration is correct, whether target server or cluster is reachable, whether required credentials are available, whether deployment files were updated correctly, and whether the deploy stage logs show permission or connectivity issues.

---

## What is the difference between manual process and Jenkins automation?

Answer:
In a manual process, developers or DevOps engineers perform build, test, and deployment steps one by one. In Jenkins automation, these steps are predefined and executed automatically, which saves time, reduces mistakes, and improves consistency.

---

## What are stages in Jenkins pipeline?

## How did you use Jenkins in your project?

## Why did you choose Jenkins in your project?

## Integration with GitHub, Docker, SonarQube, Trivy, Kubernetes, ArgoCD

---

# What would my pipeline look like in multi-agent setup

Yes — let’s map it to your exact project.

## Your current setup

Right now you have:
•	1 VM
•	Jenkins installed on that VM
•	Jenkins UI accessed on that VM
•	frontend and backend jobs created there
•	pipelines also running there
So currently:
same VM = controller + built-in agent
That is single-node Jenkins.

---

## How multi-agent would look in your project

In your case, a multi-agent setup could look like this:

### Option 1: One controller + two separate agents

#### VM 1: Jenkins Controller

Used for:
•	Jenkins UI
•	job configuration
•	plugin management
•	credentials
•	build scheduling
•	webhook receiving
•	showing logs and build history

#### VM 2: Frontend Agent

Used for frontend pipeline execution:
•	checkout frontend code
•	run Trivy fs scan
•	run SonarQube scan
•	docker build frontend image
•	Trivy image scan
•	docker push
•	update frontend deployment YAML

#### VM 3: Backend Agent

Used for backend pipeline execution:
•	checkout backend code
•	run backend scans
•	build backend image
•	push backend image
•	update backend deployment YAML

So architecture becomes:

```text
GitHub Webhook
      |
      v
Jenkins Controller (VM1)
   |               |
   |               |
   v               v
Frontend Agent   Backend Agent
   (VM2)            (VM3)
```

---

## How it actually works

Suppose you push frontend code.

### Step 1
GitHub sends webhook to Jenkins Controller.

### Step 2
Controller checks which job should run.
Example:
•	frontend repo change → frontend job

### Step 3
Controller reads the pipeline and sees which agent should run it.
Example:
•	frontend job → label frontend-agent

### Step 4
Controller looks for an available connected agent with that label.

### Step 5
Frontend agent receives the job.

### Step 6
Frontend agent creates its own workspace and runs all commands:
•	git checkout
•	trivy fs
•	sonar scan
•	docker build
•	docker push
•	git update for k8s repo

### Step 7
Logs are sent back to controller and shown in Jenkins UI.

### Step 8
Build result is stored in controller.

Same thing happens for backend on backend agent.

---

## Why this is better than your current setup

In your current setup, everything runs on one VM.
That means:
•	Jenkins UI and builds share same machine
•	heavy Docker builds can slow Jenkins
•	frontend and backend jobs compete for same system resources
•	less scalable
In multi-agent setup:
•	controller only manages
•	agents do actual work
•	frontend and backend can run in parallel
•	easier to scale
•	better isolation

---

## In your project, how you would define it

Your frontend pipeline could run on a frontend agent like this:

```groovy
pipeline {
    agent { label 'frontend-agent' }
    environment {
        SCANNER_HOME = tool 'sonarqube'
        IMAGE_NAME = "shashank20062000/mern-3tier-frontend"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        ...
    }
}
```

Your backend pipeline could run on a backend agent like this:

```groovy
pipeline {
    agent { label 'backend-agent' }
    stages {
        ...
    }
}
```

So instead of agent any, you tell Jenkins exactly which agent should execute which pipeline.

---

## How to set it up in your project

### 1. Keep Jenkins installed on one main VM
This becomes the controller.

### 2. Create one or more extra VMs
Example:
•	VM2 for frontend agent
•	VM3 for backend agent

### 3. Install required tools on those agents
Each agent should have the tools needed for its pipeline.
For your frontend agent, likely:
•	Git
•	Docker
•	Trivy
•	SonarQube Scanner
•	maybe Node/npm if needed
For backend agent, depending on backend stack:
•	Git
•	Docker
•	Trivy
•	SonarQube Scanner
•	Java/Maven or Node, whichever your backend needs

### 4. Add agents in Jenkins
In Jenkins:
Manage Jenkins → Nodes
Create a new node/agent and configure:
•	agent name
•	remote workspace path
•	number of executors
•	label like frontend-agent or backend-agent
•	launch method such as SSH or inbound

### 5. Connect controller to those agents
Common ways:
•	SSH agent
•	inbound agent using agent.jar

### 6. Update pipeline to use labels
Replace:

```groovy
agent any
```

with:

```groovy
agent { label 'frontend-agent' }
```

or

```groovy
agent { label 'backend-agent' }
```

### 7. Run pipeline
Now controller schedules the job to the correct agent.

---

## What exactly changes from your current setup

### Current
One VM
- Jenkins UI
- frontend job
- backend job
- all build steps

### Multi-agent
#### Controller VM
- Jenkins UI
- jobs
- credentials
- scheduling

#### Frontend Agent VM
- runs frontend CI pipeline

#### Backend Agent VM
- runs backend CI pipeline

---

## Very practical example using your project

Suppose both frontend and backend developers push code at the same time.

### In your current setup
Both jobs run on same Jenkins VM.
Problems:
•	CPU and RAM are shared
•	Docker builds may slow each other
•	Jenkins UI may become slow

### In multi-agent setup
Controller receives both webhooks.
Then:
•	frontend job sent to frontend-agent
•	backend job sent to backend-agent
Now both pipelines run independently and in parallel.
That is the real benefit.

---

## Best practice in companies

Usually companies keep:
•	controller only for management
•	agents for actual execution
Sometimes they even set controller executors to 0, so no builds run on controller at all.
That means:
•	controller stays clean and stable
•	heavy jobs run only on agents

---

## Best interview answer

In my current project, Jenkins is running in a single-node setup where the controller and built-in agent are on the same VM. In a multi-agent version of the same project, I would keep Jenkins UI and scheduling on a controller VM, and create separate agents for frontend and backend pipelines. The controller would receive the webhook, select the correct job, and assign it to an agent based on labels like frontend-agent or backend-agent. The agent would then perform checkout, scanning, Docker build, push, and manifest update, while logs and status would be sent back to the controller. This setup improves scalability, isolation, and parallel execution.

---

## Note: so basically we give all ssh info for master to connect to vm where it will execute the tasks there only use for task execution right?

Yes — exactly.
In an SSH-based Jenkins agent setup:
•	Jenkins controller has the job/pipeline 
•	you give the SSH details of the target VM to Jenkins 
•	Jenkins connects from controller to that VM 
•	then the task executes on that VM only

---

## What is a Jenkins workspace? / Where is Jenkinsfile stored?

A Jenkins workspace is the directory on the Jenkins node or agent where the job runs. Jenkins checks out the source code there and performs build, test, and deployment steps from that location.

---

## What is Jenkins architecture at a basic level? / What is labeled agent in Jenkins?

At a basic level, Jenkins architecture has:
•	Jenkins Controller — manages jobs, pipeline definitions, scheduling, and overall orchestration 
•	Jenkins Agents — execute the actual build, test, or deployment tasks 
A labeled agent is an agent tagged with a label like docker, linux, or maven, so Jenkins can run a job only on an agent that matches that label.

---

## What is Docker agent in Jenkins? / What is Kubernetes agent in Jenkins?

In short:
•	Docker agent = run build inside container 
•	Kubernetes agent = run build on dynamic Pod-based agent

---

## What is parameterized build in Jenkins?

A parameterized build allows the user to pass input values when starting a job, such as branch name, environment, version, or deployment target.

---

## What is downstream and upstream job in Jenkins?

An upstream job is the job that starts or triggers another job.
A downstream job is the job that runs after being triggered by another job.

---

## What is parallel stage in Jenkins?

A parallel stage means Jenkins runs multiple stages at the same time instead of one after another.

---

## What is parallel stage in Jenkins?

A parallel stage means Jenkins runs multiple stages at the same time instead of one after another.
Artifact versioning means assigning a unique version or tag to each artifact so we know exactly which build produced it.
not like Java JAR/WAR, but artifacts still exist in MERN. you get production files like:
•	HTML 
•	CSS 
•	JS bundle

---

# Lickely interview questions on ci cd project

## 1) Explain your project end to end.

Answer:
I built a CI/CD and GitOps setup for a 3-tier microservices application with separate frontend and backend services. Jenkins handled CI by checking out code, running Trivy and SonarQube scans, building and pushing Docker images, and updating the image tag in the Kubernetes manifests repository. ArgoCD handled CD by watching that manifests repository and syncing the desired state to the EKS cluster. The application was exposed using Gateway API with Envoy, and scaling was handled using HPA and node autoscaling.

---

## 2) What was Jenkins doing and what was ArgoCD doing in your project?

Answer:
Jenkins was used only for CI. It built, scanned, pushed the image, and updated the deployment manifest. ArgoCD was used for CD. It monitored the Kubernetes manifests repository and synchronized changes to the EKS cluster.

---

## 3) Why did you use Jenkins for CI and ArgoCD for CD instead of using Jenkins for both?

Answer:
I wanted a GitOps model. Jenkins is strong for build and scan automation, while ArgoCD is strong for deployment synchronization, drift detection, and keeping Git as the source of truth. So Jenkins handled image creation and manifest update, and ArgoCD handled cluster deployment.

---

## 4) Why did you create separate pipelines for frontend and backend?

Answer:
Because frontend and backend are independent microservices. Separate pipelines allow independent build, scan, release, and troubleshooting. If only frontend changes, there is no need to rebuild backend.

---

## 5) Explain your Jenkins pipeline stages.

Answer:
The stages were Checkout Source Code, Trivy filesystem scan, SonarQube analysis, Docker build and tag, Trivy image scan, Docker push image, and update deployment.yaml. These stages ensured source validation, code quality, security scanning, image creation, registry push, and GitOps manifest update.

---

## 6) Why did you use both SonarQube and Trivy?

Answer:
SonarQube was used for static code quality analysis such as bugs, code smells, and maintainability issues. Trivy was used for security scanning of source dependencies and Docker images. SonarQube checks code quality, while Trivy checks vulnerabilities.

---

## 7) Why did you scan both the source filesystem and the Docker image?

Answer:
Filesystem scan helps detect issues in source dependencies before image build. Image scan helps detect vulnerabilities in the final built image, including OS packages and runtime components. Using both gives better security coverage.

---

## 8) Why did you use BUILD_NUMBER as the image tag?

Answer:
BUILD_NUMBER gives a unique tag for every pipeline execution. It helps track which Jenkins build created which Docker image and which version was deployed. It also avoids confusion from reusing the same tag.

---

## 9) Why did Jenkins update deployment.yaml instead of directly deploying to Kubernetes?

Answer:
Because I followed GitOps. Jenkins only updated the desired state in Git. ArgoCD then read that Git change and applied it to the cluster. This kept Git as the single source of truth.

---

## 10) How did Jenkins trigger automatically when code was pushed?

Answer:
I used a GitHub webhook. When code was pushed, GitHub sent a webhook event to Jenkins, and Jenkins automatically started the pipeline.

---

## 11) How did Jenkins access private GitHub repositories and private Docker Hub?

Answer:
I used Jenkins Credentials. GitHub repository access was handled using stored Git credentials, and Docker Hub access was handled using stored registry credentials. These were referenced in the pipeline through credentialsId.

---

## 12) Since Docker Hub was private, how did Kubernetes pull the images?

Answer:
I used imagePullSecrets in Kubernetes. That secret stored the Docker Hub authentication details, and the deployment used it so the cluster could pull private images successfully.

---

## 13) Since the Kubernetes manifests repo was private, how did ArgoCD access it?

Answer:
ArgoCD was configured with private repository credentials so it could authenticate and fetch the manifests from GitHub securely.

---

## 14) How did ArgoCD know a new version was ready to deploy?

Answer:
Jenkins updated the image tag in the deployment manifest and pushed that change to the Kubernetes manifests repository. ArgoCD was watching that repo and detected the change during its sync interval, then applied the updated desired state to the cluster.

---

## 15) What is GitOps in your project?

Answer:
GitOps in my project means the Kubernetes manifests repository was the source of truth for deployment state. Jenkins updated Git, and ArgoCD synchronized the cluster to match Git. No direct manual deployment was done to the cluster.

---

## 16) What is drift detection in ArgoCD?

Answer:
Drift detection means ArgoCD compares the actual cluster state with the desired state in Git. If they do not match, ArgoCD marks the application OutOfSync and can bring it back to the Git-defined state depending on configuration.

---

## 17) What does OutOfSync mean in ArgoCD?

Answer:
OutOfSync means the live cluster resources do not match the manifests stored in Git. It usually happens when Git changed but the cluster is not updated yet, or someone manually changed the cluster.

---

## 18) You mentioned canary deployment. How exactly did you implement it?

Answer:
I need to answer this carefully based on what was actually implemented. If I only used the default Kubernetes deployment update behavior, then that is rolling update, not true canary. True canary requires controlled traffic splitting and gradual exposure of the new version. So I should only claim canary if I actually implemented that traffic control.

---

## 19) What is the difference between HPA and node autoscaling?

Answer:
HPA scales the number of pods based on metrics like CPU usage. Node autoscaling increases or decreases the number of worker nodes when cluster capacity is insufficient or underutilized. HPA scales workloads, while node autoscaling scales infrastructure.

---

## 20) What were the main challenges you faced in this project?

Answer:
The main challenges were allowing the EKS cluster to pull private Docker Hub images, configuring ArgoCD to access the private Kubernetes manifests repository, fixing issues in HTTPRoute and Service YAML files, and correctly configuring Gateway API and the load balancer for external access.
If you want, I’ll make these 20 into a very short spoken-answer revision sheet.

---

# Argocd 

## 1) What is GitOps?

Answer:
GitOps is a deployment approach where Git acts as the single source of truth for the desired state of the application and infrastructure. Any deployment change is first made in Git, and then a tool like ArgoCD applies that desired state to the Kubernetes cluster.

---

## 2) How did you implement GitOps in your project?

Answer:
In my project, the Kubernetes manifests were stored in a separate private Git repository. After Jenkins completed the CI pipeline successfully, it updated the image tag in the deployment manifest and pushed that change to the manifests repository. ArgoCD continuously watched that repository and synchronized the change to the EKS cluster.

---

## 3) Why did you use GitOps in your project?

Answer:
I used GitOps to make deployments more controlled, traceable, and consistent. It helped keep Git as the source of truth, improved auditability, made rollback easier, and allowed ArgoCD to detect drift between Git and the cluster.

---

## 4) What is ArgoCD?

Answer:
ArgoCD is a GitOps-based continuous delivery tool for Kubernetes. It monitors a Git repository that contains Kubernetes manifests and ensures that the actual cluster state matches the desired state defined in Git.

---

## 5) Why did you use ArgoCD in your project?

Answer:
I used ArgoCD because I wanted deployment to be Git-driven instead of directly controlled from Jenkins. ArgoCD continuously monitored the Kubernetes manifests repository and synchronized any changes to the EKS cluster, which fit well with my GitOps-based setup.

---

## 6) What was the role of ArgoCD in your project?

Answer:
ArgoCD handled the CD part of the project. Jenkins built and pushed the Docker image and updated the manifest in Git, and ArgoCD detected that manifest change and deployed the updated version to the EKS cluster.

---

## 7) What was Jenkins doing and what was ArgoCD doing?

Answer:
Jenkins handled CI tasks such as checkout, scanning, Docker build, Docker push, and manifest update. ArgoCD handled CD by monitoring the Kubernetes manifests repository and synchronizing the desired state to the cluster.

---

## 8) Explain the GitOps flow in your project end to end.

Answer:
A developer pushes code to GitHub, which triggers Jenkins through webhook. Jenkins runs the CI pipeline, scans the code, builds and pushes the Docker image, and updates the image tag in the Kubernetes deployment manifest stored in a separate private repo. ArgoCD watches that repo, detects the change, and syncs the updated state to the EKS cluster.

---

## 9) Why did Jenkins update deployment.yaml instead of directly deploying to Kubernetes?

Answer:
Because I followed GitOps. In GitOps, the deployment state should be maintained in Git. If Jenkins directly deployed to the cluster, Git would no longer be the source of truth. By updating deployment.yaml in Git, the deployment remained version-controlled and traceable.

---

## 10) What is the source of truth in your project?

Answer:
The Kubernetes manifests repository is the source of truth for deployment. The application source code repositories contain the app code, but the actual deployed state is defined by the manifests repo that ArgoCD watches.

---

## 11) Why did you keep application code repos and Kubernetes manifests repo separate?

Answer:
I separated them because source code and deployment state have different purposes. The application repos contain frontend and backend code, while the manifests repo contains the desired Kubernetes deployment state. This separation fits GitOps and makes deployment management cleaner.

---

## 12) How did ArgoCD know a new version was ready to deploy?

Answer:
After Jenkins updated the image tag in deployment.yaml and pushed the change to the manifests repository, ArgoCD detected that Git change during its sync interval and applied it to the cluster.

---

## 13) What does the 3-minute polling interval mean?

Answer:
It means ArgoCD checked the manifests repository approximately every 3 minutes for changes. If it found a new desired state, such as an updated image tag, it synchronized that change to the cluster.

---

## 14) How did ArgoCD access the private Kubernetes repo?

Answer:
Since the repository was private, ArgoCD was configured with repository credentials so it could securely authenticate and fetch the manifests.

---

## 15) How did you connect ArgoCD to the EKS cluster?

Answer:
ArgoCD was installed and configured to manage the EKS cluster. It was then given the application definition and repository details so it could compare the Git state with the live cluster state and synchronize changes.

---

## 16) What is application.yaml in ArgoCD?

Answer:
application.yaml defines the ArgoCD application resource. It specifies which Git repository to monitor, which path contains the manifests, which cluster to deploy to, and which namespace to use.

---

## 17) What is sync in ArgoCD?

Answer:
Sync in ArgoCD means applying the desired state from Git to the Kubernetes cluster so that the live resources match the manifests stored in the repository.

---

## 18) What does OutOfSync mean in ArgoCD?

Answer:
OutOfSync means the live cluster state does not match the desired state stored in Git. This can happen when Git has changed but the cluster is not updated yet, or when someone manually changes a resource in the cluster.

---

## 19) What is drift detection in ArgoCD?

Answer:
Drift detection means ArgoCD compares the actual cluster state with the desired state in Git. If there is any difference, ArgoCD identifies that drift and marks the application as OutOfSync.

---

## 20) What happens if someone manually changes the deployment in the cluster?

Answer:
ArgoCD detects that the live state no longer matches Git and marks the application as OutOfSync. Depending on configuration, it can either show the difference or automatically restore the Git-defined state.

---

## 21) What is self-heal in ArgoCD?

Answer:
Self-heal is a feature where ArgoCD automatically corrects drift by reapplying the desired state from Git when it detects that the live cluster state has changed manually.

---

## 22) What is prune in ArgoCD?

Answer:
Prune removes resources from the cluster that are no longer defined in Git. It helps ensure that the live environment fully matches the desired Git state.

---

## 23) How do you rollback in GitOps or ArgoCD?

Answer:
Rollback is usually done by reverting the Git commit or changing the manifest back to the previous working version. Once Git is reverted, ArgoCD detects that desired state and synchronizes the cluster back.

---

## 24) Jenkins pipeline passed, but ArgoCD did not deploy. What will you check?

Answer:
I would check whether Jenkins actually pushed the updated manifest to the correct repo, whether ArgoCD is watching the correct repository and path, whether repo credentials are valid, whether the app is OutOfSync or Synced, and whether any sync error is shown in ArgoCD.

---

## 25) What is the biggest advantage of ArgoCD over direct deployment from Jenkins?

Answer:
The biggest advantage is that ArgoCD keeps deployments Git-driven. This improves auditability, rollback, drift detection, and separation of CI and CD, while keeping Git as the source of truth.

