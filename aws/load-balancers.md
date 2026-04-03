# Load balancer

A load balancer sits in front of your app and distributes traffic to backend targets like servers, Pods, or IPs. AWS Elastic Load Balancing supports four types: ALB, NLB, GWLB, and CLB.

## 1. ALB = for websites and APIs

Use ALB when the traffic is normal web traffic:  
HTTP  
HTTPS  
websites  
REST APIs  
microservices web routing  

AWS positions ALB for application-level web traffic and supports host/path-based routing.

### Real-world example

Suppose you have one website:  
xyz.com → frontend  
xyz.com/api → backend API  
admin.xyz.com → admin panel  

Now ALB can look at the request and decide:  
/ → frontend service  
/api → backend service  
admin.xyz.com → admin service  

### Flow

User → ALB → correct web service

### Why use ALB?

Because it understands web requests and can do smart routing.

### Memory line

ALB = smart load balancer for websites

### Path-based routing

Routing based on the URL path.  
Example:  
xyz.com/api → backend service  
xyz.com/login → auth service

### Host-based routing

Routing based on the domain / subdomain name.  
Example:  
app.xyz.com → frontend service  
admin.xyz.com → admin service

---

## 2. NLB = for fast network traffic

Use NLB when you care about:  
TCP / UDP / TLS traffic  
very high performance  
very low latency  
static IP  
source IP preservation  

AWS documents NLB for transport-layer traffic such as TCP, UDP, TLS, and QUIC.

### Real-world example

Suppose you run:  
gaming backend  
voice/chat server  
custom TCP application  
very high-throughput non-HTTP service  

In that case, you usually do not care about /api or /login style routing.  
 You care more about speed and connection handling.

### Flow

Client app / game app → NLB → backend servers

### Why use NLB?

Because it is built for fast connection-level traffic, not smart website path routing.

### Memory line

NLB = fast load balancer for network traffic

A gaming platform can have many microservices:  
Login service → usually behind ALB  
Profile service → usually behind ALB  
Payment/store service → usually behind ALB  
Matchmaking service → can be ALB or NLB, depending on protocol  
Live game server / session server → often behind NLB  
Voice/chat real-time service → often behind NLB if using TCP/UDP and low latency

---

## 3. GWLB = for security appliances

GWLB does both things:  
it load balances traffic across multiple security appliances  
and those security appliances inspect/scan/filter the traffic  

### Very simple meaning

GWLB itself is not the firewall scanner.  
Instead:  
GWLB = traffic distributor + traffic entry/exit point  
security appliances behind it = actual scanners/firewalls/inspection tools  

### Easy flow

User traffic → GWLB → one of the firewall/inspection appliances → application

---

## CLB (Classic Load Balancer) → older legacy load balancer

Example: old AWS projects still using previous generation LB

---

## One-line memory trick

Direct LB = LB does the exposure.  
 Envoy Gateway = LB gives entry, Envoy does the smart routing.

---

## Ingress and Envoy Gateway

Ingress and Envoy Gateway solve a similar high-level problem:  
both are used to expose services from Kubernetes to outside traffic.  
But they are not the same level of solution.

### Very simple answer

Ingress = older, simpler Kubernetes way to expose mainly HTTP/HTTPS services  
Gateway API + Envoy Gateway = newer, more expressive, more powerful way to expose and manage traffic

### Simple understanding

#### Directly using LB

If you directly use a load balancer, traffic forwarding is usually more basic:  
expose service  
send traffic to backend  
limited routing logic  

#### Using Ingress or Envoy Gateway

Ingress or Envoy Gateway adds a Kubernetes traffic-routing layer on top of the load balancer.  
That means you can do more advanced things like:  
path-based routing  
host-based routing  
header-based routing  
traffic splitting  
canary routing  
retries / timeouts  
better policy control  

---

## Corrected flow

### 1. User uses DNS name

User hits xyz.com

### 2. Route 53 resolves DNS

Route 53 returns the load balancer DNS target. Route 53 is doing DNS resolution, not forwarding the actual HTTP request.

### 3. Client sends request to the load balancer

After DNS resolution, the browser/client sends the request to the LB.

### 4. LB sends traffic to Envoy

Since your Gateway API / Envoy setup exposed Envoy externally, the LB sends traffic to the Envoy data plane / Envoy proxy endpoint, not really to the “controller.” Envoy Gateway implements Gateway API and translates Gateway API resources into Envoy configuration; the actual request path goes through Envoy proxy, which is the traffic-handling component.

### 5. Envoy applies routing rules

Based on your Gateway / HTTPRoute rules, Envoy decides which backend Service to send traffic to. Envoy Gateway supports routing to native Kubernetes resources such as Service.

### 6. Service selects Pods

Your frontend Service selects the matching Pods using labels/selectors. Kubernetes Services expose an app through one stable endpoint even when the workload is split across multiple Pods.

### 7. kube-proxy handles Service-to-Pod routing

Yes — kube-proxy on nodes implements the Service virtual IP behavior and forwards Service traffic to one of the backend Pods/endpoints. That Pod may be on the same node or another node.

### 8. Pod responds back

The selected Pod serves the request, and the response goes back through the chain to the user.
