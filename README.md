# Object Storage ‐ basic

# 1. Object Storage

## 1.1 What is Object Storage?
## 1.2 Object
## 1.3 Bucket
## 1.4 Namespace

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

* **Flat Structure:** Files are not saved in actual physical folders, but rather loosely stored within a large container called a **Bucket**. 

* **Virtually Infinite Scalability:** You can start by storing a few megabytes and grow to petabytes of data without needing to resize disks or worry about running out of physical space.

* **High Durability:** Data is automatically replicated across multiple locations to ensure you never lose information.

* **API/HTTP Access:** Files are accessed and managed primarily through web requests, APIs, command line interface (CLI), or SDKs, rather than being mapped as a traditional network drive (like a hard drive connected to the computer).
  

**Data in Object Storage can be:**

Object Storage (Standard): Data is always available for immediate access. You make a request and the data is delivered in milliseconds.

Archive Storage: Designed for cold data that you almost never access. Because of this, the files are in a "frozen" state. To read a file from the Archive, you first need to request its restoration.

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

## 1.4 Namespace

