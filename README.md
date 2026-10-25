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

## Private DNS Zones

Private DNS zones in OCI, starting with the three DNS name resolution options available in a VCN: the default Internet and VCN Resolver (which resolves public hostnames and hostnames of instances within the same VCN), a custom resolver (allowing you to choose a DNS server over the internet, in your VCN, or on-prem), and the newer Private DNS service. 

Private DNS extends hostname resolution within VCNs, across VCNs, and between VCNs and on-premises or other private networks — a capability that previously required custom solutions. 

resources associated with private DNS zones: the zone (which holds DNS data), the view (a collection of zones, each belonging to a single view), the resolver (which holds the configuration serving DNS responses within the VCN), private DNS records (supporting A, AAAA, and PTR records, among others), and the resolver endpoint (which sets up ingress and egress, consumes private IPs, and comes in two types — listening for queries from outside the VCN/OCI, and forwarding to redirect queries to other DNS servers). 

The most important takeaway is the hierarchy: Resolver → Views → Zones → Records, which determines how DNS queries are answered within a VCN. 

A practical on-prem-to-OCI example illustrated how the listener handles inbound queries from outside OCI, while the forwarder redirects outbound queries to external DNS servers.

**DNS Options in a Virtual Cloud Network (VCN)**

Before diving into private DNS zones, it's important to understand the DNS name resolution choices available for instances provisioned in a VCN.

- Option 1: Internet and VCN Resolver (Default)

This is the default choice, and it provides two capabilities:

Internet Resolver – Lets instances resolve hostnames that are publicly published on the internet.  
VCN Resolver – Lets instances resolve hostnames of other instances in the same VCN.

Example: If you have a VCN with two subnets, and an instance in each (Instance A and Instance B), Instance A can reach Instance B using its fully qualified domain name — because the VCN resolver handles this by default.

- Option 2: Custom Resolver

With a custom resolver, you can select a DNS server of your choice. Multiple options exist:

1. A DNS server available over the internet.
2. A custom DNS server in your VCN.
3. A custom DNS server in your on-premises network.

- Option 3: Private DNS (New)

Oracle launched a service called Private DNS, which is now the third option. It provides hostname resolution:

- Within the VCN (just like the VCN resolver).
- Across VCNs — previously, this required a custom solution.
- Between a VCN and your on-premises network (or any other private network).

---

**Private DNS Zones**

Private DNS provides hostname resolution within VCNs, between VCNs, and between VCN and on-prem networks.

- Resources Associated with Private DNS Zones

Private DNS Zone  
The zone itself contains the DNS data.

Private DNS View  
A private DNS view is a collection of private DNS zones. Each private DNS zone can belong to a single view.

Private DNS Resolver  
The private DNS resolver contains the configuration that serves responses to DNS queries within the VCN.

- Hierarchy within the resolver:
   - The resolver contains views.
   - Each view contains zones.
   - Each zone contains record data.

> Key insight: The resolver holds the configuration that serves responses to DNS queries within the VCN. The views on the resolver decide the zone and record data applicable for the response.

Private DNS Records  
Private DNS zones support a number of DNS record types:

| Record Type | Mapping |
|-------------|---------|
| A | Hostname to IPv4 |
| AAAA (Quad A) | Hostname to IPv6 |
| PTR (Pointer) | Reverse DNS mapping |

Private DNS Resolver Endpoint  
The private DNS resolver endpoint sets up ingress and egress in the VCN.

- It consumes private IP addresses.
- A corresponding VNIC is created for each resolver endpoint.

Two types of resolver endpoints:

| Endpoint Type | Purpose |
|---------------|---------|
| Listening (Listener) | Responds to queries from outside the VCN or OCI |
| Forwarding (Forwarder) | Uses rules to redirect queries to different DNS servers |

# 3. Private Views

### What Is a Private View?

A private view is a collection of private DNS zones. You can think of it as a way to logically group a set of private DNS zones together.

Example: A private view could contain zones such as `samvit.com` and others.

---

### Key Rules

Rule 1: A Zone Belongs to Only One View

A zone — for example, `samvit.com` — can belong to only a single view.

- If `samvit.com` belongs to Private View A, it cannot belong to Private View B.
- This is not possible.

Rule 2: Every VCN Resolver Has a Default View

When you create a VCN, the VCN has a resolver, and this resolver has a default view. This default view is always there.

- You can add zones to the default view.
- You can also create and attach another view to the resolver, in addition to the default view.

### Why Attach Additional Views?

The purpose is that the zones that are part of a view should be resolvable in the VCN — because ultimately, zones contain records that need to be resolved.

---

# 4. HTTP redirects

This lesson introduced HTTP redirects, a supporting service in DNS management that lets you redirect HTTP traffic to another URL. 

Four scenarios were covered: redirecting all HTTP traffic for an entire zone to another zone (e.g., `test.net` → `test.com`), 

redirecting a specific subdomain to an HTTP URL, 

