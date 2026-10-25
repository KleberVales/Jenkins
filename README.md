# Networking ‐ DNS Management

# 1. What is DNS?

The main functionality of DNS is to translate easy-to-remember symbolic domain names into IP addresses.

## Two Types of DNS Servers

| Type | Role |
|------|------|
| Recursive (Non-Authoritative) | The DNS server in the middle — queries other servers on your behalf. |
| Authoritative | The servers on the right — they actually hold the records for a domain. |

> Key point: The servers on the right are authoritative and not recursive, whereas the DNS server in the center is recursive.

**How DNS Resolution Works**

Let's say you're operating a laptop (or a compute instance) with a browser, and you want to visit `www.example.com`.

The machine doesn't know what `www.example.com` is, so here's what happens:

1. The browser queries the DNS server (a recursive/non-authoritative server).
2. The DNS server reaches out to the root servers (`.`) and asks: "What is the IP for www.example.com?"
3. The root server responds: "Ask `.com`."
4. The DNS server asks the TLD (Top Level Domain) servers for `.com`: "What is the IP of www.example.com?"
5. The TLD server responds: "Ask example.com."
6. The DNS server reaches out to the customer's DNS servers — which are authoritative and actually hold the records.
7. The authoritative DNS server returns the answer — for example, the IP address for `www.example.com`.
8. The recursive DNS server returns this IP to the browser, and now you can see the contents of `example.com`.

# 2. Zones

DNS zone, is the portion of the DNS namespace managed by a specific organization or administrator — essentially an administrative space that enables granular control over DNS components like authoritative nameservers. 

A zone holds trusted DNS records (such as A, AAAA, MX, and CNAME records) that reside on OCI nameservers. When you create a zone, two records are generated automatically: a Nameserver (NS) record and a Start of Authority (SOA) record. 

OCI offers two types of zones: a Public Zone, which holds trusted DNS records on OCI nameservers for public resolution, and a Private Zone, which contains domain names that resolve DNS queries for private addresses within the Virtual Cloud Network (VCN). 

This distinction allows organizations to manage both internet-facing and internal DNS resolution within OCI.

**Records Created Automatically**

Whenever you create a zone, two records are created automatically:

- Nameserver (NS) record — associated with the zone
- Start of Authority (SOA) record

## Public DNS Zones

public DNS zones are used to resolve host names on the internet and hold trusted DNS records residing on OCI nameservers.

Implementing a public DNS zone involves three steps: first, create the public DNS zone, which automatically generates Name Server (NS) and Start of Authority (SOA) records; 

second, delegate the zone to make it accessible from the internet — this involves copying the NS records created in OCI and updating them in your domain registrar's DNS panel (such as GoDaddy), replacing the registrar's default nameservers; 

and third,  add records to the zone, where each record type contains RDATA (record data), such as an IP address for an A record. 

The zone must be created with the same name as your purchased domain (e.g., create a public DNS zone for `abc.com` after purchasing that domain). Until the zone is delegated, it remains inaccessible from the internet — making delegation the critical step that connects your OCI-hosted zone to the global DNS system.

**What Is a Public DNS Zone?**

A public DNS zone is used to resolve host names on the internet. As the name suggests, "public" means it resolves host names on the internet.

There are two types of DNS zones:
1. Public DNS zones (covered in this lesson) — resolve host names on the internet.
2. Private DNS zones — resolve host names privately (covered elsewhere).

Because a public DNS zone is still a zone, it holds trusted DNS records, and these records reside on OCI nameservers.

---

**Step 1: Create a Public DNS Zone**

When you create a public DNS zone, it results in the automatic creation of:

- Name Server (NS) records
- Start of Authority (SOA) record

The list of nameservers generated here is used in Step 2.

**Step 2: Delegate the Zone**

Delegation is what makes the hosted zone accessible from the internet. Until you delegate the zone, it is not accessible from the internet.

How delegation works — an example:

1. Assume you purchased the domain `samvit.site` from GoDaddy (a domain name registrar).
2. When you purchased the domain, GoDaddy automatically provided you with a list of nameservers.
3. To delegate the zone, you must first create a public DNS zone in OCI with the same name as your domain — in this case, `samvit.site`.
4. Then, you copy the nameserver records that were created when you created the public DNS zone in OCI.
5. You update these nameserver records in your domain registrar's DNS panel (e.g., GoDaddy) — replacing the original list provided by the registrar with the OCI nameservers.

> Key point: The delegation step is performed in the registrar's DNS panel, not in OCI.

**Step 3: Add Records to the Zone**

Once the zone is delegated, you can add records to the zone.

- Every record type contains information called RDATA (record data).
- Example: For an A record, the RDATA is an IP address.

---






