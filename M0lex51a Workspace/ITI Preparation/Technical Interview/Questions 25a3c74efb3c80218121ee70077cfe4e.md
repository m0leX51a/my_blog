# Questions

### Networking Fundamentals

### What is the OSI Model?

The OSI (Open Systems Interconnection) model is a conceptual framework used to understand and standardize the functions of a networking system. It divides network communication into seven abstract layers, each responsible for specific tasks. This model helps in troubleshooting, designing protocols, and ensuring interoperability between different systems. The layers, from bottom to top, are:

1. **Physical Layer (Layer 1)**: Deals with the physical transmission of raw bits over a medium (e.g., cables, signals). It handles hardware aspects like connectors, voltages, and bit rates.
2. **Data Link Layer (Layer 2)**: Provides node-to-node data transfer, error detection/correction, and flow control. It uses MAC addresses for local network communication (e.g., Ethernet frames). Sub-layers include LLC (Logical Link Control) and MAC (Media Access Control).
3. **Network Layer (Layer 3)**: Manages logical addressing (IP addresses) and routing packets across networks. It determines the best path for data (e.g., IP protocol).
4. **Transport Layer (Layer 4)**: Ensures end-to-end communication, reliability, and flow control. Protocols like TCP (reliable) and UDP (unreliable) operate here.
5. **Session Layer (Layer 5)**: Establishes, manages, and terminates sessions between applications. It handles dialog control and synchronization.
6. **Presentation Layer (Layer 6)**: Translates data between the application and network formats (e.g., encryption, compression, data formatting like JPEG or ASCII).
7. **Application Layer (Layer 7)**: Interfaces directly with end-user applications (e.g., HTTP for web browsing, SMTP for email). It provides network services to software.

The OSI model is theoretical and not strictly implemented in real protocols, but it's foundational for understanding networking.

### What is the TCP/IP Model?

The TCP/IP model is a practical, four-layer (sometimes five) framework that describes the core protocols of the internet. It was developed by the U.S. Department of Defense and is the basis for modern networking (e.g., the internet stack). Unlike the OSI model's seven layers, it's more streamlined:

1. **Network Access/Link Layer (Layers 1-2 in OSI)**: Combines physical and data link functions, handling hardware transmission and local addressing (e.g., Ethernet).
2. **Internet Layer (Layer 3 in OSI)**: Focuses on routing and logical addressing using IP (Internet Protocol). It ensures packets reach the destination network.
3. **Transport Layer (Layer 4 in OSI)**: Manages end-to-end delivery with TCP (reliable, connection-oriented) or UDP (fast, connectionless).
4. **Application Layer (Layers 5-7 in OSI)**: Handles high-level protocols like HTTP, FTP, DNS, and SMTP directly for user applications.

A sometimes-added fifth layer is the **Physical Layer** for hardware specifics. The TCP/IP model is implementation-focused and powers the internet, while OSI is more for standardization.

### What is Layer 3?

Layer 3 refers to the Network Layer in the OSI model (or Internet Layer in TCP/IP). It is responsible for logical addressing (using IP addresses), routing (determining paths across networks), and packet forwarding between different networks. Devices like routers operate primarily at this layer, using protocols like IP to enable internetwork communication. Unlike Layer 2 (which is local, using MAC addresses), Layer 3 allows data to travel beyond a single LAN.

### What is an IP Address? How is it Distributed in the Network? What are the Types of IP Addresses?

An IP (Internet Protocol) address is a unique numerical label assigned to every device on a network, enabling it to send and receive data. It acts like a postal address for packets. There are two main versions:

- **IPv4**: A 32-bit address (e.g., 192.168.1.1), divided into four octets (0-255). It supports about 4.3 billion unique addresses, written in dotted decimal notation. It's the most widely used but running out due to address exhaustion.
- **IPv6**: A 128-bit address (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334), hexadecimal and much larger (supports 3.4 × 10^38 addresses). It includes features like auto-configuration and better security, designed to replace IPv4.

**Types of IP Addresses**:

