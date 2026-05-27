````markdown
# Creating an Amazon ECS Task Definition

## Introduction

Amazon ECS is built around several core components that define how containers run, where they run, and how they are managed.

In the previous section, the ECS Cluster was created as the foundation for running containers.

This section focuses on the ECS Task Definition, which is one of the most important building blocks in Amazon ECS.

A task definition defines what ECS should run, how it should run it, and which resources and settings are required.

---

# What Is an ECS Task Definition?

An ECS Task Definition is a set of instructions used to run a containerized application.

It tells ECS:

- What to run
- How to run it
- Which resources to use
- Which configuration the container needs

A task definition includes key configuration details such as:

- Container image
- CPU allocation
- Memory allocation
- Network mode
- Environment variables
- IAM roles
- Logging settings
- Monitoring settings
- Storage settings

Every time you run an ECS task or create an ECS service, ECS uses a task definition as the blueprint for launching the container.

---

# ECS Task Definition Analogy

If the ECS Cluster is like an office building, then the ECS Task Definition is like a job description for a specific role inside that building.

A job description defines important information such as:

- Required skills
- Responsibilities
- Tools and technologies
- Working hours
- Salary and benefits
- Role expectations

In the same way, an ECS task definition defines:

- Which container image to run
- Required CPU and memory
- Environment variables
- IAM roles
- Networking configuration
- Volumes
- Logging settings
- Storage settings
- Health checks
- Retry policies
- Stop timeouts

Just like employees cannot properly perform their jobs without a job description, ECS cannot run containers without a task definition.

Without a task definition, ECS would not know:

- What to run
- How to run it
- Where to run it
- Which resources to assign

---

# Why Is the ECS Task Definition Important?

The ECS Task Definition is important because it provides consistency and repeatability.

Just like a job description ensures that a role is clearly and consistently defined, a task definition ensures that containers are deployed and managed in a consistent way.

ECS relies on the task definition to launch and manage containers predictably every time.

The task definition helps with:

- Defining what to run: Specifies the container image and application entry point.
- Standardizes how to run it: Sets required CPU, memory, environment variables, and roles.
- Repeatable deployments: Every time you deploy, ECS uses the same settings to avoid misconfigurations.
- Supports Scaling: As you scale up or down, every new task follows the exact same configuration.
- Facilitates Troubleshooting: Logs, health checks, and networking settings are clearly defined and reproducible.
- Version control: Each update creates a new revision, allowing rollback or comparison.

---

# Repeatable Deployments

A task definition allows ECS to deploy containers using the same configuration every time.

Whether you run one task or one hundred tasks, ECS uses the same task definition settings.

This helps avoid:

- Human errors
- Manual configuration mistakes
- Configuration drift
- Inconsistent deployments

---

# Task Definition Revisions

Each time a task definition is changed, ECS creates a new revision.

For example:

```text
Revision 1
Revision 2
Revision 3
```

Revisions allow you to:

- Track changes over time
- Compare versions
- Roll back to a previous version
- Manage application updates safely

---

# Creating an ECS Task Definition

To create a task definition, go to the Amazon ECS Console.

From the left-hand sidebar, select:

```text
Task definitions
```

There are two ways to create a task definition:

1. Create using the simple UI
2. Create using JSON

The simple UI allows you to configure the task definition without manually writing JSON.

The JSON option allows you to define the task definition manually using a JSON schema.

For this setup, the simple UI is used.

---

# Task Definition Family

The first required field is the task definition family name.

A task definition family groups multiple revisions of the same task definition.

The first task definition created in a family becomes:

```text
Revision 1
```

When changes are made later, ECS creates new revisions under the same family.

This makes it easier to manage updates and rollbacks.

---

# Infrastructure Requirements

The infrastructure requirements section defines where the task can run.

This is controlled by launch type compatibility.

The main options are:

- Amazon EC2
- AWS Fargate

The selected launch type affects which configuration options are available.

---

# Fargate Compatibility

When AWS Fargate is selected, the only supported network mode is:

```text
awsvpc
```

