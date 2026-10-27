# Object Storage ‐ Advanced

# 1 Bucket

## Object Storage Replication

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
