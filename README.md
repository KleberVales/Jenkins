# Compute ‐ advanced

# 1. Computer Instances

## Burstable Instances

Burstable instances are a cost-effective virtual machine option for workloads with low average CPU usage and occasional spikes. They provide a guaranteed baseline of CPU performance—either 12.5% or 50% of each core—and allow bursting above that baseline when the host has available resources. 

However, bursting is not guaranteed and depends on the instance's average CPU usage over the past 24 hours. Memory does not burst, only CPU. Supported shapes include VM.Standard3.Flex, VM.Standard.E3.Flex, and VM.Standard.F4.Flex. 

Creating a burstable instance involves selecting a supported shape, specifying the OCPU count, enabling the burstable option, and setting the baseline. Pricing is based solely on the baseline OCPU, making burstable instances an economical choice for microservices, dev/test environments, CI/CD tools, static websites, and monitoring systems.

### What Are Burstable Instances?

A **burstable instance** is a virtual machine instance that offers a **baseline level of CPU performance** with the ability to **burst to a higher level** when needed to support occasional spikes in usage. These instances are ideal for scenarios where the instance is typically idle or has low CPU utilization, with only occasional increases in demand.

### Supported Shapes

Not all shapes support burstable instances. Supported shapes include:

- `VM.Standard3.Flex`
- `VM.Standard.E3.Flex`
- `VM.Standard.F4.Flex`

---

### Creating a Burstable Instance

The process is similar to creating an on-demand instance, with a few key differences:

1. **Change the shape** to one of the supported burstable shapes.
2. **Specify the number of OCPUs** (maximum CPU count).
3. **Select the burstable option**.
4. **Set the baseline** (e.g., 12.5% or 50%).

---

### Pricing

Pricing for burstable instances is based on the **baseline OCPU**. Even if your instance bursts above the baseline, you still pay only for the baseline OCPU.

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

Common plugin examples include the **Bastion plugin** (required for OCI Bastion service to provide secure, time-limited access to resources without public endpoints), the **OSMS plugin** (required for the Operating System Management Service to manage patches and updates), and the **Compute Instance Run Command plugin** (which enables running scripts within the instance). 

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






