# Amazon ECS Service

## Introduction

Amazon ECS contains several core components that work together to run and manage containerized applications.

In the previous section, an ECS Task was created and run manually.

A standalone ECS task runs the container, but if the task stops, ECS does not automatically replace it.

This section focuses on the ECS Service.

An ECS Service is the component responsible for keeping containerized applications running continuously.

It ensures that the required number of tasks are always running and healthy.

---

# What Is an ECS Service?

An ECS Service represents a long-running stateless application.

Its main responsibility is to ensure that the desired number of tasks are always running based on a specific task definition.

If a task fails or stops unexpectedly, the ECS Service automatically replaces it.

This helps maintain:

- High availability
- Reliability
- Resilience
- Continuous application availability

---

# What Does an ECS Service Do?

An ECS Service acts as a controller for running applications.

It continuously manages tasks based on the selected task definition.

The service ensures that containers are launched and maintained according to the required configuration.

---

## Maintains the Desired Number of Tasks

The ECS Service keeps the desired number of task replicas running.

For example, if the desired count is set to:

```text
Desired tasks: 3
```

ECS makes sure three tasks are always running.

If one task stops, ECS starts a replacement task automatically.

---

# Automatically Replaces Failed Tasks

If a task crashes or stops unexpectedly, ECS detects the failure and replaces it.

No manual monitoring or manual restart is required.

This self-healing behavior is one of the most important features of ECS Service.

---

# Supports Load Balancing

An ECS Service can integrate with load balancers such as:

- Application Load Balancer
- Network Load Balancer

The load balancer routes traffic across all healthy tasks.

This helps distribute user traffic evenly and improves application performance and reliability.

---

# Supports Rolling Updates

ECS Service supports zero-downtime deployments through rolling updates.

During a rolling update, ECS gradually replaces old tasks with new tasks.

This allows the application to remain available while a new version is being deployed.

---

# Supports Multiple Infrastructure Types

An ECS Service can run containers on different infrastructure options:

- Amazon EC2
- AWS Fargate
- External infrastructure using ECS Anywhere

The infrastructure choice depends on the selected launch type or capacity provider.

---

# ECS Service Analogy

If an ECS Task is like an employee doing the work, then an ECS Service is like the manager responsible for keeping the work running smoothly.

The manager ensures that the correct number of employees are always present and working.

If an employee leaves, the manager immediately finds a replacement.

In ECS, if a task stops, the service starts a replacement task.

---

# Rolling Update Analogy

If an employee’s role changes, the manager does not replace everyone at once.

Instead, the manager gradually applies the change while keeping the office running.

This is similar to an ECS rolling update.

ECS gradually replaces old tasks with new ones while keeping the application available.

---

# Load Balancing Analogy

When work increases, the manager distributes tasks evenly across available employees.

Similarly, ECS Service can distribute traffic across running tasks using a load balancer.

This keeps the workload balanced and prevents one task from becoming overloaded.

---

# Monitoring Analogy

The manager also monitors employee performance and makes sure everything works as expected.

In ECS, monitoring can be done using Amazon CloudWatch.

CloudWatch helps monitor task health, performance, and behavior.

---

# Why Is the ECS Service Important?

An ECS Service is important because it turns containers into a continuously available application.

It keeps the desired number of tasks running, even if failures happen.

If a task fails or stops unexpectedly, ECS replaces it automatically.

This means no manual restart is required.

---

# High Availability

ECS Service helps keep applications highly available.

It ensures that the application continues running even when individual tasks fail.

When combined with multiple tasks and multiple Availability Zones, ECS Service can improve application resilience.

---

# Traffic Distribution

ECS Service can distribute incoming traffic across healthy tasks using a load balancer.

This improves:

- Availability
- Reliability
- Performance
- User experience

---

# Zero-Downtime Deployments

When a new version is deployed, ECS Service can perform a rolling update.

It gradually replaces old tasks with new tasks.

This keeps the application available during deployment.

---

# Scaling

ECS Service can scale the number of running tasks up or down based on demand.

When traffic increases, ECS can add more tasks.

When traffic decreases, ECS can remove tasks.

This helps the application handle traffic changes efficiently.

---

# Monitoring and Troubleshooting

ECS Service integrates with CloudWatch.

