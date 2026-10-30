# File Storage ‐ advanced

# 1. Saved

## 1.1 File System Snapshots

File system snapshots in OCI File Storage provide data protection by capturing a consistent, point-in-time view of a file system. 

Snapshots are accessible under the file system’s root directory at `.snapshot/<snapshot_name>`. For example, a snapshot named `22AprilSnapshot` can be accessed from the mount point inside the `.snapshot` directory.

Snapshots are storage-efficient: they initially consume no additional usage because they reference the original data rather than duplicating it. 

This limits usage cost, and data usage is metered only against the differentiated snapshot data. 

If nothing has changed since the last snapshot, the new snapshot does not consume more storage.


---

**What Are File System Snapshots?**

Snapshots are provided for data protection of your file systems.

Key characteristics:
- They are a consistent, point-in-time view of your file systems.
- They are accessible under the root directory of the file system at:

```text
.snapshot/<snapshot_name>
```

---

**Accessing Snapshots**

Step 1: Create a Snapshot  
Using the OCI Console, create a snapshot with a descriptive name.

Example from the transcript:  
- Snapshot name: `22AprilSnapshot`

Step 2: Access the Snapshot from the Mounted Instance  
After the file system is mounted on an instance:  
1. Navigate to the mount point.
2. Locate the `.snapshot` directory inside the mount point.
3. Open the `.snapshot` directory to see the snapshot.

Example directory structure:

```text
<mount_point>/
├── .snapshot/
│   └── 22AprilSnapshot/
└── (other file system contents)
```

---

**Snapshot Storage and Cost**

Initial Storage Usage  

Snapshots initially consume no additional usage in the file system because they reference the original data instead of duplicating it.

Benefit  
- Limits usage cost.
- Data usage is metered against the differentiated snapshot data.

What This Means  
If nothing has changed within the file system since the last snapshot was taken, the new snapshot does not consume more storage.

| Scenario | Additional Storage Consumed |
|---|---|
| Nothing changed since last snapshot | None |
| Some data changed | Only the changed (differentiated) data |

---

**Why Use Snapshots?**

| Benefit | Description |
|---|---|
| Data protection | Provides a point-in-time view for recovery. |
| Consistency | Captures a consistent state of the file system. |
| Cost efficiency | References original data instead of duplicating it. |
| Limited usage cost | Only differentiated data is metered. |

---

## 1.2 File System Cloning

File system cloning in OCI File Storage creates a new file system from a snapshot of an existing file system. 

At creation, the clone’s data is identical to the snapshot. 

After creation, the clone and the original file system evolve independently: changes in one are not reflected in the other. 

Cloning does not replicate or move data; instead, the clone references the parent file system for shared data.

Key concepts include:
- Source snapshot — the blueprint for the clone.
- Parent file system — the original file system whose data is referenced by clones.
- Hydration — indicates metadata is being copied from the source; the clone is usable during this process.
- Clone tree — a group of clones descending from the same root file system.

In the example, a snapshot named `22AprilSnapshot` was cloned to create `clonedfilesystem`, with hydration in progress and both Clone Root and Descendants set to false.

---

**Key Terms**

| Term | Definition |
|---|---|
| Clone | A new file system created based on a snapshot of an existing file system. |
| Source Snapshot | The snapshot used as a blueprint to create a clone. It is a point-in-time reference of a file system. |
| Parent File System | The file system that contains data referenced by one or more clones. |
| Hydration | Indicates whether the clone is currently copying metadata from the source. |
| Clone Tree | A group of clones that descend from the same root file system. |
| Clone Root | Indicates whether a file system is the root of a cloned tree. |
| Descendants | Indicates whether a file system has been cloned. |

> Note: The transcript uses “clone three” and “patent file system.” These are corrected here as clone tree** and parent file system.

---

**What Is a File System Clone?**

A clone is a new file system created based on a snapshot of an existing file system.

