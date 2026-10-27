# Compute ‐ advanced

# 1. Computer Instances

## 1.1 Burstable Instances

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

## 1.2 Oracle OS Management Hub

### 1.2.1 Introduction

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

### 1.2.2 Workflow

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

## 1.2.3 Components

The OS Management Hub service relies on several core components: the management station (which mirrors and distributes software and bridges managed instances to the service), profiles (blueprints for consistent system management), software sources (repositories for standardized software), agents (enabling communication between instances and the hub), network configuration (ensuring connectivity and firewall settings), Ksplice (for reboot-free kernel patching), and reports (covering software updates, security vulnerabilities, and system health). 

To set up OSMH, you must configure IAM policies, starting with user groups (defining who can manage the service), dynamic groups (automatically grouping instances based on compartment rules, including non-OCI instances via management agent resources), and policy statements (granting specific permissions). The required policies include one for dynamic groups (allowing agents to interact with OSMH) and several for the user group (managing all OSMH resources, plus management station permissions for agents and the agent key). 

Permissions can be scoped at the compartment level, but read access to profiles and software sources must be granted at the tenancy level. Finally, OSMH supports Oracle Linux and specific Windows Server versions in OCI, and Oracle Linux 7, 8, and 9 for on-premises and third-party cloud environments.

## 1.2.4 OS Management Hub for OCI Instances

First, network configuration must be verified—private subnets require a service gateway (with the All region label) or NAT gateway, public subnets require an internet gateway, and Windows instances need security rules allowing access to Windows update servers. Second, software sources must be set up, with at least one vendor software source required before registration. 

There are three types: vendor (basic vendor repository), custom (vendor base with package filters), and versioned custom (immutable, version-specific for lifecycle consistency). 

Third, profiles are configured as templates defining software sources, lifecycle stage membership, and group membership—applied only during registration and location-specific. 

Finally, registration involves three steps: creating the instance with proper network gateways, enabling the OS Management Hub agent plugin from Oracle Cloud Agent (selecting a profile during activation), and verifying the Oracle Cloud Agent version to ensure compatibility. Once complete, the instance appears as a managed instance in the OS Management Hub service within minutes.

### Key Steps Before Registration

Before you create and register compute instances with the OSMH service, there are a few key steps to follow:

1. Verify the network configuration to ensure connectivity and access.
2. Set up software sources and profiles to standardize how your instances receive updates and patches.
3. Create and register your compute instances with the service.

---

**Step 1: Network Configuration**

First, we need to make sure the network is set up correctly to support OS Management Hub resources for OCI instances.

| Instance Location | Required Configuration |
|-------------------|------------------------|
| Private Subnet | Connect to a VCN that has a service gateway with the All region label in the Oracle Services network. Alternatively, configure a NAT gateway for that VCN to enable outbound internet access. |
| Public Subnet | Configure an internet gateway to ensure your instances can communicate with the required external services. |
| Windows Instances | Set up security lists or network rules that allow access to Windows update servers. Open the necessary ports and IP addresses in your security list or network security groups for Windows updates to function properly. |

---

**Step 2: Software Sources**

Software sources are the first level of control over the content available to instances managed by the OS Management Hub service.

- You must add at least one vendor software source before registering any instances with the service.
- A software source is a collection of packages and modules similar to repositories used for updates.
- Once added to OS Management Hub, it is available across all compartments, regions, and management stations.
- This helps administrators control which packages can be installed on instances without having to set up repositories manually.

Three Main Types of Software Sources

| Type | Description |
|------|-------------|
| Vendor | The most basic type—a repository of all software provided by the OS vendor. It serves as the foundation for all other software sources. You need at least one vendor software source added to your compartment before registering instances. |
| Custom | Offers more flexibility. Start with a vendor software source as your base, then apply filters to include or exclude specific packages. Useful when you want to control exactly what is available to your instances. |
| Versioned Custom | Works similarly to a custom software source, but is assigned a specific version number. Once created, you cannot modify its contents. Perfect for managing software across different lifecycle environments (development, testing, production). |

---

**Step 3: Configuring Profiles**

Profiles help simplify and standardize the process of registering your instances. Instead of manually configuring each instance, you can use a profile to automatically apply the same settings to multiple instances at once.

Think of profiles as templates that define settings such as:

