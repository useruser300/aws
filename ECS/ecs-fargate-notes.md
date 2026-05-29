# Running Amazon ECS Tasks and Services with AWS Fargate

## Introduction

Amazon ECS can run containers using different launch types.


AWS Fargate is the serverless way to run containers on Amazon ECS.

With Fargate, there are no EC2 instances to provision or manage.

You only define the container configuration, and AWS runs the containers on managed infrastructure.

In this setup, an Nginx container will be deployed in two ways:

1. As a standalone ECS task using Fargate
2. As an ECS service using Fargate

---

# What Is AWS Fargate?

AWS Fargate is a serverless compute engine for containers.

It allows ECS tasks and services to run without directly managing EC2 instances.

With Fargate, AWS handles the infrastructure layer.

This means you do not need to manage:

- EC2 instances
- Server provisioning
- Operating system patching
- Cluster capacity
- Host-level scaling
- Infrastructure maintenance

You only focus on:

- Container image
- CPU
- Memory
- Networking
- Security group
- Logs
- Application configuration

---

# Creating an ECS Cluster for Fargate

To start, open the Amazon ECS Console from the AWS Management Console.

From the left-hand sidebar, go to:

```text
Clusters
```

Click:

```text
Create cluster
```

---

# Cluster Name

Give the cluster a meaningful name.

A clear name helps identify the purpose of the cluster later.

---

# Infrastructure Type

For the infrastructure type, keep the default option:

```text
AWS Fargate
```

This means the cluster will be configured to run tasks on AWS-managed serverless infrastructure.

---

# Monitoring Settings

Keep the default monitoring settings.

Monitoring can be configured later depending on the requirements of the workload.

---

# Encryption Settings

Keep the default encryption settings.

Encryption options can be configured later when working with security and production-level setups.

---

# Tags

No tags are added for this cluster in this setup.

Tags can be added later to help organize resources.

---

# Creating the Cluster

After reviewing the settings, click:

```text
Create
```

When the cluster is created successfully, open it from the ECS Console.

At this point:

- No services are associated with the cluster
- No tasks are running inside the cluster yet

---

# Fargate Capacity Providers

Inside the cluster, open the Infrastructure tab.

The cluster shows two capacity providers:

- Fargate
- Fargate Spot

These capacity providers define how tasks can run using Fargate infrastructure.

Capacity providers can be explored later in more detail when working with ECS autoscaling.

---

# Creating a Task Definition for Fargate

After creating the cluster, the next step is to create a task definition.

From the ECS sidebar, go to:

```text
Task definitions
```

Click:

```text
Create new task definition
```

---

# Task Definition Name

Give the task definition a clear and meaningful name.

This helps identify what the task definition is used for.

---

# Infrastructure Requirements

Under infrastructure requirements, keep the default launch type:

```text
AWS Fargate
```

This means the task definition will be compatible with Fargate.

---

# Task Size

Task size defines how much CPU and memory the task should use.

For this setup:

```text
CPU: 1 vCPU
Memory: 2 GB
```

This is enough for the Nginx example.

---

# Task Role

The task role is used by the application inside the container when it needs to access AWS services.

For this setup, the task role is left empty because the Nginx container does not need to interact with other AWS services.

---

# Task Execution Role

The task execution role is used by ECS to perform actions required to start the task.

For example, ECS uses it to:

- Pull container images
- Send logs to CloudWatch Logs

For this setup, the default option is used, allowing ECS to automatically create a new task execution role.

---

# Task Placement and Fault Injection

Task placement and fault injection settings are optional.

For this setup, they are left unchanged.

---

# Container Details

In the container details section, define the container information.

For this setup:

```text
Container name: nginx
Image URI: Public Nginx image from Amazon ECR Public Gallery
```

The image URI tells ECS which container image to pull and run.

---

# Port Mapping

Under port mappings, keep the default values for:

- Container port
- Protocol
- App protocol

Assign a proper name to the port mapping.

The Nginx container listens on port 80.

---

# Resource Allocation Limits

Resource allocation limits define the resources available to the container.

For this setup:

```text
CPU: 1
Memory hard limit: 1 GB
Memory soft limit: 0.5 GB
```

The hard limit is the maximum memory the container can use.

The soft limit is a flexible guideline Docker tries to respect when memory pressure exists.

---

# Creating the Task Definition

Leave the remaining settings as they are.

Click:

```text
Create
```

After the task definition is created, the container details appear under the Containers tab.

The JSON tab can be used to view the complete configuration generated by AWS.

Since this is the first version of the task definition, it appears as:

```text
Version 1
```

---

# Running a Standalone ECS Task with Fargate

After creating the task definition, deploy a standalone task using Fargate.

Open the task definition and select:

```text
Version 1
```

Click:

```text
Deploy
```

Then choose:

```text
Run task
```

---

# Task Details

In the task details section, keep the default configuration.

The task definition and revision are already selected.

---

# Environment Section

In the environment section, set the compute option to:

```text
Launch type
```

For launch type and platform version, keep the default selections.

Since the task definition is configured for Fargate, the task will run on Fargate infrastructure.

---

# Networking Configuration

In the networking section, choose the VPC where the task should run.

