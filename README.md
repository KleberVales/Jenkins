# Object Storage ‐ basic

# 1. Object Storage

## 1.1 What is Object Storage?
## 1.2 Object
## 1.3 Bucket
## 1.4 Namespace
## 1.5 Object Storage Tiers
## 1.6 Object Lifecycle Management

# 2. Multipart Uploads

## 2.1 Introdution
## 2.2 Recommendations and Limits
## 2.3 Object Lifecycle Policy Rules
## 2.4 Uploads


---

.\
.\
.

---

# 1. Object Storage

## 1.1 What is Object Storage?

It stores objects in an unstructured way. Within the bucket, the objects have no hierarchy.

Object Storage is a cloud storage service designed to store large amounts of unstructured data (such as text files, images, videos, backups, system logs, and audio files).

Unlike a traditional hard drive (which organizes files into folders and subfolders hierarchically), Object Storage treats each file as an autonomous "object".

* **Flat Structure:** Files are not saved in actual physical folders, but rather loosely stored within a large container called a Bucket. 

* **Virtually Infinite Scalability:** You can start by storing a few megabytes and grow to petabytes of data without needing to resize disks or worry about running out of physical space.

* **High Durability:** Data is automatically replicated across multiple locations to ensure you never lose information.

* **API/HTTP Access:** Files are accessed and managed primarily through web requests, APIs, command line interface (CLI), or SDKs, rather than being mapped as a traditional network drive (like a hard drive connected to the computer).

We learned that object storage provides strong consistency by always serving the most recent copy of data on read requests. 

It offers eleven 9s durability by storing objects redundantly across three availability domains (or three fault domains in single-AD regions). 

Data integrity is actively monitored using checksums, with corrupt data automatically detected and repaired without customer intervention. 

From a security perspective, all data is encrypted by default, tightly integrated with OCI IAM, and transferred over HTTPS via SSL endpoints. 

Object storage is highly scalable, supporting unlimited data, thousands of buckets per account, unlimited objects per bucket, and object sizes ranging from 0 bytes to 10 TiB.
  
**Key features include:**

- Unlimited data storage – You can store an unlimited amount of data
- Thousands of buckets per account – Each bucket can host an unlimited number of objects
- Object size flexibility – Objects can range from 0 bytes to 10 TiB
- Designed for unstructured data – Ideal for images, videos, and analytic data
- Regional service – Data can be accessed from anywhere using object storage endpoints
- Private access support – Supports private access from OCI resources in a VCN through a Service Gateway, allowing connection to public endpoints using private IP addresses in a private subnet
- Cost and performance flexibility – Offers multiple storage tiers:
  - Standard
  - Infrequent Access
  - Archive

---

## 1.2 Object

Each stored object consists of three main elements:

1. The Data: The file itself (e.g., a photo, a PDF document, or a compressed backup).

2. Metadata: A set of information that describes the object (such as creation date, size, file type, and custom tags).

3. A Unique Identifier (Key): A unique address or name that allows you to find and retrieve that file instantly.

## 1.3 Bucket

**Naming and Structural Rules**

- Names are case sensitive
- You cannot nest buckets – a bucket cannot contain other buckets
- You can store objects up to 10 tebibytes (TiB)

**Pre-Authenticated Requests**

There is a concept of pre-authenticated requests (PARs) that provide a way to let you access a bucket or an object without having your own credentials.

- Useful for granting temporary access to specific resources

---

**Retention Rules**

You can apply retention rules at the bucket level to provide immutable object storage options for data that is written to object storage.

Use cases for retention rules:

- Governance
- Regulatory compliance
- Legal requirements

---

**Replication Policy**

Using a replication policy for a bucket, you can automatically replicate the objects in one object storage bucket to another bucket in the same or different region.


---

## 1.4 Namespace

The first construct is the namespace.

- Think of it as a top-level container for all buckets and objects
- At account creation time, each OCI tenant is assigned one unique object storage namespace

---

**Objects and Metadata**

The data you store in object storage is stored as an object along with its metadata.

- Objects are the actual data items you upload and retrieve
- Each object carries metadata that describes it

---

**Buckets**

Next, we have the bucket, which is a logical container for storing objects.

Key rules about bucket naming:

- The name for a bucket must be unique in a region within each tenancy
- Example: You can create a bucket named `demo-bucket` in the India West (Mumbai) region
  - You cannot create another bucket with the same name `demo-bucket` in the Mumbai region
  - But you can create a bucket with the same name `demo-bucket` in the India South (Hyderabad) region

Important note: Within an object storage namespace, buckets and objects exist in a flat structure (flat hierarchy).

---

**Object Storage Endpoint**

Now, let's see how the object storage endpoint looks.

In the example:

- Object name: `oci.jpg`
- Bucket name: `training`
- Namespace name: `ociarchassociate`

The endpoint is constructed as follows:

- The object name comes after `/o`
- The bucket name comes after `/b`
- The namespace name comes after `/n`
- You can also see the service name (object storage) and the region name in the endpoint

