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

## Dividing a VCN into Subnets
## Types of Subnets: AD-Specific vs. Regional
## Subnet-Level Configuration
## Public Subnet
## Private Subnet

# 4. Route Tables

## What Does a Route Table Do?
## Two Types of Route Tables
## Anatomy of a Route Table
## Key Rule: One-to-Many Association

# 5. Gateway

## Internet Gateway
## NAT Gateway
## Service Gateway

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

## Subnet-Level Configuration

When you create a subnet, you typically specify information such as:

- Which **route table** should be associated with the subnet
- What **security list** should be associated with the subnet
- What **DHCP options** are associated with the subnet

---

## Public Subnet

<p align="justify"> A Public Subnet is a subnet within a VCN that allows instances deployed inside it to be assigned public IP addresses  and private IP addresses on their Virtual Network Interface Cards (VNICs). When combined with a route table entry pointing to an Internet Gateway and appropriate security rules, a public subnet enables instances to communicate directly with the internet.</p>

## Private Subnet

<p align="justify">A Private Subnet is a subnet within a VCN that prohibits public IP addresses for instances deployed inside it. Instances in a private subnet can only have private IP addresses. To enable outbound internet access, a NAT Gateway is used. To communicate with an on-premises environment, a Dynamic Routing Gateway (DRG) is used.</p>

---

# 4. Route Tables

A route table determines how traffic flows inside the VCN and how it flows outward when originating from the VCN, effectively defining the path to a destination. 

There are two types of route tables: the default route table, which is automatically created with every VCN, and custom route tables, which you create yourself. 

Both types contain an implicit local route—invisible in the console, making the default route table appear empty when it isn't—that enables subnets and their instances to communicate with each other without any additional rules. 

Custom route tables are useful because public and private subnets typically cannot share a single route table when you need to influence their routing differently. 

A route table consists of two columns: the destination CIDR block (such as `0.0.0.0/0` for the internet) and the target or next hop (such as an internet gateway). 

Beyond internet gateways, allowed targets include service gateways, NAT gateways, dynamic routing gateways, local peering gateways, and private IPs.

## What Does a Route Table Do?

A route table is going to determine:

- **How traffic flows inside the VCN**
- **How traffic flows when it originates from the VCN** and heads outward
- **The path for a destination**—in a way, the route table defines the path traffic takes

---

## Two Types of Route Tables

### Default Route Table

The **default route table** is **automatically created**. Whenever you create a VCN, a default route table is created for you.

**The Implicit Local Rule:**
The default route table has a rule that is **implicit**. Consider a VCN with three subnets—Subnet A, Subnet B, and Subnet C. When you create the VCN, you get the default route table, and all these subnets are associated with it. This means **they can communicate with each other**, and there is **no requirement to add any rule** to establish communication between subnets inside the VCN.

**Why is this so?** Because the default route table contains a **local route**—which is why it's called implicit. If you go to the route table, you will not be able to see this local route. It will feel like the default route table is empty—**but it is not empty**. It has a local route that facilitates communication between the subnets (and the instances within them). That is the significance of the local route and the default route table.

### Custom Route Table

The second type is the **custom route table**—one that you create yourself. When creating it, you give it a name and click Create.

**Key point:** Whether it is a default route table or a custom route table, it is **going to have the local route**.

**Why create a custom route table?** Because your VCN can have two types of subnets—**public and private**. To influence the routing of each of these subnets differently, they typically cannot be associated with a single route table. Hence, we create custom route tables and associate them with, for example, our public subnet.

---

## Anatomy of a Route Table

A route table has **two columns**:

| Destination CIDR Block | Target (Next Hop) |
|---|---|
| The CIDR range of the destination | Where to send the traffic |

### Example: Routing Traffic to the Internet

Imagine a VCN containing Subnet A, with an instance that wants to talk to the internet. Between the VCN and the internet sits an internet gateway.

- Destination CIDR block: The destination is the internet, so we specify the internet's CIDR range: `0.0.0.0/0`
- Target (next hop): We want to send the traffic to the internet gateway, so we specify the internet gateway as the target

This is just one example to show how a route table looks.

---

## Key Rule: One-to-Many Association

This is a key learning objective:

- One subnet can be associated with only one route table.
- The same route table can be associated with multiple subnets.

For example, the default route table can be associated with three subnets if there are three subnets.

Now consider two route tables—a default route table and a custom route table—and two subnets, Subnet A and Subnet B. It cannot be the case where one subnet is associated with both route tables. That configuration is not possible.

