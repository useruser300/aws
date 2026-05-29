# Creating an Amazon ECS Cluster

## Introduction

Amazon ECS is built around several core components that work together to define how containers run, where they run, and how they are managed.

The four main ECS components are:

1. ECS Cluster
2. ECS Task Definition
3. ECS Task
4. ECS Service

These components work together to manage containerized applications efficiently.

This section focuses on understanding the ECS Cluster and creating an ECS cluster from scratch.

---

# What Is an ECS Cluster?

An ECS Cluster is a logical container used to manage compute resources for running containers.

These compute resources can be:

- EC2 instances
- Fargate tasks
- External on-premises servers through ECS Anywhere

The cluster acts as the execution environment where ECS launches, runs, and manages tasks and services.

An ECS cluster does not directly store containers. Instead, it keeps track of where containers should run and helps ECS schedule them on the correct infrastructure.

Every ECS task or service must run inside a cluster. Nothing can be deployed in ECS without being associated with a cluster.

A single ECS cluster can support multiple tasks and services at the same time. This means you do not need to create a separate cluster for every application. Related workloads can be grouped together inside one cluster for better resource usage.

---

# ECS Cluster Analogy

An ECS cluster can be compared to an office building.

The office building provides the space and resources needed for work, such as:

- Rooms
- Desks
- Internet
- Electricity

However, the building itself does not do the work.

In the same way, an ECS cluster does not run containers by itself. It provides the environment where containers can run using EC2 instances, Fargate, or external infrastructure.

Just like employees must work inside the office building, ECS containers must run inside an ECS cluster.

A building can host multiple departments in different rooms. Similarly, an ECS cluster can run multiple tasks and services that share available compute and networking resources.

The cluster is therefore the foundation that makes container execution possible.

---

# Why Is the ECS Cluster Important?

The ECS cluster is the foundation of the ECS architecture.

Without a cluster, ECS would not know where or how to run containers.

The cluster helps organize compute resources into a manageable unit, whether the workload is running on EC2, Fargate, or external infrastructure.

When a task is started, ECS uses the cluster to decide the best place to run it.

ECS checks available resources such as:

- CPU
- Memory
- Placement strategies
- Available capacity

Then ECS schedules the task on the most suitable infrastructure.

---

# ECS Cluster and Scaling

As the application grows, the ECS cluster allows tasks and infrastructure to scale based on demand.

The cluster also acts as the scope for:

- Monitoring
- Health checks
- Logging
- Troubleshooting
- Resource management

Multiple services can run inside the same cluster, allowing ECS to manage and scale them together efficiently.

This is useful when services are related or when they share the same infrastructure.

---

# Environment Separation Using ECS Clusters

ECS clusters can also be separated by environment.

For example, you can create different clusters for:

- Development
- Testing
- Production

This approach helps with:

- Workload isolation
- Security
- Better control over deployments
- Better control over resource usage

---

# ECS Launch Types and Capacity Providers

Amazon ECS supports different launch types, also known as capacity providers.

The main ECS capacity providers are:

1. EC2 Launch Type
2. Fargate Launch Type
3. External Launch Type / ECS Anywhere

Each option defines where containers run and who manages the underlying infrastructure.

---

# EC2 Launch Type

With the EC2 launch type, you manage the underlying infrastructure yourself using Amazon EC2 instances.

This means you are responsible for:

- Creating EC2 instances
- Managing operating system updates
- Scaling the infrastructure
- Patching the instances
- Managing networking
- Securing the servers

In this model, EC2 instances are launched inside the ECS cluster, and containers are deployed on those instances.

This gives more control and flexibility, but also more operational responsibility.

---

## EC2 Launch Type Analogy

The EC2 launch type is like renting and managing an entire office building yourself.

You are responsible for everything, including:

- Setting up the building
- Maintaining it
- Choosing the layout
- Managing desks
- Managing electricity
- Managing internet
- Handling cleaning and operations

In ECS terms, this means you decide:

- How many EC2 instances to run
- How to scale them
- How to secure them
- How to maintain them

---

# Fargate Launch Type

AWS Fargate is a serverless launch type for ECS.

With Fargate, you do not manage the underlying servers.

You only define what the container needs, such as:

- CPU
- Memory
- Networking
- Security settings

AWS handles the rest.

AWS manages:

- Provisioning
- Compute capacity
- Scaling
- Patching
- Networking infrastructure

Fargate is useful when you want to focus on the application instead of managing infrastructure.

---

## Fargate Launch Type Analogy

Fargate is like renting desks in a fully managed co-working space.

You do not manage:

- Electricity
- Cleaning
- Internet
- Building maintenance
- IT infrastructure