Key points:
- If you take snapshots at regular intervals, you can create clones of the file system as it existed at multiple points in its lifetime.
- At the point of creation, the data in the clone is identical to the data in the snapshot.
- After creation, changes in the clone are not included in the original file system.
- Changes to the original file system are not included in the clone.
- Creating a clone does not replicate or move data from the parent file system to the clone.
- Instead, the clone references the parent file system for any data they share.

---

**Example: Creating a Clone**

Step 1: Start with a Snapshot  
A file system snapshot is created with the name:

```text
22AprilSnapshot
```

Step 2: Perform the Clone  
In the OCI Console, select the snapshot and click Clone.

This creates a new file system. In the example, the new file system is named:

```text
clonedfilesystem
```

Step 3: Review Clone Information  
Under the new file system’s information, you can see:

| Property | Example Value / Meaning |
|---|---|
| Source Snapshot | `22AprilSnapshot` — the snapshot used to create the clone. |
| Hydration | In progress — the clone is currently copying metadata from the source. |
| Parent File System | The original file system from which the snapshot was created. |
| Clone Root | Indicates whether this file system is the root of a cloned tree. |
| Descendants | Indicates whether this file system has been cloned. |

In the example:
- The original file system was used to create the snapshot.
- The snapshot was cloned to create `clonedfilesystem`.
- Both Clone Root and Descendants were false.

---

**File System Cloning Concepts**

1. Source Snapshot  
The source snapshot is the snapshot used as a blueprint to create a clone.

- A snapshot is a point-in-time reference of a file system.
- The clone starts with the same data as the snapshot.

2. Parent File System  
The parent file system is the file system that contains data referenced by one or more clones.

- The clone continues to reference the parent file system for any data they share in common.
- This is why cloning does not duplicate all data immediately.

3. Hydration  
Hydration indicates whether the clone is currently copying metadata from the source.

- You can use the clone while hydration is in progress.
- Hydration does not prevent access to the clone.

4. Clone Tree  
A clone tree is a group of clones that descend from the same root file system.

- It helps track the relationship between a root file system and its clones.
- Clone Root and Descendants properties help identify a file system’s position in the clone tree.

---

**How Data Changes Are Handled**

| Action | Effect |
|---|---|
| Create clone from snapshot | Clone data is identical to the snapshot at creation. |
| Change data in the clone | Changes are not included in the original file system. |
| Change data in the original file system | Changes are not included in the clone. |
| Shared data | Clone references the parent file system instead of copying data. |

---

## 1.3 File System Replication

File system replication in OCI File Storage asynchronously copies data updates from a source file system to a target file system. 

The source and target have active and passive roles, and the source remains usable during replication. 

Replication can occur across availability domains within a region or across regions, and a source can replicate to multiple target regions simultaneously.

Key constructs include:
- Source file system
- Target file system — must never have been exported
- Replication resource — attached to the source and controls replication
- Replication target resource — receives and applies data to the target
- Replication snapshot — captures incremental changes
- Replication interval — must be at least 60 minutes

The first transfer is the base copy. Subsequent incremental transfers are delta copies. The delta cycle has four states: Idle, Capturing, Transferring, and Applying. The replication process repeats at the specified interval, automatically deleting and recreating replication snapshots.

Common use cases include disaster recovery, protection from regional outages, data redundancy, and compliance.

---

**1. What Is File System Replication?**

File system replication copies data from a source to a target.

Key points:
- Data updates to the source file system are asynchronously replicated to the target file system.
- The source and target file systems have active and passive roles.
- You can actively use the source file system during replication.
- Replication can be configured across multiple availability domains within a region or across different regions.
- File system replication gives you consistent file system replicas, so applications can use the target file system with confidence in its consistency.
- The underlying replication technology does not rely on block-level replication.
- The source file system can be replicated to multiple target regions simultaneously.

---

**2. Key Replication Constructs**

Source File System  
The file system whose data you want to replicate to another region or availability domain at regular intervals.

Target File System  
The destination for replicated data from the source file system.

> Important: Only a file system that has never been exported can be used as a target file system.

Replication Resource  
The control component of the replication process.

