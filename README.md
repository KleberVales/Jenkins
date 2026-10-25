# Networking ‐ IP Management


# 1. IP Management 

## VNIC

A VNIC is the virtual network interface card attached to a compute instance. It is the object that actually holds IP addresses and receives NSG associations — making it a central piece of OCI networking.

---

**1. What a VNIC Is**

- Every instance has a VNIC, and the VNIC is what gets assigned private IP addresses.
- When you associate an NSG, you associate it with the VNIC (not the instance directly). As the earlier lesson stated: an NSG consists of "the set of VNICs to which this can be associated with" plus the security rules.
- Public IPs are also tied to the VNIC — a public IP "is always assigned to a private IP object on the resource," and those private IP objects live on the VNIC.

---

**2. Primary vs. Secondary VNIC**

An instance can have:

| VNIC Type | What It Has |
|---|---|
| Primary VNIC | 1 primary private IP + up to 32 secondary private IPs |
| Secondary VNIC | 1 primary private IP + up to 32 secondary private IPs |

Key point: A secondary VNIC *also* receives its own primary private IP address — it is not just an add-on without a primary.

---

**3. IP Address Capacity per VNIC**

- 1 primary private IPv4 address (always exactly one)
- 32 secondary private IPv4 addresses (maximum)
- Total: 33 private IPv4 addresses per VNIC

Each of these private IP addresses can have a public IP assigned to it, at your discretion. So the number of public IPs a resource can have scales with the number of VNICs and private IPs on them.

---

**4. VNIC and Public IP Types**

This is where the VNIC matters for public IP assignment:

| Public IP Type | Can Be Assigned To |
|---|---|
| Ephemeral | Only a VNIC's primary private IP |
| Reserved | Either a primary or a secondary private IP |

So having secondary private IPs on a VNIC is what unlocks the ability to attach multiple reserved public IPs to a single VNIC.

---


## Public IP Address

**1. What a Public IP Address Is**

A public IP address is an IP address that is reachable from the internet. Critically, in OCI it is always assigned to a private IP object on a resource — it does not exist independently. The private IP lives on the VNIC, and the public IP is attached to it.

Some OCI resources are designed to be directly reachable from the internet — for example the NAT gateway, which comes with a public IP address.

---

**2. Conditions for Internet Reachability**

Having a public IP is not sufficient on its own. For the public IP to actually be reachable over the internet, all of the following must be in place:

1. Internet Gateway created in the VCN.
2. Route table on the public subnet correctly configured — there must be a route out to the internet through the internet gateway.
3. Security lists correctly configured to permit the traffic.

So reachability = public IP + internet gateway + correct route table + correct security rules.

---

**3. How Many Public IPs a Resource Can Have**

A resource can have multiple public IP addresses, because:

- An instance can have multiple VNICs (primary + secondary).
- Each VNIC has 1 primary private IP + up to 32 secondary private IPs (33 total).
- Each of those private IPs can have a public IP assigned.

So public IP capacity scales with VNICs and their private IPs.

---

**4. The Two Types of Public IP Addresses**

- Ephemeral Public IP
   - Temporary — "ephemeral" means not permanent.
   - Exists for the lifetime of the instance.
   - If you terminate the instance, the IP goes away.
   - Can only be assigned to a VNIC's primary private IP.

- Reserved Public IP
   - Persistent — exists beyond the lifetime of the instance.
   - If the instance is terminated, the reserved IP still exists and can be reassigned to another resource.
   - Can be assigned to either a primary or a secondary private IP.
   - Never automatically deleted — it exists until you explicitly delete it.
   - When unassigned, it returns to the tenancy's pool of reserved public IPs.
   - Regional scope — can be assigned to a private IP in any availability domain in the region.
   - Creation is one at a time, and there is a limit of 50 reserved public IPs per region.
   - When moved (e.g., a private IP moving from VNIC 1 to VNIC 2), the reserved IP moves with the private IP.
   
## Private IP address

**1. What a Private IP Address Is**

A private IP address is the IP assigned to a VNIC of a resource (e.g., a virtual machine) inside a VCN. Every instance has at least one primary private IP address, and this private IP is the object that can optionally have a public IP assigned to it.

Key point: even in a public subnet, an instance has both a private IP address and a public IP address — the public IP is always attached to a private IP object.

---

**2. Use Cases of Private IP Addresses**

The lesson lists three main scenarios where private IPs are used for communication:

- Use Case 1 — Communication within the same VCN
An instance can talk to other instances in the same VCN, even if they are in different subnets — the only condition is they must be in the same VCN. This communication happens through private IP addresses.

-  Use Case 2 — Peering
When two different VCNs are connected via peering, instances talk to each other using private IP addresses. Peered VCNs can be in the same region or in different regions.

- Use Case 3 — On-premises connectivity
An instance can talk to hosts in an on-premises network using private IP addresses. This typically happens via a site-to-site VPN connection or FastConnect.

---

**3. Primary vs. Secondary Private IPs**

Each VNIC has a defined allocation of private IPv4 addresses:

| Type | Count per VNIC |
|---|---|
| Primary private IP | Exactly 1 (always) |
| Secondary private IP | Up to 32 |

This applies to both the primary VNIC and any secondary VNIC:

- Primary VNIC: 1 primary private IP + up to 32 secondary private IPs
- Secondary VNIC: 1 primary private IP + up to 32 secondary private IPs

Total per VNIC: 33 private IPv4 addresses (1 primary + 32 secondary).

> Note: A secondary VNIC also receives its own primary private IP address — it is not an exception to the rule.

