# Route 53

Let’s learn Route 53 from zero, especially the routing types.

## First, what Route 53 is

Think of Route 53 as the phonebook of the internet.  
When a user types myapp.com, the browser does not understand that name directly. It needs the IP address or AWS target behind it. Route 53 answers that question and tells the internet where to send the user. AWS describes Route 53 as its DNS service, and its main functions are DNS routing, domain registration, and health checks. 

## Very basic words

A domain name is something like myapp.com.  
A DNS record is an instruction that says where that name should go.  
A hosted zone is just a container that stores all DNS records for a domain. A public hosted zone is for internet users. A private hosted zone is for internal DNS inside a VPC.  
An alias record is a Route 53 special record that can point directly to AWS resources like load balancers, CloudFront, and S3 website endpoints. It is similar to CNAME in idea, but it is AWS-specific and is commonly used with AWS services. (AWS Documentation)  
A health check is Route 53 checking whether your app or server is healthy. Route 53 can check using HTTP, HTTPS, or TCP and can route traffic away from unhealthy resources. (AWS Documentation)  

## What “routing policy” means

A routing policy simply means:  
“When many possible destinations exist, how should Route 53 decide where to send the user?” 

So routing type is not network routing like routers and switches. Here it means DNS decision logic.

If you have:  
one load balancer  
domain xyz.com  
Route 53 alias record  
and it points to that single load balancer  
then that is Simple routing.

---

## 1. Simple routing

This is the easiest one.  
You have one app, one load balancer, one destination.

---

## 2. Weighted routing

### When you use it

When you want to split traffic between two or more targets.

### In your case

Suppose you have:  
LB1 → old version of app  
LB2 → new version of app  

Then in Route 53 you create two records for xyz.com:  
Alias record to LB1 with weight 80  
Alias record to LB2 with weight 20  

### What happens

around 80% traffic goes to old app  
around 20% traffic goes to new app  

### Real use case

canary deployment  
testing new version slowly  
blue/green rollout

---

## 4. Latency-based routing

### When you use it

When the same application is deployed in multiple AWS Regions and you want users to go to the region with the lowest latency.

### In your case

Suppose you deploy:  
one LB in Mumbai region  
one LB in Singapore region  

Then create:  
latency record for xyz.com → Mumbai LB  
latency record for xyz.com → Singapore LB  

### What happens

Indian users may go to Mumbai LB  
some Southeast Asia users may go to Singapore LB  
Route 53 chooses the endpoint with better latency  

### Real use case

global app  
multi-region deployment  
better user response time

---

## 4. Latency-based routing

Now imagine your app is deployed in:  
Mumbai  
Singapore  
London  

A user from India should go to the endpoint that gives the lowest latency, not just any endpoint.  
That is latency-based routing.  

AWS says latency routing sends users to the resource that provides the lowest latency. (AWS Documentation)  

### Real scenario:

You have the same app in multiple AWS Regions and want better user response time.

### Memory line:

Latency routing = send user to the fastest region.

### Important:

It is not exactly “nearest by map.”  
It is based on AWS latency measurements.

---

## 5. Geolocation routing

### When you use it

When you want routing based on the user’s country or continent, not just speed.

### In your case

Suppose you have:  
India LB for Indian users  
US LB for US users  
Europe LB for Europe users  

Then create geolocation records:  
India users → India LB  
US users → US LB  
Europe users → Europe LB  

### What happens

Users are sent based on where they are coming from geographically.

### Real use case

country-specific content  
language-specific websites

---

## 6. Geoproximity routing

### In your setup

Suppose you deploy your app in:  
Mumbai LB  
Singapore LB  

Both serve the same domain: xyz.com  

Now Route 53 geoproximity routing decides:  
users nearer Mumbai usually go to Mumbai LB  
users nearer Singapore usually go to Singapore LB  

But you can apply bias.

### What is bias?

Bias means:  
positive bias = send more nearby traffic to that region  
negative bias = send less nearby traffic to that region  

### Easy scenario

Suppose Mumbai is strong and can handle more traffic.  
Then you give Mumbai a positive bias.  

### Result:

even some users who are not extremely close to Mumbai may still be routed there

---

## 7. Multivalue answer routing

### Simple meaning

Route 53 returns multiple healthy records for the same domain. AWS says multivalue answer routing returns multiple healthy records in response to DNS queries.

### In your setup

Suppose instead of one LB, you have:  
LB1  
LB2  
LB3  
all serving the same application for xyz.com  

Now when a user asks for xyz.com, Route 53 can return:  
LB1  
LB2  
LB3  
but only the healthy ones.  

If LB2 is unhealthy, Route 53 returns only:  
LB1  
LB3  

### Easy scenario

You have 3 app endpoints and want simple DNS-level spreading with only healthy answers.

---

## 8. IP-based routing

Send traffic based on the client’s source IP range. AWS includes IP-based routing as a Route 53 routing policy that uses CIDR collections and the client source IP.

### In your setup

Suppose your application is used by:  
office users  
external public users  

You can define:  
if request comes from office company IP range → send to internal-special LB  
if request comes from other IP ranges → send to public LB

---

## The easiest way to compare routing types

Use this shortcut:  
Simple = one target  
Weighted = split traffic  
Failover = main and backup  
Latency = fastest region  
Geolocation = user country/location  
Geoproximity = nearest resource with traffic shaping  
Multivalue = return multiple healthy answers  
IP-based = route by client IP range 

---

## Alias record in simple words

Suppose your EKS app is exposed through an AWS load balancer.  
Instead of remembering the long load balancer DNS name, you create:  
myapp.com → alias → ALB

## Health checks in simple words

Health check means Route 53 keeps asking:  
“Is this app/server still alive?”  

If the answer is no, Route 53 can stop sending traffic there, especially in failover-style setups.

## In your current setup

Route 53 resolves xyz.com to the LB  
ALB checks backend instances/Pods using its own health checks  
Route 53 health is not doing much unless you create separate health checks or add multiple DNS targets.

## If you create a real Route 53 health check

Route 53 itself sends HTTP/HTTPS/TCP checks to the endpoint you specify  
this becomes useful mainly for failover or multi-record DNS decisions.

---

## Best simple interview answer

“Route 53 is AWS’s DNS service. It maps domain names to resources like load balancers, EC2, or other endpoints. The routing policy decides how Route 53 chooses the answer. Simple routing is for one target, weighted routing is for splitting traffic, failover routing is for primary-backup setup, latency routing is for sending users to the fastest region, and geolocation routing is for sending users based on their location.” 

---

## Easy interview answer

“In my project I did not directly configure the AWS load balancer as the main routing component. I used Envoy Gateway with Kubernetes Gateway API. That setup created an external load balancer for entry, but the actual application routing logic was handled by Envoy inside the cluster. So compared to directly creating a load balancer, my setup is more Kubernetes-native and gives more advanced L7 routing and policy control.”
