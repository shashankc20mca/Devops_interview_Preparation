## What is Kubernetes? / Why is Kubernetes used?
Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and maintain containerized applications automatically. It is used because it helps manage applications running in containers across multiple servers in a reliable and automated way.
---
## What problems does Kubernetes solve? / What is container orchestration?
Container orchestration means automating the deployment, management, scaling, networking, and recovery of containers. Kubernetes solves problems like manual container management, difficulty in scaling applications, failed container recovery, traffic distribution, service discovery, and rolling updates with minimal downtime.
---
## Why do we need orchestration if we have Docker? / What is the difference between Docker and Kubernetes?
Docker is mainly used to build, package, and run containers. Kubernetes is used to manage those containers across multiple machines. We need orchestration because Docker alone does not handle large-scale scheduling, auto-healing, auto-scaling, load balancing, and cluster-level management. In simple terms, Docker runs containers, while Kubernetes manages containers at scale.
---
## What is a Kubernetes cluster?
A Kubernetes cluster is a group of machines that work together to run and manage containerized applications. It mainly consists of a control plane and worker nodes.
---
## What are the main components of a Kubernetes cluster? / What is control plane in Kubernetes? / What is a worker node in Kubernetes?
The main components of a Kubernetes cluster are the control plane and worker nodes. The control plane is the management part of Kubernetes. It makes decisions, maintains the desired state, schedules workloads, and manages the overall cluster. Worker nodes are the machines that run the actual application workloads in the form of pods and containers.
---
## What is the role of Kubernetes in DevOps?
Kubernetes plays an important role in DevOps because it supports automation, consistent deployments, scalability, high availability, and self-healing. It helps teams release applications faster, manage environments more efficiently, and improve CI/CD workflows.




## Kubernetes architecture
### Kubernetes architecture mainly has two parts:
1.	Control Plane 
2.	Worker Nodes 
The control plane manages the cluster, and the worker nodes run the application.
---
## 1) Control Plane
The control plane is the brain of Kubernetes.
It makes decisions, schedules workloads, and keeps the cluster in the desired state.
## Components of control plane
API Server
The API Server is the main entry point of Kubernetes. All requests from kubectl, UI, or other components go through it. It validates requests, processes them, and exposes the Kubernetes API to the cluster.
etcd
etcd is the key-value database of Kubernetes. It stores the complete cluster state and configuration. Its main role is to act as the single source of truth for the cluster.
## It stores details like:
•	Pod information 
•	Node information 
•	Deployments 
•	Services 
•	Configurations 
•	Secrets 
•	Desired state and current state metadata

Scheduler
The scheduler is the component that decides which worker node a Pod should run on. It checks factors like CPU, memory, resource availability, and scheduling rules before assigning the Pod to a node.
Controller Manager
The controller manager continuously checks whether the actual state matches the desired state. If something is missing or fails, it takes corrective action.
For example, if one pod crashes and the desired count is 3, it creates a new pod to maintain 3.
---
## 2) Worker Nodes
Worker nodes are the machines where the actual application runs.
## Components of worker node
Kubelet
kubelet is the agent running on each worker node. It communicates with the control plane and ensures that the Pods assigned to that node are running properly.
Container Runtime
The container runtime is the software that actually runs the containers inside a Pod. Kubernetes uses it to pull container images and start containers. Common examples are containerd and CRI-O.
Kube-proxy
kube-proxy is the networking component running on each worker node. It handles Service networking and forwards traffic to the correct Pods.
---
## How the architecture works
Suppose you deploy an application.
First, you send a request using kubectl or a YAML file.
That request goes to the API Server.
The API server stores the cluster information in etcd.
Then the Scheduler decides which worker node should run the pod.
After that, the Kubelet on that worker node receives instructions and starts the pod using the container runtime.
If traffic comes to the application, kube-proxy helps route the traffic to the right pod.
If a pod fails, the Controller Manager notices it and creates a replacement pod.


