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

## Reserved Public IP

## Public IP Pools

# 2. BYOIP


