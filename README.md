# Networking ‐ Load Balancer

# 1. Load Balancer

A load balancer acts as a single entry point between clients and multiple backend servers, distributing incoming traffic to ensure no single server is overwhelmed.

Its three key benefits are scaling, resource utilization, and high availability.

Load balancers come in two types: public (reachable from the internet via a public IP) and private (visible only within the Virtual Cloud Network via a private IP).

# 2. Public Load Balancer

A public load balancer must be deployed in a public subnet, has a public IP address, is reachable from the internet, and has a regional scope.

It requires either a regional subnet or two AD-specific subnets. To ensure high availability, OCI provisions a primary and standby load balancer in different availability domains — though technically both are equivalent, and you cannot choose which is which.

## Placement Requirements

Key rules for public load balancers:

- A public load balancer must be created in a public subnet — it cannot be created in a private subnet.
- Because it resides in a public subnet, it has a public IP address and can directly receive traffic from the internet.
- The scope is regional.

**Subnet Requirements**

A public load balancer requires either:

- A regional subnet (e.g., Subnet 4), OR
- Two AD-specific subnets (e.g., Subnet 1 and Subnet 2) if a regional subnet is not available.

## Primary and Standby

To ensure high availability, the load balancer service provisions two load balancers — a primary and a standby — each in a different availability domain.

**How Placement Works**

- With a regional subnet: The service places the primary and standby load balancers in different availability domains automatically.
- With two AD-specific subnets: One subnet hosts the primary, and the other hosts the standby.

Important Notes

- Technically, there is no difference between the primary and standby load balancers — from a service standpoint, they are equivalent.
- You cannot specify which load balancer is primary and which is standby. The terms "primary" and "standby" are used only for explanation purposes.

## IP Address Requirements

Whenever you provision a load balancer, it requires:

- One private IP address from its host subnet (for each load balancer — primary and standby).
- One public IP address.

So because there are two load balancers (primary and standby), each requires one private IP from the host subnet, in addition to the public IP.

Floating Public IP and Failover

- The load balancer service attaches a floating public IP to the primary load balancer.
- In case of failover, the IP switches to the standby load balancer.

## Architecture: Regional Subnet

In a regional subnet deployment:

- The VCN spans all availability domains.
- The public load balancer is deployed in the public regional subnet.
- OCI places the primary load balancer in one AD and the standby load balancer in another AD.
- A public IP address is associated with the load balancer.

**Normal operation (no outage):**
- All traffic is distributed to backend servers from the primary (active) load balancer.

**Outage scenario (e.g., AD1 goes down):**
- Traffic is distributed to backend servers from the standby (failover) load balancer.

---

## Architecture: AD-Specific Subnets

In an AD-specific subnet deployment:

- The service provisions the active/primary load balancer in one AD.
- The secondary/standby load balancer is provisioned in another AD.

This forms a load balancer pair, with the same behavior:
- No outage: All traffic flows through the primary load balancer to backend servers.
- Failover: Traffic flows through the standby load balancer.

---


# 3. Private Load Balancer

Because a private load balancer is created in a private subnet, it is assigned a private IP address that acts as the entry point for incoming traffic. 

Unlike a public load balancer, which requires either a regional subnet or two AD-specific subnets, a private load balancer requires only one subnet to host both the primary and standby load balancers. 

Being isolated from the internet, it is accessible only from within the VCN that contains the host subnet. 

In terms of IP addressing, a private load balancer consumes three private IP addresses from its host subnet: one for the primary, one for the standby, and one for the floating private IP address — the private equivalent of the floating public IP used by public load balancers. 

## Subnet Requirement

Unlike a public load balancer — which requires either a regional subnet or two AD-specific subnets — a private load balancer requires only one subnet to host both the primary and standby load balancers.

## Accessibility

Since a private load balancer is isolated from the internet, it is accessible only from within the Virtual Cloud Network (VCN) that contains the host subnet.

## Floating Private IP Address

Recall that in the public load balancer lesson, we discussed the concept of a floating public IP address. In the case of a private load balancer, the equivalent concept is a floating private IP address.

### IP Address Requirements

A private load balancer created in a single subnet consumes three private IP addresses from the host subnet:

1. One for the primary load balancer
2. One for the standby load balancer
3. One for the floating private IP address

The primary and standby load balancers require an extra private IP address that acts as the floating private IP address — bringing the total to three private IP addresses.


# 4. fundamental concepts

## 1. backend servers 

Backend servers are the servers placed behind the load balancer that actually generate the content. The load balancer receives incoming traffic (which can be TCP or HTTP) and forwards or distributes it to these backend servers.

## 2. backend sets

A backend set is a logical entity — essentially a list of backend servers (e.g., Backend Server 1, Backend Server 2). It also includes:

- The health check policy
- The load balancing policy

So a backend set = list of backend servers + health check policy + load balancing policy.

## 3. health check policies 

A health check policy is a test that confirms whether a backend server is available and healthy. If a backend server fails the test, the load balancer takes it out of rotation. There are two ways to perform a health check:

- TCP level – a connection attempt
- HTTP level – a request sent to a specific URI, with the response validated
  
