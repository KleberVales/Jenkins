# Networking ‐ Load Balancer

# 1. Load Balancer

A load balancer acts as a single entry point between clients and multiple backend servers, distributing incoming traffic to ensure no single server is overwhelmed.

Its three key benefits are scaling, resource utilization, and high availability.

Load balancers come in two types: public (reachable from the internet via a public IP) and private (visible only within the Virtual Cloud Network via a private IP).

# 2. Public Load Balancer

Has a public IP address and is reachable from the internet.

# 3. Private Load Balancer

Has a private IP address taken from the hosting subnet where it resides. It is only visible from within the Virtual Cloud Network (VCN).

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


