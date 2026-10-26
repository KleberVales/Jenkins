# Compute ‐ advanced

# 1. Computer Instances

## Burstable Instances

Burstable instances are a cost-effective virtual machine option for workloads with low average CPU usage and occasional spikes. They provide a guaranteed baseline of CPU performance—either 12.5% or 50% of each core—and allow bursting above that baseline when the host has available resources. 

However, bursting is not guaranteed and depends on the instance's average CPU usage over the past 24 hours. Memory does not burst, only CPU. Supported shapes include VM.Standard3.Flex, VM.Standard.E3.Flex, and VM.Standard.F4.Flex. 

Creating a burstable instance involves selecting a supported shape, specifying the OCPU count, enabling the burstable option, and setting the baseline. Pricing is based solely on the baseline OCPU, making burstable instances an economical choice for microservices, dev/test environments, CI/CD tools, static websites, and monitoring systems.

### What Are Burstable Instances?

A burstable instance is a virtual machine instance that offers a baseline level of CPU performance with the ability to burst to a higher level when needed to support occasional spikes in usage. These instances are ideal for scenarios where the instance is typically idle or has low CPU utilization, with only occasional increases in demand.

### Supported Shapes

Not all shapes support burstable instances. Supported shapes include:

- `VM.Standard3.Flex`
- `VM.Standard.E3.Flex`
- `VM.Standard.F4.Flex`

---

### Creating a Burstable Instance

The process is similar to creating an on-demand instance, with a few key differences:

1. Change the shape to one of the supported burstable shapes.
2. Specify the number of OCPUs (maximum CPU count).
3. Select the burstable option.
4. Set the baseline (e.g., 12.5% or 50%).

---

### Pricing

Pricing for burstable instances is based on the baseline OCPU. Even if your instance bursts above the baseline, you still pay only for the baseline OCPU.

---

## Oracle OS Management Hub

### Introduction

Oracle OS Management Hub is a centralized cloud solution from Oracle for managing, updating, and monitoring operating systems (such as Oracle Linux) in a unified way.

It solves a major problem for IT administrators: managing security patches, update cycles, and compliance not only on servers running within the OCI itself, but also in hybrid and multi-cloud environments—that is, servers spread across on-premises data centers and even other clouds, such as AWS and Microsoft Azure.

---

**Common Challenges in OS and Patch Management**

1. Managing a Small Number of Instances

Even with just a few compute instances, administrators must handle:

- Software management and patching — Applications and updates follow different cycles:
  - Oracle, as an OS vendor, releases critical patch updates quarterly.
  - Linux security patches are released frequently and unpredictably as vulnerabilities are discovered.
  - Staying on top of these updates requires constant attention.
- Reporting security and compliance — Ensuring SLAs are met, generating reports, and maintaining security to meet regulatory standards.
- Monitoring and assessment — Choosing tools to monitor performance, quickly identify issues, and resolve them.

2. Scaling to Hundreds of Instances

As the number of instances grows:

- Manually managing patches and compliance becomes time-consuming, error-prone, and a major drain on team resources.
- Teams end up spending time on repetitive tasks instead of important projects.
- Risk increases: Missing a single patch or vulnerability can expose the entire system to attack.

3. Expanding Across Environments

When infrastructure spans on-premises and multiple cloud environments, complexity grows due to:

- A growing number of systems
- Diverse environments (on-premises, multiple cloud providers, different operating systems)
- Maintaining security compliance across all systems
- Lifecycle management — ensuring every system is up to date and running smoothly

> As infrastructure grows, a centralized management solution becomes essential—providing a unified view, automating tasks, and simplifying management across all environments.

---

**Introducing OCI OS Management Hub**

OCI OS Management Hub is a management-as-a-service solution designed to simplify and centralize management across your organization.

Key Benefits