What is possible: One subnet is associated with only one route table. But the reverse is possible—each route table can be associated with multiple subnets. That is exactly the case with the default route table.

---

# 5. Gateway

## Internet Gateway

An internet gateway is the gateway you add to a VCN to enable direct connectivity to the internet, and it supports both ingress traffic (internet → VCN) and egress traffic (VCN → internet)—making it a bidirectional communication path where even internet instances can reach instances within the VCN. 

To use it, three conditions must be met: the instance must be in a public subnet, the instance must have a public IP, and the subnet's route table must contain a valid entry pointing to the internet gateway (destination `0.0.0.0/0`, target: internet gateway). 

Only one internet gateway is needed per VCN—in fact, you cannot create multiple—and as a managed service, OCI handles its scaling, high availability, and fault tolerance. 

Remember that routing alone isn't enough: you must also ensure the appropriate ports are opened via security lists or network security groups, which are covered in dedicated upcoming lessons.

**Conditions for Using the Internet Gateway**

To use the gateway, some conditions must be met. Who can send traffic to the internet? For that to occur:

1. The instance must be in a public subnet.
2. The instance must have a public IP.
3. The subnet in which the instance resides must have a valid route table entry pointing to the internet gateway.

**One Gateway per VCN**

For a VCN, you just need one internet gateway—only one internet gateway is needed for each VCN. It's a managed service: you don't need to worry about scaling, high availability, or fault tolerance. Oracle Cloud Infrastructure takes care of these things.

## NAT Gateway

NAT Gateway is designed to provide access to a private subnet that contains only private IP addresses.

The NAT gateway is unidirectional: private instances can initiate outbound connections and receive responses, but inbound connections initiated from the internet are denied—unlike the bidirectional internet gateway. 

Outbound traffic carries the NAT gateway's public IP as its source IP, and when you create a NAT gateway you can choose either an ephemeral or reserved public IP. 

It supports TCP, UDP, and ICMP ping traffic, supports 20,000 concurrent connections, and as a managed, highly available solution it replaces the older NAT instance approach. 

A notable feature is the ability to block all traffic through the gateway regardless of route or security rules. 

For routing, the private subnet must be associated with a route table containing a route to the internet via the NAT gateway—and critically, a NAT gateway can only be used by resources in its own VCN, not by peered VCNs or connected on-premises networks. 

This is why the architecture places database servers in the private subnet: they can reach out to the internet when needed, but nothing on the internet can initiate a connection back to them.

---

**NAT Is Unidirectional**

There are two or three important things to know:

- Instances in the private subnet can initiate connections to the internet. Along with that, they can also receive responses—responses to the connections they have initiated. That is allowed.
- Inbound connection requests are not allowed. If these instances receive inbound connections initiated from the internet, that is denied.

So: they can initiate connections to the internet and receive responses, but inbound connections initiated from the internet are denied. Unlike the internet gateway, the NAT gateway is unidirectional in nature.

---

## Service Gateway

The Oracle Services Network is reserved for Oracle services. Typically, we reach those services over the internet because these services have public IP addresses. But is this the best practice? No. The best practice is to access those services privately without exposing the data—and that is where the service gateway comes into action.

**Key Characteristics**

Using the service gateway, you can access Oracle services privately, wherein:

- The traffic travels over the Oracle network fabric
- It never traverses the internet
- It provides a secure and private path for your network traffic

The best part: hosts in the VCN don't need public IP addresses. They can have only private IP addresses and still access Oracle services in the Oracle Services Network. What this means is: you don't need an internet gateway, and you don't need a NAT gateway. Just by using a service gateway, you can access those services privately.

---

# 6. Security

## VCN Security

VCN security in OCI relies on layering several strategies: private subnets, IAM policies, instance firewalls, route tables and gateways, security zones, and two virtual firewall constructs, security lists and network security groups. 

Security zones are tied to a compartment and prohibit public IP addresses, permitting only private subnets. 

The housing society analogy clarifies the two firewall constructs: a security list works like the main gate guard, protecting an entire subnet, while an NSG works like the guard at each house, protecting individual instances through their VNICs.

 You can use either construct alone or both together, with limits of up to five security lists per subnet and five NSGs per VNIC. 

Every new VCN also comes with a default route table and a default security list. 

---

**Security strategies**

OCI offers several complementary ways to control security for a VCN:

1. Public and private subnets: A subnet can be designated as private so its resources are not directly reachable from the internet.
2. IAM policies: Identity and Access Management policies control who can create or configure the VCN, subnets, security lists, and NSGs.
3. Route tables and gateways: Route tables and different gateway types shape how traffic flows in and out of the network.
4. Security zones: These ensure networking resources comply with Oracle's security principles and best practices.
5. Security rules: Two constructs control traffic in a VCN: security lists and network security groups (NSGs).
6. Instance-level firewall rules: Firewall rules can be configured directly on the instance.

**Security rules**

A Network Security Group (NSG) is a virtual firewall consisting of two components:

- A set of VNICs — the resources (compute instances, load balancers, mount targets, API Gateways, etc.) to which the NSG is associated.
- A set of security rules — the ingress and egress rules that control traffic.

You define the rules first, then associate the NSG with the VNICs you want those rules applied to. 

Note: when you create a VCN, a default security list is created automatically, but no default NSG is created — you must create the NSG and define its rules yourself.

**Security Zones**

This is an association between a compartment and a "recipe." Once a compartment is added to a Security Zone, OCI will verify all resource creation and modification operations within it against the policies in the recipe. A compartment can only belong to one Security Zone, and its subcompartments are included by default (though subcompartments can be individually excluded or assigned a different zone).

- A security zone is always associated with a compartment.
- It does not permit public IP addresses.
- As a result, it allows only private subnets.

**Instance-level firewall rules**

Instance-level firewall rules refer to firewall rules configured directly on the operating system of a compute instance (such as Linux's iptables/firewalld or Windows Firewall). They are host-level and are completely separate from OCI's cloud network-level security mechanisms (Security Lists, NSGs).

Core Positioning: The "Last Line of Defense"

Traffic from the external network to reach your application must pass through multiple checkpoints. Instance-level firewalls are the final checkpoint on the instance itself, operating after cloud network security rules (Security Lists/NSGs).

- Security Lists / NSGs: Operate at the cloud network level, controlling whether traffic can enter the instance's VNIC.
- Instance firewall: Operates at the operating system level, controlling whether traffic that has already entered the instance can be received by applications within the system.

Even if you have allowed port 80 in the cloud console, if the instance's internal firewall does not allow it, the external request will still be rejected.

## Network Security Groups

An NSG is a virtual firewall in which you define ingress and egress security rules, and it is associated with the VNIC of a compute instance—though NSGs also support other resources such as load balancers, mount targets, and API gateways. 

---

**Ingress Rules**

Ingress rules control traffic flowing into the instance and require a source type (CIDR block, service, or NSG), the corresponding source value, an IP protocol (such as RDP, SSH, or ICMP), and optional source and destination port ranges—the destination port range being automatically populated based on the chosen protocol. 

Assume we have an instance with a virtual NIC (VNIC). This is where we associate the network security group.

An *ingress rule means ingress to the instance—the direction of the arrow points toward the instance.

What You Define in an Ingress Rule

Since traffic is coming from outside, you must decide which traffic is allowed inside the instance. To do this, you define:

- Source type — where the traffic is coming from. There are three types of source supported in an NSG ingress rule:
  1. CIDR block
  2. Service (for example, the object storage service)
  3. NSG
- Depending on the source type, you provide the source CIDR, source service, or source NSG
- IP protocol — different protocols are supported, for example RDP, SSH, ICMP
- Source port range — optional
- Destination port range — optional; it gets automatically populated depending on the IP protocol you select

---

**Egress Rules**

An egress rule controls traffic flowing out of an instance.,

Because traffic is leaving the instance, you must specify where it is going:

| Field | Description |
|---|---|
| Destination Type | CIDR block, Service (e.g., Object Storage), or NSG |
| Destination | The actual value based on the destination type (destination CIDR, service, or NSG) |
| IP Protocol | E.g., TCP, RDP, SSH, ICMP |
| Source Port Range | Optional |
| Destination Port Range | Optional |

**Stateful vs. Stateless Rules**

While creating ingress or egress rules, you have an option to specify the rule type: stateful or stateless.

- Stateful Rules
Responses are automatically tracked and allowed—fundamentally, you are using connection tracking.

Example: Consider an instance and the internet. You define an ingress rule for destination port 80, with source `0.0.0.0/0`. The response traffic is automatically allowed if the rule is stateful—responses are automatically tracked and allowed.

- Stateless Rules
You need to define egress rules if you want the response to be allowed.

That is the fundamental difference between stateful and stateless rules.

**Security by Tier**

**Architecture**

## Security Lists