Some EC2-specific options are not available with Fargate, such as:

- Task placement strategies
- Task placement constraints

This is because AWS manages the infrastructure for Fargate.

---

# EC2 Compatibility

When Amazon EC2 is selected, more configuration options are available.

With EC2, you can use different network modes and task placement options.

EC2 gives more flexibility because you manage the underlying infrastructure.

---

# Multiple Launch Type Compatibility

A task definition can be compatible with multiple launch types.

For example, a task definition can support both:

- EC2
- Fargate

However, when running a task or creating a service, you must choose one launch type.

You cannot mix EC2 and Fargate in the same task run or the same service deployment.

For this setup, the EC2 launch type is used.

---

# Operating System and CPU Architecture

The operating system and CPU architecture setting defines the platform where the task will run.

For this setup, the default value is used.

This means ECS will use the default operating system and architecture selected in the console.

---

# Network Mode

The network mode determines how containers connect to the network.

For this setup, host mode is selected.

ECS networking modes define how containers communicate with:

- The host
- Other containers
- The internet
- Other AWS services

Networking modes can be explored in more detail later.

---

# Task Size

The task size defines how much CPU and memory the task should reserve.

CPU is measured in virtual CPUs.

Memory is measured in gigabytes.

For this setup:

```text
CPU: 1 vCPU
Memory: 1 GB
```

---

# CPU and Memory with EC2 Launch Type

When using the EC2 launch type, the requested CPU and memory cannot exceed what the EC2 instance provides.

This is because ECS tasks are running directly on EC2 instances.

For EC2 tasks, CPU and memory fields are technically optional.

However, defining them is considered a best practice.

If CPU and memory are not defined, ECS treats the tasks as unconstrained.

Unconstrained tasks may consume more resources than expected, which can affect other tasks running on the same EC2 instance.

---

# Task Role

The task role is the IAM role used by the application running inside the container.

If the application needs to call AWS services, the task role must include the required permissions.

Examples of AWS services the application may need to access include:

- Amazon S3
- Amazon DynamoDB
- Other AWS APIs

The task role is for the application inside the container.

---

# Task Execution Role

The task execution role is different from the task role.

The task execution role is used by ECS itself.

It allows the ECS container agent or Fargate agent to perform actions such as:

- Pulling container images from Amazon ECR
- Sending container logs to CloudWatch Logs

When creating a task definition, ECS does not always automatically create these roles.

However, the ECS console can offer to create them if they do not already exist.

For this setup, the default role creation option is used, allowing ECS to create the required role in the background.

---

# Task Role vs Task Execution Role

The difference between the two roles is important.

## Task Role

The task role is used by the application inside the container.

It controls what AWS services the application can access.

## Task Execution Role

The task execution role is used by ECS to start and manage the task.

It controls ECS-level operations such as pulling images and sending logs.

---

# Task Placement Constraints

Task placement constraints control where tasks run inside the ECS cluster.

They can be used to make sure tasks run only on specific instances.

Placement can be controlled based on:

- Custom attributes
- Instance types
- Specific infrastructure requirements

This is useful when you need tighter control over task placement.

For this setup, the default placement behavior is used, allowing ECS to handle placement automatically.

---

# Fault Injection

Fault injection allows you to simulate failure scenarios.

For example, you can intentionally stop tasks or containers to test how the application behaves when something goes wrong.

This is useful for testing:

- Resilience
- Recovery behavior
- Failure handling
- Application stability

For this setup, fault injection is disabled.

---

# Container Definition

A container definition provides the configuration required to run a specific container.

This information is passed to the Docker daemon, which uses it to start the container.

A task definition can include one or more container definitions.

For this setup, only one container is used:

```text
Nginx container
```

---

# Container Details

The container needs a clear and descriptive name.

The container image URI must also be provided.

For this setup, the official Nginx image from the Amazon ECR Public Gallery is used.

The image URI tells ECS which container image to pull and run.

---

# Port Mapping

Port mapping allows the container to communicate through specific ports on the host machine.

For the Nginx container, the port mapping is:

