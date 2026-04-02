# Prometheus and Grafana

---

## 1. What is Monitoring?

Monitoring means continuously checking the health, performance, and availability of systems, servers, applications, and services.

**It helps answer:**

- Is the system running?
- Is CPU or memory high?
- Is the application slow?
- Is anything failing?

In simple terms, monitoring helps us detect problems early and keep systems healthy.

---

## 2. What is Prometheus?

Prometheus is an open-source monitoring and alerting tool. It collects metrics such as CPU, memory, disk, network, and application metrics, stores them as time-series data, and helps query them.

It is mainly used to monitor servers, containers, Kubernetes, and applications.

---

## 3. What is Grafana?

Grafana is an open-source visualization tool used to create dashboards from monitoring data.

It connects to data sources like Prometheus and shows metrics in the form of graphs, panels, charts, and dashboards.

In simple terms, Prometheus collects data and Grafana displays it.

---

## 4. Why use Prometheus + Grafana?

Prometheus and Grafana are commonly used together because:

- Prometheus collects and stores metrics
- Grafana visualizes those metrics in dashboards
- Prometheus can trigger alerts
- Grafana makes monitoring easy to understand

**In short:**

- Prometheus = metrics collection and alerting
- Grafana = visualization and dashboards

---

## 5. How Prometheus works

Prometheus works using a pull model.

**Flow:**

1. Target systems expose metrics on an HTTP endpoint
2. Prometheus scrapes those metrics at regular intervals
3. It stores them in its database
4. Users query the data using PromQL
5. Alerts can be triggered based on conditions

**Example:**

- Node Exporter exposes server metrics
- Prometheus pulls those metrics
- Grafana shows them in a dashboard

---

## 6. How Grafana works

Grafana connects to a data source like Prometheus and fetches the stored metrics.

**Flow:**

1. Grafana connects to Prometheus
2. User creates panels and dashboards
3. Grafana sends queries to Prometheus
4. Prometheus returns metric data
5. Grafana displays it visually

Grafana does not collect metrics by itself. It only reads data from data sources and visualizes it.

---

## 7. What is Node Exporter?

Node Exporter is a Prometheus exporter used to collect hardware and OS-level metrics from a Linux server.

**It provides metrics like:**

- CPU usage
- memory usage
- disk usage
- filesystem stats
- network stats
- system load

It is commonly installed on servers so Prometheus can monitor them.

---

## 8. How Node Exporter works

Node Exporter runs as a process on the server and exposes system metrics on an HTTP endpoint, usually:

`http://<server-ip>:9100/metrics`

**Flow:**

1. Node Exporter collects system-level metrics from the server
2. It exposes them in Prometheus format
3. Prometheus scrapes that endpoint regularly
4. Grafana uses Prometheus data to show dashboards


Yes. Now I’ll keep it exactly to what you **actually need for interview** based on **your Prometheus config + alert_rules.yml**.

I am using these facts from your files:

* Prometheus scrape interval: **15s**
* Evaluation interval: **15s**
* Alertmanager target: **35.153.198.185:9093**
* Prometheus target: **35.153.198.185:9090**
* Node discovery: **`ec2_sd_configs` in `us-east-1` on port `9100`**
* Filtering only **running EC2 instances**
* Filtering only **your EKS nodes** using cluster autoscaler tag = `owned`
* Scraping with **private IP**
* Blackbox Exporter address: **35.153.198.185:9115**
* Blackbox monitored:

  * **ALB URL**
  * **`http://35.153.198.185:8080/health`**
* Alerts:

  * **InstanceDown**
  * **WebsiteDown**
  * **HostOutOfMemory**
  * **HostOutOfDiskSpace**
  * **HostHighCpuLoad**
  * **ServiceUnavailable**
  * **HighMemoryUsage**
  * **FileSystemFull**

---

# Say this first if they ask “Explain your monitoring project”

**Answer:**
In my EKS project, I implemented monitoring using Prometheus, Grafana, Alertmanager, Node Exporter, and Blackbox Exporter. Prometheus scraped worker-node metrics every 15 seconds using EC2 service discovery with private IPs and relabel filters, and Blackbox Exporter monitored the ALB URL and application health endpoint. I created warning and critical alerts for availability, CPU, memory, and disk conditions, and Alertmanager sent email notifications. Grafana dashboards helped visualize node health and endpoint availability for faster troubleshooting.

---

# Super important questions you MUST answer

## 1. Why did you use Prometheus in this project?

