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





