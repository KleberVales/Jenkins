# Networking ‐ Connectivity


# 1. VCN Connectivity Options

Connectivity deals with the connection between VCNs, which can be VCN to VCN in the same region, VCN to VCN in different regions, and VCN to on-premises.

# 2. VCN to VCN

Peering means connecting two virtual cloud networks so they can communicate with each other. Connections can use either a public IP address (public internet) or a private IP address (private direct access).

## Local VCN Peering

Local VCN peering privately connects two VCNs in the same region. Instead of communicating over the public internet using public IP addresses, peered instances communicate through private IP addresses.

A VCN can have up to 10 local peering gateways and can be attached to one dynamic routing gateway.

To configure it using local peering gateways, you need non-overlapping CIDRs, one LPG on each VCN, a connection between the two LPGs, route rules on both sides, and supporting security rules.

To configure it using an upgraded dynamic routing gateway, you still need non-overlapping CIDRs, but instead of two LPGs, you create one DRG and attach it to both VCNs.

If VCNs are peered, one VCN cannot use the other VCN’s internet gateway for outbound internet traffic, although it can receive traffic in the reverse direction.

Local peering can be configured in two ways:  

- Local Peering Gateway (LPG)

  This approach connects two VCNs within the same region privately. Provides extremely high bandwidth and super low latency.

  For this type of connection, you need to create a LPG for each VCN and connect them.
  
- Dynamic Routing Gateway (DRG)
  
  A DRG can be attached to up to 300 VCNs. The single DRG is attached to both VCN 1 and VCN 2.

  - Create VCN 1 and VCN 2 with non-overlapping CIDRs.  
  - Create a single dynamic routing gateway.  
  - Attach the DRG to both VCN 1 and VCN 2.  
  - Configure route tables:  
     - For a subnet in VCN 1:  
          Destination: VCN 2 CIDR, such as 192.168.0.0/16  
          Target: Dynamic Routing Gateway  
     - For a subnet in VCN 2:  
          Destination: VCN 1 CIDR, such as 10.0.0.0/16  
          Target: Dynamic Routing Gateway  
   - Configure security rules.

## Remote VCN Peering

Remote peering connects VCNs in different regions. To make this connection, one uses   Dynamic Routing Gateway (DRG) to facilitate communication between VCNs in different regions.

so that their resources can communicate privately using private IP addresses without sending traffic over the internet.

To set it up, you need two VCNs with non-overlapping CIDRs, a dynamic routing gateway attached to each VCN, a remote peering connection on each DRG, and a connection between the two RPCs.

You must also configure route tables and security lists. Remote peering connections are typically used across different regions but can technically be used within the same region.

legacy DRGs only support remote peering within the same tenancy, while upgraded DRGs support both same-tenancy and cross-tenancy connections.

To configure remote VCN peering, you need:

- Two VCNs with non-overlapping CIDRs in different regions.
- A dynamic routing gateway (DRG) created and attached to each VCN.
- A remote peering connection (RPC) created on each DRG.
- A connection between the two RPCs.
- Supporting route tables and security lists.
  
In short:

- Region 1 contains VCN-1.
- Region 2 contains VCN-2.
- VCN-1 is attached to DRG-1.
- VCN-2 is attached to DRG-2.
- DRG-1 has RPC-1.
- DRG-2 has RPC-2.
- A connection is established between RPC-1 and RPC-2.

# 3. VCN to On-Premises

## Public Internet

Communication between the on-premises network and the VCN occurs via the public internet. To achieve this, you use specific gateways within the VCN:

**Internet Gateway (IGW):** allows resources with public IP addresses in the VCN to communicate directly with the internet (and therefore with the on-premises environment).

**NAT Gateway:** allows resources without public IP addresses (in private subnets) to initiate outbound connections to the internet without being directly accessible from the outside.

In other words, traffic leaves the VCN → passes through the gateway → traverses the public internet → reaches the on-premises environment (and vice versa, depending on the configuration).

**Use Case**

Proof of Concept (POC): useful for quick tests, validations, and temporary environments, precisely because it is simple to configure and does not require dedicated links.

SaaS Applications: when accessing services exposed on the internet, using an IGW/NAT Gateway is the natural approach.

**Security**

Traffic is unencrypted by default when traversing the internet.

It is possible to apply application-level encryption (e.g., HTTPS/TLS, SSH), but this depends on the application—it is not network protection.

Therefore, it is not recommended for sensitive data or critical workloads without additional layers of protection.

## Site-to-Site VPN

Site-to-Site, also known as VPN or IPsec VPN, provides an encrypted IPsec connection between an on-premises environment and an OCI VCN.

The connection uses the public internet but keeps traffic encrypted. IPsec has two modes: transport mode, which encrypts only the payload, and tunnel mode, which encrypts and authenticates the entire packet; Oracle supports tunnel mode.

