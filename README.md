# Serverless REST API with DynamoDB and API Gateway

![AWS Architecture Diagram](architecture%20diagram.png)

## Overview

This architecture demonstrates a **serverless backend** solution using AWS services to handle user requests through an API. Built with scalability, cost-efficiency, and modularity in mind, leveraging the benefits of **AWS Lambda** for compute, **DynamoDB** for storage, and **CloudWatch** for monitoring and logging.**CloudFront** for global caching and fast delivery.**AWS Cognito**  for user authentication.**API Gateway** the entry point for client requests. The app serves a simple to-do list interface where users can manage their tasks

---

## Architecture Workflow
1. When a user accesses the domain, they are served a static frontend from an Amazon S3 bucket.
2. CloudFront caches and delivers the frontend globally, improving speed and reducing latency.
3. The user signs in or registers through Amazon Cognito.
4. As they interact with the frontend, API calls are made to Amazon API Gateway.
5. **API Gateway**:
  - Validates requests.
  - Authorizes them using Cognito authorizers.
  - Routes them to the appropriate AWS Lambda function.
6. Lambda functions handle the business logic (CRUD operations) and interact with DynamoDB to fetch or modify data.
7. CloudWatch tracks metrics, errors, logs, and alarms throughout the system.
---

## Used AWS Services and Roles

1.**Amazon S3**
   - Hosts the minified static frontend.
   - Public-read or CloudFront OAC access.

2. **Amazon CloudFront**
   - Caches static assets close to users.
   - Improves performance and reduces latency.

3.**Amazon Cognito**
  - Provides user authentication and authorization.
  - Manages user pools and tokens for secure access to the API Gateway.

4. **Amazon API Gateway**
  - Acts as the entry point for all API requests.
  - **Handles**:
     - Input validation
     - Throttling and rate limiting
     - Authentication via Cognito
     - Logging and metrics
     - Routing requests to Lambda

5.**AWS Lambda**
  - Stateless, event-driven compute.
  - **Handles**:
    - Creating, reading, updating, and deleting to-dos
    - Authentication/authorization checks
    - Data validation
    - Only authorized functions can access DynamoDB.
6. **Amazon DynamoDB**
    - Stores structured to-do list data per user.
    - Uses a combination of partition key (e.g., userId) and sort key (e.g., todoId).
    - Runs in on-demand mode with optional Point-in-Time Recovery.

7. **IAM**
    - Manages secure access between services.
    - Only Lambda functions can access DynamoDB via scoped IAM roles.
```json
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:GetItem",
    "dynamodb:PutItem",
    "dynamodb:UpdateItem",
    "dynamodb:DeleteItem"
  ],
  "Resource": "arn:aws:dynamodb:REGION:ACCOUNT_ID:table/Todos"
}
```

8. **Amazon CloudWatch**
    - Collects logs and metrics from Lambda and API Gateway.
    - **Used to**:
      - Alert on errors and latency
      - Diagnose slow performance
      - Track system usage over time

## Benefits of This Architecture

- **Scalable**: Serverless services scale automatically with user demand.
- **Cost-efficient**: Pay-per-request model; no idle compute charges.
- **Modular**: Each Lambda function can be updated independently.
- **Secure**: IAM roles and Cognito-based authorization.
- **Globally fast**: CloudFront CDN for static assets.
- **Observable**: CloudWatch enables real-time and historical insights.

## How it Aligns with the Well-Architected Framework

| Pillar                 | How This Architecture Aligns                                                                                                   |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| **Security**           | - IAM roles enforce least-privilege access between services. <br> - Amazon Cognito handles user authentication and issues tokens for authorization. <br> - API Gateway integrates with Cognito to secure API routes. <br> - Data in DynamoDB is encrypted at rest. <br> - S3 objects are restricted via policies or served securely via CloudFront. <br> - HTTPS is enforced through API Gateway for all external requests. |
| **Operational Excellence** | - CloudWatch monitors logs, metrics, and Lambda performance. <br> - Alarms can be triggered on errors or high latency. <br> - Structured logging in Lambda helps with debugging and root cause analysis. <br> - All services are event-driven and modular, enabling quick updates without full redeployment. |
| **Reliability**        | - All services are AWS-managed and automatically highly available. <br> - Lambda and DynamoDB run in multiple availability zones (Multi-AZ). <br> - DynamoDB offers Point-in-Time Recovery (PITR) for backups. <br> - API Gateway provides throttling, caching, and automatic retries on transient errors. <br> - CloudFront and S3 offer global caching and durability (S3: 11 9s durability). |
| **Performance Efficiency** | - Serverless architecture auto-scales with demand. <br> - DynamoDB uses on-demand mode with partition/sort key-based access for optimized querying. <br> - CloudFront caches frontend assets near users. <br> - API Gateway supports input validation to avoid unnecessary compute usage. <br> - Lambda functions are right-sized to minimize cold starts and optimize memory/CPU cost trade-offs. |
| **Cost Optimization**  | - Pay-as-you-go pricing model (Lambda, API Gateway, DynamoDB on-demand). <br> - No idle infrastructure; resources are used only when needed. <br> - Logs retention is configured to avoid unnecessary storage cost. <br> - Usage is monitored with AWS Budgets or Cost Explorer to avoid surprises. <br> - Caching (via CloudFront and optional API Gateway) reduces repeated backend calls. |
| **Sustainability**     | - Serverless components reduce energy consumption by scaling to zero when idle. <br> - Efficient resource usage through event-driven design. <br> - Use of managed services allows AWS to optimize data center sustainability. <br> - Continuous monitoring helps identify and eliminate waste. |



---