## Notes: Pods are usually created by higher-level Kubernetes objects like ReplicaSet, Deployment, StatefulSet, Job, or DaemonSet. In the common case, a Deployment creates a ReplicaSet, and the ReplicaSet creates the Pods.


## How does Kubernetes communicate between components?
Components usually do not communicate directly with each other all the time; most communication goes through the API Server

## What is a Pod?
A Pod is the smallest deployable unit in Kubernetes. Kubernetes does not deploy containers directly; it deploys them inside Pods.


## Can a Pod have multiple containers?
Yes, a Pod can have multiple containers. They share the same network, IP, and storage. This is usually used when helper containers work closely with the main application container.

## What is the lifecycle of a Pod?
## The main Pod phases are:
•	Pending – Pod is created but not running yet 
•	Running – Pod is running 
•	Succeeded – Pod completed successfully 
•	Failed – Pod ended with error 
•	Unknown – State cannot be determined

## What is a ReplicaSet?
A ReplicaSet ensures that the required number of Pod replicas are always running. If a Pod fails, it creates a new one. Usually, ReplicaSet is managed by a Deployment.

## What is the difference between Pod and container?
A container is the actual application runtime unit that contains the app code, dependencies, and libraries.
A Pod is the Kubernetes object that wraps and manages one or more containers.
## What is a Deployment?
A Deployment is a higher-level Kubernetes object used to manage stateless  application deployment and updates. It helps with rolling updates, rollbacks, scaling, and maintaining the desired number of Pods with the help of replicaset.

•  ReplicaSet = maintains Pod count 
•  Deployment = manages ReplicaSet and application updates.

Stateless application = data or identity need not  be preserved Eg frontend
For stateless applications, the common object is Deployment.
Stateful application = data or identity must be preserved Eg database
For stateful applications, the common object is StatefulSet.


## What is a StatefulSet?
A StatefulSet is a Kubernetes object used to manage stateful applications like databases.
## Difference between Deployment and StatefulSet
Deployment is for applications where Pods are identical and replaceable.
StatefulSet is for applications where each Pod must keep its own identity and storage.
## 1) Type of application
•	Deployment → stateless apps 
•	StatefulSet → stateful apps 
## 2) Pod identity
In a Deployment, Pods are interchangeable.
Their names are not important, and one Pod can replace another.
### In a StatefulSet, each Pod has a stable unique name like:
•	mysql-0 
•	mysql-1 
•	mysql-2 
That identity is preserved.
## 3) Storage
In a Deployment, Pods usually use shared or temporary storage, and storage is not tied to a specific Pod identity.
In a StatefulSet, each Pod can get its own persistent volume.
That storage stays associated with that Pod.
### Example:
•	mysql-0 gets its own PVC 
•	mysql-1 gets its own PVC 
## 4) Scaling behavior
In a Deployment, scaling is flexible and Pods can come up in any order.
In a StatefulSet, scaling happens in order.
### For example:
•	while scaling up: app-0, then app-1, then app-2 
•	while scaling down: reverse order 
## 5) Updates
In a Deployment, updates are mainly for stateless apps and Pods are replaced more freely.
In a StatefulSet, updates are more controlled because Pod identity and data matter.
## 6) Use cases
### Deployment is used for:
•	frontend apps 
•	backend APIs
### StatefulSet is used for:
•	databases


## What is a DaemonSet? / What is the use of DaemonSet?
A DaemonSet is a Kubernetes object that ensures one Pod runs on every worker node or on selected nodes in the cluster.
## Common use cases are: node exporters

## What is a Job in Kubernetes? / What is a CronJob in Kubernetes?
A Job is a Kubernetes object used to run a task one time until it completes successfully. It is mainly used for batch tasks or short-lived tasks like data processing, backup, or script execution.
A CronJob is used to run a Job on a schedule time.

