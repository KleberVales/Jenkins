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

## Compute Vertical Scaling

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






