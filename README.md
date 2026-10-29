# Block Storage ‐ advanced

# 1. Block volume

## 1.1 Resizing a Volume

Resizing a block volume in OCI allows you to increase storage capacity, but you cannot decrease it. 

Online resizing is the preferred method, allowing you to expand a volume without detaching it from an instance. 

If cross-region replication is enabled, you must disable it before resizing and re-enable it afterward. 

After resizing, you must extend the partition, and if the volume is attached, you must also run rescan commands. 

Offline options include expanding a detached volume, restoring from a backup to a larger volume, or cloning to a new, larger volume. Remember that the first backup after a resize is a full backup, and the key rule is that volume size can only increase.

---

**Volume Size Can Only Increase**

The most important rule to remember:

- You can only increase the size of a volume.
- You cannot decrease the size.

Example: If a volume is 1,024 GB, you can increase it to up to 32 TB, but you cannot decrease it to 512 GB.

---

**Online Resizing**

What Is Online Resizing?

Online resizing allows you to expand an existing volume in place without detaching the volume from an instance.

Key Characteristics
| Feature | Description |
|---|---|
| Detachment Required | No |
| Size Change | Increase only |
| Recommended | Yes, preferred way to avoid impact on applications or workloads |
| First Backup After Resize | Full backup |

Cross-Region Replication Consideration

If cross-region replication is enabled for the volume you want to resize:

1. Disable cross-region replication before resizing.
2. Resize the volume.
3. Re-enable cross-region replication after resizing.

Post-Resize Steps

After the resize operation:

- You must extend the partition for the resize to take effect.
- If the volume is attached to an instance, you must also run the applicable rescan commands in addition to extending the partition manually.
- If the volume is not attached to any instance, you only need to extend the partition.

---

**Offline Resizing Options**

There are three offline options to increase volume size:

1. Expand an Existing Detached Volume
- Detach the volume from the instance before expanding.
- Resize the volume.
- Reattach the volume.
- Extend the partition.
- No rescan needed because the volume was detached.

2. Restore from a Volume Backup to a Larger Volume
- While restoring a backup, you can also increase the size of the volume.

3. Clone an Existing Volume to a New, Larger Volume
- Clone an existing volume to a new, larger volume.
- Backups and clones are covered in upcoming lessons.

---

## 1.2 Save a copy

### 1.2.1 Block Volume Backups

Block Volume Backups provide point-in-time snapshots of volume data. 

Backups are encrypted and stored in OCI Object Storage, and they can be restored as new volumes to any availability domain within the same region. 

You can copy backups between regions for broader disaster recovery. 

Backups can be initiated manually or through policies. 

Oracle-defined policies include Bronze, Silver, and Gold, while user-defined policies let you customize schedules and retention. 

Backup types are incremental and full, with the first incremental backup effectively acting as a full backup. 

You can back up volumes whether they are attached or detached, and after a resize, the first backup is always a full backup.

**What Is a Block Volume Backup?**

Using Block Volume Backups, you can make a point-in-time snapshot of the data on a Block Volume.

Key characteristics:

- Backups are encrypted.
- Backups are stored in OCI Object Storage.
- Backups can be restored as new volumes to any availability domain within the same region.
- Provides disaster recovery benefits within the same region.
- When a volume backup is created, the source volume stacks are automatically included in the volume backup.

---

**Backup and Restore Architecture**

Example scenario:

1. A Block Volume is created in Availability Domain 1.
2. It is attached to an instance running in the same availability domain.
3. A backup is taken and stored in Object Storage.
4. At restoration time, the backup can be restored to another availability domain, such as Availability Domain 2.
5. The restored volume can be attached to an instance running in Availability Domain 2.

---

**Additional Backup and Restore Considerations**

| Consideration | Description |
|---|---|
| Cross-Region Copy | You can copy block volume backups between regions. This helps rebuild applications and data in the destination region if a region-wide disaster occurs in the source region. |
| Resized Volumes | After a volume has been resized, the first backup on the resized volume is a full backup. |
| Attached or Detached | You can back up a volume when it is attached to an instance or while it is detached. |

