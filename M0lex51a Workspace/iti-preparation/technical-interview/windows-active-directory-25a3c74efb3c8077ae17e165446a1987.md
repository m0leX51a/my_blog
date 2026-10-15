# Windows (Active Directory )

### Study Notes: MCSA Topics (Windows Server Administration)

Hey, study buddy! 😎 Switching to MCSA (Microsoft Certified Solutions Associate) topics—focusing on Windows Server fundamentals like Active Directory (AD). This builds on our previous admin chats (e.g., Red Hat file management to Windows domain management). Notes are concise for review; deep dive below has details, examples, and ties. These are key for exams like 70-740/741/742 (legacy MCSA: Windows Server 2016).

---

### 1. **Active Directory**

- **Definition**: Centralized database for managing users, computers, and resources in a Windows domain.
- **Key Components**: Domains, trees, forests, objects (users/groups).
- **Tie-In**: Uses LDAP for queries; integrates with DNS/DHCP.

---

### 2. **Difference Between Domain Controller and Domain**

- **Domain**: Logical group of networked objects (users, PCs) sharing a security database.
- **Domain Controller (DC)**: Server hosting AD database, authenticating users.
- **Tie-In**: Multiple DCs per domain for redundancy; domain is the boundary.

---

### 3. **How to Join a Domain**

- **Definition**: Adding a computer to an AD domain for centralized management.
- **Key Steps**: Prep (DNS, admin creds), System Properties > Change Settings > Join.
- **Tie-In**: Requires DNS resolution; post-join, apply GPOs.

---

### 4. **Organizational Unit (OU)**

- **Definition**: Container in AD for organizing objects (users, groups, computers).
- **Key Features**: Delegate admin, apply Group Policies (GPOs).
- **Tie-In**: Nested in domains; not security boundaries like domains.

---

### 5. **DNS Server**

- **Definition**: Resolves names to IPs in Windows (integrated with AD).
- **Key Components**: Zones (forward/reverse), records (A, CNAME, MX).
- **Tie-In**: AD requires DNS; dynamic updates via DHCP.

---

### 6. **DHCP Configuration**

- **Definition**: Automates IP assignment in Windows networks.
- **Key Steps**: Install role, create scope, options (gateway, DNS), authorize.
- **Tie-In**: Integrates with AD/DNS; reservations for static IPs.

---

### 7. **LDAP**

- **Definition**: Protocol for accessing/modifying directory services like AD.
- **Key Features**: Port 389 (unsecure), 636 (LDAPS); queries via DN (Distinguished Name).
- **Tie-In**: AD's query language; used by apps for authentication.

---

### Deep Dive: Detailed Explanations of MCSA Topics (Windows Server Administration)

Let's dive deep! 😄 Each topic includes definitions, how it works, key commands/tools (PowerShell/GUI in Windows Server), examples, advantages/limitations, and ties to others or our previous studies (e.g., Linux DHCP vs. Windows, or AD like centralized file management). Focus on Windows Server 2016/2019/2022 for MCSA relevance.

### 1. **Active Directory**

- **Definition**: Active Directory (AD) is Microsoft's directory service—a hierarchical, replicated database storing objects like users, groups, computers, and printers—for authentication, authorization, and management in a Windows network.
- **How It Works in Detail**: AD uses a multi-master replication model (changes on one DC sync to others via RPC). Structure: Domain (basic unit), Tree (multiple domains sharing namespace), Forest (multiple trees, top-level). Schema defines object classes/attributes. Functional levels (e.g., Windows Server 2016) enable features like fine-grained passwords. Install via Server Manager > Add Roles > Active Directory Domain Services (AD DS).
- **Key Commands/Tools**: PowerShell: `Install-WindowsFeature AD-Domain-Services`, `Install-ADDSForest` (promote to DC). Tools: AD Users and Computers (dsa.msc), AD Sites and Services.
- **Examples**: Create user: In ADUC, right-click OU > New > User. Output: User object with SID (Security Identifier).
- **Advantages/Limitations**: Advantages: Scalable (millions of objects), single sign-on (SSO) via Kerberos. Limitations: Complex setup; vulnerable to attacks (e.g., Pass-the-Hash—mitigate with MFA).
- **Real-World Example**: In a company, AD manages 1000 users—logins sync across DCs, policies enforce password rules.
- **Tie to Other Topics**: AD relies on DNS (integrated zones) and DHCP (IP assignment); LDAP queries AD; like Linux /etc/passwd but centralized/scalable.