Site-to-Site VPN is cost effective, quick to set up, encrypted, and provides two redundant tunnels per connection.

Routing can be static, dynamic using BGP, or policy-based, and it is configured per tunnel.

**In a Site-to-Site VPN setup:**

- The on-premises side has a Customer Premises Equipment (CPE) device.
- The OCI side has a Dynamic Routing Gateway (DRG).
- An IPsec connection is established between them.

**IPsec Modes**

To understand IPsec modes, first distinguish between:

Header – like an envelope or box; contains packet information such as origin IP and destination IP.
Payload – the actual data or content.

Transport Mode

- The header stays intact.
- IPsec encrypts only the payload.

Tunnel Mode

- IPsec encrypts and authenticates the entire packet.
- This includes both the header and the payload.

Important: Oracle supports tunnel mode.

**Customer Premises Equipment (CPE) and CPE Object**

The Customer Premises Equipment (CPE) can be:

- Software-based
- Hardware-based

The CPE lies on the on-premises side. However, when creating a Site-to-Site VPN connection in OCI, you must provide a virtual representation of the CPE on the OCI side. This virtual representation is called the CPE object.

The CPE has a public IP address. This public IP address can have up to eight IPsec connections.

**IPsec Connection**

An IPsec connection connects the CPE object to the Dynamic Routing Gateway on the OCI side. It represents the Site-to-Site VPN connection.

Each IPsec connection has two tunnels for redundancy. Each tunnel includes configuration information such as:

- A public IP address
- A shared secret
  
This configuration information is required when configuring the CPE on the on-premises side.

**NAT and IKE Considerations**

Sometimes the CPE device may lie behind a NAT device on the on-premises side.

By default, Oracle uses the CPE’s public IP address. However, if the CPE is behind a NAT device, you need to change the CPE Internet Key Exchange (IKE) identifier and point it to the CPE’s private IP address.

Oracle supports both:

- IKE version 1
- IKE version 2

**CPE Object**

When creating an IPsec connection, you need a CPE object. This object is the virtual representation of your on-premises router. To create a CPE object, you provide:

- A name
- A public IP address

A single CPE object public IP can have up to eight IPsec connections.

## FastConnect

Oracle FastConnect is a dedicated, private connection between an on-premises environment and Oracle Cloud Infrastructure, offering higher bandwidth, greater reliability, and more consistent network performance.

### A. Physical vs. Logical Connection

FastConnect has two aspects:

- Cross-Connect

  Cross-connect is the physical connection in FastConnect. It is a physical cable connecting your network to Oracle in a FastConnect location.

- Virtual Circuit

  A virtual circuit is the logical connection in FastConnect. It represents an isolated network path that runs over the physical network.

  There are two types of virtual circuits:

  - Public virtual circuit: Used for a dedicated connection between an on-premises network and public OCI services.
  - Private virtual circuit: Used for private communication between an on-premises environment and a Virtual Cloud Network.
 
### B. Three Connectivity Models

**Oracle partner**

An Oracle partner is a network service provider that has already established a connection with the FastConnect location. Because this connection already exists, there is no need for a Letter of Authorization (LOA).

You work with the Oracle partner to provision a physical connection from your data center to the partner edge inside the FastConnect location. You can think of this as connecting through the partner network.

Configuration

- The OCI region contains a VCN with a private subnet and a public subnet.
- The FastConnect location contains an Oracle cage with an edge device.
- The customer data centers contain Customer Premises Equipment (CPE).
- In the case of an Oracle partner, the partner has already established redundant cross-connects with the FastConnect edge devices. Because of this, no Letter of Authorization (LOA) is needed.

**Third-party provider**

FastConnect with a third-party provider uses a network carrier that is not an Oracle partner to establish the physical connection between your on-premises environment and the Oracle FastConnect edge devices.

In this model, the third-party provider is responsible for the physical connection from your data center to the provider and for setting up the physical fiber cross-connect in the FastConnect location on your behalf.

Oracle provides a Letter of Authorization, which you forward to the third-party provider so they can provision the circuit. This model is suitable if you already have a relationship with a third-party provider or if Oracle partners do not serve your data center.

Configuration

- The OCI region contains the VCN.
- The FastConnect location contains the Oracle cage and FastConnect edge devices.
- A third-party provider (network carrier) is responsible for the physical connection between the on-premises environment and the Oracle FastConnect edge devices.

The third-party provider is not an Oracle partner.

- Physical Connections
   - Connection from the customer data center to the provider  
      -  This connects your on-premises environment to the third-party provider.
   - Physical fiber connection (cross-connect) in the FastConnect data center  
      - The third-party provider sets up this cross-connect in the FastConnect location on your behalf.