- Contains all necessary configuration information for replication.
- Attached to the source file system.
- Captures updates by creating a replication snapshot.
- Transmits the snapshot to the replication target.

Replication Target Resource  
Resides in the destination region or availability domain.

- Receives the replication snapshot from the replication resource.
- Applies the data to the target file system.
- Automatically created when you create a replication resource.
- The only manual operation you can perform on it is to delete it during a failover procedure.

Replication Snapshot  
Captures incremental changes in the source file system since the last snapshot.

- Managed by the service.

Replication Interval  
The interval in minutes that determines the frequency of data replication.

- Must be greater than or equal to 60 minutes.
- Select an interval that meets your business needs.

---

**3. Replication Relationship: Base Copy and Delta Copy**

A replication relationship is established between the source file system and the target file system. This relationship is represented by the replication resource.

Base Copy  
The initial data transfer from the source to the target file system is called the base copy.

Delta Copy  
When the base copy is complete:
- Periodic system-driven snapshots are taken on the source.
- Incremental data is securely transferred to the target.
- These increments are called delta copies.

The base copy and delta copy happen without any intervention from the user. The frequency of the delta copy is controlled by the replication interval you specify.

---

**4. Delta Cycle**

There are four constructs in a delta cycle:

| State | Meaning |
|---|---|
| Idle | Replication is not capturing or applying data. |
| Capturing | Replication is capturing differentiated data in the source snapshot. |
| Transferring | Replication is both capturing and committing snapshot data. |
| Applying | Replication is committing the snapshot data to the target file system. |

---

**5. Replication Process**

1. In the primary availability domain, there is a source file system that is exported.
2. In the secondary availability domain, there is a target file system that is unexported.
3. Instances perform data updates on the source file system.
4. A replication resource is created on the source site.
5. The replication resource creates a replication snapshot in the source file system.
6. The snapshot is transferred to the replication target resource.
7. The replication target resource writes the new data to the target file system.
8. The replication snapshot remains in both the source and target file systems until the next interval.
9. It is not deleted immediately. At the next interval, the application process automatically deletes the old replication snapshot and creates a new one.
10. This process repeats at the specified interval as long as replication is in effect.

The replication captures the delta of data written to the source file system in a replication snapshot. The data is then transferred from the replication resource to the replication target resource and copied to the target file system.

---

**6. Use Cases**

Disaster Recovery  
Cross-region replication for file storage:
- Provides protection from regional outages.
- Aids in disaster recovery efforts.
- Addresses data redundancy compliance requirements.

Example:   
An Oracle E-Business Suite customer with primary operations in one availability domain in Phoenix can choose to have a backup or recovery site in another availability domain. The transcript uses “Aspen” for this example.

This functionality is critical for customers who need disaster recovery solutions to protect critical business data while adhering to compliance requirements.

---

# 2. Security

## In-transit Encryption

In-transit encryption for OCI File Storage secures data between instances and mounted file systems using TLS. 

It provides end-to-end security and requires TCP port 2051 to be open for encrypted access. 

To enforce encrypted access only, you can disable the standard access ports.

To enable it, install the `oci-fss-utils` package on your instance. This package creates a network namespace, a virtual network interface, and a local NFS endpoint, and it runs the OCI FSS forwarder in the background. The file system is mounted using a special command that initiates encryption. After mounting, the OCI FSS forwarder receives NFS client requests, encrypts them, and sends them to the Mount Target through a TLS tunnel.

The broad setup steps are:
1. Download `oci-fss-utils`.
2. Install it.
3. Use the in-transit encryption command to mount the file system.

---

**What Is In-Transit Encryption?**

In-transit encryption provides a way to secure your data between instances and mounted file systems using Transport Layer Security (TLS) encryption.

Key points:
- It provides end-to-end security.
- It protects data while it is moving between the instance and the mounted file system.
- It uses a TLS tunnel to send encrypted requests to the Mount Target.

---

**Prerequisite: Open TCP Port 2051**

Before using in-transit encryption, you must complete a prerequisite:

- Open rules for TCP port 2051.
- This port is required for encrypted access.

Enforcing Encrypted Access Only  
If you want to enforce encrypted access only, you can disable the standard access ports.

---

**Enabling In-Transit Encryption**

To enable in-transit encryption, install the following package on your instance:

```text
oci-fss-utils
```

Full name:
```text
Oracle Cloud Infrastructure File Storage Service Client Utility
```

What the Package Does  
Once downloaded and installed, the package:

1. Creates a network namespace.
2. Creates a virtual network interface.
3. Provides a local NFS endpoint.
4. Runs a background forwarder process called the OCI FSS forwarder.

---

**How the Encryption Process Works**

The file system is mounted using a special command that initiates encryption.

> Note: The exact mount command syntax is shown in the slide but is not included in the transcript. Refer to OCI documentation for the specific command.

After the file system is mounted:

1. The OCI FSS forwarder connects the local NFS client to the NFS endpoint.
2. The forwarder receives requests from the NFS client.
3. The forwarder encrypts those requests.
4. The forwarder sends them to the Mount Target using a TLS tunnel.

Process Flow

```text
Local NFS Client
       ↓
OCI FSS Forwarder
       ↓ (encrypts requests)
TLS Tunnel
       ↓
Mount Target
       ↓
File System
```

---

**Steps to Set Up In-Transit Encryption**

1. Download the `oci-fss-utils` package.
2. Install the package on your instance.
3. Use the in-transit encryption command to mount the file system.

Before You Begin  
- Open TCP port 2051 for encrypted access.
- Optionally, disable standard access ports to enforce encrypted access only.

---

## File System Security

Oracle Cloud Infrastructure (OCI) File Storage use four layers of access control

Each layer protects file storage at a different level, from OCI-wide permissions to network access, file system exports, and operating system permissions.

OCI File Storage uses four layers of access control:

1. IAM Service — uses policies to control what users can do in OCI.
2. Network Security — controls which IP addresses or CIDR blocks can connect to a Mount Target using VCN security lists and NSGs. It is all-or-nothing.
3. Export Options — applies access control per file system export based on source IP address and bridges network security with NFS/UNIX security.
4. UNIX Security — controls what users can do on the instance, including mounting file systems and reading/writing files.

Together, these layers provide defense in depth for OCI File Storage.

---

**Key Terms**

| Term | Definition |
|---|---|
| IAM | Identity and Access Management. Uses policies to control what users can do in OCI. |
| Network Security | Controls which instance IP addresses or CIDR blocks can connect to a file system. |
| Security List Rules | VCN rules that allow or deny traffic to a Mount Target. |
| Network Security Groups (NSGs) | VCN constructs used to block ports from specific IPs/CIDR blocks. |
| Export Options | Access control applied per file system export based on source IP address. |
| UNIX Security Layer | Controls what users can do on the instance, such as mounting file systems and reading/writing files. |
| Mount Target | The NFS endpoint used by clients to connect to a file system. |

---

**Four Layers of File Storage Access Control**

The OCI File Storage service uses four different layers of access control:

1. IAM Service
2. Network Security
3. Export Options
4. UNIX Security

---

Layer 1: IAM Service

IAM stands for Identity and Access Management.

- Uses policies to control what users can do within OCI.
- Controls actions such as:
  - Creating instances
  - Creating a Virtual Cloud Network (VCN) and its security rules
  - Creating Mount Targets
  - Creating file systems

Purpose: Controls OCI-level permissions and who can manage file storage resources.

---

Layer 2: Network Security

Network Security controls which instance IP addresses or CIDR blocks can connect to a file system.

- Uses VCN security list rules to allow or deny traffic to the Mount Target.
- Since file systems are accessed through Mount Targets, controlling traffic to the Mount Target controls access to associated file systems.
- You can use VCN Network Security Groups (NSGs) and security rules to:
  - Block appropriate ports from specific IP addresses and CIDR blocks.
  - Restrict host access.

