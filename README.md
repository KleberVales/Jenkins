# Object Storage ‐ Advanced

# 1. Bucket

## 1.1 Object Storage Replication

**Introduction**

Replication allows you to automatically copy objects from one bucket (source) to another bucket (destination), which can be in the same region or a different region.

What Is Object Storage Replication?

Using a replication policy, you can replicate objects in one bucket to another bucket in the same or different region.

Key behaviors:

- After the replication policy is created, the destination bucket becomes read-only and is updated only by replication from the source bucket
- Objects uploaded to a source bucket after policy creation are asynchronously replicated to the destination bucket
- Objects uploaded to the source bucket before policy creation are not replicated
- Objects deleted from the source bucket after policy creation are automatically deleted from the destination bucket

Example: As shown in the slide, on the left-hand side we have the OCI Mumbai region, and on the right, we have the Hyderabad region. There are two buckets in the Mumbai region. Using a replication policy, you can replicate the objects in these buckets to either another bucket in the same region or to a target bucket in a different region.

---

**Benefits of Object Storage Replication**

Let's talk about the benefits of performing object storage replication:

1. Disaster recovery – Aids in disaster recovery efforts
2. Data residency and compliance – Addresses data residency and compliance requirements
3. Protection from regional outages – Your data is copied to another region
4. Reduced latency – Maintaining multiple copies of the data in regional locations closer to user access can help reduce latency

---

**Creating a Replication Policy**

To create a replication policy, you need to provide:

- A name
- Select a destination region
- Specify a destination bucket

Important notes:

- This replication policy does not automatically create the destination bucket
- There is a small note that says replication is going to overwrite any objects in the destination bucket that have the same name as an object in the source bucket
- Another key thing to remember: objects uploaded to the source bucket before policy creation are not replicated

---

**Limitations and Other Considerations**

Let's talk about some limitations and other considerations:

- Destination bucket must be created first – Replication policy creation does not automatically create a destination bucket; you must create the destination bucket before creating the replication policy on the source bucket
- Storage tier support – A source or destination bucket can be in the Standard or the Archive storage tier
- Policy limits:
  - Maximum of one replication policy per source bucket
  - Maximum of one source for each replication destination bucket
  - Maximum of one destination for each replication source bucket
- No chain replication – A destination bucket cannot also be the replication source; chain replication is not supported
- Destination bucket becomes read-only – After you create the replication policy, the destination bucket becomes read-only, meaning you cannot upload objects to that bucket directly. The only option is to upload objects to the source bucket, which then get replicated
- Cannot delete destination bucket directly – You cannot delete a replication destination bucket unless you stop replication and make the bucket writable again

Two ways to stop replication:

1. Delete the replication policy from the source
2. Make the destination bucket writable by stopping the replication in the destination

---

## 1.2 Object Versioning

**Introduction**

This feature allows object storage systems to automatically create a new version of an object every time a change occurs (upload, overwrite, or deletion).

---

What is Object Versioning?

Versioning directs object storage to automatically create an object version whenever:
- A new object is uploaded.
- An existing object is overwritten.
- An object is deleted.

Key Rules
- Bucket Level: Versioning is enabled at the bucket level.
- Timing: You can enable it at bucket creation or later.
- Cannot Disable: You cannot disable versioning once enabled; you can only suspend it.

Data Protection & Costs
- Purpose: Protects against accidental or malicious updates, overwrites, or deletions.
- Version States: There is always one latest version and zero or more previous versions.
- Billing: You are charged for all versions (latest and previous).
- Retention: Previous versions are retained until you explicitly delete them.

---

**Object Versioning Statuses**

There are three distinct states of versioning:

A. Disabled
- Overwrite Behavior: Uploading an object with the same name overwrites the existing object. The old object is not retained or recoverable.
- Delete Behavior: Deletion is permanent. Objects cannot be recovered.

B. Enabled
- Overwrite Behavior: Uploading an object with the same name creates a new latest version. The existing object becomes a previous version.
- Delete Behavior: When you delete an object, object storage retains a version of the deleted object.
- Identifiers: Each uploaded object is assigned a unique version identifier.

