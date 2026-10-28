# Block Storage ‐ basic

# 1. Block Volume

## Block Volume
## Local NVMe

# 2. Volume Attachment Types

## iSCSI
## Paravirtualized

# 3. Volume Access Types

## Read/Write
## Read/Write Shareable
## Read-Only Shareable

# 4. Boot Volumes

---

.\
.\
.\
.\
.

---

# 1. Block Volume

A “virtual disk” that exists separately from the compute instance. You can create it, attach it, connect it, and move it between instances. Once attached and connected, it behaves like a regular hard drive.

OCI provides two flavors of block storage:

| Storage Type | Persistent? | Durability | Best For |
|---|---|---|---|
| Local NVMe | Non-persistent (survives reboots) | Customer-managed | Big data, OLTP, high-performance workloads |
| Block Volume | Persistent | 99.99% (4 nines) annual durability | Sand-like features, persistent storage |

---

## Block Volume

Block Volume provides persistent, durable (99.99% annual durability), and high-performance storage with automatic replication and encryption.

It is ideal for persistent storage, expanding instance storage, and scaling. 

Block Volumes range from 50 GB to 32 TB, with up to 32 volumes per instance, and are always encrypted at rest with AES 256-bit encryption. 

- Block Volume Specifications

| Feature | Specification |
|---|---|
| Size Range | 50 GB to 32 TB in 1 GB increments |
| Default Size | 1 TB |
| Max Volumes per Instance | Up to 32 volumes |
| Replication | Automatically replicated for data loss protection |
| Data Redundancy | Multiple copies stored redundantly across multiple storage servers with built-in repair mechanisms |
| Encryption at Rest | Always encrypted using AES with 256-bit encryption |
| In-Transit Encryption | Optional for virtualized volume attachments on VM instances |

---

**Block Volume Performance**

OCI Block Volume service offers four performance levels: Lower Cost, Balanced, Higher Performance, and Ultra High Performance.

- Lower Cost

Lower Cost is ideal for throughput-intensive workloads but is not available for boot volumes. 

Lower Cost Performance Level

| Characteristic | Value |
|---|---|
| Recommended For | Throughput-intensive workloads |
| IOPS | 2 IOPS per GB up to a maximum of 3,000 IOPS per volume |
| Throughput | 240 KBPS per GB up to a maximum of 480 MBPS per volume |
| Cost | Pay only for storage cost; no additional VPU cost |
| Availability | Block volumes only (not available for boot volumes) |
| Recommended Workloads | Streaming, log processing, data warehouses |

---

- Balanced

Balanced is the default and provides a good balance between performance and cost. 

Default Performance Configuration

When you create a volume:

- By default, it is configured for Balanced performance level.
- You can change this when you create the volume, or update it at any point after creation.
- You can control the target volume performance using either:
  - The slider, or
  - The volume performance unit control

---

Balanced Performance Level

| Characteristic | Value |
|---|---|
| Recommended For | Most workloads; default performance level |
| IOPS | 60 IOPS per GB up to 25,000 IOPS per volume |
| Throughput | 480 KBPS per GB up to a maximum of 480 MBPS per volume |
| Cost | 10 volume performance units (VPUs) per GB per month |
| Availability | Block and boot volumes |
| Benefits | Good balance between performance and cost-saving |

---


- Higher Performance

Higher Performance is for high I/O workloads, while Ultra High Performance is for the most demanding workloads and is only available for block volumes. 

Higher Performance Level

| Characteristic | Value |
|---|---|
| Recommended For | Workloads with high I/O requirements that don't require ultra high performance |
| IOPS | 75 IOPS per GB up to a maximum of 50,000 IOPS per volume |
| Throughput | 600 KBPS per GB up to a maximum of 680 MBPS per volume |
| Cost | 20 VPUs per GB per month |
| Availability | Block and boot volumes |

---


- Ultra High Performance

Ultra High Performance Level

| Characteristic | Value |
|---|---|
| Recommended For | Workloads with highest I/O requirements requiring best possible performance |
| IOPS | 225 IOPS per GB (range: 90 to 225) |
| Throughput | 1,800 KBPS per GB up to a maximum of 2,680 MBPS per volume |
| Cost | 30 to 120 VPUs per GB per month |
| Availability | Block volumes only |

---


**Dynamic performance scaling**