You only bring the workers and define what they need to do.

In ECS, this means you deploy containers without touching EC2 instances directly.

---

# External Launch Type / ECS Anywhere

The external launch type is also known as ECS Anywhere.

It allows ECS tasks to run on infrastructure outside AWS.

This can include:

- On-premises servers
- Local data centers
- Edge environments
- Hybrid infrastructure

In this setup, the ECS agent runs on your own servers and connects them to your ECS cluster.

---

## ECS Anywhere Analogy

ECS Anywhere is like building and managing your own private office outside the standard managed environment.

You own and operate everything, including:

- Power
- Networking
- Security
- Hardware
- Maintenance

This gives maximum control.

It is useful for:

- Hybrid environments
- Edge locations
- Compliance requirements
- Data residency requirements

For example, if a workload must run inside a local data center for compliance reasons, ECS Anywhere allows it to be managed through ECS while still running physically on-premises.

---

# Creating an ECS Cluster

The process starts from the Amazon ECS Console.

## Step 1: Open the ECS Console

Go to the Amazon ECS Console.

From the left-hand sidebar, select:

```text
Clusters
```

Then click:

```text
Create cluster
```

---

# Cluster Name

Give the cluster a meaningful name.

A clear cluster name helps identify the purpose of the cluster later, especially when working with multiple environments or projects.

---

# Service Connect Default Namespace

The Service Connect default namespace option can be skipped for this setup.

Service Connect is related to ECS networking and service-to-service communication.

It can be configured later when working with ECS networking in more detail.

---

# Infrastructure Options

In the infrastructure section, choose where the containers should run.

The main available options are:

1. AWS Fargate
2. Amazon EC2 instances

If you are using ECS Anywhere, external instances can be registered after the cluster has been created.

---

# AWS Fargate Option

AWS Fargate is a serverless, pay-as-you-go compute engine.

With Fargate, you do not need to provision or manage servers.

When a new ECS cluster is created, it comes preconfigured with:

- Fargate capacity provider
- Fargate Spot capacity provider

This allows containers to run immediately without manually managing infrastructure.

---

# Amazon EC2 Instances Option

Amazon EC2 instances provide full control over the compute environment.

With EC2, you choose:

- Instance type
- Number of instances
- Scaling configuration
- Security configuration
- Update and patching strategy

This option gives more flexibility, but it also adds more operational overhead compared to Fargate.

---

# EC2 vs Fargate Setup Complexity

When selecting the EC2 launch type, there are more configuration options.

This is because you are responsible for managing the infrastructure.

With Fargate, the setup is simpler because AWS manages the infrastructure behind the scenes.

An ECS cluster can have both EC2 and Fargate capacity providers associated with it.

This allows some tasks to run on EC2 and others on Fargate, depending on the workload requirements.

---

# Selected Setup

For this setup, the EC2 launch type is used.

This means the cluster will be created with EC2 instances managed through an Auto Scaling Group.

---

# Auto Scaling Group Workflow

When using the EC2 launch type, the Auto Scaling Group acts as the capacity provider.

If no Auto Scaling Group already exists, a new one can be created during the ECS cluster setup.

The Auto Scaling Group defines how many EC2 instances should be available for the cluster.

---

# Provisioning Model

The provisioning model defines how EC2 instances are purchased and launched.

The available options are:

- On-Demand instances
- Spot instances

For this setup, On-Demand instances are used.

---

# Container Instance Amazon Machine Image

The container instance AMI refers to the Amazon ECS-optimized AMI used for EC2 instances in the Auto Scaling Group.

ECS-optimized AMIs come preconfigured with:

- ECS agent
- Docker runtime
- Required ECS settings

For this setup, the default AMI selection is used.

---

# EC2 Instance Type

The EC2 instance type defines the compute capacity of the instances.

It controls the available:

- CPU
- Memory
- Network performance

For this setup, the default EC2 instance type is used.

---

# EC2 Instance Role

The EC2 instance role is an IAM role assigned to EC2 instances in the ECS cluster.

This role gives the ECS container agent and Docker daemon the permissions required to communicate with Amazon ECS.

The role allows the instances to perform required actions on behalf of ECS.

For this setup, a new IAM role is created specifically for the ECS cluster.

---

# Desired Capacity

Desired capacity defines how many EC2 instances the Auto Scaling Group should start with.

You also define:

- Minimum capacity
- Maximum capacity

The minimum value defines the lowest number of instances allowed.

The maximum value defines the highest number of instances allowed.

For the group to scale out, the maximum capacity must be greater than zero.

For this setup:

```text
Minimum capacity: 1
Maximum capacity: 2
```

