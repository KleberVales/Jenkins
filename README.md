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

## Private IP address

## Reserved Public IP

## Public IP Pools

# 2. BYOIP


