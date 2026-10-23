# Networking ‐ Connectivity


# 1. VCN Connectivity Options

Connectivity deals with the connection between VCNs, which can be VCN to VCN in the same region, VCN to VCN in different regions, and VCN to on-premises.

### VCN to VCN

**Local VCN Peering**

Peering means connecting two virtual cloud networks so they can communicate with each other. Connections can use either a public IP address (public internet) or a private IP address (private direct access).

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

  Create VCN 1 and VCN 2 with non-overlapping CIDRs.  
  Create a single dynamic routing gateway.  
  Attach the DRG to both VCN 1 and VCN 2.  
  Configure route tables:  
     For a subnet in VCN 1:  
          Destination: VCN 2 CIDR, such as 192.168.0.0/16  
          Target: Dynamic Routing Gateway  
     For a subnet in VCN 2:  
          Destination: VCN 1 CIDR, such as 10.0.0.0/16  
          Target: Dynamic Routing Gateway  
   Configure security rules.

**Remote VCN Peering**

Configuration:  
- Dynamic Routing Gateway (DRG)


### VCN to On-Premises

**Public Internet**

**Site-to-Site VPN**

**FastConnect**

# 4. BGP Basics

# 5. Dynamic Routing Gateway 

# 6. Site-to-Site VPN 

# 7. Fast Connect Overview

# 8. Fast Connect With an Oracle Partner

# 9. Fast Connect With a Third-Party Provider

# 10. Fast Connect Colocation with Oracle

# 11. FastConnect Redundancy Best Practices

# 12. Transit Routing Scenarios