| Benefit | Description |
|---------|-------------|
| Centralized Management | Gain complete control over updates throughout your entire environment, reducing administrative overhead and improving efficiency. |
| Security and Compliance | A clear, consolidated dashboard allows you to monitor compliance, track patch statuses, and review reports to stay proactive and aligned with security standards. |
| Zero Downtime Updates | With Ksplice integration, apply critical Oracle Linux updates without rebooting systems, minimizing downtime and disruption. |
| Multi-Environment Support | Manage systems across OCI, third-party clouds, and on-premises environments from a single unified view. |

Supported Environments

No matter where your infrastructure resides—on-premises data centers, OCI, or third-party clouds like Microsoft Azure and AWS—the OS Management Hub supports easy management. It offers tools and resources for Oracle Linux, enhancing your experience and streamlining management.

### Workflow

The OS Management Hub workflow begins by grouping instances that share the same OS type, release, and architecture, enabling centralized and unified management. 

For each managed instance, a management agent is deployed to gather patch status and compliance data and report it back to the OS Management Hub. 

When infrastructure expands beyond OCI to on-premises or third-party cloud environments, management stations act as a bridge—mirroring software repositories and locally distributing updates to reduce bandwidth usage. 

Only the management stations need direct communication with the OCI service, while instances connect to the management station and rely on agents to keep everything in sync. 

From the OS Management Hub console, administrators can monitor all instances regardless of location, view available security and bug updates, verify communication status, access real-time compliance reports with detailed security advisory information, apply or schedule updates, and download reports for further review. 

The service offers four key benefits: enhanced security through up-to-date patches, time savings through automation, cost reduction by eliminating third-party tools and reducing administrative needs, and simplified management via a centralized console with grouping and automated updates. 

Ultimately, OS Management Hub simplifies OS management across diverse environments—on-premises, OCI, or third-party clouds—improving security, saving time, lowering costs, and reducing management complexity.

**Scenario: Managing Instances in OCI**

Suppose you have a few instances running in OCI that need to be managed. To simplify this process, you can group these instances together—provided they share the same:

- OS type
- Release
- Architecture

This grouping makes it easier to:
- Manage tasks
- Apply updates
- Maintain consistency across your environment

When your instances are grouped, the OS Management Hub service provides centralized, unified management across all your systems. It automates tasks like:
- Patching
- Monitoring compliance
- Ensuring your system stays secure and up to date

This helps reduce manual effort.

---

**The Role of the Management Agent**

For each managed instance, a management agent is deployed.

The agent:
- Gathers important data about the instance, such as patch status and compliance
- Sends this data back to the OS Management Hub

This setup enables you to:
- Easily automate updates
- Manage patches across all systems
- Maintain consistency

---

**Extending to On-Premises and Third-Party Clouds**

If your infrastructure expands to include instances not just in OCI but also on-premises or within third-party clouds, OS Management Hub can seamlessly manage all these systems from one centralized location—making it adaptable to your scaling needs.

**Management Stations**

To make this possible, you deploy one or more management stations, which act as a bridge between your on-premises or cloud instances and the OS Management Hub in OCI.

Key points about management stations:
- They mirror the software repository and locally distribute updates, reducing bandwidth usage for patching.
- Only the management stations need to communicate directly with the OCI service.
- Your instances connect to the management station, receive updates from it, and don't need direct access to the OCI or OS Management Hub service.
- Management agents are still deployed on both your managed instances and the management station, so the agents keep everything in sync by reporting back to OS Management Hub via the management station.

---

**Monitoring from the OS Management Hub Console**

From the OS Management Hub console, you as an administrator can easily:

- Monitor the status of all your instances—whether they're on-premises, in a third-party cloud, or within OCI.
- Get a clear view of available security and bug updates that haven't yet been applied across your managed environment.
- See a status check to verify that all managed instances are properly communicating with the service.
- Access real-time reports giving you a comprehensive view of your security patch compliance status.
  - These reports include detailed security advisory information, so you can quickly identify and address vulnerabilities.
- Apply updates immediately or schedule them for a later time.
- Download these real-time reports at any time for further review.

---

**Four Key Benefits of OS Management Hub**