- **Public IP**: Globally unique, routable on the internet, assigned by ISPs (e.g., from IANA blocks). Used for direct internet access.
- **Private IP**: Reserved for internal networks (not routable on the public internet). Defined in RFC 1918: Class A (10.0.0.0–10.255.255.255), Class B (172.16.0.0–172.31.255.255), Class C (192.168.0.0–192.168.255.255). Devices use these in LANs.
- **APIPA (Automatic Private IP Addressing)**: A fallback mechanism (169.254.0.0–169.254.255.255) when DHCP fails. Devices auto-assign these for local communication without a server.
- **Static IP**: Manually assigned and fixed.
- **Dynamic IP**: Automatically assigned, often temporary (e.g., via DHCP).
- **Loopback IP**: 127.0.0.1 (IPv4) or ::1 (IPv6) for testing local machine.

**Distributing IPs in a Network**: IPs are distributed using:

- **Manual Assignment (Static)**: Admin configures IPs directly on devices (e.g., via OS settings). Suitable for servers needing fixed addresses.
- **Dynamic Host Configuration Protocol (DHCP)**: A server automatically assigns IPs, subnet masks, gateways, and DNS from a pool. Clients request via broadcast; server responds with lease (e.g., for 24 hours). This is efficient for large networks.
- **Zero Configuration Networking (Zeroconf)**: For small networks, like APIPA or mDNS (e.g., .local domains).

### Difference Between MAC and IP Addresses

- **MAC Address**: A 48-bit hardware address (e.g., 00:1A:2B:3C:4D:5E) burned into network interfaces by manufacturers. It's unique per device, used at Layer 2 for local (LAN) communication. Divided into 24-bit OUI (Organizationally Unique Identifier) for vendor and 24-bit for device-specific. Not routable beyond local networks.
- **IP Address**: A logical, software-assigned address (Layer 3) for global/routable communication. It can change, unlike MAC. MAC is physical/permanent; IP is virtual/changeable.

MAC handles "who's on the wire locally"; IP handles "where to send across networks."

### Difference Between Private IP and Public IP

- **Private IP**: Used within internal networks (LANs), not visible/routable on the public internet. Saves public addresses; multiple devices can share one public IP via NAT. Ranges are non-overlapping globally.
- **Public IP**: Assigned by ISPs, unique worldwide, directly accessible from the internet. Used for websites, servers, or external communication. Scarce, so often shared via NAT for home/office use.

### Difference Between Private IP and APIPA

- **Private IP**: Manually or dynamically (DHCP) assigned from RFC 1918 ranges for internal use. Requires configuration or a DHCP server.
- **APIPA**: A specific type of private IP (169.254.x.x) auto-assigned by Windows/macOS when DHCP fails (no server response). It's temporary, for link-local communication only (no internet/routing). Private IPs are broader and configurable; APIPA is a fallback subset.

### What is NAT? What Does It Do?

NAT (Network Address Translation) is a technique that maps private IP addresses to a public IP for internet access, conserving public IPs. It operates on routers/firewalls.

- **How it Works**: When a device sends data out, NAT replaces the private source IP/port with the router's public IP/port in the packet header. Responses are reversed. Types include:
    - **Static NAT**: One-to-one mapping (e.g., for servers).
    - **Dynamic NAT**: Pool of public IPs assigned temporarily.
    - **PAT (Port Address Translation, or Overloaded NAT)**: Many privates share one public IP using different ports (most common for homes).
- **Benefits**: Hides internal network, enhances security, allows multiple devices to share one public IP.
- **Drawbacks**: Complicates peer-to-peer apps, can break some protocols.

### What is a Router? Difference Between Router and Switch

- **Router**: A Layer 3 device that connects multiple networks (e.g., LAN to WAN/internet). It uses IP addresses to route packets intelligently based on routing tables/protocols. Features include NAT, firewalling, and path optimization. Essential for internet access.
- **Switch**: A Layer 2 device for connecting devices within a single network (LAN). It uses MAC addresses to forward frames via a MAC address table (learning ports). Faster for local traffic but doesn't route between networks.

**Key Differences**:

| Aspect | Router | Switch |
| --- | --- | --- |
| Layer | 3 (Network) | 2 (Data Link) |
| Addressing | IP (logical) | MAC (physical) |
| Function | Routes between networks | Connects devices in one network |
| Scope | Inter-network (e.g., LAN-WAN) | Intra-network (LAN only) |
| Intelligence | High (path selection, NAT) | Low (fast forwarding) |
| Ports | Fewer, higher speed | Many, for hubs/segments |
| Example Use | Home internet gateway | Office LAN connections |