## What is a Service in Kubernetes? / Why do we need Service?
A Service in Kubernetes is an object that provides a stable network endpoint to access a set of Pods.
We need a Service because Pod IPs are not permanent. Pods can be recreated, restarted, or replaced, so their IP addresses can change. A Service gives a fixed way to reach the application even when Pods change.
It also helps with service discovery and load balancing traffic across multiple Pods.


## Service discovery means applications can find and communicate with each other using a stable Service name instead of changing Pod IP addresses.
## Example
Suppose your frontend wants to talk to your backend.
### Frontend calls:
http://backend-service
### With service discovery:
•	Kubernetes gives the backend a Service name 
•	frontend can use that name, like backend-service

### Instead of:
http://10.244.1.5
Because Pod IP may change, but Service name remains stable.

---
## What are the types of Services in Kubernetes?
### The main types of Services are:
•	ClusterIP – default type; exposes the application inside the cluster only
•	NodePort – exposes the application on a port of each node <NodeIP>:<NodePort>.
•	LoadBalancer – exposes the application externally using a cloud load balancer



## What is Ingress in Kubernetes?
Ingress is a Kubernetes object used to manage external HTTP and HTTPS access to services inside the cluster. It provides routing rules based on hostnames and paths.
## In simple terms:
•	Service = gives access to Pods 
•	Ingress = manages external web traffic to Services
Important point: Ingress itself does not handle traffic alone; it needs an Ingress Controller like NGINX

## How ingress works?
If I have frontend, backend, and database microservices, I expose only the frontend using Ingress. External user traffic comes to the Ingress, and Ingress routes it to the frontend Service. The frontend Service load balances the request to one of the frontend Pods to serve the UI. When the frontend needs data, it communicates internally with the backend through a internal Service, and the backend communicates internally with the database through another internal Service. So Ingress is only for external traffic entry, while service-to-service communication inside the cluster happens through Kubernetes Services.
## Traffic flow from Ingress to Pod
1.	User request reaches Ingress Controller
The browser request first reaches the Ingress Controller (for example NGINX Ingress Controller), because the Ingress object itself is only a rule definition. 
2.	Ingress Controller checks Ingress rules
### It checks:
o	host 
o	path 
o	matching backend Service 
### Example:
myapp.com → frontend-service
3.	Ingress Controller sends request to frontend Service
Now traffic is sent to the frontend Service. 
4.	Service identifies matching Pods using labels
The Service does not send traffic randomly.
It uses its selector labels to find which Pods belong to it.
### Example:
app=frontend
5.	Kubernetes maintains endpoints for that Service
Based on matching labels, Kubernetes creates Endpoints / EndpointSlices containing the IPs of frontend Pods. 
6.	kube-proxy handles Service routing
kube-proxy on the node where ingress controller is installed uses routing rules to forward traffic from the Service virtual IP to one of the actual frontend Pod IPs. 
7.	Traffic reaches the selected Pod
The selected Pod may be on any worker node.
The cluster network sends the packet to that node and then to the Pod. 
8.	Container inside the Pod processes the request
The frontend container serves the UI or returns the response. 
9.	Response goes back the same way
Pod → Service path → Ingress Controller → User browser

```text
User Browser
    |
    v
External URL / Domain
    |
    v
Ingress Controller
(checks Ingress rules: host/path)
    |
    v
frontend-service
(virtual Service with stable name + ClusterIP)
    |
    v
Service selector matches Pod labels
    |
    v
Endpoints / EndpointSlices
(list of frontend Pod IPs)
    |
    v
kube-proxy rules on the node
where Ingress Controller received the request
    |
    v
Selected Frontend Pod IP
    |
    +----------------------+
    |                      |
    v                      v
Same node               Different node
    |                      |
    v                      v
Frontend Pod         Cluster network sends
(container serves)   traffic to that node
                           |
                           v
                      Frontend Pod
                      (container serves)
    |
    v
Response back
    |
    v
Ingress Controller
    |
    v
User Browser
```