**Answer:**
I used Prometheus to collect metrics, store time-series data, and evaluate alert rules. It helped me monitor node health, resource usage, and endpoint availability in my EKS environment.

---

## 2. Why did you use Grafana?

**Answer:**
I used Grafana to visualize the metrics collected by Prometheus. It made CPU, memory, disk, and endpoint health easy to understand through dashboards.

---

## 3. Why did you use Alertmanager?

**Answer:**
Prometheus detects alert conditions, but Alertmanager handles alert routing and notifications. I used it to send email alerts when critical conditions stayed true for the configured duration.

---

## 4. What exactly were you monitoring?

**Answer:**
I monitored two things mainly: infrastructure health and endpoint availability. For infrastructure, I monitored CPU, memory, disk, and node availability using Node Exporter. For endpoint monitoring, I used Blackbox Exporter to check my ALB URL and application health endpoint.

---

## 5. How did Prometheus discover your EKS nodes?

**Answer:**
I used AWS EC2 service discovery with `ec2_sd_configs`. Since EKS worker nodes are EC2 instances, Prometheus could automatically discover them from AWS instead of using static IPs.

---

## 6. Why did you use EC2 service discovery instead of static targets?

**Answer:**
Because EKS nodes are dynamic. They can be added, removed, or replaced during scaling or cluster operations, so service discovery avoids manual updates in Prometheus.

---

## 7. How did you make sure Prometheus scraped only your EKS nodes?

**Answer:**
I used relabeling rules. I kept only instances that were in `running` state and only those with my cluster autoscaler tag value set to `owned`, so unrelated EC2 instances were filtered out.

---

## 8. Why did you use private IP for scraping?

**Answer:**
Because my EKS worker nodes were in private subnets. Prometheus could reach them internally, so scraping with private IP was more secure and appropriate than using public IPs.

---

## 9. What is Node Exporter and why did you use it?

**Answer:**
Node Exporter is a Prometheus exporter that exposes system-level metrics such as CPU, memory, disk, and filesystem metrics. I used it to monitor the health of EKS worker nodes.

---

## 10. What is Blackbox Exporter and why did you use it?

**Answer:**
Blackbox Exporter is used for synthetic monitoring. Instead of collecting internal server metrics, it checks whether an endpoint is actually reachable over HTTP, so it tells me whether the application is available from an external point of view.

---

## 11. What is the difference between Node Exporter and Blackbox Exporter?

**Answer:**
Node Exporter gives internal infrastructure metrics like CPU, memory, and disk usage. Blackbox Exporter checks external availability, like whether a website or health endpoint is reachable.

---

## 12. Which endpoints did you monitor using Blackbox Exporter?

**Answer:**
I monitored the application load balancer URL and the application health endpoint at port 8080. This helped me track both public accessibility and application health.

---

## 13. What does `probe_success == 0` mean?

**Answer:**
It means the Blackbox probe failed. In my project, that means the website or health endpoint was not reachable successfully.

---

## 14. What does `up == 0` mean?

**Answer:**
It means Prometheus could not scrape the target successfully. Usually that indicates the exporter is down, the node is unreachable, or there is a network issue.

---

## 15. Why did you keep scrape interval and evaluation interval at 15 seconds?

**Answer:**
I kept both at 15 seconds to get faster visibility and faster alert evaluation. Since this was a monitoring-heavy lab-style setup, I wanted quicker detection instead of waiting one minute for every scrape or rule evaluation.

---

## 16. What is the use of `for:` in alert rules?

**Answer:**
The `for:` value makes sure the condition stays true continuously for that time before firing the alert. This helps avoid false alerts from temporary spikes or brief failures.

---

## 17. Why did you use warning and critical severities?

**Answer:**
I used warning for early-stage resource pressure and critical for serious problems like service downtime or severe memory/disk conditions. This helps prioritize alerts properly.

---

## 18. Explain the `InstanceDown` alert.

**Answer:**
This alert checks if Prometheus is unable to scrape a target for more than 1 minute. It is useful for detecting unreachable exporters, node issues, or connectivity problems.

---

## 19. Explain the `WebsiteDown` alert.

**Answer:**
This alert is based on `probe_success == 0` for 1 minute. It means the monitored endpoint is unavailable continuously for 1 minute, so it helps detect public service outages quickly.

---

## 20. Explain the `ServiceUnavailable` alert.

**Answer:**
This alert specifically checks if the `node_exporter` job is down for 2 minutes. It helps me identify monitoring failure or node exporter unavailability on worker nodes.

---