---

# SSH Key Pair

The SSH key pair is used to securely connect to EC2 instances.

It verifies your identity when connecting to the instance.

If you do not already have a key pair, a new one can be created from the Amazon EC2 Console.

For this setup, an existing key pair is selected.

---

# Root EBS Volume

The root EBS volume is the main storage volume attached to the EC2 instance.

It contains:

- Operating system
- Docker images
- ECS metadata
- Required instance files

By default, the root volume size is:

```text
30 GB
```

For this setup, the field is left blank so AWS uses the default size automatically.

---

# Network Settings for EC2 Instances

The network settings define the VPC and subnets where the EC2 instances will be launched.

By default, instances launch inside the default VPC of the selected AWS Region.

For this setup, a new VPC is created.

---

# Creating a New VPC

To create a new VPC:

1. Click create a new VPC.
2. Select VPC and more.
3. Give the VPC a name.
4. Choose two Availability Zones.
5. Create two public subnets.
6. Create two private subnets.
7. Do not add a NAT Gateway for this setup.
8. Skip creating VPC endpoints.
9. Create the VPC.

---

# Public Subnet Configuration

After creating the VPC, update the VPC settings to enable auto-assign public IPv4 addresses for the two public subnets.

This allows EC2 instances launched inside those public subnets to receive public IP addresses automatically.

---

# Selecting the New VPC in ECS

After creating the VPC:

1. Return to the ECS Console.
2. Refresh the VPC dropdown list.
3. Select the newly created VPC.
4. From the subnet dropdown, select the two public subnets.

The EC2 instances for this setup will be launched inside the public subnets.

---

# Security Group

If no security group exists, create a new security group.

Give the security group a meaningful name.

Add an inbound rule:

```text
Type: HTTP
Source: Anywhere
```

This allows public web traffic to reach the EC2 instances.

This security group controls network traffic for the EC2 instances running the containers, not directly for the containers themselves.

---

# Auto Assign Public IP

The auto-assign public IP option controls whether EC2 instances receive a public IPv4 address.

Instances need a public IP if they must be reachable from the internet.

Since the public subnets were already configured to auto-assign public IPs, the default subnet settings can be used.

No additional change is required here.

---

# Monitoring Settings

In the monitoring section, CloudWatch Container Insights can be enabled.

CloudWatch Container Insights provides detailed metrics and logs for ECS workloads.

For this setup, it is not enabled.

Monitoring can be configured later when focusing on ECS observability.

---

# Encryption Settings

Encryption settings are used to secure data.

They are important for production environments, but are not configured in this setup.

Encryption and ECS security can be handled later as part of a dedicated security configuration.

---

# Tags

Tags are labels assigned to ECS resources.

They help organize and manage resources, especially when there are many clusters.

For this setup, a simple tag is added:

```text
ECSP
```

Tags can be viewed later on the ECS cluster page under the Tags tab.

---

# Creating the Cluster

After completing all configurations, create the ECS cluster.

During the creation process, AWS automatically provisions the required resources.

AWS also creates a CloudFormation stack in the background to manage the infrastructure.

---

# Cluster After Creation

Once the cluster is created, there are no tasks or services running inside it yet.

The cluster has only prepared the foundation where tasks and services can run later.

Because the EC2 launch type was selected, AWS also creates and launches the required EC2 infrastructure.

---

# Resources Created Automatically

After creating the ECS cluster with EC2 launch type, several resources are created automatically.

These include:

- EC2 instance
- Auto Scaling Group
- Launch Template
- IAM roles
- CloudFormation stack

---

# EC2 Instance

In the EC2 Console, an EC2 instance is launched automatically.

This instance becomes part of the ECS cluster and can run ECS tasks.

---

# Auto Scaling Group

An Auto Scaling Group is created for the cluster setup.

It manages the number of EC2 instances available to the ECS cluster.

The Auto Scaling Group can scale the infrastructure based on the configured minimum and maximum capacity.

---

# Launch Template

A Launch Template is created and used by the Auto Scaling Group.

The launch template defines how new EC2 instances should be created when the Auto Scaling Group scales out.

It contains configuration such as:

- AMI
- Instance type
- IAM role
- Network settings
- Storage settings

---

# IAM Roles

The IAM Console can be used to verify that the required IAM roles were created.

These roles allow ECS and EC2 instances to communicate and perform the required actions.

---

# Final Result

At the end of the setup, the ECS cluster is created successfully.

The cluster now provides the foundation required to deploy containers using ECS.

Although no tasks or services are running yet, the core infrastructure is ready.

The next step is to create task definitions, run tasks, and deploy ECS services inside the cluster.
