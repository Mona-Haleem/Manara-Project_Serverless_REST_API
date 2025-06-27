# Manara-Project_-Serverless_REST_API
# Serverless Web Application Architecture on AWS

![AWS Architecture Diagram](architecture%20diagram.png)

## Overview

This architecture demonstrates a **serverless backend** solution using AWS services to handle user requests through an API. It provides a scalable, cost-effective approach that leverages the benefits of **AWS Lambda** for compute, **DynamoDB** for storage, and **CloudWatch** for monitoring and logging. API Gateway acts as the entry point for client requests.

---

## Detailed Explanation

### 1. **Users and Client**
- **Users** interact with the system through a **frontend client**, which could be a web or mobile application.
- This client sends HTTP requests (e.g., REST API calls) to the backend via the AWS infrastructure.

---

### 2. **API Gateway**
- Serves as the **entry point** for all client requests.
- Provides features such as:
  - Request/response transformation.
  - Authentication and authorization.
  - Rate limiting.
- Routes incoming HTTP requests to appropriate **AWS Lambda functions**.

---

### 3. **AWS Lambda**
- A **serverless compute service** that executes backend logic in response to events triggered by API Gateway.
- Benefits:
  - No need to provision or manage servers.
  - Automatically scales with demand.
- Can perform operations like:
  - Processing input data.
  - Accessing/Modifying data in DynamoDB.
  - Logging and monitoring.

---

### 4. **Amazon DynamoDB**
- A **NoSQL database service** designed for high performance and scalability.
- Stores structured or semi-structured data.
- Lambda reads from or writes to DynamoDB as needed (e.g., saving form inputs, retrieving user data).

---

### 5. **Amazon CloudWatch**
- Collects **logs** and **metrics** from AWS Lambda.
- Used for:
  - Monitoring function performance.
  - Setting alarms or alerts.
  - Debugging via logs and visualizing application behavior.

---

## Benefits of This Architecture

- **Scalability**: Serverless services like Lambda and DynamoDB automatically scale based on demand.
- **Cost-efficiency**: Pay only for what you use (e.g., per request, execution time).
- **Reduced Operational Overhead**: No server maintenance.
- **Modular and Event-driven**: Easy to update or extend individual components.

---