- Software sources — the locations from which software updates and packages are retrieved and made available to managed instances.
- Lifecycle stage membership — defines where an instance fits within a lifecycle environment (development, testing, or production).
- Group membership — allows you to assign instances to specific groups, making it easier to manage and update them together.

> Important Notes:
> - Profiles only impact instances during the registration process. Once an instance is registered, the profile no longer affects it.
> - Profiles are location-specific, meaning you cannot use a profile created for Oracle Cloud OCI on an instance in your own data center.

---

**Step 4: Registering Your Instance**

The registration process has three main steps:

1. Create the Instance
- Create the instance based on your required configuration.
- Ensure network setup is configured with the required gateways (service gateways for private subnets or internet gateways for public subnets).

2. Enable the OS Management Hub Agent
- From Oracle Cloud Agent, activate the OS Management Hub agent plugin.
- This plugin allows communication between your instance and the OS Management Hub service.
- During activation, you will select a profile for your instance.
  - This profile automatically applies all necessary configurations, including software sources, lifecycle stage membership, and group membership.
- Once completed, your instance becomes ready for centralized management and will appear as a managed instance in the OS Management Hub service.

3. Verify the Oracle Cloud Agent Version
- Running a specific command to install or update to the latest version ensures compatibility.
- This is useful for registering existing OCI instances with the OS Management Hub service.
- Once you've confirmed the agent version, you can enable the OS Management Hub agent plugin.
- Within a few minutes, your instance will appear in the list of Managed Instances, ready to be managed through the OS Management Hub service.

---

## 1.3 Security instance

### 1.3.1 Shielded Instances

Shielded instances protect virtual machines and bare metal instances against low-level threats like Rootkits and Bootkits, which have kernel-level privileges, can infect firmware and the operating system, and are very difficult to detect. 

Enabling shielded instances follows the standard instance deployment flow—simply choose an image and shape with a shield icon, then enable Secure Boot and Measured Boot in the advanced options (enabling Measured Boot for VMs automatically enables TPM). 

