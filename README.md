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