## understood is traffic flow in gateway api same ?
Yes, high-level traffic flow is almost the same. The main difference is which Kubernetes resources define the routing. In Ingress, you use Ingress + Ingress Controller. In Gateway API, you usually use GatewayClass + Gateway + HTTPRoute with a Gateway API implementation/controller.

## What is DNS in Kubernetes? / How do Pods communicate with each other?
Pods usually communicate with each other through Services, not by directly using Pod IPs. A Pod can call another application using the Service name, and Kubernetes DNS usually resolves the Service name to the ClusterIP of that Service.

## What is kube-proxy role in networking?
kube-proxy handles Service networking inside the cluster. It maintains routing rules on each node so that traffic sent to a Service IP is forwarded to one of the backend Pod IPs.
Kube-proxy uses kernel networking mechanisms to send request to backend pods

## What is port and targetPort in Service?
•  port is the port on which the Service is exposed 
•  targetPort is the port on the Pod container where the application is actually running


## What is ConfigMap? / What is Secret in Kubernetes?
A ConfigMap is used to store non-sensitive configuration data such as environment variables, config files, or application settings.
A Secret is used to store sensitive data such as passwords, API keys, tokens, and certificates.
## The difference is:
•	ConfigMap → non-sensitive data 
•	Secret → sensitive data
Sensitive data should not be hardcoded as it makes the application not secure in production 

## What is volume in Kubernetes? / Why do we need volumes? / What is persistent storage?
A volume in Kubernetes is storage attached to a Pod so containers can read and write data.
We need volumes because container data inside the container filesystem is temporary. If the container restarts, that data can be lost.
Persistent storage means the data remains available even if the container or Pod is restarted or recreated.

PersistentVolume is the actual storage resource in the cluster. 
PersistentVolumeClaim is the request for that storage 
StorageClass defines how storage should be created, 
dynamic provisioning means Kubernetes automatically creates a PV when a PVC requests storage using a StorageClass.
## ull simple flow
## Static provisioning flow
1.	Admin creates PV 
2.	App creates PVC 
3.	PVC matches PV 
4.	Pod uses PVC 
5.	Storage gets mounted into Pod 
## Dynamic provisioning flow
1.	StorageClass exists 
2.	App creates PVC 
3.	Kubernetes creates PV automatically 
4.	PVC binds to PV 
5.	Pod uses PVC 
## Dynamic provisioning
Here you create StorageClass and PVC.
Kubernetes automatically creates PV.
### StorageClass YAML
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```
In real cloud environments, provisioner can be something like AWS EBS, GCE PD, Azure Disk, etc.
### PVC YAML using StorageClass
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: dynamic-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: fast-storage
  resources:
    requests:
      storage: 10Gi
```
## What happens here?
•	PVC asks for 10Gi 
•	PVC refers to fast-storage 
•	Kubernetes uses that StorageClass 
•	PV gets created automatically 
•	PVC gets bound to that PV






## Static provisioning
Here admin creates PV manually, then app creates PVC.
### PV YAML
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data/my-storage
```
### PVC YAML
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```
## What happens here?
•	my-pv is actual storage 
•	my-pvc asks for 5Gi 
•	Kubernetes matches PVC with PV 
•	PVC gets bound to PV 
---
## Using PVC in a Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
    - name: app-container
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: my-volume
  volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: my-pvc
