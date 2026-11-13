# IP Addressing Plan 

**project:** Design, Implementation & Hardening of a Multi-Site Enterprise Network 
**Version:** 1.0

## 1. Purpose 
  
This document defines the IP addressing scheme for the whole network: sites,
VLANs, WAN links, and router loopbacks. Every later phase (routing, security,
monitoring, automation) is built on top of it.
## 2. Design Rules

- Each site gets its own '/16' block, and the **second octet identifies the site**.
- **VLAN ID = third octet**, so VLAN 20 is always 'x.x.20.0/24'.
- The default gateway of every VLAN os the **'.1'** address.
- Point-to-point WAN links use '/30'; router loopbacks use '/32' .
- Unused switch ports go into VLAN 999 and are shut down. 
- Trunk native VLAN is 998 (unused, carries no traffic).


## 3.Sites

| Site | Location | Block | Role |
|------|----------|-------|------|
| HQ | Main office | 10.10.0.0/16 | core site: all department, servers, DMZ |
| Branch 1 | Amman | 10.10.0.0/16 | Branch: staff and network management |
| Branch 2 | Irbid | 10.30.0.0/16 | Branch: staff and network management |

## 4. HQ VLANs 

| VLAN | Name | Network | Gateway | Purpose |
|------|------|---------|---------|---------|
| 10 | Management-staff | 10.10.10.0/24 | 10.10.10.1 | Mangement staff user | 
| 20 | Finance | 10.10.20.0/24 | 10.10.20.1 | Finance department (restricted access) |
| 30 | IT | 10.10.30.0/24 | 10.10.30.1 | IT team |
| 40 | Servers | 10.10.40.0/24 | 10.10.40.1 | Internal servers |
| 50 | Guest | 10.10.50.0/24 | 10.10.50.1 | Guests: internet only |
| 99 | Network-Mgmt | 10.10.99.0/24 | 10.10.99.1 | Management of network devices |
| 100 | DMZ | 10.10.100.0/24 | 10.10.100.1 | Servers exposed to the internet |
| 998 | Native | none | none | Trunk native VLAN (unused) |
| 999 | Blackhole | none | none | Unused ports (shutdown) |

## 5. Branch VLANs
 
| Site | VLAN | Name | Network | Gateway |
|------|------|------|---------|---------|
| Branch 1 | 10 | Staff | 10.20.10.0/24 | 10.20.10.1 |
| Branch 1 | 20 | Finance | 10.20.20.0/24 | 10.20.20.1 |
| Branch 1 | 99 | Network-Mgmt | 10.20.99.0/24 | 10.20.99.1 |
| Branch 2 | 10 | Staff | 10.30.10.0/24 | 10.30.10.1 |
| Branch 2 | 20 | Finance | 10.30.20.0/24 | 10.30.20.1 |
| Branch 2 | 99 | Network-Mgmt | 10.30.99.0/24 | 10.30.99.1 |

## 6. WAN Links 

| Link | Network | Our router | ISP side |
|------|---------|------------|----------|
| HQ - ISP | 203.0.113.0/30 | 203.0.113.1 | 203.0.113.2 |
| Branch 1 - ISP | 203.0.113.4/30 | 203.0.113.5 | 203.0.113.6 |
| Branch 2 - ISP | 203.0.113.8/30 | 203.0.113.9 | 203.0.113.10 |

## 7. Router Loopbacks

| Router | Loopback | Used for |
|--------|----------|----------|
| HQ-R1 | 10.255.0.1/32 | Router ID (OSPF/BGP) |
| BR1-R1 | 10.255.0.2/32 | Router ID (OSPF/BGP) |
| BR2-R1 | 10.255.0.3/32 | Router ID (OSPF/BGP) |

## 8. Notes

- 203.0.113.0/24 is a documentation range (TEST-NET-3), which is fine for a lab.
- Router names above are placeholders; rename them to match your GNS3 topology.
