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

## File System Cloning
## File System Replication

# 2. Security

## In-transit Encryption
## File System Security

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

Step 2: Add Data to the Clone
- Create a 2 GB file in `testclone`.
- Now:
  - Testfilesystem (parent) is metered for the data it shares with testclone.
  - testclone is metered for its metadata plus the 2 GB of change data incurred by the new file creation.

Clone Metering Summary

| File System | Metered For |
|---|---|
| Parent (Testfilesystem) | Data and metadata; data shared with descendant clones |
| Clone (testclone) | Metadata; incremental changes to its data |