---

**Ways to Initiate a Backup**

1. Manual Backup
- Think of it as an on-demand, one-off backup.
- You specify whether an incremental or full backup should be performed.

2. Policy-Based Backup
- A policy defines a set backup schedule.
- There are two kinds of backup policies:
  - Oracle-defined
  - User-defined

---

**2.5 Backup Policies**

Oracle-Defined Policies

Oracle-defined policies are predefined backup policies with a set backup frequency and retention period. You cannot modify these policies.

There are three Oracle-defined backup policies:

| Policy | Backups Included | Retention |
|---|---|---|
| Bronze | Monthly incremental backups on the first day of the month. | 12 months |
| Bronze | Yearly incremental backup during the first part of January. | 5 years |
| Silver | In addition to Bronze: weekly incremental backups on Sunday. | Retained for weeks |
| Gold | In addition to Silver and Bronze: daily incremental backups. | 7 days, plus weekly, monthly, and yearly backups |

User-Defined Policies

User-defined policies are custom backup policies that you can create.

A user-defined backup policy has two parts:

1. The backup policy itself
2. One or more schedules in the policy

Schedules define:

- Backup frequency: daily, weekly, monthly, or yearly.
- Retention period: amount of time to keep the backup in days, weeks, months, or years.

You can also enable scheduled cross-region automated backups using user-defined policies.

---

**Volume Backup Types**

| Backup Type | Description |
|---|---|
| Incremental | Includes only the changes since the last backup. |
| Full | Includes all the changes since the volume was created. |

### 1.2.2 Block Volume Clones

Block Volume Clones provide a fast way to create a copy of an existing block volume without using the backup and restore process. 

A clone is a point-in-time, direct disk-to-disk deep copy of the source volume. 

The clone operation occurs immediately, and the volume becomes available quickly, though data is copied in the background and can take up to 30 minutes. 

Clones must be created within the same region, Availability Domain, and tenant. 

If the source volume is attached, you can only create one clone at a time; if detached, you can create up to 10 clones simultaneously. 

You can clone both volumes and volume groups. Remember that subsequent changes to the source volume are not copied to the clone.

**What Is a Block Volume Clone?**

Cloning enables you to make a copy of an existing block volume without needing to go through the backup and restore process.

Key characteristics:

- A cloned volume is a point-in-time, direct disk-to-disk deep copy of the source volume.
- All data in the source volume at the time the clone is created is copied to the cloned volume.
- Subsequent changes to the data on the source volume are not copied to the clone.

---

**Clone Operation and Availability**

| Aspect | Description |
|---|---|
| Operation Timing | The clone operation occurs immediately. |
| Usability | You can attach and use the cloned volume as a regular volume as soon as the state changes to available. |
| Background Copy | Volume data is copied in the background. |
| Copy Duration | Can take up to 30 minutes, depending on the size of the volume. |

---

**Clone Scope and Location**
You can only create a clone for a volume within the:

- Same region
- Same Availability Domain
- Same tenant

The clone is created in the same Availability Domain as the original volume.

Example: If you have a block volume created in Availability Domain 81 in the Phoenix region, and you create a clone, the clone is created in the same Availability Domain 81.

---

**Clone Limits Based on Source Volume State**

| Source Volume State | Clone Limit |
|---|---|
| Attached | You can only create one clone at a time. You must wait for the first clone operation to complete before creating additional clones. |
| Detached | You can create up to 10 clones from the same source volume simultaneously. |

---

**What Can Be Cloned?**
You can create clones of:

- Volumes
- Volume groups

---

### 1.2.3 Block Volume: Cross-Region Replication

Cross-region replication provides ongoing asynchronous replication of boot and block volumes to other regions. 

It is complementary to backup, not a replacement. 

