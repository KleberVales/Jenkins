# 1. CIDR Blocks/Prefixes 

## What is CIDR?
## CIDR Notation
## Octets and IPv4 Bits
## How Many IP Addresses Are in a CIDR Block?
## Reserved IP Addresses

# 2. Virtual Cloud Network

## What is a VCN?
## Availability Domains

# 3. Subnets

# 4. Route Tables

# 5. Gateway

# 6. Security

---

.\
.\
.\
.\
.

---

# 1. CIDR Blocks/Prefixes

## What is CIDR?

CIDR, or Classless Inter-Domain Routing, is a compact notation used to represent a range of IP addresses.

It is written as an IP address followed by a slash and a prefix length, such as 10.0.0.0/16.

The prefix length tells how many leading bits are fixed, while the remaining bits can vary to create the address range.

Since IPv4 addresses have 32 bits and each octet has 8 bits, the total number of addresses in a CIDR block is calculated with 2^(32 − prefix length).

## CIDR Notation

A.B.C.D/x

- A.B.C.D is an IP address.
- / is the slash character.
- x is a decimal number called the prefix length.
  
For example, instead of writing:

**10.0.0.0 to 10.0.255.255**

we can write:

10.0.0.0/16

## Octets and IPv4 Bits

An **octet** is a group of 8 bits. IPv4 addresses have four octets:

**10 . 0 . 0 . 0**

Each octet contains 8 bits, so IPv4 has a total of:

**4 × 8 = 32 bits**

In the example **10.0.0.0/16**, the first two octets are fixed because the prefix length is 16. That means the first 16 bits are the network portion, and the remaining 16 bits are available for hosts. So the range becomes:

**10.0.0.0 – 10.0.255.255**

## How Many IP Addresses Are in a CIDR Block?

To calculate the total number of IP addresses in a CIDR block, use this formula:

**2^(32 − prefix length)**

The **32** comes from the total number of bits in an IPv4 address. The **prefix length** is the number after the slash.

For **10.0.0.0/16**:

**2^(32 − 16) = 2^16 = 65,536 addresses**

So a `/16` block contains 65,536 total IP addresses. In practical networking, the first and last addresses may be reserved for the network address and broadcast address, but the total block size is still 2^16.

Another example: a `/24` block has:

**2^(32 − 24) = 2^8 = 256 addresses**

## Subnetting

Subnetting is the process of breaking a larger network into smaller networks. Using subnetting, a larger CIDR prefix can be divided into smaller CIDR prefixes.

For example, a larger network can be split into multiple smaller subnets to organize devices, improve routing, or separate departments.

We will explore subnetting in more detail in upcoming sections.

### Reserved IP Addresses

Within any CIDR block, some IP addresses are reserved and cannot be assigned to hosts. The reserved addresses are:

1. The **first IP address** — network address
2. The **second IP address** — subnet default gateway address
3. The **last IP address** — broadcast address

#### Example: 192.168.0.0/24

Consider the CIDR block:

**192.168.0.0/24**

The reserved IP addresses are:

| Address | Purpose |
|---|---|
| 192.168.0.0 | Network address |
| 192.168.0.1 | Subnet default gateway |
| 192.168.0.255 | Broadcast address |

These three IPs cannot be allocated to hosts.

# 2. Virtual Cloud Network

## What is a VCN?

A Virtual Cloud Network,  is a software-defined private network that you create inside an Oracle Cloud Infrastructure region. 

VCN acts like a private, isolated network for your cloud resources. 

When you create a VCN, you must attach at least one CIDR block, and you can have up to five IPv4 CIDR blocks and up to five IPv6 CIDR blocks, as long as they do not overlap. 

VCN CIDR sizes can range from `/16` to `/30`, and CIDR blocks can be resized after creation. 

Inside any CIDR, the first IP is the network address, the second IP is the subnet default gateway, and the last IP is the broadcast address; these are reserved and cannot be assigned to hosts. 

A VCN is regional, meaning it resides in one region, but it can span multiple availability domains within that region—it cannot span multiple regions. 

For IPv6, you can use an Oracle-allocated `/56` prefix or bring your own IPv6 prefix. Understanding VCNs is essential for designing secure and scalable OCI network architectures.

## Availability Domains

An OCI region can have:

- One availability domain, or
- Three availability domains

A VCN is regional, so it resides in a single region. However, if the region has multiple availability domains, the VCN can span across them. This allows resources in different ADs to be part of the same VCN.

Remember:

- **VCN can span multiple ADs within one region.**
- **VCN cannot span multiple regions.**

# 3. Subnets

A VCN can be divided into subnets, each represented in CIDR notation, and that a larger CIDR number means a smaller network. 

Subnet IP address ranges within a VCN must not overlap—if one CIDR is part of another, they overlap and cannot both be used. 

Subnet CIDRs can be changed even after creation. 

Subnets come in two types: AD-specific subnets, which reside in a single Availability Domain, and regional subnets, which can span all Availability Domains in a multi-AD region and are the recommended best practice. 

When creating a subnet, you specify its route table, security list, and DHCP options, and all instances in that subnet inherit this configuration. 

Subnets can be public (VMs get both public and private IPs) or private (VMs get only private IPs), and this choice must be made at creation time since it cannot be changed later—databases typically belong in private subnets, while web servers typically belong in public subnets.

## Dividing a VCN into Subnets

The first thing to understand is that a VCN can be divided into subnets. For example, if you have a VCN with the CIDR block `10.0.0.0/16`, you can divide it into multiple subnets. In our example, we create two subnets:

- **Subnet A:** `10.0.1.0/24`
- **Subnet B:** `10.0.2.0/24`

Like the VCN itself, each subnet is represented in CIDR notation.

**Key Rule: Larger Number = Smaller Network**

Out of `/16` and `/24`, the `/16` is the bigger network, and the `/24` is the smaller network compared to `/16`. Keep this relationship in mind: **the larger the number after the slash, the smaller the network.**

---

## Types of Subnets: AD-Specific vs. Regional

A VCN always resides in a region. Some OCI regions have multiple Availability Domains (ADs)—for example, AD 1, AD 2, and AD 3—and a VCN can span multiple Availability Domains.

Subnets come in two types:

### AD-Specific Subnet
A subnet created in a **specific Availability Domain**. For example:
- Subnet A created inside Availability Domain 1
- Subnet B created inside Availability Domain 2
- Subnet C created inside Availability Domain 3

### Regional Subnet (Recommended)
A **regional subnet** can span **all three Availability Domains** in a region. This is typically the choice when creating a subnet and is considered the **best practice / recommended option**.

**Important caveat:** This multi-AD spanning only applies in a **multi-AD region**. If your region has a single Availability Domain, AD-specific and regional subnets are one and the same.

---

# 4. Route Tables

# 5. Gateway

# 6. Security