| Benefit | Description |
|---------|-------------|
| Enhanced Security | Ensures your systems stay up to date with the latest security patches and bug fixes, minimizing vulnerabilities and keeping your OS environment secure and reliable. |
| Time Savings | Automation for patching policies and scheduling updates streamlines processes, saving valuable time—especially when managing large-scale deployments. |
| Cost Reduction | As a fully managed service, it reduces the need for additional administrative resources and eliminates the cost associated with third-party management tools, allowing you to achieve more with fewer resources. |
| Simplified Management | Provides a centralized, simplified console to manage instances at scale with grouping for fleet management and automated updates, making large-scale operations easier to handle. |

> In short, OS Management Hub helps you secure your environment, save time, reduce cost, and simplify management—all from one unified platform.

---

## Components

The OS Management Hub service relies on several core components: the **management station** (which mirrors and distributes software and bridges managed instances to the service), **profiles** (blueprints for consistent system management), **software sources** (repositories for standardized software), **agents** (enabling communication between instances and the hub), **network configuration** (ensuring connectivity and firewall settings), **Ksplice** (for reboot-free kernel patching), and **reports** (covering software updates, security vulnerabilities, and system health). 

To set up OSMH, you must configure IAM policies, starting with **user groups** (defining who can manage the service), **dynamic groups** (automatically grouping instances based on compartment rules, including non-OCI instances via management agent resources), and **policy statements** (granting specific permissions). The required policies include one for dynamic groups (allowing agents to interact with OSMH) and several for the user group (managing all OSMH resources, plus management station permissions for agents and the agent key). 

Permissions can be scoped at the compartment level, but read access to profiles and software sources must be granted at the tenancy level. Finally, OSMH supports Oracle Linux and specific Windows Server versions in OCI, and Oracle Linux 7, 8, and 9 for on-premises and third-party cloud environments.



# 2. Scalling

## 2.1 Compute Vertical Scaling

Compute vertical scaling—also known as scaling up or scaling down—allows you to change the shape of a virtual machine instance to add or remove resources without rebuilding instances or redeploying applications. 

Scaling up increases performance, while scaling down saves costs. 

When performing vertical scaling, the instance's public and private IP addresses, volume attachments, and VNIC attachments remain unchanged. However, the number of OCPUs, memory, network bandwidth, and maximum number of VNICs are all affected. 

The target shape you can select depends on the current shape series and image, and if the instance is running during the change, it will be rebooted. 

You can automate vertical scaling by using the monitoring service to trigger alarms that call functions to adjust the shape as needed. Just remember: scaling a virtual machine will always cause a reboot.

### What Is Vertical Scaling?

Vertical scaling means you add additional resources to an instance so that it meets demand. It allows you to change the shape of a virtual machine instance without having to rebuild your instances or redeploy your applications.

There are two directions you can scale:

- Scale Up: Increases performance by adding more resources.
- Scale Down: Helps you save costs by reducing resources.

As shown in the screenshot, there is an option to edit the shape of your instance.

---

### What Stays the Same?

When you perform vertical scaling, the following remain unchanged:

- IP address of the instance (both public and private)
- Volume attachments
- VNIC attachments

---

### What Changes?

When a vertical scaling operation is performed, the following are affected:

- Number of OCPUs
- Memory
- Network bandwidth
- Maximum number of VNICs

> Note: If the instance is running when you change the shape, it is rebooted as part of the change shape operation.

---

### Automation Possibility

You can use the monitoring service to monitor metrics and raise an alarm when a particular metric is met. The alarm notification can then call a function to adjust the shape of the virtual machine as needed—allowing you to automate the vertical scaling process.

1. Automation: Monitoring + alarms + functions can automate shape adjustments.
2. Reboot Required: Scaling a virtual machine will cause a reboot.

---

## 2.2 Compute AutoScaling

Compute autoscaling relies on two key constructs: instance configurations (templates defining instance settings) and instance pools (groups of instances managed together within a region). 

An instance configuration can be created from an existing instance or defined manually, and it excludes block volume contents. An instance pool can have only one instance configuration but can be used across multiple pools, and its size can be adjusted manually or automatically. 