For this setup, an existing VPC is selected.

If no VPC exists, a new VPC can be created from the console.

For this demo, only public subnets are selected.

Private subnet deployments can be used later in more advanced setups.

---

# Security Group

Create a new security group for the task.

Give it a descriptive name.

For inbound rules, configure:

```text
Port range: 80
Source: Anywhere
```

This allows HTTP traffic from the internet to reach the task.

---

# Creating the Standalone Task

After completing the networking and security group configuration, create the task.

Once the task is created successfully, open it to view its details.

The task details page shows information about the running Fargate task.

---

# Accessing the Nginx Application

Click the public IP address of the running task.

The default Nginx homepage should appear in the browser.

This confirms that the standalone ECS task has been deployed successfully using Fargate.

---

# Where Does the Public IP Come From in Fargate?

With Fargate, you do not manage EC2 instances.

However, every Fargate task gets its own Elastic Network Interface inside the VPC.

Because the task was launched in a public subnet with auto-assign public IP enabled, AWS automatically attaches a public IPv4 address to the task’s network interface.

This public IP comes from Amazon’s IP pool.

Traffic reaches the task through the VPC Internet Gateway.

Because the security group allows inbound traffic on port 80, the task is accessible directly from the internet.

---

# Public IP Behavior in Fargate

The public IP assigned to a Fargate task is ephemeral.

If the task stops and starts again, the public IP will likely change.

In real-world deployments, a Fargate service is usually placed behind an Application Load Balancer.

The load balancer provides a stable endpoint instead of relying on changing task public IP addresses.

---

# Viewing Task Logs

Go back to the ECS Console.

Open the Logs section of the task.

The logs for the task are available there.

These logs are generated automatically in Amazon CloudWatch Logs.

---

# Stopping a Standalone Fargate Task

To test standalone task behavior, stop the running task manually.

Steps:

1. Select the running task.
2. Click stop selected.
3. Confirm the stop action.

After the task stops, refresh the web page.

The Nginx page is no longer accessible.

---

# What Happens After Stopping a Standalone Fargate Task?

After stopping the standalone task, ECS does not create a replacement task automatically.

This is because the task is not managed by an ECS Service.

A standalone task runs only when started manually.

If it stops, ECS considers it completed and does not restart it.

---

# Deploying an ECS Service with Fargate

After testing the standalone task, the next step is to deploy an ECS Service using Fargate.

An ECS Service keeps the desired number of tasks running.

If a task stops, the service automatically creates a replacement.

---

# Creating the Fargate Service

Open the ECS Console and go to the create service page.

Under task definition family, choose the task definition created earlier.

For task definition revision, select:

```text
Version 1
```

This is the latest revision in this setup.

---

# Service Name

Give the service a clear and meaningful name.

This makes it easier to identify and manage later.

---

# Environment Section

In the environment section, set the compute option to:

```text
Launch type
```

Keep the launch type and platform version at their default values.

The service will run using Fargate.

---

# Deployment Configuration

Keep the default deployment configuration settings.

These settings control how ECS deploys and replaces tasks for the service.

---

# Networking Configuration for the Service

Under networking configuration, select the same VPC used previously.

Choose the two public subnets for the service.

Select the security group created earlier.

Leave the remaining configuration settings at their default values.

---

# Creating the ECS Service

Create the ECS Service.

Once the service is deployed successfully, one task should be running.

Go to the Tasks tab and open the running task to view its details.

---

# Verifying the Fargate Service

Inside the running task details, find the public IP address.

Open the public IP in the browser.

The default Nginx homepage should appear.

This confirms that the ECS Service successfully deployed the task using Fargate.

---

# Testing Automatic Replacement in Fargate Service

Go back to the task section in the ECS Console.

Select the running task and stop it manually.

After a few moments, ECS automatically starts a new task.

This happens because the ECS Service maintains the desired task count.

---

# Accessing the Replacement Task

Open the new task created by the service.

Click the public IP address of the new task.

The Nginx homepage should appear again.

This confirms that the application is running on the replacement ECS task managed by the service.

---

# Difference Between Standalone Task and Service on Fargate

A standalone Fargate task runs only when started manually.

If it stops, ECS does not replace it.

An ECS Service keeps the application running continuously.

If a task stops, the service automatically starts a new task.

This is why ECS Services are used for long-running applications.

---

# EC2 vs Fargate Behavior

With EC2 launch type, tasks run on EC2 instances that you manage.

When accessing the application, you may be accessing the EC2 instance public IP.

With Fargate, each task gets its own Elastic Network Interface and can receive its own public IP.

This means Fargate task public IPs can change when tasks are replaced.

For production workloads, a stable endpoint should be provided through a load balancer.

---

# Final Result

At the end of this setup, an ECS cluster is created using AWS Fargate.

A task definition is created for an Nginx container.

The Nginx container is deployed first as a standalone Fargate task.

The standalone task runs successfully, but when stopped manually, ECS does not replace it.

Then the same container is deployed as an ECS Service using Fargate.

When the service task is stopped manually, ECS automatically launches a replacement task.

This demonstrates the main difference between standalone ECS tasks and ECS services when using the Fargate launch type.