### 2. **Difference Between Domain Controller and Domain**

- **Definition**: A **domain** is a logical boundary grouping network resources (users, computers) sharing a common security policy and database. A **domain controller (DC)** is a Windows Server role that hosts/runs the AD database (NTDS.dit), handles authentication, and replicates changes.
- **How It Works in Detail**: Domain: Defined by namespace (e.g., [contoso.com](http://contoso.com/)), contains objects with unique SIDs. DC: Promoted server (dcpromo.exe legacy, now PowerShell) storing AD; Read-Only DC (RODC) for branches. Multiple DCs per domain for fault tolerance (e.g., global catalog for forest-wide searches). Domain is abstract; DC is physical/virtual server.
- **Key Commands/Tools**: Check DC: `nltest /dsgetdc:domain`. Promote: `Install-ADDSDomainController`.
- **Examples**: Domain: [example.com](http://example.com/) (boundary). DC: [server1.example.com](http://server1.example.com/) (hosts database). If DC fails, another DC authenticates.
- **Advantages/Limitations**: Domain: Centralized management. DC: Redundancy but requires secure placement (e.g., firewall ports 389, 636 open).
- **Real-World Example**: In a school, "[school.edu](http://school.edu/)" domain groups all PCs; DCs in data center authenticate logins.
- **Tie to Other Topics**: Join domain to add computers; OUs organize within domain; DNS resolves domain names; like Linux realms but Windows-native.

### 3. **How to Join a Domain**

- **Definition**: Joining a domain integrates a Windows client/server into AD for centralized auth, policies, and resource access.
- **How It Works in Detail**: Prerequisites: DNS pointing to DC, network connectivity, domain admin creds. Process: System Properties > Computer Name > Change > Member of Domain > Enter domain name > Creds > Restart. Creates computer object in AD. PowerShell: `Add-Computer -DomainName contoso.com -Credential admin`. Offline join via `djoin.exe`. Post-join: GPOs apply, user logins use domain accounts.
- **Key Commands/Tools**: GUI: sysdm.cpl. PowerShell: `Test-ComputerSecureChannel` (verify join).
- **Examples**: Join Win10 to [contoso.com](http://contoso.com/): Enter creds, reboot. Error if DNS fails: "Domain not found."
- **Advantages/Limitations**: Advantages: Roaming profiles, centralized updates. Limitations: Requires reboot; offline domains tricky.
- **Real-World Example**: New employee PC: Join domain, login with AD account, auto-get printers/policies.
- **Tie to Other Topics**: Needs DNS resolution; place in OU post-join; DHCP can provide domain info; contrasts Linux `realm join`.

### 4. **Organizational Unit (OU)**

- **Definition**: An OU is a container object in AD for grouping related objects (users, groups, computers) to simplify administration, like applying policies or delegating control.
- **How It Works in Detail**: OUs are created in ADUC (right-click domain > New > OU). Nested (e.g., Sales > East). Link GPOs to OUs for settings (e.g., software install). Delegate via wizard (e.g., allow HR to reset passwords in HR OU). Not a security principal (unlike groups)—inheritance from domain.
- **Key Commands/Tools**: PowerShell: `New-ADOrganizationalUnit -Name "Sales" -Path "DC=contoso,DC=com"`. Move objects: `Move-ADObject`.
- **Examples**: Create OU: "Marketing" under domain. Move user: Drag in ADUC.
- **Advantages/Limitations**: Advantages: Granular delegation (e.g., without full domain admin). Limitations: No built-in security boundary (use domains for that); over-nesting complicates.
- **Real-World Example**: Company: OUs for Depts (HR, IT)—apply wallpaper GPO to Marketing OU only.
- **Tie to Other Topics**: Within domains; join computers to OUs; GPOs query via LDAP; like Linux dirs for organization.

### 5. **DNS Server**

- **Definition**: DNS (Domain Name System) server in Windows resolves hostnames to IPs, essential for AD (stores SRV records for DCs).
- **How It Works in Detail**: Install via Server Manager > DNS role. Zones: Primary (writable), Secondary (read-only), Stub (delegation). Records: A (host), PTR (reverse), CNAME (alias), SRV (services). Dynamic updates (secure via AD-integrated). Forwarders/conditional for external queries. AD-integrated zones replicate via AD.
- **Key Commands/Tools**: `dnscmd /zoneadd`, PowerShell: `Add-DnsServerPrimaryZone`. Console: dnsmgmt.msc.
- **Examples**: Add A record: "[web.contoso.com](http://web.contoso.com/)" → 192.168.1.10. Test: `nslookup web.contoso.com`.
- **Advantages/Limitations**: Advantages: Integrated with AD (auto SRV). Limitations: DDoS vulnerable (use DNSSEC); complex zones.
- **Real-World Example**: AD setup: DNS hosts _ldap._tcp.contoso.com SRV for DC discovery.
- **Tie to Other Topics**: Required for domain join; DHCP options include DNS servers; LDAP uses DNS for locating DCs.

### 6. **DHCP Configuration**

- **Definition**: DHCP (Dynamic Host Configuration Protocol) server assigns IPs, masks, gateways dynamically in Windows.
- **How It Works in Detail**: Install role > Authorize in AD (prevents rogues). Create scope (IP range, exclusions). Options: 003 (router), 006 (DNS), 015 (domain). Reservations by MAC. Failover: Hot standby or load balance. Superscopes for multiple subnets. Leases: 8 days default.
- **Key Commands/Tools**: `netsh dhcp add server`, PowerShell: `Add-DhcpServerv4Scope`. Console: dhcpmgmt.msc.
- **Examples**: Scope: 192.168.1.50-192.168.1.200 /24. Option: Set DNS to 192.168.1.1.
- **Advantages/Limitations**: Advantages: Centralized (no manual IPs); integrates with DNS (dynamic updates). Limitations: Lease exhaustion; security (use IPsec policies).
- **Real-World Example**: Office: DHCP assigns IPs to 500 devices, with reservation for printer.
- **Tie to Other Topics**: Config via AD (authorized DCs); provides DNS/domain for joins; like Linux dhclient but Windows-managed.

### 7. **LDAP**

- **Definition**: Lightweight Directory Access Protocol—a standard for querying/updating directories like AD over TCP/IP.
- **How It Works in Detail**: Client-server: Bind (authenticate), Search (query), Modify (update). Ports: 389 (LDAP), 636 (LDAPS/SSL), 3268 (Global Catalog). DN: cn=user,ou=sales,dc=contoso,dc=com. Filters: (&(objectClass=user)(department=IT)). AD uses LDAPv3; tools query objects.
- **Key Commands/Tools**: `ldp.exe` (GUI bind/search), PowerShell: `Get-ADUser -LDAPFilter "(department=Sales)"`. ADSI Edit for low-level.
- **Examples**: Search users: Filter (samAccountName=admin). Bind as [admin@contoso.com](mailto:admin@contoso.com).
- **Advantages/Limitations**: Advantages: Open standard (apps like Outlook use it); efficient. Limitations: Unencrypted vulnerable (use LDAPS); query limits.
- **Real-World Example**: Script: LDAP query for expired passwords in AD.
- **Tie to Other Topics**: AD's access protocol; DNS locates LDAP servers (SRV records); ties to OUs (DN paths); like grep in Linux for searching.