CloudWatch can monitor:

- Task health
- CPU usage
- Memory usage
- Logs
- Service events
- Performance behavior

This makes it easier to detect and troubleshoot issues.

---

# Standalone Task vs ECS Service

A standalone task is started manually.

If it stops, ECS does not automatically replace it.

A standalone task is useful for:

- One-time jobs
- Manual tests
- Batch jobs
- Temporary workloads

An ECS Service is used for long-running applications.

If a task stops, the service automatically starts a replacement.

An ECS Service is useful for:

- Web applications
- APIs
- Production workloads
- Continuously running applications

---

# Creating an ECS Service

There are two ways to create an ECS Service.

---

# Option 1: Create a Service from the ECS Cluster

Open the ECS cluster page.

Go to the:

```text
Services
```

tab.

Click:

```text
Create
```

This starts the ECS Service creation workflow.

---

# Option 2: Create a Service from the Task Definition

Open the ECS Console.

Go to:

```text
Task definitions
```

Select the task definition created earlier.

Choose the latest revision.

From the deploy dropdown, select:

```text
Create service
```

For this setup, the service is created from the task definition page.

---

# Task Definition Family

The task definition family tells ECS which task definition to use when launching tasks for the service.

Since the service is created directly from the task definition page, the task definition family is already selected.

No change is required.

---

# Task Definition Revision

The task definition revision defines the specific version of the task definition used to launch the tasks.

ECS automatically selects the latest revision by default.

Using the latest revision is usually preferred because it contains the most recent configuration.

However, an older revision can be selected if a rollback is needed.

---

# Service Name

The service should have a meaningful and unique name inside the ECS cluster.

A clear service name helps identify and manage the service later.

---

# Environment Configuration

The environment configuration defines which ECS cluster the service belongs to.

If only one ECS cluster exists, it is selected automatically.

The service will run its tasks inside this selected cluster.

---

# Compute Options

Compute options define where and how the ECS tasks should run.

There are two main options:

1. Capacity provider strategy
2. Launch type

---

# Capacity Provider Strategy

A capacity provider strategy allows ECS to place tasks across one or more capacity providers.

Capacity providers manage how the cluster infrastructure scales to run tasks.

This option is useful for more advanced autoscaling scenarios.

---

# Launch Type

The launch type defines the infrastructure used to run the service tasks.

Available options include:

- Fargate
- EC2
- External

For this setup, the EC2 launch type is selected.

This means service tasks run on EC2 instances registered to the ECS cluster.

---

# Deployment Configuration

The deployment configuration defines how the ECS scheduler places and manages tasks in the cluster.

The scheduler ensures the selected scheduling strategy is followed.

If a task fails, the scheduler automatically reschedules it.

---

# Service Type

There are two main ECS service types:

1. Replica
2. Daemon

---

# Replica Service Type

With the replica service type, ECS maintains the number of tasks you specify.

ECS spreads tasks across the cluster and Availability Zones by default.

This is the most common service type for web applications and APIs.

For this setup, the replica service type is used.

---

# Daemon Service Type

With the daemon service type, ECS places exactly one task on each active container instance that matches the placement rules.

This is useful for workloads that should run once per EC2 instance, such as:

- Monitoring agents
- Log collectors
- Security agents

---

# Desired Tasks

The desired tasks option defines how many tasks should be running.

This option applies to the replica service type.

For this setup:

```text
Desired tasks: 1
```

This means ECS keeps one task running at all times.

---

# Availability Zone Rebalancing

Availability Zone rebalancing helps keep tasks distributed evenly across Availability Zones.

It is recommended to run multi-task services across multiple Availability Zones for higher availability.

When enabled, ECS detects if tasks are unevenly distributed.

If an imbalance exists, ECS redistributes tasks to keep the service balanced and reliable.

---

# Health Check Grace Period

The health check grace period defines how long ECS waits before monitoring the health of a new task.

This is important because some applications need time to start.

If ECS starts health checks immediately, it may incorrectly mark a task as unhealthy before the application is ready.

A proper grace period gives the application time to initialize and pass health checks.

This helps avoid unnecessary restarts and improves service stability.

---

# Deployment Options

Deployment options and deployment failure detection can be configured for more advanced deployment behavior.

These settings are useful for controlling how ECS handles updates and failures.