Autoscaling can be metric-based (triggered by performance thresholds) or schedule-based (triggered at specific times). To configure autoscaling, you create an instance configuration, create an instance pool from it, configure the pool (size, instances, load balancers), and define an autoscaling policy. 

For example, a pool with an initial size of 2, a minimum of 1, and a maximum of 4 can scale out by adding 2 instances when CPU or memory utilization exceeds 70%, and scale in by removing 2 instances when utilization drops below 70%. This ensures consistent performance during high demand while reducing costs during low demand.

### Instance Configuration

An instance configuration is a template that defines the settings to use when creating compute instances.

- You can use an instance configuration to create one or more instances in an instance pool.
- You can also use it to create individual instances.
- Important: It does not include the contents of any block volumes attached to the instance.

You have the option to:
- Launch an instance from the instance configuration.
- Create an instance pool from the instance configuration.

> Note: To modify an existing instance configuration, you must create a new instance configuration with the desired settings.

When creating an instance configuration, you can either:
- Use an existing compute instance as a template, or
- Provide a list of configuration settings.

The instance configuration typically contains:
- Operating system image
- Metadata
- Shape
- VNICs
- Storage
- Subnet information

---

### Instance Pool

Instance pools allow you to create and manage multiple compute instances within the same region as a group.

Key points:
- You can use the same instance configuration for multiple instance pools.
- However, an instance pool can have only one instance configuration associated with it.
- After creating an instance pool, you can:
  - Update the size of the pool
  - Add or remove existing instances from the pool
  - Attach or detach load balancers
- You can adjust the number of instances running in the pool using either metrics or a schedule.

Instances in a pool can be:
- Deployed in different availability domains
- Placed behind a load balancer

> The whole point of grouping multiple instances together is to manage them as a group.

---

### Types of Autoscaling Policies

**Metric-Based Autoscaling**

An autoscaling action triggered when a performance metric meets or exceeds a threshold.

Benefits:
- Provides consistent performance for end users during periods of high demand.
- Helps reduce costs during periods of low demand.

**Schedule-Based Autoscaling**

An autoscaling event that takes place at specific times that you schedule.

---

# 3. Oracle Cloud Agent

Oracle Cloud Agent is a lightweight process that manages plugins on compute instances and is installed by default on current platform images. 

Plugins are installed as part of the agent and are responsible for tasks such as collecting performance metrics, installing OS updates, and performing other instance management functions. 

To use plugins, the Oracle Cloud Agent software must be installed, and the plugins must be enabled and running. 

Common plugin examples include the Bastion plugin (required for OCI Bastion service to provide secure, time-limited access to resources without public endpoints), the OSMS plugin (required for the Operating System Management Service to manage patches and updates), and the Compute Instance Run Command plugin (which enables running scripts within the instance). 

Plugins can be enabled or disabled as needed. While Oracle Cloud Agent is installed by default on current platform images, it can also be manually installed on instances using other supported images.

## What Do Plugins Do?

Plugins perform a variety of instance management tasks, including:

- Collecting performance metrics
- Installing operating system updates
- Performing other instance management tasks

---

## Requirements for Using Plugins

To use plugins on an instance, the following conditions must be met:

1. The Oracle Cloud Agent software must be installed on the instance.
2. The plugins must be enabled.
3. The plugins must be running.

---

## Plugin Examples

### 1. Bastion

- OCI provides a service known as OCI Bastion, which offers restricted and time-limited secure access to resources that don't have a public endpoint.
- To use the Bastion service, you require the Bastion plugin to be enabled and running on the instance.

### 2. OSMS Agent

- The Operating System Management Service (OSMS) manages patches and updates for the operating system environment in an instance.
- To use OSMS, you require the OSMS plugin to be enabled and running.

### 3. Compute Instance Run Command

- This plugin facilitates running scripts within the instance using the run command feature.

> You can enable or disable a plugin as needed.

---

## Installation

- If you create an instance using a current platform image, Oracle Cloud Agent is installed by default.
- It is also possible to manually install the Oracle Cloud Agent software on an instance that uses another supported image.

---







