# Object Storage ‐ basic

# 1. Object Storage

## 1.1 What is Object Storage?
## 1.2 Object
## 1.3 Bucket


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
