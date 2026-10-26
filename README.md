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