Replication starts with an initial sync, then continues continuously without downtime. 

You cannot resize a volume while replication is enabled, and replication is not supported for volumes using customer-managed vault encryption keys. 

You can create a new volume by activating a replica. 

Costs include replica storage in the destination region and network costs, with the replica billed at the lower cost block storage price. 

Use cases include disaster recovery, migration, and business expansion. 

To enable replication, ensure you are subscribed to the target region, then edit the volume and select the destination region, availability domain, and replica name.

**What Is Cross-Region Replication?**

Using cross-region replication, you can perform ongoing automatic asynchronous replication of block volumes to other regions.

Key points:

- Supports both boot volumes and block volumes.
- Replication is asynchronous.
- Cross-region replication is complementary to backup, not a replacement.
  - Backups give you a point-in-time snapshot of volumes.
  - Replicas give you the current version of the data.
- Cross-region replication does not cause downtime or impact on source volumes.

---

**How Replication Works**

When you enable replication for a volume:

1. An initial sync of data occurs from the source to the replica.
   - Depending on volume size and amount of data written, the sync can take hours.
2. After the initial synchronization completes, replication becomes continuous.

---

**Resizing a Volume with Cross-Region Replication**

You cannot resize a volume while cross-region replication is enabled.

To resize a volume:

1. Disable cross-region replication.
   - This deletes the volume replica.
2. Resize the volume.
3. Re-enable cross-region replication.
   - Replication starts again from scratch.

---

**Encryption Limitation**

Cross-region replication is not supported for volumes encrypted using customer-managed vault encryption keys.

---

**Creating a Volume from a Replica**

To create a new volume from a volume replica, you must activate the replica.

- The activation process creates a new volume by cloning the replica.

---

**Cost Considerations**

From a cost standpoint, your bill includes:

- Storage costs for the volume replica in the destination region.
  - The replica is billed using the block storage lower cost option price, regardless of the volume type in the source region.
- Any applicable network costs for the replication process between regions.

---

**Destination Region Selection**

When you enable replication for a volume, you select a destination region.

- The source region determines which target regions are available as destination regions.

Examples:

| Source Region | Available Destination Regions |
|---|---|
| UK West | UK South or London |
| US West / Phoenix | Ashburn and San Jose |

---

**Use Cases for Cross-Region Replication**

Cross-region replication can help with:

- Disaster recovery if the primary region is unavailable.
- Migration scenarios.
- Business expansion scenarios.

---

**How to Enable Cross-Region Replication**

Steps to enable:

1. Ensure you are subscribed to the target region available as a possible destination region.
2. Edit the volume properties.
3. Enable cross-region replication.
4. Select the region and, if applicable, the availability domain.
5. Provide a name for your volume replica.

### 1.2.4 Block Volume: Cross Availability Domain Replication

Cross availability domain replication allows you to replicate block volumes, boot volumes, and volume groups to another availability domain within the same region. Key points to remember:

- No network costs (unlike cross-region replication).
- Storage costs apply for the replica in the destination region.
- The replica is billed at the block storage lower cost option.
- You cannot resize a volume while replication is enabled.
- Disabling replication deletes the replica; re-enabling starts replication from scratch.
- Not supported for volumes encrypted with customer-managed vault keys.
- Activate the replica to create a new volume.
- No downtime or impact on source volumes.
- To enable, select the same region and a different availability domain, then provide a name for the replica.

This feature is ideal for disaster recovery and high availability within a single region without incurring network costs.

**Learning Objectives**

By the end of this lesson, you should be able to:

- Explain what cross availability domain replication is.
- Identify the volume types that can be replicated.
- Describe cost considerations for cross availability domain replication.
- Explain limitations related to resizing and encryption.
- Describe how to activate a replica to create a new volume.
- Explain how to enable cross availability domain replication.

---

**What Is Cross Availability Domain Replication?**

Using cross availability domain replication, you can perform replication of:

- Block volumes
- Boot volumes
- Volume groups