---

**4. Private IPs and Public IP Assignment**

Every private IP address can have a public IP address assigned to it at your discretion. This is what allows a resource to have multiple public IPs — because it can have many private IPs across its VNICs.

However, the two public IP types differ in what they can attach to:

| Public IP Type | Can Be Assigned To |
|---|---|
| Ephemeral | Only a VNIC's primary private IP |
| Reserved | Either a primary or a secondary private IP |

So secondary private IPs are what enable attaching multiple reserved public IPs to a single VNIC.

---

**5. Private IP Mobility and Reserved IPs**

A private IP (e.g., a secondary private IP) can be moved from one VNIC to another. When this happens, any reserved public IP attached to it moves along with it. This portability is one of the advantages of reserved public IPs.

---

# 2. BYOIP

Bring Your Own IP (BYOIP) allows customers to migrate their own public IPv4 CIDR blocks or IPv6 prefixes from their on-premises environment to Oracle Cloud Infrastructure. 

This is particularly valuable when applications have hard-coded IP addresses or architectural dependencies on specific IPs, enabling smooth migration and solution continuity. 

The process involves requesting an import, receiving a verification token from Oracle, adding that token to your Regional Internet Registry (RIR) records, creating a Route Origin Authorization (ROA) with Oracle's BGP ASN, and waiting for Oracle to validate ownership. 

Once validated, the IP addresses are provisioned to your compartment, where you can manage them through IP pools and advertise them to the internet.

---

**1. What Is Bring Your Own IP (BYOIP)?**

When you use a public IP address in OCI, it is typically owned by Oracle. However, some organizations have their own public IP addresses that they want to use in the cloud.

- BYOIP allows you to:
   - Bring your own IPv4 CIDR block or IPv6 prefix to OCI.
   - Use those IP addresses with your OCI resources.
   - Maintain IP address continuity during migration.

- Why Use BYOIP?
   - Hard-coded IP addresses: Applications may have IP addresses hard-coded in devices or configurations.
   - Smooth migration: Enables seamless migration to OCI without changing IP addresses.
   - Solution continuity: Ensures that existing architectures and dependencies remain intact.

---

**2. Prerequisites and Limits**

- Ownership Validation
   - You must be the owner of the IPv4 CIDR block or IPv6 prefix.
   - Ownership must be registered with a supported Regional Internet Registry (RIR).
   - Oracle validates your ownership before importing the addresses.

- Limits

| Address Type | Minimum | Maximum |
|---|---|---|
| IPv4 CIDR Block | /24 | /8 |
| IPv6 Prefix | /48 or larger | — |

---

**3. BYOIP Process Overview**

The BYOIP import process involves several steps:

- Step 1: Request to Import
   - You request to import a public IPv4 CIDR block or IPv6 prefix that you own into OCI.

- Step 2: Receive Verification Token
   - Oracle issues a verification token.
   - You must add this token to the information about your IP addresses kept by your Regional Internet Registry (RIR).
   - This step can take up to one day.

- Step 3: Create Route Origin Authorization (ROA)
   - Create a ROA with your RIR.
   - Provide Oracle's BGP ASN for the commercial cloud.
   - The ROA allows Oracle to advertise the BYOIP CIDR block.

- Step 4: Request Oracle to Finish Import
   - Request that Oracle complete the import process.
   - This workflow can take up to 10 business days.
   - Oracle communicates with the RIR to verify ownership.

- Step 5: Provisioning
   - Oracle provisions the BYOIP addresses to your compartment within your tenancy.
   - The IPv4 CIDR block or IPv6 prefix is now yours to manage.

- Step 6: Advertise to Internet
   - You can advertise the CIDR block or prefix to the internet.

## Public IP Pools

**1. What Is a Public IP Pool?**

Public IP Pools are sets of IPv4 CIDR blocks allocated to a tenancy, created from Bring Your Own IP (BYOIP) address space. 

These pools are exclusively available for your tenancy and provide a centralized way to manage your public IP addresses. 

Once you import your addresses and create a public IP pool, you have two options: create reserved public IPs from the pool and attach them to resources like NAT gateways, load balancers, or compute instances, or enable direct launch from the pool where resources are launched with an IP directly allocated from the pool. 

Public IP pools currently support IPv4 CIDR blocks, with a minimum size of /28 and a maximum size of /24.


---

**2. Using Public IP Pools**

Once you have a Public IP Pool, you have two options for using the IP addresses:

- Option 1: Create Reserved Public IPs
    - Create a reserved public IP from the pool.
    - Attach the reserved IP to resources such as:
         - NAT Gateway
         - Load Balancer
         - Compute Instances
    - This provides flexibility to reassign the IP as needed.

- Option 2: Direct Launch from Pool
    - Launch resources directly with an IP allocated from the pool.
    - No need to create a reserved IP first.
    - The IP is directly allocated from the Public IP Pool during resource creation.

Comparison

| Method | Description | Use Case |
|---|---|---|
| Create Reserved IP | Reserve an IP from the pool, then attach to a resource | When you need flexibility to reassign IPs |
| Direct Launch from Pool | Launch a resource with an IP directly from the pool | When you want immediate allocation without reservation |

---

**3. Limits and Quotas**

When creating a Public IP Pool:

| Parameter | Value |
|---|---|
| Number of CIDR Ranges | Zero or more |
| Minimum Size Range | /28 |
| Maximum Size Range | /24 |

> Note: The pool can have zero CIDR ranges initially, and you can add ranges later.

---