C. Suspended
- Upload/Delete Behavior: Same as a bucket with versioning Disabled.
- Retention: Object versions created *before* suspension are retained (unless explicitly deleted).
- Re-enabling: You can re-enable object versioning at any time.

---

## 1.3 Retention Rules

**Introduction**

These rules are designed to protect your data and provide immutable, WORM-compliant (Write Once, Read Many) storage options.

What are Retention Rules?

Retention rules protect data from accidental or malicious update, overwrite, or deletion. They provide immutable WORM-compliant storage options for data returned to object storage and archive storage.

Key Rules
- Locking: Retention rules can be locked to prevent rule modification and data deletion/modification, even by administrators.
- Configuration Level: Retention rules are configured at the bucket level.

Two Types of Retention Rules

| Type | Description |
| :--- | :--- |
| Time-Bound | You specify a duration during rule creation. Object modification and deletion are prevented for that duration. |
| Indefinite | Object modification and deletion are prevented until you delete the rule. No duration is specified. |

---

**Configuring Retention Rules**

Time-Bound Configuration  
When selecting Time-Bound, you must specify:
- Retention Time Amount
- Retention Time Unit

You can optionally enable Retention Rule Lock.

Indefinite Configuration  
When selecting Indefinite, you do not specify a Retention Time Amount.

Retention Rule Lock
- Irreversible Operation: Not even a tenancy administrator can delete a locked rule.
- 14-Day Delay: There is a mandatory 14-day delay before a rule is locked. This allows you to test, modify, or delete the rule or rule lock before permanent locking.
- Deletion: Once locked, the rule can only be deleted by deleting the bucket.

---

**Retention Duration for Time-Bound Rules**

Even though retention rules are created at the bucket level, the duration is applied to each object individually based on the object's last modified timestamp.

Example Scenario

Bucket: OCI  
Objects:
- `OCI test` — Last modified 14 months ago
- `OCI demo` — Last modified 3 months ago

Retention Rule: Time-bound, duration of 1 year

| Object | Last Modified | Retention Duration | Outcome |
| :--- | :--- | :--- | :--- |
| `OCI test` | 14 months ago | 1 year (12 months) | Allowed for modification/deletion (14 months > 12 months) |
| `OCI demo` | 3 months ago | 1 year (12 months) | Prevented for next 9 months (12 - 3 = 9 months remaining) |

Important Notes  
- You can create multiple retention rules for a bucket.
- Indefinite retention rules are applied before any time-bound rule is considered.

---

## 1.4 Object Copy

**Introduction**

This feature allows you to copy objects between buckets and regions, with flexible rules to control overwriting behavior.

What is Object Copy?

Object copy allows you to copy objects to other buckets in the same region or to buckets in other regions.

Key Characteristics
- Asynchronous Handling: The object storage service handles copy requests asynchronously.
- Archive Storage Limitation: Objects cannot be copied directly from archive storage.
- Bucket Creation: The copy operation does not automatically create buckets.
- New ETag: When an object is copied, the destination object receives a new ETag value.

---

**Overwrite Rules**

There are four different overwrite rules available for copy operations:

| # | Overwrite Rule | Description | When to Use |
| :--- | :--- | :--- | :--- |
| 1 | Override Destination Object | Default option. Can be used for any copy operation. | When you do not want to limit the copy operation by ETag value. |
| 2 | Do Not Overwrite Any Destination Object | Prevents overwriting of an existing copy of an object in the destination location, regardless of the destination object's ETag value. | To prevent overwriting of an existing object in the destination. |
| 3 | Override Destination Object Only If It Matches the Specified ETag | Copy succeeds only if the ETag you supply matches the ETag of the destination object. | To prevent accidental overwriting of a destination object that does not have the specified ETag. |
| 4 | Copy Object Only If the Source Matches the Specified ETag | Copy succeeds only if the ETag you supply matches the ETag of the source object. | To ensure that only the specified version of the object is copied. |

---

**Copy Operation Parameters**

When performing a copy operation, you need to provide the following:

Required Parameters
- Destination Namespace
- Destination Region
- Destination Bucket

Optional Parameters
- Destination Object Name
- Destination Storage Tier

Overwrite Rule Selection  
You can select any one of the four overwrite rules based on your requirement before performing the copy operation.


