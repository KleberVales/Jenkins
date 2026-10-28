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

**Block Volume Performance**

- Lower Cost
- Balanced
- Higher Performance
- Ultra High Performance

**Dynamic performance scaling**

- Performance-Based Autotuning
- Detached-Volume Autotuning

## Local NVMe

Local NVMe provides extremely low latency and high performance but is non-persistent and not protected by OCI—customers are responsible for data durability.

Local NVMe devices are found on certain instance shapes like BM.DenseIO2.52 and are best for high-performance workloads such as big data and OLTP.

# 2. Volume Attachment Types

## iSCSI
## Paravirtualized

# 3. Volume Access Types

## Read/Write
## Read/Write Shareable
## Read-Only Shareable

# 4. Boot Volumes