> Important: Network security is all-or-nothing.
> - The client either can or cannot access the Mount Target.
> - Therefore, the client either can or cannot access all file systems associated with that Mount Target.

Purpose: Controls network-level access to the Mount Target.

---

Layer 3: Export Options

Export Options apply access control or file system export based on source IP address.

- This layer provides more granular control than network security.
- It applies access rules at the file system export level.
- It bridges the network security layer and the NFS/UNIX security layer.

Purpose: Controls which clients can access which file systems and with what level of access.

---

Layer 4: UNIX Security Layer

The UNIX security layer controls what users can do on the instance.

It governs actions such as:

- Installing applications
- Mounting external file systems
- Creating directories
- Reading and writing files

Purpose: Controls operating-system-level permissions for users on the client instance.

# 3. File System Usage and Metering

OCI File Storage reports metered file system utilization hourly, based on the metered bytes value in the API. Changes may take up to one hour to appear. Use `df` to see the same value as metered bytes, and `du` to see directory hierarchy usage, which can be larger.

Snapshots initially consume no additional usage because they reference original data. Their metered size is included in the file system’s metered bytes. In the example, a 1 GB file system with a snapshot remained at 1 GB until 0.5 GB was overwritten, raising metered bytes to 1.5 GB.

Clones reference the parent file system’s data. The initial clone cost is metadata only. The parent is metered for shared data, while the clone is metered for its metadata and incremental changes. In the example, adding a 2 GB file to the clone caused the clone to be metered for its metadata plus 2 GB of change data.

---

### Metered Utilization Basics

The File Storage service:

- Reports metered file system utilization.
- Updates hourly.
- Gets the metered file system utilization from the metered bytes value in the API.

**Important Timing Note**

When you add or remove files from your file system, it can take the File Storage service up to one hour to report the change in metered size.

---

### Viewing Usage with `df` and `du`

OCI File Storage supports the Network File System (NFS) protocol. From the instance command line, you can use:

- `df` — shows the amount of storage metered for your file system.
- `du` — shows the storage used by a directory hierarchy.

**Key Differences**

| Command | What It Reports | Relationship to Metered Bytes |
|---|---|---|
| `df` | Amount of storage metered for the file system | Reports the same value as metered bytes |
| `du` | Storage used by a directory hierarchy | Can be much larger than the metered bytes value |

**Best Practice**

Use `df` to accurately view the file system size, because it reports the same value as metered bytes.

---

### Snapshot Metered Utilization

A snapshot is a point-in-time view of your file system.

Key points:
- Snapshots initially consume no additional usage in the file system.
- They reference the original data instead of duplicating it.
- This limits usage cost.
- The metered size of a snapshot is included in the reported metered bytes value of the file system it belongs to.

**Snapshot Metering Example**

Step 1: Create a File System
- Create a file system named Testfilesystem.
- Add file1.
- The new file system contains 1 GB, including metadata.
- After the hourly update cycle, the total metered bytes = 1 GB.

Step 2: Create a Snapshot
- Create a snapshot.
- After the hourly update cycle, total metered bytes = 1 GB.
- Reason: There is no differentiated data yet.

Step 3: Overwrite Part of the File
- Overwrite the first 0.5 GB of `file1`.
- Now `Testfilesystem` has a file that is different from the version captured in the snapshot.
- Metered bytes value becomes 1.5 GB.

Why 1.5 GB?

| Source | Amount |
|---|---|
| Snapshot | 1 GB |
| Differentiated data between live file system and snapshot | 0.5 GB |
| Total | 1.5 GB |

---

### Clone Metered Utilization

A clone references the parent file system’s data instead of duplicating it.

Key points:
- The initial cost of a file system clone is based on its metadata.
- A clone’s parent file system is metered for all data shared with descendant clones.
- A clone is metered for:
  - All its metadata
  - Incremental changes made to its data

**Clone Metering Example**

Step 1: Create a Clone
- Create a clone of Testfilesystem named testclone.
- At creation, before any data is entered:
  - Testfilesystem (parent) is metered for its data and metadata.
  - testclone is metered only for its metadata.