# NetCore Systems Ltd. — Enterprise Network Design and Security

![Cisco Packet Tracer](https://img.shields.io/badge/Simulator-Cisco%20Packet%20Tracer-blue)
![Networking](https://img.shields.io/badge/Domain-Enterprise%20Networking-informational)
![Security](https://img.shields.io/badge/Focus-ACL%20Security-red)
![Project](https://img.shields.io/badge/Type-Capstone%20Project-success)

## 📌 Project Overview

This capstone project focuses on the design, deployment, and security configuration of a multi-department enterprise network for **NetCore Systems Ltd.** using Cisco Packet Tracer.

The network connects three organizational departments: Information Technology (IT), Sales, and Human Resources (HR). Each department operates on a separate IP subnet, with a Cisco router providing inter-network connectivity and a centralized IT server hosting HTTP and FTP services.

The primary security objective is to restrict ICMP traffic originating from the Sales subnet to the IT subnet without preventing Sales users from accessing essential server services. This is accomplished through an Extended Access Control List (ACL) configured on the router.

The project demonstrates practical networking concepts, including IP addressing, subnetting, default gateways, DHCP-based host configuration, inter-subnet communication, router interface configuration, access control, and connectivity verification.

## 🎯 Project Objectives

The objectives of this project are to:

- Design a multi-department enterprise network using Cisco Packet Tracer.
- Establish network connectivity between IT, Sales, and HR.
- Assign separate IPv4 subnets to each department.
- Configure a centralized IT server for HTTP and FTP services.
- Support dynamic IP addressing for Sales and HR hosts through DHCP.
- Implement an Extended ACL to restrict ICMP traffic from Sales to IT.
- Permit other IP traffic under the documented ACL policy.
- Verify that Sales cannot ping the IT network while retaining access to the IT server's web and file-transfer services.
- Confirm that HR retains connectivity to the IT network.
- Document the topology, addressing plan, security policy, and verification results.

## 🛠️ Tools and Technologies

| Technology | Purpose |
|---|---|
| Cisco Packet Tracer | Network design, simulation, configuration, and testing |
| Cisco 2911 Router | Inter-subnet routing and ACL enforcement |
| Cisco 2960 Switches | Departmental LAN connectivity |
| IPv4 addressing | Identification and separation of departmental networks |
| DHCP | Dynamic host addressing for Sales and HR |
| Extended ACLs | Filtering traffic based on source, destination, and protocol |
| ICMP | Network reachability testing using ping |
| HTTP | Access to the web service hosted on the IT server |
| FTP | File-transfer service hosted on the IT server |

## 🏗️ Network Architecture

The simulated enterprise network consists of three departmental LANs interconnected through a Cisco 2911 router.

Each department has a dedicated Cisco 2960 switch. The IT department contains the centralized server, while Sales and HR use PCs configured for dynamic IP addressing.

### Hardware Components

The network uses the following equipment:

| Device | Quantity | Role |
|---|---:|---|
| Cisco 2911 Router | 1 | Interconnects departmental networks and enforces traffic filtering |
| Cisco 2960 Switches | 3 | Provide LAN connectivity for the departments |
| PCs | 6 | Represent departmental end-user devices |
| Server | 1 | Hosts HTTP and FTP services for the enterprise network |

**Total simulated devices:** 11.

### Logical Network Design

The high-level logical architecture is:

```text
                     +----------------------+
                     |  Cisco 2911 Router   |
                     | Inter-subnet Routing |
                     |    Extended ACL      |
                     +----------+-----------+
                                |
              +-----------------+-----------------+
              |                 |                 |
        +-----+------+    +-----+------+    +-----+------+
        | IT Switch  |    |Sales Switch|    | HR Switch  |
        | Cisco 2960 |    |Cisco 2960  |    |Cisco 2960  |
        +-----+------+    +-----+------+    +-----+------+
              |                 |                 |
        +-----+------+    +-----+------+    +-----+------+
        | IT Server  |    | Sales PCs  |    | HR PCs     |
        | HTTP / FTP |    | DHCP Hosts |    | DHCP Hosts |
        +------------+    +------------+    +------------+
```

*Note: This is a logical representation of the documented network, not an exact physical-port diagram. Refer to the Cisco Packet Tracer topology for the actual cable and interface connections.*

## 🌐 IP Addressing Plan

Each department is assigned a dedicated `/24` IPv4 subnet. This separates departmental broadcast domains at Layer 3 and enables routing policies to be applied between the networks.

| Department | Network Address | Subnet Mask | Default Gateway | Host Configuration |
|---|---|---|---|---|
| IT | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | IT server uses `192.168.10.2` |
| Sales | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | Dynamic DHCP hosts |
| HR | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | Dynamic DHCP hosts |

### IT Department

- Network: `192.168.10.0/24`
- Default gateway: `192.168.10.1`
- Server address: `192.168.10.2`
- Services: HTTP and FTP

The IT subnet contains the centralized server that provides web and file-transfer services to authorized network clients.

### Sales Department

- Network: `192.168.20.0/24`
- Default gateway: `192.168.20.1`
- Host addressing: DHCP

Sales clients receive their IP configuration dynamically. Traffic originating from this subnet is subject to the project's ICMP restriction when directed toward the IT subnet.

### Human Resources Department

- Network: `192.168.30.0/24`
- Default gateway: `192.168.30.1`
- Host addressing: DHCP

HR clients receive their IP configuration dynamically. The documented verification confirms successful ping connectivity between an HR PC and the IT network.

## 🔐 Security Design: Extended Access Control List

### Security Objective

The central security requirement is to **block ICMP traffic from the Sales subnet to the IT subnet while permitting other IP traffic** under the configured ACL policy.

This allows the organization to restrict a specific type of network communication without unnecessarily disabling access to the IT server's web and file-transfer services.

### ACL Policy

The documented policy contains two principal rules:

| Rule | Source | Destination | Protocol or Traffic | Action |
|---|---|---|---|---|
| 1 | `192.168.20.0/24` | `192.168.10.0/24` | ICMP | Deny |
| 2 | Any source | Any destination | Other IP traffic allowed by the ACL | Permit |

The second rule corresponds to the documented policy `permit ip any any`.

### Why Use an Extended ACL?

An Extended ACL can filter traffic based on criteria such as:

- Source IP address
- Destination IP address
- Protocol, such as ICMP, TCP, or UDP
- Transport-layer ports when applicable

This provides more granular control than a basic rule that filters traffic solely by source or destination.

For this project, the ACL distinguishes ICMP traffic from other IP traffic. This is important because the network must restrict ping requests from Sales while allowing Sales PCs to access the HTTP service on the IT server.

### Expected Security Behavior

| Traffic Flow | Expected Behavior | Documented Result |
|---|---|---|
| Sales subnet → IT subnet using ICMP | Block | Blocked |
| Sales PC → IT server HTTP service | Permit | Allowed |
| Sales PC → IT server FTP service | Permit | Allowed |
| HR PC → IT network | Permit | Successful connectivity |

The table reflects the project's stated results. The FTP result is reported in the project summary; the HTTP result is additionally supported by the documented web-access verification.

### ACL Application

The project documentation identifies an inbound security filter on router interface `GigabitEthernet0/1`.

The ACL is described as being applied to the Sales gateway interface, with inbound application confirmed in the router-interface configuration output.

An inbound ACL evaluates traffic as it enters the router through the interface on which the ACL is applied. For this policy to work as intended, the configured interface and direction must match the actual topology and the direction of Sales-originated traffic.

**Important:** The exact ACL identifier, full router configuration, and interface IP assignments are not reproduced in the project summary. Consult the original Packet Tracer configuration before assuming these details.

## ⚙️ Implementation Workflow

The following workflow summarizes the project's network deployment and security implementation.

### Step 1: Build the Network Topology

Open Cisco Packet Tracer and create the enterprise network using:

- One Cisco 2911 router
- Three Cisco 2960 switches
- Six PCs
- One server

Connect the departmental LANs through the router according to the intended topology.

### Step 2: Configure Departmental IP Addressing

Use the addressing plan to establish the IT, Sales, and HR subnets.

The gateway addresses are:

```text
IT Gateway:     192.168.10.1
Sales Gateway:  192.168.20.1
HR Gateway:     192.168.30.1
```

The IT server uses:

```text
IT Server IP:   192.168.10.2
```

Configure the router interfaces and end devices to match the actual topology.

### Step 3: Configure DHCP for Sales and HR

Sales and HR hosts are documented as dynamically addressed.

Configure DHCP so that clients receive suitable IP addresses and the correct departmental default gateway. The project summary does not specify the DHCP pool ranges, lease settings, or DNS configuration, so these should be taken from the original Packet Tracer configuration.

### Step 4: Configure the IT Server

Configure the IT server with the documented IP address `192.168.10.2`.

Enable the HTTP and FTP services in Cisco Packet Tracer and verify that the services are available on the server.

The web portal is accessed using:

`http://192.168.10.2`

### Step 5: Implement the Extended ACL

Configure an Extended ACL on the router to:

1. Deny ICMP traffic from `192.168.20.0/24` to `192.168.10.0/24`.
2. Permit other IP traffic according to the documented `permit ip any any` rule.

Apply the ACL inbound on the appropriate Sales-facing router interface, as supported by the actual topology and the project's interface-configuration evidence.

The precise ACL number or name and the complete IOS commands are not specified in the project summary; therefore, the original configuration should be used as the authoritative implementation reference.

### Step 6: Verify Connectivity and Security

Perform the verification tests described in the following section to confirm that the network behaves according to the intended policy.

## 🧪 Testing and Verification

Testing is essential to confirm both connectivity and security. The project documents several verification activities covering host addressing, permitted connectivity, ICMP restrictions, and HTTP access.

### Test 1: Verify Sales PC IP Configuration

The project includes an IP-configuration screenshot for Sales PC2.

The purpose of this check is to verify that the Sales client has received an appropriate IP configuration for its subnet.

Check the following fields in the PC's IP Configuration window:

- IP address
- Subnet mask
- Default gateway

The expected subnet is `192.168.20.0/24`, and the default gateway is `192.168.20.1`.

The exact dynamically assigned IP address for Sales PC2 is not stated in the project summary.

### Test 2: Verify HR-to-IT Connectivity

The project records successful ping connectivity between an HR PC and the IT network.

A representative verification command, when executed from a compatible terminal, is:

```bash
ping 192.168.10.2
```

In Cisco Packet Tracer, the equivalent test can be performed using the PC's Command Prompt.

**Recorded outcome:** Successful ping between the HR PC and the IT network.

This demonstrates that the documented ICMP restriction does not prevent the tested HR-to-IT communication.

### Test 3: Verify the Sales ICMP Restriction

From a Sales PC, test connectivity to the IT server:

```bash
ping 192.168.10.2
```

**Expected and documented outcome:** The ping is blocked by the Extended ACL.

This confirms the intended restriction on ICMP traffic originating from the Sales subnet and directed toward the IT subnet.

A failed ping alone is not definitive proof of an ACL block; the result should be interpreted alongside the documented ACL configuration and router-interface evidence.

### Test 4: Verify Router ACL Application

Inspect the router's interface configuration and ACL assignment in Cisco Packet Tracer.

The project includes router configuration output confirming inbound application of the security filter on `GigabitEthernet0/1`.

This provides evidence that the policy is attached to a router interface rather than merely being defined without application.

### Test 5: Verify HTTP Access from Sales

From a Sales PC, open a web browser and navigate to:

`http://192.168.10.2`

**Documented outcome:** Sales PCs can access the web service hosted on the IT server despite the ICMP restriction.

This is an important verification because it demonstrates that restricting ping traffic does not automatically disable the permitted HTTP service.

### Test 6: Verify FTP Access from Sales

The project summary reports that FTP access from Sales to the IT server remains available.

The precise FTP client steps, credentials, and transfer results are not included in the summary. Use the original Packet Tracer configuration and screenshots to reproduce the exact test.

## 📊 Verification Results Summary

| Test | Source | Destination | Service or Protocol | Result |
|---|---|---|---|---|
| Sales IP configuration | Sales PC2 | Local configuration | DHCP/IP configuration | Configuration screenshot documented |
| HR connectivity | HR PC | IT network | ICMP | Successful |
| Sales security restriction | Sales subnet | IT subnet | ICMP | Blocked |
| Router security configuration | Router interface | `GigabitEthernet0/1` | Inbound ACL | Application confirmed |
| Sales web access | Sales PC | `192.168.10.2` | HTTP | Allowed |
| Sales file-transfer access | Sales PC | IT server | FTP | Reported as allowed |

## 📸 Project Evidence

The original capstone summary contains screenshots supporting the configuration and verification activities.

For a GitHub repository, organize the original screenshots into a directory such as:

```text
netcore-enterprise-network/
├── README.md
├── screenshots/
│   ├── sales-pc2-ip-configuration.png
│   ├── hr-to-it-ping.png
│   ├── sales-icmp-restriction.png
│   ├── router-acl-interface.png
│   └── sales-http-access.png
└── packet-tracer/
    └── netcore-enterprise-network.pkt
```

The filenames above are suggested names for organizing the corresponding evidence. Include the Packet Tracer file only if you have the actual simulation file.

To display an image in GitHub Markdown, use a relative image path, for example:

```markdown
![Sales PC2 IP Configuration](screenshots/sales-pc2-ip-configuration.png)
```

This allows readers to inspect the visual evidence alongside the explanation of each test.

## 🛡️ Security Considerations and Limitations

This project demonstrates protocol-based traffic filtering in a simulated enterprise environment. Several points are important when interpreting its results.

### ICMP Filtering Is Not Complete Network Isolation

Blocking ICMP does not prevent all forms of communication between Sales and IT. Other permitted protocols may still be available, depending on the router configuration and services running on the network.

### The Permit Rule Is Broad

The documented `permit ip any any` rule allows all other IP traffic not previously denied by the ACL, subject to other network controls.

This is consistent with the project's stated objective, but a production enterprise network may require a more restrictive policy based on specific applications, destinations, and ports.

### HTTP and FTP Have Different Security Properties

The project demonstrates HTTP and FTP service availability. It does not document encryption, user authentication policies, or secure file-transfer alternatives.

In production environments, administrators should assess whether encrypted web and file-transfer protocols are appropriate for protecting credentials and data.

### Simulation Results Have a Limited Scope

Cisco Packet Tracer is useful for learning and validating network behavior in a controlled environment. A successful simulated test does not establish that the same configuration is secure under every real-world network condition.

### Verification Should Cover Both Allowed and Denied Traffic

A complete ACL test should confirm that prohibited traffic is blocked and required traffic continues to work. This project follows that approach by testing Sales ICMP restrictions alongside HTTP access and HR connectivity.

## 📚 Key Learning Outcomes

Through this capstone project, the following concepts are demonstrated:

- Enterprise network design using Cisco Packet Tracer.
- IPv4 subnetting and departmental network segmentation.
- Router and switch roles in an enterprise LAN.
- Default gateway configuration.
- DHCP-based addressing for end-user devices.
- Centralized HTTP and FTP service deployment.
- Extended ACL policy design.
- Protocol-specific filtering of ICMP traffic.
- Inbound ACL application on a router interface.
- Verification of permitted and denied network traffic.
- Technical documentation of network configuration and test results.

These skills are relevant to entry-level network engineering, IT support, network administration, and cybersecurity roles.

## 🔮 Potential Future Improvements

The project can be extended in several ways:

1. **More granular ACL rules:** Replace broad permit behavior with explicit policies for required services and destinations.
2. **Expanded service testing:** Document the exact FTP authentication and file-transfer verification steps.
3. **Additional network validation:** Test permitted and denied traffic across multiple hosts in each department.
4. **Configuration documentation:** Include sanitized router and switch configurations alongside the Packet Tracer topology.
5. **Improved network monitoring:** Record ACL counters and interface status to support troubleshooting.
6. **Secure service alternatives:** Investigate HTTPS and secure file-transfer protocols for environments where confidentiality is required.
7. **Evidence-based reporting:** Include original screenshots, a network topology image, and the `.pkt` simulation file when available.

These are suggested enhancements rather than features confirmed as implemented in the supplied capstone summary.

## 🏁 Conclusion

The NetCore Systems Ltd. capstone project demonstrates the design and security configuration of a multi-department enterprise network using Cisco Packet Tracer.

The network connects IT, Sales, and HR through a Cisco 2911 router and three Cisco 2960 switches. It uses separate IPv4 subnets, dynamic host addressing for Sales and HR, and a centralized IT server at `192.168.10.2` hosting HTTP and FTP services.

The principal security control is an Extended ACL that blocks ICMP traffic from the Sales subnet to the IT subnet while permitting other IP traffic under the documented policy. Verification results show that the Sales ICMP restriction is enforced, HR-to-IT ping connectivity succeeds, and Sales users can access the IT server's web service.

Overall, the project demonstrates practical skills in network design, IP addressing, access control, service availability, and network verification. It provides a foundation for more advanced enterprise networking and cybersecurity projects.

## 👤 Author and Project Information

- **Organization:** NetCore Systems Ltd. (project scenario)
- **Project:** Enterprise Network Design and Security
- **Project Type:** Networking and Cybersecurity Capstone
- **Simulation Platform:** Cisco Packet Tracer
- **Core Technologies:** IPv4, DHCP, ICMP, HTTP, FTP, Extended ACLs
- **Status:** Completed — based on the supplied capstone summary

**Author:** Add your name here

---

*This README documents the design and results reported in the supplied capstone project summary. Exact interface addressing, ACL identifiers, complete device configurations, DHCP pool details, and the original simulation file should be added from the Packet Tracer project where available.*

