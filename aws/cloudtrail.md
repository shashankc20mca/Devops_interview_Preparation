## CloudTrail

### What CloudTrail is

CloudTrail is an AWS service for auditing and tracking account activity. It records events for actions taken by users, roles, and AWS services, including activity from the AWS Management Console, AWS CLI, SDKs, and APIs.

### Why it is used

It is mainly used for:

- security auditing
- tracking who did what
- investigating incidents
- governance and compliance

### How it works

When someone or some AWS service performs an action, CloudTrail records an event. You can view recent management events in Event history, store ongoing logs through a trail, or run SQL-based queries through CloudTrail Lake.

---

## Most expected fresher interview questions with answers

### 1. What is CloudTrail?

CloudTrail is an AWS service that records account activity and API actions for auditing, governance, and compliance. It logs actions performed through the console, CLI, SDKs, and AWS APIs. (AWS Documentation)

### 2. What does CloudTrail record?

CloudTrail records events. An event is a record of activity in an AWS account

---

## CloudTrail has 4 main event types

### 1. Management events

These are control-plane actions: creating, updating, deleting, or describing AWS resources and account settings.

**Simple example:**

You run Terraform and it creates a VPC or changes an IAM policy. Those AWS API calls are typically management events.

### 2. Data events

These are resource-level operations inside a resource, not just creating or configuring the resource itself.

**Simple example:**

Creating an S3 bucket = management event  
Uploading file.txt into that bucket = data event

### 3. Network activity events

These record AWS API calls made through VPC endpoints from a private VPC to an AWS service.

**Simple example:**

An application in your private subnet uses a VPC endpoint to call an AWS service.

### 4. Insights events

These are not normal action logs. They are events generated when CloudTrail notices unusual activity, such as an unusual spike in API call rate or API error rate

**Simple example:**

Normally your account makes around 20 DeleteBucket API calls per minute, but suddenly it jumps to 100 per minute.

### 3. Is CloudTrail enabled by default?

Yes, Event history is available by default, and it gives you the last 90 days of management events in each Region.

### 4. What is Event history in CloudTrail?

Event history is the built-in CloudTrail view for the last 90 days of management events. It is searchable and downloadable

management events are available in Event history by default, but data events, Insights events, and network activity events must be explicitly enabled on a trail or event data store. Even after enabling them, they are not shown in Event history

Data events and network activity events are shown if you log them through either a trail or an event data store.

### 5. What is a trail?

A trail is the CloudTrail configuration that delivers event log files to an S3 bucket for an ongoing record of events. Trails can also integrate with CloudWatch Logs, SNS notifications, KMS encryption, and log file validation.

### 6. What is the difference between Event history and a trail?

Event history is the default 90-day recent view of management events. A trail is something you create for continuous logging and delivery of logs to S3.

### 11. What is CloudTrail Lake?

CloudTrail Lake is a CloudTrail feature that lets you store events in event data stores and run SQL-based queries on them.

### 12. Can CloudTrail tell who created or deleted a resource?

Yes. CloudTrail records details such as the event name, user identity, time, source IP, and related resource information, so it is commonly used to determine who made a change.

### 13. Can CloudTrail show console login activity?

Yes. AWS IAM documents that CloudTrail logs sign-in events, including successful and failed sign-ins, for IAM users and federated principals. (AWS Documentation)

### 14. Can CloudTrail detect if its log files were changed?

CloudTrail supports log file integrity validation. When enabled, CloudTrail delivers digest files, and you can validate whether log files were modified or deleted after delivery. For new trails, log file validation is enabled by default. (AWS Documentation)

---

## Core concepts you must know

### Event

An event is the record of an action in the AWS account. (AWS Documentation)

### Event history

Built-in last 90 days of management events only. (AWS Documentation)

### Trail

Continuous log delivery to S3, with options like multi-Region logging, KMS encryption, CloudWatch Logs, SNS, and validation. (AWS Documentation)

### CloudTrail Lake

SQL query layer for event data stores. (AWS Documentation)

### Log file integrity validation

Checks whether delivered CloudTrail log files were tampered with after delivery. (AWS Documentation)

### Organization trail

A trail that logs events for all accounts in an AWS Organization and can be created by the management or delegated administrator account. (AWS Documentation)

---

## Important differences interviewers may test

### CloudTrail vs CloudWatch

CloudTrail is mainly for who did what and API/account activity. CloudWatch is mainly for metrics, logs, alarms, and operational monitoring. CloudTrail can also deliver logs to CloudWatch Logs, but they are different services with different purposes. (AWS Documentation)

### Event history vs Trail

Event history = default 90-day management-event view.  
Trail = ongoing log delivery and retention in S3. (AWS Documentation)

### Management events vs Data events

Management events = control-plane actions like create/update/delete resource configuration.  
Data events = resource-level actions like object-level access in S3; not logged by default and often high volume. (AWS Documentation)

### Trail vs CloudTrail Lake

Trail = continuous delivery of logs, commonly to S3.  
CloudTrail Lake = event data stores plus SQL queries for analysis. (AWS Documentation)

---

## Project-based questions they may ask you

Since your projects involve EKS, Terraform, IAM, and AWS infra changes, these are likely:

### 1. If someone deleted an EC2 instance or changed a security group, how would you find who did it?

Use CloudTrail to search the relevant event and check details like user identity, event name, event time, and source IP. (AWS Documentation)

### 2. If your Terraform apply or CloudFormation stack caused several AWS API calls, would CloudTrail show only one event?

No. CloudTrail can record the original action and also additional API calls made on the user’s behalf by AWS services. AWS gives the example that a CloudFormation CreateStack call can lead to additional EC2, RDS, or EBS API calls. (AWS Documentation)

### 3. In an EKS project, why is CloudTrail useful?

It helps track who created, modified, or deleted related AWS resources and helps during security reviews, incident investigation, and compliance auditing. CloudTrail records actions made by users, roles, and AWS services. (AWS Documentation)

### 4. If you only rely on Event history, is that enough for long-term auditing?

No. Event history only shows the last 90 days of management events. For ongoing records, create a trail or an event data store. (AWS Documentation)

### 5. If you want to analyze unusual spikes in API failures, what CloudTrail feature helps?

CloudTrail Insights. It identifies unusual API call rates and API error rates. (AWS Documentation)

---

## Scenario-based questions

If an interviewer asks: “How do you find who terminated an EC2 instance?”  
Answer:  
“I would use CloudTrail and search for the relevant EC2 event, such as the termination event, then check user identity, time, and source details.” CloudTrail records EC2 API activity and event details. (AWS Documentation)

If they ask: “How do you keep a long-term audit trail?”  
Answer:  
“I would create a trail so CloudTrail continuously delivers logs to S3.” Event history alone is only 90 days. (AWS Documentation)

If they ask: “How do you query CloudTrail logs with SQL?”  
Answer:  
“I would use CloudTrail Lake, which stores events in event data stores and supports SQL-based queries.” (AWS Documentation)

If they ask: “How do you know if CloudTrail log files were tampered with?”  
Answer:  
“Enable log file integrity validation and validate the logs using the digest files.” (AWS Documentation)
