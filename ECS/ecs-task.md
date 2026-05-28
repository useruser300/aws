````markdown
# Running an Amazon ECS Task

## Introduction

Amazon ECS is built around several core components that work together to run and manage containers.

In the previous section, the ECS Task Definition was created. The task definition acts as the blueprint that tells ECS how to run containers.

This section focuses on the ECS Task.

An ECS Task is the actual running instance of a containerized application inside an ECS cluster.

ECS launches the task based on the configuration defined in the task definition.

---

# What Is an ECS Task?

An ECS Task is the live running instance of a containerized application.

It is the application actually running inside the ECS cluster.

A task is not just configuration. It is the real workload running in the infrastructure.

Every ECS task starts from a task definition.

The task definition acts as the blueprint, and the task is created by following that blueprint.

When a task starts, ECS places it on the compute platform selected for the workload.

This can be:

- Amazon EC2 instances
- AWS Fargate
- External infrastructure using ECS Anywhere

ECS handles the deployment and keeps track of the task status.

---

# ECS Task Usage

ECS tasks can be used in two main ways.

## Standalone Tasks

A task can be run manually as a standalone task.

This is useful for:

- One-time jobs
- Testing
- Batch jobs
- Temporary workloads
- Manual executions

In this case, ECS starts the task, but it does not automatically replace it if it stops.

## Tasks Managed by an ECS Service

Tasks can also be managed by an ECS Service.

An ECS Service keeps tasks running continuously.

If a task fails, the service automatically starts a replacement task.

A service can also manage scaling and ensure the desired number of tasks is always running.

---

# ECS Task Analogy

If the ECS Task Definition is like a job description, then an ECS Task is like an employee actively working based on that job description.

The job description defines:

- Responsibilities
- Skills
- Expectations
- Tools needed for the role

When someone is hired based on that job description, that person starts doing the actual work.

In ECS, every time ECS launches a task from a task definition, it is like hiring someone to perform that role.

The running task represents the container in action.

Just like multiple employees can be hired for the same job, ECS can run multiple tasks from the same task definition.

Each task is a separate independent running instance.

---

# Why Is the ECS Task Important?

The ECS Task is important because it is the actual unit of work in ECS.

Without a task, the containerized application does not start.

The task is what brings the application to life.

Each task runs in an isolated environment with its own assigned configuration.

This can include:

- CPU
- Memory
- Networking
- Security settings
- Environment variables
- Secrets
- Runtime overrides

Tasks make it possible to run the same application once or many times.

For example, ECS can run:

- One task
- Ten tasks
- Hundreds of tasks

All of them can be launched from the same task definition.

---

# Task Isolation

Each ECS task runs with its own isolated runtime configuration.

This isolation helps provide better control over:

- Performance
- Security
- Resource usage
- Networking behavior

Tasks can have dedicated CPU and memory limits.

They can also have their own security-related settings depending on the launch type and networking mode.

---

# Task Scaling

Tasks make scaling simple.

If more application capacity is needed, ECS can start more tasks from the same task definition.

If less capacity is needed, ECS can reduce the number of running tasks.

This horizontal scaling model allows the application to run multiple identical instances.

---

# Task Monitoring

ECS tasks can be monitored individually.

Amazon CloudWatch can track task-level behavior such as:

- CPU usage
- Memory usage
- Logs
- Runtime behavior
- Container output

This makes troubleshooting easier because each task can be inspected separately.

---

# Running the First ECS Task

To run the first ECS task, start from the task definition created previously.

Open the ECS Console and go to the task definition.

Select:

```text
Revision 1
```

Then click:

```text
Run task
```

This opens the Run Task workflow.

---

# Running a Standalone Task

The Run Task workflow allows running a standalone task without creating an ECS Service.

This is useful when a task only needs to run once and then exit.

Examples include:

- Scheduled batch jobs
- Test runs
- One-time scripts
- Manual container execution

ECS automatically chooses the right compute resources based on the selected launch type or capacity provider.

---

# Task Definition Family

The task definition family is already selected during the Run Task workflow.

The family is the name or group under which all revisions of the task definition are organized.

Each time the task definition is updated, ECS creates a new revision.

By default, ECS selects the latest revision.

This means the task will run using the most recent version of the task configuration.

---

# Desired Tasks

The desired tasks field defines how many copies of the task should be launched.

Example:

```text
Desired tasks: 1
```

This launches one running instance of the task.

If the value is set to:

```text
Desired tasks: 3
```

ECS attempts to run three identical tasks at the same time using the same task definition.

---

# Task Group

The task group field is used to group related tasks under a common name.

This can be helpful when using placement strategies such as spread placement.

With spread placement, ECS can distribute tasks across:

- Availability Zones
- EC2 instances

For this setup, the task group is left blank.

---

# Selecting the ECS Cluster

The existing cluster option defines where the task should run.

If only one ECS cluster exists, it is selected automatically.

The task must run inside an ECS cluster.

---

# Compute Options

Compute options control how the ECS task is placed on the infrastructure inside the cluster.

There are two main compute options:

1. Capacity provider strategy
2. Launch type

---

# Capacity Provider Strategy

When using a capacity provider strategy, ECS distributes tasks across one or more capacity providers.

Capacity providers manage how the cluster infrastructure scales to run tasks.

Capacity provider strategies are useful for advanced scaling scenarios.

They can be configured later when working with ECS autoscaling.

---

# Launch Type

When selecting launch type as the compute option, you tell ECS where the task should run.

Available launch type options include:

- Fargate
- EC2
- External

---

## Fargate Launch Type

Fargate runs tasks on AWS serverless infrastructure.

With Fargate, you do not manage servers.

AWS handles the infrastructure behind the scenes.

---

## EC2 Launch Type

EC2 runs tasks on Amazon EC2 instances registered to the ECS cluster.

This requires EC2 instances to already exist and be connected to the ECS cluster.

For this setup, EC2 is selected because the cluster was created using EC2 capacity.

---

## External Launch Type

External launch type allows tasks to run on your own on-premises servers or virtual machines.

These external machines must be registered and managed through ECS Anywhere.

---

# Matching Cluster and Task Launch Type

The task launch type must match the infrastructure available in the ECS cluster.

Tasks with EC2 launch type run only on EC2 instances registered to the cluster.

Tasks with Fargate launch type run only on the Fargate serverless platform.

You cannot run a Fargate task in a cluster that only has EC2 capacity available.

For this setup, the cluster was configured using EC2, so the task is run using EC2 launch type.

---

# Task Placement

Task placement defines how Amazon ECS places tasks on EC2 instances inside the cluster.

Task placement can control where tasks are scheduled based on:

- Available resources
- Placement strategies
- Placement constraints
- Availability Zones
- EC2 instance attributes

For this setup, detailed task placement configuration is not changed.

---

# Volumes

The volume workflow allows configuring data volumes that ECS can create and attach to tasks.

Volumes are useful when containers need storage for:

- Runtime files
- Temporary data
- Shared data
- Application state
- Logs or cache data

For this setup, no additional volume configuration is changed.

---

# Task Override

The task override section allows changing some task-level settings for this specific task run.

For example, you can choose different IAM roles for this task.

This overrides the IAM roles defined in the task definition.

Task overrides are useful when the same task definition needs to be reused with different runtime settings.

---

# Container Override

The container override section allows changing container-level settings for this specific task run.

This can include:

- Command override
- Environment variable override

---

## Command Override

A command override allows specifying a different Docker command to run inside the container.

This replaces the command defined in the task definition for this specific task run.

---

## Environment Variable Override

Environment variables can be overridden at runtime.

These are key-value pairs that replace or add to the environment variables defined in the task definition.

---

# Tags

Tags help identify and organize ECS resources.

For this setup, tags are propagated from the task definition.

This keeps related resources labeled consistently.

---

# Creating the ECS Task

After completing the configuration, create the ECS task.

ECS launches the task onto the EC2 instances registered with the cluster.

Once the task is created successfully, it appears in the task list with the status:

```text
Running
```

---

# Reviewing the Running Task

Open the running task to view its configuration details.

Inside the task details, you can see information such as:

- Task status
- Task definition revision
- Container details
- Image URI
- Networking details
- Logs
- Resource configuration

The image URI should match the image that was configured in the task definition.

---

# Accessing the Nginx Application

After the task is running, click the public IP link.

The Nginx homepage should appear successfully.

This confirms that the Nginx container was deployed and is running correctly.

The public URL used here is the public IPv4 address of the EC2 instance running the ECS task.

The browser is accessing the EC2 instance through its public IP on port 80.

---

# How the Public Access Works

The ECS task launched the Nginx container on the EC2 instance.

The task definition configured port binding:

```text
Host port: 80
Container port: 80
```

Because of this port binding, traffic coming to the EC2 instance on port 80 is forwarded to the Nginx container on port 80.

This is why opening the EC2 instance public IP displays the Nginx homepage.

---

# Viewing ECS Task Logs

Go back to the ECS task page and open the Logs tab.

The logs generated by the running task are displayed there.

These logs are collected through Amazon CloudWatch Logs.

Logs help with:

- Troubleshooting
- Monitoring
- Checking container output
- Understanding application behavior

---

# Viewing Logs in CloudWatch

Open the CloudWatch Console.

Go to:

```text
Log groups
```

Select the log group created for the ECS task.

Inside the log group, a new log stream is created for the running task.

This log stream matches the logs visible in the ECS Console.

---

# Stopping the ECS Task

Return to the ECS Console.

Inside the ECS cluster, one task is currently running.

To stop it:

1. Select the running task.
2. Click stop selected.
3. Confirm by clicking stop.

After the task stops, wait a few seconds.

---

# What Happens After Stopping a Standalone Task?

After stopping the standalone task, ECS does not automatically launch a new task.

This happens because the task is not managed by an ECS Service.

A standalone task runs only when started manually.

If it stops, ECS considers it finished and does not replace it.

---

# Application After Stopping the Task

After the task is stopped, refresh the public IP address in the browser.

The Nginx website is no longer accessible.

This confirms that the application was running only because the ECS task was active.

Once the task stopped, the container stopped, and the website went down.

---

# Why ECS Service Is Needed

An ECS Service is needed when the application must remain running continuously.

A service keeps the desired number of tasks running.

If a task fails or stops, the service starts a replacement task automatically.

This is the main difference between:

- Running a standalone task
- Running tasks through an ECS Service

Standalone tasks are useful for one-time jobs.

ECS Services are used for long-running applications.

---

# Final Result

At the end of this setup, the first ECS task is created and run successfully.

The task launches an Nginx container based on the task definition.

The application becomes accessible through the EC2 instance public IP on port 80.

CloudWatch Logs collect and display task logs.

When the standalone task is stopped, ECS does not restart it automatically.

This demonstrates that standalone tasks are not designed for continuous application availability.

For continuous running workloads, ECS Services are required.
````
