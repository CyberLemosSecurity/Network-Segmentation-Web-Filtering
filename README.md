🛡️ FortiGate NGFW: Network Segmentation & Web Filtering

   1. Objective
      
This project demonstrates the implementation of a FortiGate Next-Generation Firewall (NGFW) in a virtualized environment to manage and segment traffic between an internal network (LAN) and the external network (WAN/Internet). The primary focus was establishing granular Application Layer control and ensuring full traffic visibility through advanced network diagnostic tools.

   3. Lab Topology & Architecture
      
The infrastructure was segmented via VMware to simulate a real-world corporate environment:
WAN Interface (Port1): Outbound gateway configured via DHCP to receive the external link.
LAN Interface (Port2): Internal network gateway (10.0.1.254/24), acting as the mandatory inspection point.
Endpoints: Windows and Ubuntu stations with static addressing, forcing all data flow through the firewall.

   5. Implementation Phases
1. Traffic Governance & Routing
A critical challenge solved was involuntary Split Tunneling. In hybrid environments (Physical + Virtual), the OS tends to prioritize physical interfaces (Wi-Fi) over virtual ones.

Action: Strategic readjustment of interface metrics (Metric 10 for VMnet) and implementation of manual static routes via CMD (route add) to ensure 100% of test traffic was intercepted by the FortiGate.

2. Firewall Policies (Security Policies)
Unlike home routers, FortiGate operates under the Zero Trust principle (Implicit Deny).

Configuration: Created the Internet_Access policy mapping the LAN (Port2) -> WAN (Port1) flow.

NAT (Network Address Translation): Enabled to allow internal hosts to communicate with the internet under a single public IP.

   3. Web Filtering & SSL Inspection (Layer 7)
Implementation of filtering based on FortiGuard categories.

Security: Blocked the Social Networking category for corporate compliance.

Inspection: Utilized Certificate Inspection mode to analyze SNI headers in HTTPS packets, allowing for precise domain blocking even in encrypted traffic.

 4. Critical Analysis & Technical Diagnostics
As an analyst, validation was not limited to the Graphical User Interface (GUI). CLI-based diagnostic methods were used to ensure flow integrity:

Handshake Validation: Executed the diag sniffer packet port2 'none' 4 command to visualize the TCP Three-Way Handshake (SYN, SYN-ACK, ACK).

Interface Troubleshooting: Identified and corrected a logic error in the firewall rule (initially set as WAN -> WAN), redirecting it to LAN -> WAN to enable edge inspection.

   5.Validation Evidences

Traffic Monitor: CLI console print showing internal host packets traversing the firewall.
<img width="1486" height="288" alt="image" src="https://github.com/user-attachments/assets/58e47082-c688-452f-9f99-91b7891077f5" />

Log Dashboard: Print from the Log & Report > Web Filter menu showing denied access attempts.
<img width="736" height="365" alt="image" src="https://github.com/user-attachments/assets/ccc2fec6-f5d6-4905-a547-be701c76ffe2" />

Endpoint View: Browser print on Ubuntu/Windows displaying the Fortinet Replacement Message (Block Page).

<img width="1107" height="773" alt="image" src="https://github.com/user-attachments/assets/a7893025-1e3d-42f2-8d4e-a2c582512964" />
