# File Storage ‐ basic

# 1. File Storage

## Introduction

OCI File Storage is a fully managed, elastic, shared file system compatible with NFS v3.0. It is designed to store files that need to be accessed simultaneously by multiple instances.

You can connect to OCI File Storage from: Bare metal instances, Virtual machine instances, Container instances and Resources inside your Virtual Cloud Network (VCN)

You can also access the file system from outside the VCN using: VCN peering, OCI FastConnect and IPsec VPN connection

Key features:
- Supports Network File System version 3.0 (NFS v3.0).
- Data is replicated for durability within each availability domain.
- Scales automatically to accommodate growth up to 8 exabytes.
- Provisioning is fully managed and automatic.
- Scales from a single byte to exabytes without upfront provisioning.
- Well-suited for applications that require a shared file system.
- Provides a durable, scalable, secure, enterprise-grade network file system.
- Supports the Network Lock Manager (NLM) protocol for file locking.

**Data Protection and Security**

OCI File Storage includes several protection features:

- Snapshots for data protection of your file system.
- Encryption at rest by default for all stored data and snapshot data using AES-256.
- By default, file systems use Oracle-managed encryption keys.
- Customers can optionally use customer-managed keys through OCI Vault.
- In-transit encryption can be configured using TLS to secure data between instances and mounted file systems.

# 2. File System

The file system is the actual storage location where files exist. It is exported or made available through Mount Targets.

# 3. Mount Target

A Mount Target is an NFS endpoint that:
- Lives in a subnet.
- Is highly available.
- Provides the IP address or DNS name used in the mount command.
- Allows NFS clients to connect to a file system.

File systems are exported through Mount Targets. You can reuse the same Mount Target for multiple file systems by specifying an export in the Mount Target for each file system.

# 4. Exports

## Export

The NFS control layer that controls how NFS clients access file systems when they connect to a Mount Target.

## Export Set

A collection of one or more exports maintained by a Mount Target.

## Export Path

A path specified when an export is created. It uniquely identifies the file system within the Mount Target.

The export path is specified when an export is created. It:
- Uniquely identifies the file system within the Mount Target.
- Lets you associate many file systems to a single Mount Target.

## Export Options

Parameters within an export that specify the level of access granted to NFS clients. Up to 100 options are allowed per export.

Export options are a set of parameters within the export. They specify the level of access granted to NFS clients when they connect to a Mount Target. You can have up to 100 options per export.