```
## What happens here?
•	Pod uses my-pvc 
•	PVC is already connected to a PV 
•	storage gets mounted inside container 
•	app can read/write data there



### Static provisioning:
Admin creates PV → App creates PVC → PVC binds to existing PV 
### Dynamic provisioning:
StorageClass exists → App creates PVC → Kubernetes creates PV automatically → PVC binds to that PV


## What is StorageClass?
A StorageClass defines how Kubernetes should create storage for a PVC. It acts like a storage template or policy for dynamic provisioning. 
## Important fields in StorageClass
## 1) provisioner
This tells Kubernetes which storage driver should create the volume.
### Example:
provisioner: ebs.csi.aws.com
## 2) parameters
These are driver-specific settings used while creating the volume.
example, in AWS EBS this may include volume type like gp3, filesystem type,
### parameters:
type: gp3
fsType: ext4
## 3) reclaimPolicy
This decides what happens to the volume after the PVC is deleted. The common values are:
•	Delete → volume is deleted 
•	Retain → volume is kept for manual cleanup/reuse
reclaimPolicy: Retain

## 4) volumeBindingMode
This decides when the PV should be provisioned and bound.
•	Immediate → volume is created as soon as PVC is created 
•	WaitForFirstConsumer → volume is created only when a Pod actually uses the PVC
volumeBindingMode: WaitForFirstConsumer

## 5) allowVolumeExpansion
This tells whether the volume can be expanded later if you need more storage. 
### Example:
allowVolumeExpansion: true


## What is scaling in Kubernetes? / What is horizontal scaling? / What is vertical scaling?
Scaling in Kubernetes means increasing or decreasing resources to handle application load.
Horizontal scaling means increasing or decreasing the number of Pods.
Vertical scaling means increasing or decreasing the CPU and memory of a Pod/container.

## What is Horizontal Pod Autoscaler (HPA)?
HPA is a Kubernetes object that automatically scales the number of Pods based on metrics like CPU, memory, or custom metrics.

## What is rolling update? / What is zero downtime deployment?
A rolling update means Kubernetes updates the application gradually, by replacing old Pods with new Pods step by step.
This helps achieve zero downtime deployment, because some Pods stay available while others are being updated.
## What is rollback in Kubernetes?
Rollback means reverting the application to the previous working version if the new deployment causes issues.
## What is canary deployment in Kubernetes?
Canary deployment means releasing the new version to a small percentage of users or traffic first. If it works well, traffic is gradually increased.
## What is blue-green deployment in Kubernetes?
Blue-green deployment means running two versions of the application at the same time:
•	Blue = current version 
•	Green = new version 
Traffic is switched from blue to green once the new version is verified.
It is used for safer releases and quick rollback.

## What is a namespace in Kubernetes? / Why do we use namespaces?
A namespace is a logical way to divide and organize resources inside a Kubernetes cluster.
### We use namespaces to:
•	separate environments like dev, test, and prod 
•	isolate teams or applications 
•	avoid name conflicts 
•	apply resource control and access control more easily 
In short, namespaces help organize and isolate resources within the same cluster.
---
## What is default namespace? / What is kube-system namespace?
The default namespace is the namespace Kubernetes uses if you do not specify any namespace while creating resources.
The kube-system namespace is reserved for Kubernetes internal system components like DNS, kube-proxy, controller manager related pods, and other cluster services.

## What is resource quota?
A ResourceQuota is used to limit the total amount of resources that can be used in a namespace.
### It can control things like:
•	total CPU 
•	total memory 
•	total number of Pods 
•	total number of PVCs or other objects 
It is used to prevent one namespace from consuming all cluster resources.


## What is limit and request in Kubernetes? / What happens if resource limits are exceeded?
equest is the minimum amount of CPU or memory Kubernetes guarantees to a container for scheduling.
Limit is the maximum amount of CPU or memory a container is allowed to use.
•  if a container exceeds memory limit, it can be killed 
•  if a container exceeds CPU limit, it is usually not allowed to use more CPU than its limit

## What is a YAML file in Kubernetes?
A YAML file in Kubernetes is a configuration file used to define Kubernetes objects like Pods, Deployments, Services, ConfigMaps etc

## What are common fields in a Kubernetes YAML file?
### The common fields in a Kubernetes YAML file are:
•	apiVersion → tells Kubernetes which API version to use for that object 
•	kind → tells what type of object it is, like Pod, Deployment, or Service 
•	metadata → contains basic information about the object, such as name, namespace, labels, and annotations 
•	spec → contains the desired configuration of that object
## What is selector in Kubernetes? / What is label in Kubernetes?
A label is a key-value tag attached to objects like Pods. A selector is used by Kubernetes objects like Services to find matching Pods based on those labels. Labels are important because they help Kubernetes group, identify, and connect resources. For example, a Service uses a selector to send traffic only to Pods with matching labels.

## What is annotation?
An annotation is additional metadata attached to a Kubernetes object.

## What is kubectl? / How do you create a resource using kubectl?
kubectl is the command-line tool used to interact with a Kubernetes cluster.
### kubectl apply -f file.yaml //To create a resource using a YAML file:
### Difference:
•	kubectl create creates a new resource 
•	kubectl apply creates the resource if it does not exist and updates it if it already exists 
kubectl get all shows the common resources in a namespace such as Pods, Services, Deployments, and ReplicaSets.

o list Pods:
kubectl get pods
### To describe a Pod:
`kubectl describe pod <pod-name>`
### To delete a resource:
`kubectl delete -f file.yaml`
`kubectl delete pod <pod-name>`
### To check Pod logs:
`kubectl logs <pod-name>`
### To execute a command inside a Pod:
`kubectl exec -it <pod-name> -- /bin/sh`

### Difference between describe and logs:
•	kubectl describe shows detailed resource information, events, IP, node, status, and configuration 
•	kubectl logs shows the application output from the container


## How does scheduler decide where to place a pod? / What is node selector? / What is node affinity? / What is taints and tolerations?
The scheduler decides where to place a Pod based on available CPU and memory, scheduling rules, node conditions, and constraints.
nodeSelector is the simplest way to force a Pod onto a node with a specific label.
Node affinity is a more advanced way to control Pod placement using label-based rules. It is more flexible than nodeSelector.
Taints are applied on nodes to repel Pods. Tolerations are added to Pods to allow them to run on tainted nodes.



## What happens if a node goes down? / What is pod eviction? / What is cordon and drain in Kubernetes?
If a node goes down, Pods on that node may become unavailable. Kubernetes marks the node as not ready, and if those Pods are managed by a controller like a Deployment, replacement Pods are created on other healthy nodes.
Pod eviction means a Pod is removed from a node, usually because of node problems, resource pressure, or maintenance.
cordon marks a node as unschedulable, so new Pods are not placed on it.
drain safely removes existing Pods from the node and prepares it for maintenance.

## What is RBAC in Kubernetes? / Why is RBAC important? / What is role and role binding? / What is cluster role and cluster role binding?
RBAC stands for Role-Based Access Control. It is used to control who can do what in the Kubernetes cluster.
A Role defines permissions within a namespace.
A RoleBinding attaches that Role to a user, group, or service account.
A ClusterRole defines permissions at the cluster level or for cluster-wide resources.
A ClusterRoleBinding attaches that ClusterRole to a user, group, or service account.
RBAC is important because it enforces least privilege and prevents unauthorized access.
---
## What is service account in Kubernetes?
A ServiceAccount is an identity used by applications or Pods to communicate with the Kubernetes API.
It is mainly used when a Pod needs permissions to access cluster resources.

## Why should containers not run as root?
Containers should not run as root because it is a security risk. If the container is compromised, root access can lead to greater damage on the host or cluster.
---
## What is network policy?
A NetworkPolicy is used to control traffic flow between Pods. It defines which Pods are allowed to communicate with other Pods or external endpoints.


## What is secret management in Kubernetes?
Secret management means storing and using sensitive data such as passwords, tokens, and API keys securely through Kubernetes Secrets instead of hardcoding them in code or images.
---
## How do you monitor Kubernetes cluster? / What is metrics-server? / Why is monitoring important in Kubernetes?
A Kubernetes cluster is monitored using tools such as Metrics Server, Prometheus, and Grafana.
Metrics Server collects basic resource metrics like CPU and memory usage of nodes and Pods.
Monitoring is important to track cluster health, resource usage, performance, failures, and scaling needs.
---
## What is logging in Kubernetes? / How do you check pod logs?
Logging in Kubernetes means collecting application and system logs to understand what is happening inside Pods and the cluster.
### To check Pod logs:
`kubectl logs <pod-name>`


## What is GitOps in Kubernetes? / How does ArgoCD work with Kubernetes?
GitOps is a deployment approach where Git is the single source of truth for Kubernetes manifests.
ArgoCD continuously watches the Git repository and compares it with the actual cluster state. If there is a difference, it syncs the cluster to match the manifests in Git.
### In short:
•	Git stores the desired state 
•	ArgoCD watches Git 
•	ArgoCD applies changes to Kubernetes automatically

## If a pod is not starting, what will you check? / If a pod is stuck in Pending state, what could be the reason?
First, I check the Pod status and events using kubectl describe pod <pod-name>.
### If it is in Pending, common reasons are:
•	insufficient CPU or memory on nodes
•	PVC not bound
•	node selector / affinity mismatch
•	taints without tolerations
•	image pull issue
•	scheduler could not place the Pod
### If it is created but not starting, I check:
•	container image name
•	command and entrypoint
•	environment variables
•	ConfigMap / Secret mount issues
•	volume mount issues
•	readiness or startup probe failures
---
## If a pod is in CrashLoopBackOff, what does it mean?
CrashLoopBackOff means the container is starting, crashing, and Kubernetes is restarting it again and again with backoff delay.
### I check:
•	kubectl logs <pod-name>
•	kubectl describe pod <pod-name>
•	wrong command or entrypoint
•	application crash
•	missing env variables
•	ConfigMap / Secret issue
•	dependency not reachable
•	port mismatch
•	liveness probe failure
---
## If a pod cannot access another pod, what will you check? / If a service is not accessible, what could be wrong?
### I check:
•	whether the target Pod is running
•	whether the Service exists
•	Service selector and Pod labels match or not
•	Endpoints / EndpointSlices are created or not
•	correct port and targetPort
•	DNS resolution of Service name
•	NetworkPolicy blocking traffic
•	application listening on the expected port
### For service issue, the most common reasons are:
•	wrong selector
•	wrong port mapping
•	no healthy backend Pods
•	Pod not listening on targetPort
---
## If an application is not exposed externally, what will you check?
### I check:
•	Service type is correct or not, like NodePort or LoadBalancer
•	if using Ingress, whether Ingress rules are correct
•	Ingress Controller is installed and running
•	host and path mapping are correct
•	backend Service name and port are correct
•	external IP or load balancer is created
•	firewall or security group rules
•	application is reachable internally first
---
## If deployment fails, what will you check first? / If you want to update application version, what will you do?
### First, I check:
•	kubectl get deploy
•	kubectl describe deploy <deployment-name>
•	ReplicaSet and Pod status
•	rollout status
•	events and logs
•	image name and tag
•	readiness probe failures
To update application version, I update the image in the Deployment YAML or use:
`kubectl set image deployment/<name> <container>=<image>:<tag>`
Then I verify rollout status. If it fails, I rollback.
---
## If a node goes down, what happens to pods?
If a node goes down, Pods on that node become unavailable. Kubernetes marks the node NotReady. If those Pods are managed by a Deployment, ReplicaSet, or StatefulSet, replacement Pods are scheduled on healthy nodes, depending on workload type and storage constraints.
---
## If you want to scale your application, what will you do? / If pods are consuming high CPU, what will you check?
For manual scaling, I increase the number of replicas in the Deployment.
For automatic scaling, I use HPA.
### If Pods are using high CPU, I check:
•	actual CPU usage
•	resource requests and limits
•	traffic spike
•	inefficient application code
•	HPA configuration
•	whether more replicas are needed
---
## If you want zero downtime deployment, what will you use?
I use a Deployment with rolling update strategy.
This replaces old Pods gradually with new Pods while keeping some Pods available, so the application stays accessible during deployment.
---
## If configuration changes are needed, what will you use? / If sensitive data needs to be stored, what will you use?
For normal configuration, I use ConfigMap.
For sensitive data like passwords, API keys, and tokens, I use Secret.
---
## If image is not pulling, what could be the reason?
### I check:
•	image name and tag are correct
•	image exists in registry
•	registry access is available
•	ImagePullSecret is configured for private registry
•	network issue from node to registry
•	authentication failure
•	typo in image reference
### Common errors are:
•	ImagePullBackOff
•	ErrImagePull
---
## If container logs are not visible, what will you check?
### I check:
•	Pod and container name are correct
•	correct namespace
•	if Pod has multiple containers, specify container name
•	container is starting and generating logs
•	previous logs if container restarted using --previous
•	application is writing logs to stdout/stderr
---
## If storage is not mounting, what will you check?
### I check:
•	PVC status is Bound or not
•	PV exists or not
•	StorageClass is correct
•	access mode matches
•	volume name and claim name are correct
•	node can attach the volume
•	CSI driver is installed and working
•	events in Pod and PVC
•	permission or filesystem issue inside container
## How does Kubernetes ensure high availability?
Kubernetes ensures high availability by running multiple replicas of the application and continuously maintaining the desired state.
### It uses:
•	ReplicaSet / Deployment to keep the required number of Pods running 
•	self-healing to recreate failed Pods automatically 
•	scheduler to place Pods on available healthy nodes 
•	Services to load balance traffic across multiple Pods 
•	readiness and liveness probes to send traffic only to healthy Pods and restart unhealthy containers 
•	multi-node cluster setup so if one node fails, Pods can be recreated on another node

## Workload objects

## Easy memory grouping
•	Run apps → Pod, Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob 
•	Expose apps → Service, Ingress, NetworkPolicy 
•	Store config/data → ConfigMap, Secret, PV, PVC, StorageClass 
•	Control access → ServiceAccount, Role, RoleBinding, ClusterRole, ClusterRoleBinding 
•	Manage cluster/resources → Namespace, ResourceQuota, LimitRange, HPA, Node


## Kubernetes objects cheat sheet
### Workloads
•	Pod — runs one or more containers 
•	ReplicaSet — keeps required Pod count running 
•	Deployment — manages stateless app rollout/update 
•	StatefulSet — manages stateful apps with identity 
•	DaemonSet — runs one Pod per node 
•	Job — runs task until completion 
•	CronJob — runs Jobs on schedule 
### Networking
•	Service — stable endpoint for Pod access 
•	Ingress — routes external HTTP/HTTPS traffic 
•	NetworkPolicy — controls Pod-to-Pod network access 
•	Endpoints / EndpointSlice — stores backend Pod IPs 
•	IngressClass — defines which controller handles Ingress 
### Config and storage
•	ConfigMap — stores non-sensitive configuration data 
•	Secret — stores sensitive data securely 
•	PersistentVolume (PV) — actual storage resource in cluster 
•	PersistentVolumeClaim (PVC) — request for storage resource 
•	StorageClass — defines dynamic provisioning rules 
### Access and security
•	ServiceAccount — Pod identity for API access 
•	Role — namespace-level permission definition 
•	RoleBinding — attaches Role to user/serviceaccount 
•	ClusterRole — cluster-wide permission definition 
•	ClusterRoleBinding — attaches ClusterRole to subject 
### Resource and organization
•	Namespace — logical separation inside cluster 
•	ResourceQuota — limits total namespace resource usage 
•	LimitRange — sets min/max/default resource values 
### Scaling and availability
•	HorizontalPodAutoscaler (HPA) — auto scales Pods by metrics 
•	PodDisruptionBudget (PDB) — limits voluntary Pod disruptions 
•	PriorityClass — defines Pod scheduling priority 
### Cluster and extension
•	Node — worker machine running Pods 
•	Event — records cluster object changes 
•	CustomResourceDefinition (CRD) — adds custom Kubernetes object types