Block Volume dynamic performance scaling with autotuning allows OCI to automatically adjust volume performance. 

Block Volume provides **dynamic performance scaling with autotuning**. This feature enables you to configure your volumes so that the service can **adjust the performance level automatically** to optimize performance.

There are two types of dynamic performance scaling with autotuning:

1. Performance-Based Autotuning
2. Detached-Volume Autotuning

- Performance-Based Autotuning

Adjusts performance between default and maximum VPU levels based on monitored metrics such as throttled operations, guaranteed VPUs, IOPS, and throughput. Default VPUs cannot be 0 or 120, and maximum must be at least 10 VPUs higher than default.

If both are enabled, performance-based autotuning takes over when the volume is reattached. When performance-based autotuning is turned off, the volume reverts to the default VPUs per GB setting. This feature helps optimize performance and cost based on actual workload demands and volume attachment state.

How It Works

When this option is enabled, Block Volume adjusts the volume's performance between the levels you specify, based on the monitored performance for the volume.

Configuration Settings

When you enable performance-based autotune, you specify:

| Setting | Description |
|---|---|
| Default VPUs per GB | The lowest performance level the volume will be adjusted to when attached to an instance. |
| Maximum VPUs per GB | The maximum performance level the volume will be adjusted to. |

Important Rules

- The default VPUs per GB cannot be set to either zero or 120.
- The maximum VPUs per GB must be at least 10 VPUs per GB higher than the default VPUs per GB.
- When you turn off performance-based autotuning, the volume's performance changes back to the default VPUs per GB setting.

Metrics Used for Monitoring

Block Volume monitors the volume's performance using the following metrics:

| Metric | Description |
|---|---|
| Volume Throttled Operations | Total sum of all I/O operations throttled during a given time interval. |
| Volume Guaranteed VPUs per GB | The guaranteed VPUs per GB for the volume. |
| Volume Guaranteed IOPS | The guaranteed IOPS for the volume. |
| Volume Guaranteed Throughput | The guaranteed throughput for the volume. |

These metrics help the service determine the load on the Block Volume and decide whether the performance level needs to be adjusted.

---

- Detached-Volume Autotuning

Adjusts performance to Lower Cost (0 VPUs per GB) when the volume is detached and returns to the previous setting when reattached.

How It Works

When this option is enabled, Block Volume adjusts the volume's performance level based on whether the volume is attached or detached from an instance.

Behavior

| Volume State | Performance Adjustment |
|---|---|
| Detached | Performance level is automatically changed to Lower Cost, which means zero VPUs per GB for both block volumes and boot volumes. |
| Reattached | Performance is automatically adjusted back to the previous setting (default VPUs per GB). |

Interaction with Performance-Based Autotuning

If performance-based dynamic scaling with autotuning is also enabled, it will take effect when the volume is reattached to further dynamically scale performance as needed by workloads using the volume.

## Local NVMe

Local NVMe provides extremely low latency and high performance but is non-persistent and not protected by OCI—customers are responsible for data durability.

Local NVMe devices are found on certain instance shapes like BM.DenseIO2.52 and are best for high-performance workloads such as big data and OLTP.

**Local NVMe SSD Devices**

What Are Local NVMe Devices?

Some compute instance shapes, such as BM.DenseIO2.52, include locally attached NVMe devices.

**Characteristics**

| Feature | Description |
|---|---|
| Latency | Extremely low |
| Performance | High-performance block storage |
| Ideal For | Big data, OLTP, and any workload benefiting from high-performance block storage |
| Persistence | Non-persistent but survive reboots |
| Data Protection | OCI does not take images, backups, or use RAID or other methods to protect data on NVMe devices |

**Customer Responsibility**

It is the customer's responsibility to protect and manage the durability of data on Local NVMe devices.

**Viewing Block Devices**

You can use the command `lsblk` to display details about block devices. Depending on the compute instance shape, you will see different drives.

Example: For the BM.DenseIO2.52 shape, you will see eight drives: NVMe0 to NVMe7.

---

# 2. Volume Attachment Types

When attaching a block volume to a VM instance, you can choose between iSCSI and paravirtualized attachment types. 

Choose iSCSI when performance is critical, and paravirtualized when simpler configuration is more important.

## iSCSI

iSCSI uses the guest OS storage stack and network hardware virtualization without hypervisor involvement, making it the better choice for high IOPS workloads. 