---

## 1.5 Object Storage Tiers

**Purpose of Storage Tiers**

Storage tiers help you:

- Maximize access performance where appropriate
- Minimize storage costs

---

**Bucket Creation and Storage Tiers**

There are two options at bucket creation level with respect to storage tiers:

1. Standard storage tier bucket – Can contain a mix of objects with different storage tier assignments
2. Archive storage tier bucket – Can only contain objects with an archive storage tier assignment

Important notes:

- When creating a bucket, you cannot change the default storage tier for the bucket
- You specify the default storage tier during bucket creation (either Standard or Archive)

---

**Object-Level Storage Tier Assignment**

If the object is being uploaded to a standard default storage tier bucket, you can explicitly assign any storage tier to the object:

- Standard
- Infrequent Access
- Archive

Note: You can also change the storage tier for an object, but this is only possible in the case of a standard storage tier bucket.

---

**How Storage Tiers Affect Cost**

Every object uploaded to object storage is assigned to a storage tier, which determines:

- Its storage cost
- Associated retrieval fees

There are three storage tiers: Standard, Infrequent Access, and Archive.

---

**Standard Tier**

- Default storage tier
- Used for data you need to access quickly, immediately, and frequently
- Has higher storage cost compared to other tiers
- When you choose a standard default storage tier during bucket creation, you can also enable auto-tiering

Use cases:

- Content repository for scalable data (images, logs, videos)
- Data repository for Hadoop or big data workloads

---

**Infrequent Access Tier (Cool Tier)**

- Used for data that you access infrequently, but that must be available immediately when needed
- Storage costs are lower than standard
- Has a minimum storage retention period of 31 days
- You will be charged the prorated cost of storing the data for the full 31 days if you delete or overwrite objects in this tier
- When you need to access objects, you are charged a per GB data retrieval fee

Use cases:

- Backup for on-premises data

---

**Archive Storage Tier**

- Meant for data that is rarely accessed, but must be retained for long periods of time
- Archive storage tier buckets can only contain objects with archive storage tier assignment
- Provides the lowest storage cost
- Objects must be restored before they are available for access
- Minimum storage retention period is 90 days
- When you restore objects, you are returning those objects to the standard tier for access
- Restore time is close to an hour

Use cases:

- Compliance and audit mandates
- Storing historical data

---


# 2. Multipart Uploads

## 2.1 Introdution

Instead of sending a large file all at once (which can easily fail if the connection drops), Multipart Upload splits the file into smaller pieces and sends them in parallel.

Main advantage id if sending a piece fails, you only need to resend the part that failed, not the entire file.

Is Flexibility it's possible to pause the sending of specific parts and resume it later.

Multipart uploads upload data in parallel, making them faster and more efficient than uploading a large object in a single upload.

If a multipart upload fails for any reason, instead of restarting the entire object, you only need to retry uploading the part that failed

## 2.2 Recommendations and Limits

The recommendation is that if the object size is greater than 100 mebibytes (MiB), you should consider using multipart uploads.

Key limits and specifications:

- An object can be divided into a maximum of 10,000 parts
- Each part must be at least 10 MiB in size
- The upper limit of an object part is 50 gibibytes (GiB)
- You have the flexibility to pause uploads of individual parts and resume when schedule and resources allow

---

## 2.3 Object Lifecycle Policy Rules

You can use object lifecycle policy rules to automatically delete any uncommitted or failed multipart upload after a specific number of days.

- This operation can be performed using the OCI Console

---

## 2.4 Uploads

### A. Multipart Uploads Using the API

Let's look at how multipart uploads can be performed using the Multipart Upload API. There are four main steps:

**Step 1: Create the Parts**

Before you initiate an upload, you need to create the parts.

- Example: Object size is around 200 MB, split into 10 parts of 20 MB each

**Step 2: Initiate an Upload**

Make a CreateMultipartUpload REST API call.

- Object storage provides an Upload ID
- The upload is marked as active and remains active until you commit or abort it

**Step 3: Upload Object Parts**

Make an UploadPart request.

- Object storage returns an entity tag (ETag) value for each part uploaded
- When committing the upload, you require both the part number and the corresponding ETag value for each part

**Step 4: Commit the Upload**

Make a CommitMultipartUpload request.

- Object storage reconstructs the object from its constituent parts

These are the steps required to perform a multipart upload using the Multipart Upload API.

---

### B. Multipart Uploads Using the CLI

Now let's look at how to perform multipart uploads using the CLI. There are some key differences compared to using the API:

1. No need to split the object into parts – Just specify the part size and object storage will split the object into parts
2. Set maximum parallel parts – You can choose to set the maximum number of parts to be uploaded in parallel
3. No commit required – You don't have to perform any commit after the upload is complete

**CLI Command Notes**

- You must specify the mandatory part size flag, and the value must be an integer
  - This denotes the size of each part in mebibytes (MiB)
- Optionally, use parallel upload count to set the maximum number of parallel uploads allowed

---