## 21. Explain the `HostOutOfMemory` alert.

**Answer:**
This is a warning alert that fires when available memory drops below 25% for 5 minutes. It acts as an early warning that the node is under memory pressure.

---

## 22. Explain the `HighMemoryUsage` alert.

**Answer:**
This is a critical alert that fires when memory usage remains above 90% for 10 minutes. I kept the duration longer because I wanted to catch sustained memory pressure, not short-lived spikes.

---

## 23. Explain the `FileSystemFull` alert.

**Answer:**
This is a critical alert that fires when the filesystem has less than 10% free space for 5 minutes. Low disk space can affect logs, writes, and system stability, so it is a serious condition.

---

## 24. Explain the `HostHighCpuLoad` alert.

**Answer:**
This is a warning alert meant to detect sustained high CPU usage for 5 minutes. The idea is to identify resource pressure early before it impacts application performance.

---

## 25. Why did you monitor both node health and endpoints?

**Answer:**
Because both are important and they answer different questions. Node monitoring tells me whether infrastructure is healthy, while endpoint monitoring tells me whether the actual application is reachable by users.

---

# Very likely follow-up questions

## 26. If the website is down but the node is healthy, what does that mean?

**Answer:**
That usually means the issue is not at the infrastructure level. The problem may be in the application, service, ingress, load balancer, or health path.

---

## 27. If Prometheus is not scraping a node, what will you check?

**Answer:**
I will check whether the EC2 instance is in running state, whether it matches the cluster tag filter, whether Node Exporter is running on port 9100, and whether Prometheus can reach the private IP over the network.

---

## 28. If a new worker node is added, will Prometheus detect it automatically?

**Answer:**
Yes. Because I used EC2 service discovery, Prometheus automatically discovers new EC2 instances, and if they match the relabel filters, it starts scraping them.

---

## 29. What is synthetic monitoring?

**Answer:**
Synthetic monitoring means actively probing an endpoint to check whether it is reachable and responding correctly. In my project, this was done using Blackbox Exporter.

---

## 30. Why is out-of-cluster monitoring useful?

**Answer:**
It keeps the monitoring stack independent from the application cluster. So even if the cluster has issues, monitoring and alerting can still continue.

---

## 31. Why did you monitor the ALB URL?

**Answer:**
Because that is the public entry point for end users. If the ALB URL is unavailable, it directly affects user access to the application.

---

## 32. Why did you monitor the `/health` endpoint also?

**Answer:**
Because the health endpoint helps confirm whether the application itself is responding properly, not just whether the public URL is reachable.

---

## 33. What is relabeling in Prometheus?

**Answer:**
Relabeling is used to modify, filter, or rewrite labels before Prometheus starts scraping the target. In my setup, I used it to keep only the correct EC2 instances and build the scrape address using the private IP.

---

## 34. Why did you filter only running instances?

**Answer:**
Because only running instances are valid scrape targets. Scraping stopped or terminated instances would just create failed targets and unnecessary noise.

---

## 35. What is the role of the cluster autoscaler tag in your config?

**Answer:**
It helps identify which EC2 instances belong to my EKS cluster. I used it to make sure Prometheus monitored only those nodes and not every EC2 instance in the region.

---

## 36. What happens when an alert condition becomes true?

**Answer:**
Prometheus evaluates the rule every 15 seconds. If the condition stays true for the defined `for:` duration, Prometheus fires the alert to Alertmanager, and Alertmanager sends the email notification.

---

## 37. Why do we need both dashboards and alerts?

**Answer:**
Dashboards are useful for visualization and analysis, while alerts are needed for active notification when something goes wrong. Both are important in a practical monitoring setup.

---

# Questions they may ask to test whether you really did the project

## 38. Why was your Blackbox target not directly scraped by Prometheus?

**Answer:**
Because Prometheus does not itself perform the HTTP probe logic. It sends the target URL to Blackbox Exporter through `/probe`, and Blackbox Exporter performs the actual check and returns probe metrics.

---

## 39. Why is the Blackbox exporter address replaced with `35.153.198.185:9115`?

**Answer:**
Because that is the real exporter endpoint Prometheus scrapes. The actual target URL is passed as a parameter, and Blackbox Exporter runs the HTTP probe against it.

---

## 40. What kind of alerts were critical in your project?

**Answer:**
The critical alerts were `InstanceDown`, `WebsiteDown`, `ServiceUnavailable`, `HighMemoryUsage`, and `FileSystemFull`, because these indicate either service unavailability or severe resource pressure.