The primary purpose is to determine whether backend servers are available and healthy.

## 4. listeners

A listener checks for incoming traffic on the load balancer's IP address. To configure a listener, you provide:

- Protocol – HTTP, HTTP/2, HTTPS, or TCP
- Port number
  
If you are handling multiple traffic types, you need to configure at least one listener per traffic type.

## 5. load balancing policy

The load balancing policy tells the load balancer how to distribute incoming traffic among the backend servers. There are three policies:

- Round Robin – Incoming traffic is sent sequentially to each server. This is the default policy.
- Least Connection – Traffic is sent to the backend server with the fewest active connections. For example, if Backend 1 has the fewest active connections, the load balancer sends traffic there.
- IP Hash – The incoming source IP address is used as a hashing key. Using this policy, the load balancer routes non-sticky traffic from a particular IP address to the same backend server.

Important note: The load balancing policy applies differently depending on the traffic type:

- TCP load balancers
- Sticky sessions
- Sticky HTTP
- Non-sticky HTTP

## 6. Shape of the Load Balancer

In OCI, load balancers use a flexible shape. This means you must specify two values:

- Minimum value – Specifies instant readiness for the load.
- Maximum value – Gives you control over cost.
  
The "value" here refers to the load balancer's bandwidth. In OCI, you can specify a range from 10 Mbps to 8,000 Mbps. For example, you could set a minimum of 10 Mbps and a maximum of 3,000 Mbps.

## 7. SSL

SSL provides an encrypted link between parties. OCI supports three SSL configurations:

- SSL Termination – The client-to-load-balancer connection is encrypted, but the load balancer decrypts the traffic and sends it unencrypted to the backend instances.
- Point-to-Point SSL – SSL is terminated at the load balancer, but the load balancer then re-initiates SSL to the backend. Both segments (client-to-LB and LB-to-backend) are encrypted.
- SSL Tunneling – The load balancer tunnels incoming SSL traffic directly to the backend/application server without terminating it.

## 8. Session Persistence

Session persistence means that all requests originating from a client are sent to a single backend server. This is useful for scenarios like shopping carts and login sessions, where consistent server handling matters.

There are two ways to enable session persistence:

- Application cookie stickiness
- Load balancer cookie stickiness

## 9. Certificates

When creating a listener that uses HTTPS, you must associate an SSL server certificate with the load balancer. Using this certificate, the load balancer terminates the connection and decrypts the request.

# 5. Network Load Balancer

OCI network load balancer, a load balancing solution that operates at the connection level (Layer 3 and Layer 4). 

Its key characteristics include preserving the source and destination IP addresses and port, providing high throughput with ultra-low latency, and handling volatile traffic patterns — making it ideal for latency-sensitive workloads. 

A major advantage is that it requires no bandwidth configuration because it is elastically scalable by design, automatically adjusting to client traffic demands. 

The network load balancer supports three load balancing policies: the default 5-tuple hash (based on source IP, source port, destination IP, destination port, and protocol), the 3-tuple hash (source IP, destination IP, and protocol), and the 2-tuple hash (source IP and destination IP). 

**Load Balancing Policies**

A network load balancer supports three load balancing policies:

1. 5-Tuple Hash (Default) – Routes incoming traffic based on a 5-tuple, which consists of:
   - Source IP
   - Source port
   - Destination IP
   - Destination port
   - Protocol

2. 3-Tuple Hash – Uses a hash of:
   - Source IP
   - Destination IP
   - Protocol

3. 2-Tuple Hash – Uses a hash of:
   - Source IP
   - Destination IP

# 6. Web Application Acceleration 

Web application acceleration, an OCI service that speeds up traffic on a Layer 7 load balancer using caching and compression. 

### What Is Web Application Acceleration?

Web application acceleration is a service that speeds up traffic on a Layer 7 load balancer. It achieves this through two mechanisms:

1. Caching
2. Compression

By applying caching and compression, web application acceleration improves the performance of traffic flowing through a Layer 7 (HTTP) load balancer.

### How to Work with Web Application Acceleration

**Step 1: Create a Web Application Acceleration Policy**

While creating the policy, you define what it should include. You have two choices:

- Caching only
- Caching and compression

**Step 2: Create an Acceleration Resource**

Creating the policy alone is not enough. To associate (bind) the policy to a specific load balancer, you must create a separate acceleration resource. This resource is what binds the policy to a particular load balancer.

### Caching and Compression Explained

**Caching**

Imagine a Layer 7 load balancer sitting in front of three backend servers. An application user sends a request to the load balancer, which distributes it to the backend pool, and the response is served back to the user.

When web application acceleration is enabled, the load balancer caches some data from the backend responses.

Why cache? Two main reasons:

1. Reduced load on backend servers – Future requests can be served directly from the load balancer.
2. Reduced latency – The request is served from the load balancer rather than traveling to the backend servers.

**Compression**

Compression means that backend server responses are compressed before being returned to the web application user.

Two advantages:

1. Reduced bandwidth required.
2. Increased speed of the server's response over the network.

> Note: Currently, only the gzip compression algorithm is supported by web application acceleration.

---