```text
Container port: 80
Host port: 80
Protocol: TCP
App protocol: HTTP
```

This means external traffic coming to the host on port 80 is forwarded to the container on port 80, where Nginx is listening.

---

# Resource Allocation Limits

Resource allocation limits define the resources reserved or limited for a container.

These settings help prevent one container from consuming too many resources on the host.

---

# Container CPU

The CPU field defines the number of CPU units the ECS container agent reserves for the container.

For this setup:

```text
CPU: 1 CPU unit
```

---

# Container GPU

The GPU field defines the number of GPU units reserved for the container.

If a workload needs GPU, the ECS cluster must use an EC2 instance type that supports GPU.

For the Nginx container, GPU is not required.

---

# Memory Hard Limit

The memory hard limit defines the maximum amount of memory the container can use.

If the container tries to use more memory than the hard limit, it will be stopped.

For this setup:

```text
Memory hard limit: 1 GB
```

---

# Memory Soft Limit

The memory soft limit is a flexible memory guideline.

Docker tries to keep the container within this limit when the system is under memory pressure.

It is not a hard stop.

The soft limit should be lower than the hard limit.

For this setup:

```text
Memory soft limit: 0.5 GB
```

This helps the container run smoothly without exhausting host resources.

---

# Environment Variables

Environment variables can be added as key-value pairs.

They are injected into the container at runtime.

Environment variables can be added:

- One by one
- In bulk using a file stored in Amazon S3

For this setup, no environment variables are used.

---

# Log Collection

Log collection defines where container logs are sent.

For this setup, the default logging option is used:

```text
Amazon CloudWatch Logs
```

CloudWatch Logs collects logs from the container and makes them available for troubleshooting and monitoring.

Other logging destinations can also be selected depending on the logging strategy.

---

# Volumes

Volumes can be attached when creating the task definition or later during deployment.

Volumes are used when containers need storage beyond the container filesystem.

They can be useful for:

- Temporary files
- Runtime data
- Caching
- Shared storage

---

# Fargate Ephemeral Storage

By default, AWS Fargate tasks receive:

```text
20 GB temporary storage
```

If the application needs more temporary storage, the value can be increased.

Fargate ephemeral storage can be increased up to:

```text
200 GB
```

This can be configured in the task definition.

---

# Monitoring

The monitoring section allows enabling application tracing and metrics collection.

This can be done using:

```text
AWS Distro for OpenTelemetry
```

OpenTelemetry helps monitor and analyze applications by collecting telemetry data such as:

- Metrics
- Logs
- Traces

For this setup, monitoring is not configured in detail.

---

# Tags

Tags are labels assigned to task definitions.

They help identify and organize resources.

Tags are useful when managing many task definitions across larger environments.

---

# Creating the Task Definition

After completing the required configuration, create the task definition.

Once created, ECS shows a summary that includes:

- Container details
- Allocated CPU
- Allocated memory
- Task definition family
- Revision number

---

# Reviewing the Task Definition

After creating the task definition, open it from the Task Definitions section.

The first created version appears as:

```text
Revision 1
```

When the task definition is updated later, ECS creates new revisions.

Updates may include changes such as:

- Container image version
- Resource limits
- Environment variables
- Logging configuration
- Networking settings

---

# JSON Configuration

Inside the task definition page, the JSON tab shows the full task definition configuration.

This JSON is the exact configuration ECS generated behind the scenes when the task definition was created through the console.

The JSON format can also be used to create or manage task definitions manually.

---

# IAM Role Verification

After creating the task definition, the IAM Console can be used to verify that the required task execution role was created.

This role allows ECS to perform required operations such as:

- Pulling container images
- Sending logs to CloudWatch Logs

---

# Final Result

At the end of the setup, the first ECS Task Definition is created successfully.

The task definition now provides ECS with the instructions needed to run the container.

It defines:

- Which image to run
- Which resources to allocate
- Which ports to expose
- Which IAM roles to use
- Where logs should go
- How the container should be configured

The next step is to use this task definition to run ECS tasks.
````