The replication target is another availability domain within the same region.

Key characteristics:

- Cross availability domain replication does not cause any downtime or impact on source volumes.
- It is not supported for volumes encrypted using customer-managed vault encryption keys.

---

**Cost Considerations**

| Cost Component | Cross Availability Domain Replication | Cross Region Replication |
|---|---|---|
| Storage Costs for Replica | Yes (in destination region) | Yes (in destination region) |
| Network Costs | No network costs | Applicable network costs apply |
| Replica Billing | Block storage lower cost option, regardless of source volume type | Block storage lower cost option, regardless of source volume type |

Important consideration: Unlike cross-region replication, cross availability domain replication incurs no network costs.

---

**Resizing a Volume with Cross Availability Domain Replication**

You cannot resize a volume while cross availability domain replication is enabled.

To resize a volume:

1. Disable cross availability domain replication.
   - This deletes the volume replica.
2. Resize the volume.
3. Re-enable cross availability domain replication.
   - Replication starts again from scratch.

---

**Encryption Limitation**
Cross availability domain replication is not supported for volumes encrypted using customer-managed vault encryption keys.

---

**Creating a Volume from a Replica**
To create a new volume from a volume replica, you must activate the replica.

- The activation process creates a new volume by cloning the replica.

---

**How to Enable Cross Availability Domain Replication**

Steps
1. Edit the volume properties.
2. Enable replication.
3. Select a destination region.
   - Important: For cross availability domain replication, the destination region is the same as the source region.
4. Select another availability domain to which the block volume will be replicated.
5. Provide a name for your volume replica.

Example

In the screenshot example:

- The block volume is being created in the Ashburn region.
- The destination region is also Ashburn (same region).
- You select a different availability domain as the destination.
- You provide a name for the volume replica.

Key difference from cross-region replication: The region name is the same (Ashburn to Ashburn), and you simply select another availability domain.


## 1.3 Volume Groups

Volume groups in OCI allow you to group multiple volumes—both boot and block volumes—into a single logical unit. 

You can create point-in-time, crash-consistent backups and clones of the entire group. 

A volume group can contain up to 32 volumes with a maximum combined size of 128 TB. 

Each volume can only belong to one volume group. 

Deleting a volume group does not delete its volumes, and you must remove a volume from a group before deleting it. 

Volume groups simplify backing up multi-tier enterprise applications and support scheduled, policy-based backups with full group restore capability. 

Best of all, volume groups are available at no additional charge.


**What Is a Volume Group?**

OCI Block Volume service provides the capability to group together multiple volumes in a volume group.

A volume group can include both types of volumes:

- Boot volumes – the system disks for your compute instances.
- Block volumes – for your data storage.

You can use volume groups to create volume group backups and clones that are point-in-time and crash-consistent.

---

**Volume Group Limits**

| Limit | Value |
|---|---|
| Maximum volumes per volume group | 32 |
| Maximum combined size | 128 TB |
| Volume size variation | Volumes can vary in size |
| Combined size constraint | The overall combined size of all block and boot volumes must be 128 TB or less |

---

**Volume Group Membership Rules**

Important rules to remember:

- Each volume may only be in one volume group.
- When you delete a volume group:
  - The individual volumes in the group are not deleted.
  - Only the volume group is deleted.
- When you delete a volume that is part of a volume group:
  - You must first remove it from the volume group before you can delete it.

---

**Use Case: Enterprise Application**

Consider an enterprise application that spans multiple storage volumes across multiple instances:

| Tier | Storage |
|---|---|
| Web Tier | Block Volume |
| Application Tier | Block Volume |
| Database Tier | Block Volume |

Volume groups can simplify the process to create time-consistent backups of running enterprise applications.

Additional Capabilities

- Perform volume group backups automatically on a schedule.
- Retain backups based on a particular backup policy.
- Restore an entire group of volumes from a volume group backup.

Cost

Volume groups are available at no additional charge.

---


