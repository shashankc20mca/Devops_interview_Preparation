## The flow from zero

### 1. You have an S3 bucket

Think of S3 bucket as a storage folder in AWS.  

Example:  
bucket name: my-bucket

### 2. You create a Lambda function

This function contains your code.  

Example in simple words:  
“Whenever a file is uploaded, print the bucket name and file name.”

### 3. You connect S3 to Lambda

You configure the S3 bucket so that when an event like ObjectCreated happens, S3 triggers the Lambda function.

This is done by adding  an event notification configuration on the bucket that says: for this event type, send the event to this Lambda function.

### 4. User uploads a file

Example:  
user uploads photo.jpg

### 5.

S3 builds a JSON message containing:

- event type
- bucket name
- file name

### 6.S3 sends that JSON event notification  to Lambda.

```json
event = {
  "Records": [
    {
      "eventName": "ObjectCreated:Put",
      "s3": {
        "bucket": {"name": "my-bucket"},
        "object": {"key": "photo.jpg"}
      }
    }
  ]
}
```

### 7.

Lambda starts your function.

### 8.

Inside your Lambda code, you read:

- bucket name
- file name

### 9.

Then your own logic runs.  

Examples:

- print file name
- print bucket name

### Step 8

When your code finishes, Lambda stops.

---

upload happens → S3 sends event → Lambda runs → work gets done.

---

## how does s3 knows it need to trigger lambda funcition

### Simple flow

- You create a Lambda function.
- In the S3 bucket, you add an event notification rule.
- In that rule, you choose:
  - event type, like s3:ObjectCreated:*
  - destination, which is your Lambda function

After that, whenever a file upload matches that rule, S3 triggers Lambda by sending json messege

---

“Lambda functions can be written in languages like Python, Node.js, Java, .NET, and Ruby. AWS also supports custom runtimes for other languages.”

---

## Most expected fresher interview questions with answers

### 1. What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code without managing servers and scales automatically based on incoming requests or events.

### 2. Why is Lambda called serverless?

Because you do not provision or manage servers yourself. AWS manages the underlying compute environment for you.

### 3. What are common Lambda triggers?

Common triggers include API Gateway, S3, SQS, DynamoDB Streams, and other AWS services that send events to Lambda.

### 4. What is the maximum execution time of Lambda?

The maximum timeout is 15 minutes.

### 5. Can Lambda be used to build APIs?

Yes. AWS documents Lambda integration with API Gateway to expose HTTP endpoints for Lambda functions.

### 6. What is the difference between Lambda and EC2?

Lambda is serverless and event-driven for short-running functions. EC2 is a virtual server where you manage the OS, instance sizing, patching, and long-running applications yourself

## Most expected fresher interview questions with answers

### 1. What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code without managing servers and scales automatically based on incoming requests or events.

### 2. Why is Lambda called serverless?

Because you do not provision or manage servers yourself. AWS manages the underlying compute environment for you.

### 3. What are common Lambda triggers?

Common triggers include API Gateway, S3, SQS, DynamoDB Streams, and other AWS services that send events to Lambda.

### 4. What is the maximum execution time of Lambda?

The maximum timeout is 15 minutes.

### 5. Can Lambda be used to build APIs?

Yes. AWS documents Lambda integration with API Gateway to expose HTTP endpoints for Lambda functions.

### 6. What is the difference between Lambda and EC2?

Lambda is serverless and event-driven for short-running functions. EC2 is a virtual server where you manage the OS, instance sizing, patching, and long-running applications yourself

---

Reserved concurrency means you keep a fixed portion of Lambda capacity for one function. It does two things at the same time: it guarantees capacity for that function, and it also limits how many copies of that function can run at once. Example: if you set reserved concurrency to 10, that function can use up to 10 concurrent executions, and other functions cannot take that reserved part

Provisioned concurrency means Lambda keeps some execution environments already warm and ready before requests arrive, so startup is faster and cold starts are reduced. Example: if you set provisioned concurrency to 5, Lambda keeps 5 ready-to-run environments waiting. This is mainly for low-latency APIs or interactive workloads

reserved concurrency is about capacity control, while provisioned concurrency is about startup speed.

---

## Now the 4 basic words you must know:

- A function is your code.
- An event is the input that tells Lambda what happened.
- A trigger is the AWS service or setup that sends that event.
- A runtime is the language environment, like Python or Node.js, in which your code runs.

---

## Scenario-based answers

If interviewer asks: “When would you choose Lambda?”  

“I would choose Lambda for event-driven, short-running workloads where I don’t want to manage servers, such as API handlers, S3 file processing, queue consumers, or scheduled jobs.”

If interviewer asks: “When would you not choose Lambda?”  

“I would avoid Lambda for workloads that need long-running servers, deep OS-level control, or applications better suited to continuously running containers or VMs.” This is a practical inference from Lambda’s serverless execution model and 15-minute timeout.

If interviewer asks: “How can you reduce cold starts?”  

“I can reduce cold starts by optimizing initialization and using provisioned concurrency for latency-sensitive workloads.”

If interviewer asks: “How do you monitor concurrency?”  

“Lambda publishes CloudWatch concurrency metrics, and I can use those to monitor function concurrency and throttling behavior.”

If interviewer asks: “How does Lambda process SQS messages?”  

“Lambda polls SQS through an event source mapping, receives message batches, invokes the function with the batch, and deletes messages after successful processing.”