**iSCSI Attachment**

How It Works

- iSCSI block storage attachment utilizes the internal storage stack in the guest operating system and network hardware virtualization to access block volumes.
- The hypervisor is not involved in the iSCSI attachment process.

**Key Characteristics**

- Uses the guest OS storage stack.
- Uses network hardware virtualization.
- Hypervisor is not part of the data path.
- Recommended when storage IOPS performance is of paramount importance.

---

## Paravirtualized

Paravirtualized attachment uses hypervisor APIs for direct access, simplifying configuration but reducing maximum IOPS for larger block volumes due to virtualization overhead. 

How It Works

- Paravirtualization is a light virtualization technique.
- The VM or guest operating system utilizes hypervisor APIs to access remote storage directly as if it were a local device.
- This is more efficient than full virtualization.

Key Characteristics

- Simplifies block storage configuration.
- Removes the extra commands needed before accessing a volume.
- Due to virtualization overhead, it reduces maximum IOPS performance for larger block volumes compared to iSCSI attachments.

---

# 3. Volume Access Types

OCI Block Volume service offers three access types: Read/Write, Read/Write Shareable, and Read-Only Shareable. 

## Read/Write

Read/Write is the default and attaches to a single instance. 

Read/Write Access Type

- Configures the volume attachment as read/write.
- Not shareable with other instances.
- Enables attachment to a single instance.
- This is the default configuration.

---

## Read/Write Shareable

Read/Write Shareable and Read-Only Shareable can attach to up to eight instances. 

When using Read/Write Shareable, you must configure a clustered file system, such as OCFS2, because OCI does not coordinate concurrent write operations to a shared volume. 

- Read/Write Shareable Access Type

   - Configures the volume attachment as read/write that is shareable with other instances.
   - Enables read/write attachments to multiple instances.
   - To prevent data corruption, you must configure a clustered file system on the volume.
   - The Block Volume service does not provide coordination for concurrent write operations to a shared volume.

---


**Read-Only Shareable**

- Read-Only Shareable Access Type
   - Configures the volume attachment as read-only.
   - Enables attachment to multiple instances.
 
**Maximum Attachments**

- You can attach a block volume as read/write shareable or read-only shareable up to a maximum of eight instances.

---

**Comparison of Access Types**

| Access Type | Read/Write? | Shareable? | Max Instances | Requires Clustered File System? |
|---|---|---|---|---|
| Read/Write | Yes | No | 1 | No |
| Read/Write Shareable | Yes | Yes | 8 | Yes |
| Read-Only Shareable | No (Read-only) | Yes | 8 | No |

---

# 4. Boot Volumes

Boot volumes in OCI provide remote boot disks for bare metal and virtual machine instances. 

They are encrypted by default, offer faster performance and lower launch times, and provide higher durability. 

Both backup and clone operations are supported. 

After instance creation, you can change the boot volume performance to balanced or higher performance. 

Default boot volume sizes are 46.6 GB for Linux and 47 GB for Windows, but you can specify any size from 50 GB to 32 TB. 

When terminating an instance, you can preserve the boot volume and its data. 

For troubleshooting, you can detach a boot volume and attach it as a block volume to another instance.

**What Is a Boot Volume?**

Boot volumes provide remote boot disks for your bare metal and virtual machine instances.

Key features:

- Encrypted by default
- Faster performance and lower launch times
- Higher durability for bare metal and virtual machine instances
- Supports both backup and clone operations

---

**Boot Volume Performance**

Once an instance has been created, you can change the volume performance of the boot volume to either:

- Balanced
- Higher performance level

---

**Boot Volume Size Options**

When you launch an instance, you can choose:

- The selected image's default boot volume size, or
- Specify a size up to 32 terabytes

**Default Boot Volume Sizes**

| Operating System | Default Boot Volume Size |
|---|---|
| Linux | 46.6 GB |
| Windows | 47 GB |

**Customizable Size Range**

You can specify any number between 50 GB and 32 TB.

---

**Boot Volume Preservation**

When you terminate the instance, you can preserve the boot volume and its data. This allows you to keep the boot volume for future use or attach it to another instance.

---

**Troubleshooting with Boot Volumes**

From a troubleshooting standpoint, you can:

1. Detach a boot volume from its associated instance.
2. Attach it to an instance as a block volume.

This allows you to access and repair the boot volume without booting the original instance.

---