redirecting a subdomain to a URL with a port number (e.g., `http://sample.test.net:8080`), 

and permanently redirecting a domain name using a 301 response code — which is especially useful when a domain has been deprecated, as it informs search engines and browsers how to handle the change. 

For any redirect to work, you must also create the relevant CNAME record in the DNS zone. 

When configuring a redirect in the console, you select a zone and specify the domain/subdomain plus target details including protocol, host, port, and optionally path and query — along with the response code (301 for permanent, 302 for temporary). 

The console also offers an option to automatically create the required DNS record, simplifying the setup of HTTP redirects within OCI DNS.

### What Are HTTP Redirects?

The DNS service lets you redirect HTTP traffic to another URL. This is useful when you want traffic hitting one domain or subdomain to be sent somewhere else.

Example: A company owns both `test.net` and `test.com`. Using HTTP redirect, you can redirect all HTTP traffic for `test.net` and send it to `test.com`.

> Note: This is a one-to-one mapping.

---

### Four HTTP Redirect Scenarios

Scenario 1: Redirect All HTTP Traffic for a Zone  
Redirect all HTTP traffic for an entire zone (e.g., `test.net`) to another zone (e.g., `test.com`).

Scenario 2: Redirect a Specific Subdomain to an HTTP URL  
Redirect a specific subdomain to an HTTP URL.

Scenario 3: Redirect a Subdomain to a URL with a Port Number  
Redirect a subdomain — for example, `example.test.net` — to a URL with a port number, such as `http://sample.test.net:8080`.

Scenario 4: Permanently Redirect a Domain Name  
Use this when a domain name has been deprecated. You typically want to display a 301 response code.

Benefit: Permanently redirecting a domain name informs search engines and browsers what to do with the information.

---

### Required DNS Record

After creating the redirects, you must also create the relevant DNS record — specifically a CNAME record.

> Important: You must add the CNAME record to the DNS zone for the redirect to work successfully.

---

### Configuring HTTP Redirects (Console View)

When configuring an HTTP redirect, you provide:

- Zone – Select the zone.
- Domain and Subdomain – Specify the source.
- Target details:
  - Protocol
  - Host
  - Port
  - Path (optional)
  - Query (optional)

# 5. Traffic Management

Traffic management, a key component of the OCI DNS service that sits between users making DNS queries and the endpoints that serve them, providing intelligent responses based on conditions rather than simply returning static records. 

Unlike a regular public DNS zone (which holds records like A, AAAA, and MX), traffic management defines conditions — such as the user's geographical location or the health of endpoints — and routes traffic accordingly to different answer pools. 

### Traffic Management Components

**1. Steering Policies**

Steering policies contain rules that help intelligently serve DNS answers. Think of them as the logic behind traffic management.

**2. Attachment**

Attachment means linking a steering policy to a public DNS zone.

> Important: When you link a steering policy to a public DNS zone, the DNS response is constructed from the steering policy — not from the records you created in the public DNS zone.

Even though the public DNS zone has its own records, the DNS response always comes from the steering policy.

**3. Rule**

A rule is a guideline that steering policies use to filter answers based on the DNS request and its properties.

Examples of rule properties:

- Geolocation
- Health of the endpoint

**4. Answers**

When you create a traffic management steering policy, there is a concept of an answer pool. These answer pools contain the records that will be provided as answers.

In the earlier example, the endpoints (Endpoint 1, Endpoint 2, Endpoint 3) are actually answer pools.

---

# 6. Traffic Management-Policy Types

## The Five Traffic Management Steering Policies

OCI offers five steering policy types:

1. Failover
2. Load Balancer
3. Geolocation
4. ASN Steering
5. IP Prefix

---

### Failover Policy

Use this when you have multiple endpoints (e.g., two) and want to prioritize the order in which answers are served.

**How it works:**
- You designate one endpoint as primary and the other as secondary.
- OCI health checks monitor both endpoints.
- If both are healthy, DNS traffic is steered to the primary.
- If the primary goes down (or its answer is unhealthy), traffic is automatically sent to the secondary.

---

### Load Balancer Policy

Use this to distribute DNS traffic across multiple endpoints.

**How it works:**
- You can configure weights for each endpoint.
  - 50/50 → traffic distributed evenly.
  - Custom (e.g., 10/90) → 10% to Endpoint 1, 90% to Endpoint 2.
- If one endpoint becomes unhealthy, traffic is sent to the healthy endpoint.

> Note: This is about DNS traffic distribution.

---

### Geolocation Steering

These policies distribute DNS traffic based on where the end user is located.

**Example:**
- A user in a particular country → routed to a specific endpoint.
- A user in Australia → routed to a separate endpoint.

---

### Steering

Steers traffic based on ASN (Autonomous System Numbers).

- A DNS query originating from a specific ASN can be steered to a specific endpoint.

---

### IP Prefix Steering

Enables customers to steer DNS traffic based on the IP prefix of the originating query.

---







