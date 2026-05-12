# Enterprise Network Architecture & Troubleshooting Lab

## 📌 Project Overview
This project demonstrates the design, configuration, and troubleshooting of a multi-site enterprise network using Cisco Packet Tracer. The lab simulates a Headquarters (HQ) and a Branch office connected over an ISP core network, integrating advanced routing, switching, and security protocols.

## 🛠️ Technologies & Protocols Implemented
* **LAN & Switching:**
  * **VLANs:** Logical segmentation of broadcast domains.
  * **Inter-VLAN Routing:** Enabling seamless communication between different internal subnets.
  * **EtherChannel (LACP):** Link aggregation for increased bandwidth and redundancy between switches.
* **Dynamic Routing:**
  * **OSPF (Open Shortest Path First):** Interior Gateway Protocol (IGP) used for dynamic routing within the local enterprise sites.
  * **eBGP (External Border Gateway Protocol):** Used for interconnecting the enterprise edge with the simulated ISP core *(Note: iBGP was specifically omitted to accommodate Packet Tracer's simulation limitations)*.
* **Security & Services:**
  * **NAT (Network Address Translation):** Configured PAT (Overload) to allow internal users access to the outside network.
  * **ACLs (Access Control Lists):** Implemented for traffic filtering and crucial NAT exemption mapping.
  * **Site-to-Site IPsec VPN:** Built a secure, encrypted overlay tunnel over the public ISP to bridge the HQ and Branch private networks (Utilizing AES-256, SHA, and DH Group 5).

## 🏗️ Architecture & Traffic Flow
1. **Local Area Network:** Internal traffic is segmented via VLANs and routed dynamically via OSPF. LACP ensures resilient Layer 2 connections.
2. **Edge Connectivity:** The edge routers use NAT for internet-bound traffic. eBGP peers with the ISP routers to advertise public transit IPs and enterprise subnets.
3. **Secure Branch Connectivity:** An IPsec VPN is established between the HQ and Branch edge routers. "Interesting traffic" is caught by an ACL, bypassing NAT, and safely encapsulated to cross the public domain.

## 🔍 Key Challenges & Troubleshooting
During the implementation, several real-world network engineering challenges were encountered and successfully resolved:
* **VPN & NAT Exemption:** Configured precise Access Control Lists to prevent the NAT engine from translating traffic destined for the VPN tunnel, ensuring Phase 1 and Phase 2 traffic flow without interference.
* **Routing Before Encryption:** Diagnosed and resolved an issue where the VPN tunnel (Phase 1 `MM_NO_STATE`) failed to establish due to missing BGP transit routes. Applied the fundamental rule: *The router must have a valid route to the destination before the Crypto Map can intercept and encrypt the packet.*
* **Asymmetrical/One-Way VPN Traffic:** Troubleshot a scenario where IPsec `#pkts encaps` and `#pkts decaps` counters revealed one-way traffic flow. This was ultimately resolved by properly advertising the newly added local subnets into the eBGP process, ensuring a complete and valid return path for the branch side.

## 🎯 Conclusion
This lab successfully demonstrates the interoperability of complex networking technologies. It highlights not just the configuration of these protocols, but the critical order-of-operations (e.g., Routing vs. Encryption, NAT vs. VPN) required to build a robust, secure, and scalable enterprise infrastructure.