Shielded instances combine three components: Secure Boot (a UEFI feature that prevents unauthorized, unsigned bootloaders and operating systems from booting), Measured Boot (which stores measurements of boot components like bootloaders, drivers, and operating systems, and verifies they don't change between boots), and Trusted Platform Module (a specialized security chip used by Measured Boot to securely store boot measurements in Platform Configuration Registers, or PCRs, which summarize all measurement results). 

Secure Boot and TPM are available on all supported bare metal and VM instances, while Measured Boot is only available on VM instances—and all these services are provided for the OCI platform at no charge.

**The Threat: Rootkits and Bootkits**

Threats like Rootkits and Bootkits that have kernel-level privileges can:
- Infect the firmware and operating system
- Be very difficult to detect

Shielded instances protect virtual machines and bare metal instances against these low-level threats.

---

**The Three Components of Shielded Instances**

A shielded instance uses a combination of:

1. Secure Boot
2. Measured Boot
3. Trusted Platform Module (TPM)

Availability

| Feature | Availability |
|---------|--------------|
| Secure Boot | All supported bare metal and virtual machine instances |
| Trusted Platform Module | All supported bare metal and virtual machine instances |
| Measured Boot | Only VM instances |

> These services are provided for the OCI platform at no charge.

---

### 1.3.2 Confidential Computing

Confidential computing takes encryption to the next level by securing data while it is loaded in memory, not just at rest or in transit—previously, all data running in memory was accessible as clear text. It protects data in use at the hardware level by encrypting and isolating both the data and the application processing it, preventing unauthorized access or modification. 

It also minimizes the list of trusted parties (operating system, ecosystem partners, administrators), reducing the risk of data exposure. Powered by AMD EPYC processors, confidential computing enables confidential virtual machines via Secure Encrypted Virtualization (SEV) and confidential bare metal servers via Secure Memory Encryption (SME)—features of AMD Infinity Guard available in second and third generation AMD EPYC processors in all OCI E3 and E4 shapes. 

Supported VM shapes include VM.Standard.E4.Flex and VM.Standard.E3.Flex (on Oracle Linux 7.x or 8.x platform images), while supported bare metal shapes include BM.DenseIO.E4.128, BM.Standard.E4.128, and BM.Standard.E3.128 (on any platform image). 

The key benefits are: improved isolation using real-time encryption with a per-VM key generated at VM creation that resides solely in the AMD secure processor and is inaccessible to any application, hypervisor, or OCI; no application changes required; and little to no performance impact.

### Supported Compute Shapes

**Virtual Machine Compute Shapes**
- `VM.Standard.E4.Flex`
- `VM.Standard.E3.Flex`

Supported on: Oracle Linux 7.x or 8.x platform images.

**Bare Metal Instance Compute Shapes**
- `BM.DenseIO.E4.128`
- `BM.Standard.E4.128`
- `BM.Standard.E3.128`

Supported on: Any platform image.

---

### Benefits of Confidential Computing

**1. Improved Isolation Using Real-Time Encryption**
- Data and applications are encrypted using a per-VM encryption key generated during VM creation.
- The key resides solely in the AMD secure processor, which is part of the CPU.
- The key is not accessible from any application—whether the VM, the instance, the hypervisor, or Oracle Cloud Infrastructure.

**2. No Application Changes Required**
- If you want to enable confidential VMs, you don't require any change in the application.

**3. Little to No Performance Impact**
- Applications experience little to no performance impact with confidential computing enabled.

---

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

# 4. Remote management and access

## 4.1 Run Command

Run command is a feature that allows you to remotely configure, manage, and troubleshoot compute instances by running scripts within the instance—even when the instance does not have SSH access or open inbound ports. 

It is supported on platform images including Oracle Autonomous Linux, Oracle Linux, CentOS, and Windows Server, but not Ubuntu. 

On Linux instances, scripts run in a bash shell by default, while on Windows instances they run as a batch file. 

There are size limits to be aware of: script files uploaded directly in plain text are limited to 4 KB (use object storage for larger files), and plain text script output is limited to the last 1 KB (again, use object storage for larger responses). 

Common use cases include automating tasks like configuring secondary VNICs and troubleshooting SSH connectivity.

### What Is a Run Command?

Using run command, you can remotely configure, manage, and troubleshoot compute instances by running scripts within the instance.

Key capability:
- You can run commands on an instance even when the instance does not have SSH access or open inbound ports.

---

### Supported Platforms

The run command feature is supported on compute instances that use platform images.

| Supported | Not Supported |
|-----------|---------------|
| Oracle Autonomous Linux | Ubuntu |
| Oracle Linux | |
| CentOS | |
| Windows Server | |

---

### Script Execution Details

| Instance Type | Script Runs As |
|---------------|----------------|
| Linux | Bash shell (by default) |
| Windows | Batch file |

---

### Size Limits

| Item | Limit | Workaround |
|------|-------|------------|
| Script file uploaded directly in plain text | 4 KB maximum | Save the file in an object storage location to provide a larger file. |
| Script output written as plain text | Last 1 KB | Save the output to an object storage location to save a larger response. |

---

### Examples of Use Cases

- Automate tasks, such as configuring secondary virtual network interface cards (VNICs).
- Troubleshoot SSH connectivity.

---

## 4.2 Instance Console Connection

Instance console connection provides a display of what is happening during the boot process, allowing you to remotely troubleshoot malfunctioning instances—it is a special case intended for troubleshooting, not normal operations. 

There are two connection options: Cloud Shell connection (which automatically creates the console connection and a temporary SSH key) and local connection (which requires generating an SSH key pair or uploading your own public key). 

There are two types of console connections: serial console connection, used mainly for Linux servers and CLI-based operating systems and accessed via SSH, and VNC console connection, used for servers with a graphical user interface and accessed by setting up a secure tunnel to the VNC server and connecting with a VNC client. 

Note that Cloud Shell can only be used for serial console connections, not VNC. Use cases include editing system configuration files, adding or resetting SSH keys for the OPC user, troubleshooting imported or customized images that fail to boot, and recovering instances that suddenly stop responding.

### What Is a Console Connection?

The idea of a console connection is to give you a display of what is happening during the boot process. This allows you to remotely troubleshoot malfunctioning instances.

> Important: This is a special case—you do not use it for normal operations, but for troubleshooting.

---

### Connection Options

There are two options for creating a console connection:

**1. Cloud Shell Connection**

- The Cloud Shell integration automatically creates the instance console connection and the temporary SSH key.

**2. Local Connection**

- When you create a local connection, you need to generate an SSH key pair, or you can upload your own public key.

---

### Types of Instance Console Connections

**1. Serial Console Connection**

- Mainly used for Linux servers and other CLI-based operating systems.
- After you create the console connection for the instance, you can connect to the serial console using a Secure Shell (SSH) connection.

**2. VNC Console Connection**

- Commonly used for connecting to servers that have a graphical user interface (GUI).
- You need to set up a secure tunnel to the VNC server on the instance, and then you can connect with a VNC client.

> Note: You cannot use Cloud Shell for VNC console connections. You can only use it for serial console connections.

---

### Use Cases

Once you are connected with an instance console connection, you can perform tasks such as:

- Editing system configuration files
- Adding or resetting the SSH keys for the OPC user

**Troubleshooting Scenarios**

- An imported or customized image that is not booting successfully
- An instance that was working fine but suddenly stops responding

In these scenarios, you can make use of instance console connections for troubleshooting.

---


# 5. Infrastructure Maintenance

Infrastructure maintenance is a necessary part of any environment, and OCI performs routine maintenance on physical infrastructure for VM instances while offering three migration options to minimize inconvenience: live migration, reboot migration, and manual migration. 

Live migration moves supported VM instances from a host needing maintenance to a healthy host with minimal disruption—it emits maintenance events at start and end, is only supported by Linux operating systems, and is restricted to specific shapes. 

If live migration isn't supported, OCI schedules a reboot migration due date within 14 to 16 days and sends a notification; if you don't proactively reboot before the due date, the instance is stopped, migrated, and restarted, with a short downtime you can control by rebooting proactively. Note that rebooting the operating system does not trigger migration—only rebooting the instance from the Console, CLI, or API does. 

Manual migration is sometimes required for instances without a reboot maintenance date, requiring termination and recreation (preserve the boot volume). 

When infrastructure fails, standard VMs are recovered via reboot migration to a healthy host, while dense I/O VMs are recovered by rebooting on the same host; if that's not possible, OCI notifies you to delete the instance within 14 days, after which it disables and deletes it within seven more days—though boot volumes and remote-attached data volumes are preserved.

## Introduction

Maintenance is a necessary part of any environment. Oracle Cloud Infrastructure (OCI) also performs routine maintenance on the physical infrastructure for virtual machine instances. OCI strives to minimize this inconvenience by offering different options to relocate virtual machines:

1. Live migration
2. Reboot migration
3. Manual migration

---

## Live Migration

During an infrastructure maintenance event, OCI live migrates supported virtual machine instances from the physical VM host that needs maintenance to a healthy VM host.

Key points:
- There is minimal disruption to running instances.
- When a live migration begins and ends, an infrastructure maintenance event is emitted.
- Live migration is only supported by Linux operating systems and restricted to specific shapes.

---

## Reboot Migration

If the virtual machine instance does not support live migration, OCI schedules a due date for reboot migration within 14 to 16 days and sends you a notification.

Key points:
- If you do not proactively reboot the instance before the due date, the instance is stopped and migrated to a healthy host and restarted.
- There is a short downtime during the migration.
- You can control when the downtime occurs by proactively reboot migrating the instance before the maintenance due date.

> Important Callout: Rebooting the operating system is not the same as rebooting the instance from the Console, CLI, or API. The operating system reboot does not trigger the migration.

---

## Manual Migration

Despite all efforts, sometimes a manual migration will be required.

Key points:
- For instances without a date in the reboot maintenance field, you must move the instance manually.
- In this scenario, the instance will need to be terminated and recreated.
- When it is time to terminate, don't forget to preserve the boot volume.

---

## Infrastructure Failure Recovery

When the underlying infrastructure of a virtual machine instance fails, OCI automatically attempts to recover the instance.

| Instance Type | Recovery Method |
|---------------|-----------------|
| Standard VM Instances | Recovered using reboot migration, which automatically restores the VM on a healthy host. |
| Dense I/O VMs | Recovered by rebooting the instance on the same physical host. |

### If Recovery on the Same Host Is Not Possible (Dense I/O VMs)

- OCI notifies you to delete or terminate the instance within 14 days.
- If you don't delete the instance before the deadline, OCI disables the instance on the deadline and deletes it within the next seven days.
- However, the boot volume and remote-attached data volumes are preserved.

---



