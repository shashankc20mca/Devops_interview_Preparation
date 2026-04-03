# CloudWatch

## 1. What is CloudWatch?

Amazon CloudWatch is AWS’s monitoring and observability service. It helps you monitor AWS resources and applications using metrics, logs, alarms, dashboards

## 2. Why is it used? / What problem does it solve?

Without CloudWatch, it is hard to know:  
whether a server is healthy  
whether CPU or memory is high  
whether an application is failing  
whether you should send an alert or take automatic action

## 3. Important concepts / terms

A metric is a measurable value over time, like CPU utilization,disk usage,memory utilization

A namespace is like a container for metrics. AWS service metrics and custom metrics are organized into namespaces

A dimension is a name-value pair that identifies a metric more specifically, such as InstanceId=i-123....

Suppose you have 3 EC2 instances.  
All of them send the same metric name:  
CPUUtilization  

Now CloudWatch needs to know:  
CPU of which instance?  
instance 1?  
instance 2?  
instance 3?  

That is why dimension is used.

For example:  
Metric name = CPUUtilization  
Namespace = AWS/EC2  
Dimension = InstanceId=i-123

A CloudWatch alarm watches a metric based on a threshold, such as CPU > 80%. It can then notify(using sns service) or trigger actions

CloudWatch Logs is the part of CloudWatch used to collect, store, view, search, and analyze log data from your applications, servers, and AWS services. It centralizes logs in one place and keeps them ordered by time.

Note:EC2 logs are sent to CloudWatch Logs only if you configure log collection, usually by installing and configuring the CloudWatch agent on the EC2 instance

A dashboard is a visual screen where you can see important metrics, graphs, and alarms in one place.

The CloudWatch agent is installed on EC2, on-prem servers, or containers to collect extra metrics, logs, and traces like memory, disk, and application logs, which basic default monitoring does not fully provide.

### what is logs and metrics difference

Metrics are numeric measurements over time, like CPU utilization, memory usage, request count, or latency.  
Logs are detailed event records that help explain what was happening in the system or application at that time.

### CPU example

Metric: CPU utilization is 95%  
Log: process X started, query Y ran, service Z threw repeated errors

A metric filter lets you create metrics from log data by matching patterns in logs, and then you can alarm on those metrics.

### Example

You can say:  
“If my application logs contain the word ERROR more than 5 times in 5 minutes, trigger an alarm.”

To do that:  
send logs to CloudWatch Logs  
create metric filter on "ERROR" -> WORD  
metric becomes something like ErrorCount  
create alarm on ErrorCount > 5 (means if error word appears in the log for more then 5 times in 5 minutes send alarm)

CloudWatch Events is now RENAMED AS Amazon EventBridge.

CloudWatch alarm state changes can send events to EventBridge for automation

### Simple EC2 CPU example

Suppose you created a CloudWatch alarm:  
Alarm name: HighCPU  
Rule: CPU > 80%  

Now imagine CPU becomes 90%.  
 CloudWatch alarm changes from:  
OK → ALARM  

When that state change happens, CloudWatch sends an event to EventBridge. Then EventBridge can match a rule and take action, such as:  
notify you  
trigger a Lambda  
start automation

## How long does CloudWatch keep metrics?

CloudWatch metric data is kept for 15 months.

## 5. Practical fresher questions

### If CPU usage goes above 80%, how will you monitor it?

Create a CloudWatch alarm on the CPUUtilization metric and set the threshold to 80%.

### If you want memory utilization from EC2, is default CloudWatch enough?

Usually no. You commonly need the CloudWatch agent to collect memory metrics.

### If an app is writing error logs, how can CloudWatch help?

Send logs to CloudWatch Logs, search them, create metric filters from error patterns, and alarm on them.

### If you want one screen to see CPU, network, alarms, and logs, what will you use?

CloudWatch dashboard.

### If you want automation when an alarm changes state, what service is commonly used with CloudWatch?

EventBridge

## 6. Quick revision

CloudWatch = monitoring + observability  
Metrics = numbers over time  
Logs = text/event records  
Alarm = threshold-based alert  
Dashboard = visualization  
Agent = extra metrics/logs/traces from EC2/on-prem/containers  
Metric filter = create metric from logs  
CloudWatch Events = now EventBridge  
Metrics retention = 15 months

## Usecase of cloudwatch with ec2 instance

Suppose you launched one EC2 instance running your application.

Now you want to know:  
Is CPU usage normal?  
Is the server under heavy load?  
Should I get an alert if CPU becomes too high?  

This is where CloudWatch helps.

### Exact flow

EC2 instance → EC2 publishes CPU metric → CloudWatch stores it → you create alarm on that metric → CloudWatch checks threshold → if threshold is crossed(i.e If CPU goes above 80% for 5 minutes) send me an alert , alarm changes state and triggers action (can notify through SNS )

That means:  
Metric = CPUUtilization  
Namespace = AWS/EC2  
Dimension = InstanceId=<your-instance-id>  
Threshold = > 80%  
Period = 5 minutes if using basic monitoring, or 1 minute if using detailed monitoring  
Action = send notification, usually through SNS

## Without agent metrics that are by default measured

CPU utilization  
Network in/out  
Disk read/write activity  
Status checks  

## With agent metrics that are measured

Memory usage  
Disk usage percentage / free / used  
More detailed OS-level metrics

Note As soon as you create and run an EC2 instance, Amazon EC2 sends some default metrics to CloudWatch automatically. By default those datapoints are sent at 5-minute intervals. If you enable detailed monitoring, they are sent at 1-minute intervals.

## Doubt:ok as soon as metric cross threshold cloudwatch alarm state changes from ok to alarm which will trigger the event bridge to do the task we specified like sending notification to sns

## Answer:

So there are two common ways:

### 1. Direct path

Metric crosses threshold  
→ CloudWatch alarm changes to ALARM  
→ CloudWatch alarm directly sends notification to SNS  

This is the simplest and most common alerting flow.

### 2. EventBridge path

Metric crosses threshold  
→ CloudWatch alarm changes to ALARM  
→ CloudWatch sends an alarm state change event to EventBridge  
→ EventBridge rule matches it  
→ EventBridge triggers target such as Lambda / SNS / automation  

CloudWatch sends alarm state change events to EventBridge whenever an alarm changes state.

## So what is the difference?

Use direct SNS action when you just want a simple alert.  
Use EventBridge when you want more flexible event-driven automation, like matching specific alarm events and then triggering different targets or workflows.