They can be adjusted later when working with ECS deployment strategies.

---

# Service Connect

Service Connect allows ECS services to communicate with each other using simple service names and standard ports.

It simplifies service-to-service communication inside ECS.

This feature can be configured later when working with ECS networking.

---

# Service Discovery

Service Discovery allows ECS services to be found using DNS names.

It uses Amazon Route 53 to create a namespace.

Other services can then locate and connect to the ECS service using DNS.

---

# Elastic Load Balancing

Elastic Load Balancing can distribute incoming traffic across healthy tasks in an ECS Service.

This improves availability and prevents a single task from becoming overloaded.

Supported load balancers include:

- Application Load Balancer
- Network Load Balancer

---

# Amazon VPC Lattice

Amazon VPC Lattice is a fully managed networking service.

It helps ECS users securely connect, monitor, and control traffic between services.

It can work across:

- Different VPCs
- Different AWS accounts

A major benefit is that application code does not need to be changed.

---

# Service Auto Scaling

Service Auto Scaling automatically increases or decreases the number of running tasks based on CloudWatch alarms.

It helps the application respond to traffic changes.

For example:

- Scale out when demand increases
- Scale in when demand decreases

This helps maintain performance while controlling cost.

---

# Task Placement

Task placement defines how ECS distributes tasks across EC2 instances in the cluster.

Several placement strategies are available.

Task placement can be configured based on:

- Availability Zones
- Instance attributes
- Resource availability
- Placement constraints

This can be adjusted later when working with ECS scheduling strategies.

---

# Volumes

The volume section allows defining data volumes that ECS creates and attaches to tasks.

Volumes are useful for:

- Persistent data
- Shared data
- Runtime files
- Data shared between containers

---

# Tags

Tags are key-value pairs assigned to ECS resources.

They help organize, identify, and manage resources.

For this setup, tags are propagated from the task definition.

This applies task definition tags consistently to the tasks created by the service.

---

# Creating the ECS Service

After completing the configuration, create the ECS Service.

Once the service is created successfully, it appears as active under the Services tab.

---

# Verifying the Service

Go to the Tasks tab inside the ECS cluster.

A new task should be launched by the service.

The task should appear in the running state.

Open the task and copy the public IP address.

Open the public IP in the browser.

If everything works correctly, the Nginx default homepage appears.

This confirms that the Docker image is running successfully on ECS.

---

# Testing Automatic Task Replacement

To test the ECS Service behavior, stop the running task manually.

Steps:

1. Select the running task.
2. Click stop.
3. Wait a few seconds.

ECS automatically launches a new task to maintain the desired task count.

This is the core behavior of ECS Service.

The service always tries to keep the desired number of tasks running.

---

# Public IP Behavior with EC2 Launch Type

In this setup, the EC2 launch type is used with only one EC2 instance.

Because of this, the public IP address remains the same even when ECS creates a replacement task.

This happens because the browser accesses the public IP address of the EC2 instance, not the task itself.

The new task runs on the same EC2 instance, so the public IP stays the same.

---

# Public IP Behavior with Fargate Launch Type

With Fargate, the public IP address can change each time a new task is created.

This is because each Fargate task gets its own separate infrastructure and network interface.

When a Fargate task is replaced, the new task may receive a different public IP address.

---

# Stopping Automatic Task Replacement

To stop ECS from automatically launching replacement tasks, update the ECS Service.

Steps:

1. Go to the Services tab.
2. Select the service.
3. Click update.
4. Scroll to the deployment configuration section.
5. Set desired task count to:

```text
0
```

6. Save the changes.

Once saved, ECS stops the running task and does not launch a replacement.

---

# Application After Desired Count Is Set to Zero

After the desired task count is set to zero, no tasks are running.

The Nginx homepage is no longer accessible.

This happens because the service is no longer maintaining any running tasks.

---

# Final Result

At the end of this setup, an ECS Service is created successfully.

The service launches and maintains an ECS task based on the task definition.

When the task is stopped manually, ECS automatically creates a replacement task.

This demonstrates the self-healing behavior of ECS Service.

When the desired task count is changed to zero, ECS stops running tasks and the application becomes unavailable.

The ECS Service is the component that turns containers into a continuously running, self-healing, scalable application.