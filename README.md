# Networking ‐ Load Balancer

A load balancer acts as a single entry point between clients and multiple backend servers, distributing incoming traffic to ensure no single server is overwhelmed.

Its three key benefits are scaling, resource utilization, and high availability.

Load balancers come in two types: public (reachable from the internet via a public IP) and private (visible only within the Virtual Cloud Network via a private IP).

## Public Load Balancer

Has a public IP address and is reachable from the internet.

## Private Load Balancer

Has a private IP address taken from the hosting subnet where it resides. It is only visible from within the Virtual Cloud Network (VCN).

## fundamental concepts

### 1. backend servers 

Backend servers are the servers placed behind the load balancer that actually generate the content. The load balancer receives incoming traffic (which can be TCP or HTTP) and forwards or distributes it to these backend servers.

### 2. backend sets

A backend set is a logical entity — essentially a list of backend servers (e.g., Backend Server 1, Backend Server 2). It also includes:

- The health check policy
- The load balancing policy

So a backend set = list of backend servers + health check policy + load balancing policy.

### 3. health check policies 

A health check policy is a test that confirms whether a backend server is available and healthy. If a backend server fails the test, the load balancer takes it out of rotation. There are two ways to perform a health check:

- TCP level – a connection attempt
- HTTP level – a request sent to a specific URI, with the response validated
  
The primary purpose is to determine whether backend servers are available and healthy.

### 4. listeners

### 5. load balancing policy

### 6. Shape of the Load Balancer

### 7. SSL

### 8. Session Persistence

### 9. Certificates