- Role of the Letter of Authorization (LOA)
  
In this model, Oracle provides you with a Letter of Authorization (LOA) . You must provide this LOA to the third-party provider. The third-party provider then uses it to provision the circuit for you.

**Co-location with Oracle**

FastConnect co-location with Oracle is a connectivity model where the customer already has a presence in the FastConnect data center location, with their own cage and Customer Premises Equipment deployed there.

This model is suitable when you already have equipment in an Oracle FastConnect location. To establish the connection, you create a direct cross-connect between your edge and the FastConnect edge device in the Oracle cage.

Oracle provides a Letter of Authorization, which you hand over to the data center provider to configure the direct cross-connect.

This model offers a direct, high-bandwidth, private connection for customers who already have infrastructure in an Oracle FastConnect location.

- Configuration

   - OCI region containing the VCN.
   - FastConnect location containing:
      - Oracle cage with FastConnect edge devices.
      - Customer cage with Customer Premises Equipment.
   - Customer data center (separate from the FastConnect location).
     
In co-location, you establish a redundant physical connection (cross-connect) between the customer equipment and the FastConnect edge device deployed in the Oracle cage.

**Redundancy Best Practices**

FastConnect redundancy best practices focus on eliminating single points of failure.

Layer 2 vs. Layer 3 Connections

Partner connections come in two types:

Layer 2 Connection  
The FastConnect BGP session is directly between the customer edge and Oracle.  
At a minimum, each partner has two separate physical connections to Oracle.  
Each virtual circuit should go to a different physical router in the FastConnect location.

Layer 3 Connection  
There is only a single virtual circuit, and it is automatically redundant and diverse.  
The FastConnect BGP session is between the customer edge and the Oracle Partner.  
Separate redundant BGP sessions already exist between Oracle and the Oracle Partner.  
The customer must ensure that the connection between the customer edge and the provider is redundant and diverse.



### C. BGP 

Border Gateway Protocol (BGP) is the routing protocol of the internet. It determines the best network routes for data traveling across multiple networks.

An autonomous system is a network controlled by a single administrative entity, and each autonomous system has an autonomous system number.

BGP peers sit at the edge of autonomous systems and exchange routing information with neighboring peers.

BGP performs route discovery and path selection, choosing the best path based on available routes—such as selecting the path with fewer hops in the example from AS 1 to AS 3.

In OCI, BGP is especially important because Site-to-Site VPN can use BGP, static routing, or both, while FastConnect always uses BGP for route advertisements.

**Autonomous Systems and BGP Peers**

An autonomous system (AS) can be thought of as a smaller network under the control of a single administrative entity. It is essentially a collection of connected IP networks managed by one organization.

Each autonomous system has an autonomous system number (ASN). For example, you may have AS 1 and AS 2.

A BGP peer is a device on the edge or boundary of an autonomous system. BGP peers exchange routing information with neighboring BGP peers. This exchange helps BGP discover routes and choose the best path.

**How BGP Works**

BGP performs two major functions:

a. Route discovery – It learns about available network paths.  
b. Path selection – It chooses the best route for traffic.

When data moves from source to destination, it may travel through several networks. BGP evaluates the possible paths and determines the best network route.

### D. Dynamic Routing Gateway 

Um Gateway de Roteamento Dinâmico (DRG) é um roteador virtual que pode ser conectado a VCNs, conexões de peering remotas, túneis IPsec, circuitos virtuais e conexões de loopback.

O roteamento em um DRG é controlado pelas tabelas de roteamento e distribuições de rotas do DRG.

Quando um DRG é criado, duas tabelas de roteamento são criadas automaticamente: uma para conexões RPC, circuitos virtuais e IPsec, e outra para conexões VCN.

## Transit Routing Scenarios

Transit routing enables an on-premises network to communicate with multiple VCNs through a single connection to a hub VCN, avoiding the cost and overhead of multiple Site-to-Site VPN or FastConnect connections.

The hub-and-spoke topology uses a centralized hub VCN that connects to the on-premises network via Site-to-Site VPN or FastConnect, while spoke VCNs connect to the hub via local peering gateways.

All hub and spoke VCNs must be in the same region, though they can be in different tenancies. To implement transit routing, you must configure route tables for each subnet, the DRG, and each local peering gateway.

**Hub-and-Spoke Topology**

In transit routing, you use a hub-and-spoke topology:

Hub VCN: A centralized VCN that connects to the on-premises network.  
Spoke VCNs: Other VCNs that connect to the hub VCN via local peering.

- Architecture
  
The on-premises environment contains a Customer Premises Equipment (CPE).  
The hub VCN connects to the on-premises network using either Site-to-Site VPN or FastConnect.  
Each spoke VCN connects to the hub VCN using a local peering gateway (LPG) .



