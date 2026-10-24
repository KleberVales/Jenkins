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

A Virtual Cloud Network,  is a software-defined private network that you create inside an Oracle Cloud Infrastructure region. 

VCN acts like a private, isolated network for your cloud resources. 

When you create a VCN, you must attach at least one CIDR block, and you can have up to five IPv4 CIDR blocks and up to five IPv6 CIDR blocks, as long as they do not overlap. 

VCN CIDR sizes can range from `/16` to `/30`, and CIDR blocks can be resized after creation. 

Inside any CIDR, the first IP is the network address, the second IP is the subnet default gateway, and the last IP is the broadcast address; these are reserved and cannot be assigned to hosts. 

A VCN is regional, meaning it resides in one region, but it can span multiple availability domains within that region—it cannot span multiple regions. 

For IPv6, you can use an Oracle-allocated `/56` prefix or bring your own IPv6 prefix. Understanding VCNs is essential for designing secure and scalable OCI network architectures.

## Availability Domains

# 3. Subnets
# 4. Route Tables
# 5. Gateway
# 6. Security