### What is Routing? What are Its Types? What are Routing Protocols?

Routing is the process of selecting paths in a network along which to send data (packets) from source to destination. Routers maintain routing tables to decide the best path based on metrics like hop count, bandwidth, or cost.

**Types of Routing**:

1. **Static Routing**: Manually configured routes by admins. Simple, secure, but inflexible for large/changing networks.
2. **Dynamic Routing**: Automatically updates routes using protocols. Adapts to changes (e.g., link failures).
3. **Default Routing**: Sends all non-specific traffic to a gateway (e.g., for stub networks).
4. **Classful vs. Classless**: Classful ignores subnet masks (older); classless (CIDR) uses them for efficient allocation.

**Routing Protocols**: Algorithms for dynamic routing, classified as:

- **Interior Gateway Protocols (IGPs)**: For internal networks.
    - **Distance-Vector**: Shares entire tables periodically (e.g., RIP - uses hop count; simple but slow convergence).
    - **Link-State**: Builds topology map (e.g., OSPF - uses Dijkstra's algorithm for shortest path; faster, scalable).
    - **Hybrid**: Combines both (e.g., EIGRP - Cisco proprietary, fast).
- **Exterior Gateway Protocols (EGPs)**: For internet-scale (e.g., BGP - path-vector for internet routing between ASes; policy-based).

### What is VLAN?

A VLAN (Virtual Local Area Network) is a logical segmentation of a physical LAN into multiple broadcast domains at Layer 2. It groups devices based on criteria like department or function, without needing separate hardware. Benefits: Reduces broadcast traffic, improves security (isolates traffic), and enhances manageability. Configured on switches via port assignments or tags (802.1Q standard).

### Difference Between Switchport and Trunk Port

- **Access Port (Switchport)**: A standard switch port assigned to one VLAN. It carries untagged traffic for end devices (e.g., PCs). Only one VLAN per port; used for hosts.
- **Trunk Port**: Carries traffic for multiple VLANs between switches/routers. Uses tagging (e.g., 802.1Q VLAN ID in frames) to identify VLANs. Allows inter-switch VLAN communication; requires configuration to allow specific VLANs.

**Key Differences**:

| Aspect | Access Port (Switchport) | Trunk Port |
| --- | --- | --- |
| VLAN Support | Single VLAN, untagged | Multiple VLANs, tagged |
| Use Case | Connect end devices | Connect switches/routers |
| Tagging | No | Yes (802.1Q) |
| Default | Most ports | Special config needed |

### What is STP?

STP (Spanning Tree Protocol) is a Layer 2 protocol (IEEE 802.1D) that prevents loops in Ethernet networks by creating a loop-free logical topology. Loops cause broadcast storms and MAC table instability.

- **How it Works**: Elects a root bridge (lowest Bridge ID), then builds a tree by blocking redundant paths (ports in blocking/listening/learning/forwarding states). Uses BPDUs (Bridge Protocol Data Units) for communication. Variants: RSTP (Rapid STP, faster convergence), MSTP (Multiple Spanning Trees).
- **Benefits**: Ensures redundancy without loops; reconverges on failures (30-50s in classic STP).

### How Can Different VLANs Communicate with Each Other?

VLANs are isolated broadcast domains, so inter-VLAN communication requires Layer 3 routing (VLANs are L2). Methods:

1. **Router-on-a-Stick**: A router connected to a trunk port on a switch. Subinterfaces on the router (e.g., Gi0/0.10 for VLAN 10) handle each VLAN with IP addresses. Routes between VLANs via the router.
2. **Layer 3 Switch**: A switch with routing capabilities (e.g., Cisco Catalyst). Configure SVIs (Switched Virtual Interfaces) for each VLAN (e.g., interface VLAN 10 with IP). Enables intra-switch routing.
3. **External Router with Multiple Interfaces**: One physical interface per VLAN (less common, port-intensive).
Traffic flow: Device in VLAN A sends to VLAN B → Switch forwards to router/L3 switch → Router routes (changes frame to new VLAN tag) → Forwards to destination VLAN.

### What is DHCP and DNS? What are Their Ports?

- **DHCP (Dynamic Host Configuration Protocol)**: Automates IP assignment in networks. Clients broadcast DISCOVER; server offers IP via OFFER; client requests (REQUEST); server acknowledges (ACK). Provides IP, subnet mask, default gateway, DNS servers. Ports: 67 (server, UDP), 68 (client, UDP).
- **DNS (Domain Name System)**: Translates human-readable domain names (e.g., [google.com](http://google.com/)) to IP addresses. Hierarchical (root → TLD → authoritative servers). Types: Recursive (client asks resolver), Iterative (server refers). Ports: 53 (UDP for queries, TCP for zone transfers/large responses).

### Difference Between Telnet and SSH

- **Telnet**: An insecure protocol for remote terminal access over TCP/IP. Sends data in plaintext (including passwords). Port: 23. Easy but vulnerable to eavesdropping/man-in-the-middle attacks. Deprecated.
- **SSH (Secure Shell)**: Encrypted protocol for secure remote login/command execution/file transfer (SCP/SFTP). Uses public-key cryptography (RSA/DSA) or passwords. Port: 22. Provides confidentiality, integrity, and authentication.

**Key Differences**:

| Aspect | Telnet | SSH |
| --- | --- | --- |
| Security | None (plaintext) | Encrypted (strong) |
| Port | 23 (TCP) | 22 (TCP) |
| Use | Basic remote access | Secure remote management |
| Authentication | Simple username/pass | Keys/passwords |

### What Does SSH Do?

SSH provides secure remote access to devices/servers. It encrypts all communication to prevent interception. Uses: Remote command execution (e.g., ssh user@host), tunneling (port forwarding for secure proxies), file transfer (SFTP/SCP). Supports key-based auth (public/private keys) for passwordless logins. Tunneling example: Forward local port to remote service securely.

### Linux Basics

### How to Install an Application on Linux?

Installation varies by distribution (e.g., Ubuntu/Debian use APT, Fedora/RHEL use DNF/YUM) and package type. Common methods:

1. **Package Managers (Recommended)**:
    - **APT (Debian/Ubuntu)**: Update repo (`sudo apt update`), install (`sudo apt install appname`, e.g., `sudo apt install vim`).
    - **DNF/YUM (Fedora/RHEL)**: `sudo dnf install appname` or `sudo yum install appname`.
    - **Zypper (openSUSE)**: `sudo zypper install appname`.
2. **From Source**: Download tarball, extract (`tar -xvf file.tar.gz`), configure (`./configure`), compile (`make`), install (`sudo make install`).
3. **Flatpak/Snap/AppImage**: Universal formats. E.g., `flatpak install app` or `snap install app`.
4. **RPM/DEB Files**: `sudo dpkg -i file.deb` or `sudo rpm -i file.rpm`.
Always use `sudo` for system-wide installs; check repos first for security.

### Linux Commands

These are essential for file/directory management in the terminal:

- **pwd (Print Working Directory)**: Displays the full path of the current directory (e.g., `/home/user`). Useful for orientation.
- **cd (Change Directory)**: Navigates filesystems. Examples: `cd /path` (absolute), `cd ..` (up one level), `cd ~` (home), `cd -` (previous dir).
- **ls (List)**: Shows files/directories in current path. Options: `ls -l` (detailed, permissions/size), `ls -a` (hidden files), `ls -R` (recursive).
- **cat (Concatenate)**: Displays file contents to stdout. E.g., `cat file.txt`. Also used for creating files (`cat > file.txt`) or combining (`cat file1 file2 > combined.txt`). Not for editing.
- **cp (Copy)**: Copies files/directories. E.g., `cp file1 file2` (copy file), `cp -r dir1 dir2` (recursive for dirs).
- **mv (Move)**: Moves/renames files/directories. E.g., `mv oldname newname` (rename), `mv file /path/` (move).
- **mkdir (Make Directory)**: Creates directories. E.g., `mkdir newdir` (one), `mkdir -p /path/to/nested` (parents if needed).

### Virtualization and Containers

### What is a Hypervisor? What is it Used For? What are Its Types?

A hypervisor is software/firmware that creates and manages virtual machines (VMs) by abstracting hardware resources (CPU, memory, storage) for multiple OSes on one physical host. Used for: Server consolidation (run multiple VMs on one server), testing (isolated environments), disaster recovery, and cloud computing.

**Types**:

1. **Type 1 (Bare-Metal)**: Runs directly on hardware (no host OS). High performance, secure. Examples: VMware ESXi, Microsoft Hyper-V, Xen.
2. **Type 2 (Hosted)**: Runs on a host OS (e.g., as an app). Easier setup but overhead. Examples: VMware Workstation, VirtualBox, Parallels.

### Difference Between Dockerfile, Docker Image, and Container

Docker is a platform for containerization (lightweight VMs sharing host kernel).

- **Dockerfile**: A text file with instructions to build an image (e.g., FROM baseimage, RUN commands, COPY files). It's a blueprint/script.
- **Docker Image**: A read-only, layered template created from a Dockerfile (e.g., `docker build -t myapp .`). Immutable, portable package containing app code, runtime, libraries, and config. Stored in registries like Docker Hub.
- **Container**: A running instance of an image (e.g., `docker run myimage`). Isolated, runtime environment with processes. Can be stopped/started; multiple from one image.

**Key Differences**:

| Aspect | Dockerfile | Docker Image | Container |
| --- | --- | --- | --- |
| Nature | Script/instructions | Static template | Running instance |
| Usage | Build images | Base for containers | Execute apps |
| State | Text file | Immutable layers | Mutable (changes lost on stop unless committed) |
| Example Cmd | N/A (edited in editor) | docker build | docker run |

### DevOps and Cloud Computing

### What is Cloud Computing? How Has It Changed Company Systems?

Cloud computing delivers on-demand IT resources (compute, storage, databases) over the internet, pay-as-you-go, without owning hardware. Models: Scalable, elastic, accessible anywhere.

**Impact on Companies**:

- **Cost Efficiency**: Shift from CapEx (buy hardware) to OpEx (rent resources). Reduced maintenance.
- **Scalability/Agility**: Auto-scale for demand (e.g., e-commerce peaks); faster deployment (CI/CD).
- **Innovation**: Enables AI/ML, big data without upfront investment. Changed from siloed on-prem to hybrid/remote work.
- **Global Reach**: Disaster recovery, multi-region data. Examples: Netflix streams via AWS; startups launch without servers.
Drawbacks: Security concerns, vendor lock-in. Transformed IT from rigid to dynamic.

### What is DevOps?

DevOps is a cultural/philosophical approach integrating Development (Dev) and Operations (Ops) teams to shorten development cycles, improve collaboration, and deliver high-quality software continuously. Principles: Automation (CI/CD pipelines), monitoring, infrastructure as code (IaC). Tools: Jenkins (CI), Docker/Kubernetes (containerization), Ansible/Terraform (automation). Benefits: Faster releases, fewer errors, better feedback.

### Most Famous Cloud Providers

1. **Amazon Web Services (AWS)**: Largest market share; broad services.
2. **Microsoft Azure**: Strong in enterprise, integrates with Windows/Office.
3. **Google Cloud Platform (GCP)**: Excels in data analytics, AI/ML.
Others: IBM Cloud, Oracle Cloud, Alibaba Cloud.

### IaaS, PaaS, SaaS

Cloud service models (from low to high abstraction):

| Model | Description | Examples | Responsibility |
| --- | --- | --- | --- |
| **IaaS (Infrastructure as a Service)** | Provides virtualized hardware (VMs, storage, networks). User manages OS/apps. | AWS EC2, Azure VMs, GCP Compute Engine | Provider: Hardware; User: Everything else |
| **PaaS (Platform as a Service)** | Managed platform for app development (OS, runtime, DB). User focuses on code. | Google App Engine, Heroku, AWS Elastic Beanstalk | Provider: Runtime/DB; User: App code |
| **SaaS (Software as a Service)** | Fully managed apps delivered over web. User just uses. | Google Workspace, Salesforce, Microsoft 365 | Provider: All; User: Data/input |

### Public, Private, and Hybrid Cloud

- **Public Cloud**: Resources owned by third-party (e.g., AWS), shared multi-tenant. Pros: Cost-effective, scalable. Cons: Less control/security.
- **Private Cloud**: Dedicated resources for one organization (on-prem or hosted). Pros: Secure, compliant. Cons: Expensive, less elastic.
- **Hybrid Cloud**: Combines public/private (e.g., sensitive data private, scalable apps public). Uses bursting for overflow. Pros: Best of both; Cons: Complex integration.

### Commonly Used AWS Services

- **AWS Lambda**: Serverless compute; runs code on events without managing servers (e.g., API triggers). Pay-per-execution.
- **Amazon S3 (Simple Storage Service)**: Scalable object storage for files/backups. Durable (99.999999999%), versions, access policies.
- **Amazon RDS (Relational Database Service)**: Managed SQL databases (MySQL, PostgreSQL). Handles backups, scaling, patching.
- **Amazon EC2 (Elastic Compute Cloud)**: Virtual servers (VMs) for apps. Choose instance types (e.g., t3.micro), auto-scale.
- **Amazon DynamoDB**: NoSQL database; serverless, low-latency for high-traffic apps (e.g., gaming leaderboards).
- **Amazon VPC (Virtual Private Cloud)**: Isolated network in AWS; custom subnets, security groups, VPNs for private connectivity.
- **Amazon SNS (Simple Notification Service)**: Pub/sub messaging for alerts/notifications (e.g., email/SMS on events).
- **Amazon Cloud9**: Browser-based IDE for coding/debugging in the cloud; integrates with AWS services.
- **Amazon Config**: Tracks resource changes/configurations for compliance/auditing.

### Directory Services and Other Concepts

### What is Active Directory?

Active Directory (AD) is Microsoft's directory service for Windows networks. It stores/manages info about users, computers, groups, and resources in a hierarchical database (domains, trees, forests). Uses LDAP for queries, Kerberos for auth. Functions: Centralized auth (logins), policy enforcement (GPOs), resource access control. Essential for enterprise Windows environments.

### What are MAC Bits?

A MAC (Media Access Control) address is 48 bits (6 bytes), expressed as 12 hexadecimal digits (e.g., 00-1A-2B-3C-4D-5E). Structure:

- **First 24 bits (3 bytes)**: OUI (Organizationally Unique Identifier), assigned by IEEE to vendors (e.g., 00-1A-2B for a company).
- **Last 24 bits (3 bytes)**: Vendor-specific, unique per device.
Bits are fixed in hardware; used for Layer 2 identification. Locally administered MACs can be changed (e.g., for virtualization).

### Security Concepts

### What is Security?

In IT/networking, security is the practice of protecting systems, networks, and data from unauthorized access, damage, or disruption. It involves CIA triad: **Confidentiality** (data privacy, e.g., encryption), **Integrity** (data accuracy, e.g., hashing), **Availability** (uptime, e.g., redundancy). Includes physical, technical (firewalls, IDS), and procedural (policies) measures.

### What are Types and Steps of Attacks?

**Types of Attacks**:

1. **Passive**: Eavesdropping (e.g., sniffing traffic) without altering data.
2. **Active**: Modifies/disrupts (e.g., malware, DoS).
3. **Insider**: By trusted users.
4. **External**: From outsiders (e.g., hackers).
Specific: Phishing (social engineering), Malware (viruses/trojans), SQL Injection (code exploits).

**Steps of a Typical Attack (Cyber Kill Chain)**:

1. **Reconnaissance**: Gather info (e.g., scanning ports).
2. **Weaponization**: Create exploit (e.g., malware payload).
3. **Delivery**: Send to target (e.g., email phishing).
4. **Exploitation**: Trigger vulnerability.
5. **Installation**: Gain foothold (e.g., backdoor).
6. **Command & Control**: Communicate with attacker.
7. **Actions on Objectives**: Steal data, disrupt, etc.

Mitigation: Firewalls, updates, training.

### What is Brute Force?

Brute force is an attack trying all possible combinations (e.g., passwords) until success. Exhaustive, compute-intensive (e.g., dictionary attacks use common words). Targets: Logins, encryption. Counters: Strong passwords, rate limiting, CAPTCHA, multi-factor auth. Tools: Hydra, John the Ripper.

### What is DoS?

DoS (Denial of Service) floods a target with traffic to overwhelm resources (CPU, bandwidth), making it unavailable. Types:

- **Simple DoS**: Single source (e.g., SYN flood - incomplete TCP handshakes).
- **DDoS (Distributed)**: From botnets (e.g., Mirai attacks).
Impacts: Website downtime, lost revenue. Counters: Rate limiting, CDNs, IDS/IPS. Not data theft, but disruption.