## Objective

The objective of this project is to design, build, and maintain a virtual enterprise IT environment using Microsoft Hyper-V, Windows Server 2022, and Windows 11. This environment will serve as a hands-on training platform for developing practical systems administration, networking, and enterprise troubleshooting skills.

The lab will provide opportunities to configure and manage Active Directory Domain Services (AD DS), DNS, DHCP, Group Policy, user accounts, security permissions, and other essential Windows infrastructure services. It will also allow me to simulate real-world technical incidents, investigate their root causes, and practice resolving problems in a controlled environment.

As the project progresses, I will expand my knowledge of PowerShell automation, Microsoft 365, Microsoft Entra ID, SharePoint Online, and cloud technologies such as Microsoft Azure and Amazon Web Services (AWS). Cloud-based exercises will be introduced as suitable training environments become available.

The ultimate goal is to strengthen my technical capabilities, develop confidence in independently troubleshooting enterprise systems, and demonstrate my practical knowledge through documented configurations, troubleshooting exercises, and completed projects published to GitHub.

## Lab Environment

### Host System

The enterprise IT lab is hosted on a Dell Precision 7670 workstation running Windows 11 Pro. Microsoft Hyper-V is used as the virtualization platform, allowing multiple operating systems to run independently on the same physical computer.

This environment provides a controlled platform for configuring, managing, and troubleshooting enterprise infrastructure without requiring dedicated physical servers or networking equipment.

**Host Configuration:**

- **Computer Name:** AEGIS
- **Manufacturer:** Dell
- **Model:** Precision 7670
- **Operating System:** Windows 11 Pro
- **Virtualization Platform:** Microsoft Hyper-V

### Virtual Machines

The lab currently consists of two virtual machines designed to simulate a basic enterprise client-server environment.

**1. Windows Server 2022**

- **Computer Name:** SRV-DC-01
- **Operating System:** Windows Server 2022
- **Virtualization Platform:** Hyper-V
- **Planned Role:** Active Directory Domain Controller
- **Current Status:** Operating system installed, server renamed, initial configuration in progress

This virtual machine will eventually provide centralized authentication, user and group management, DNS, DHCP, and Group Policy services for the lab environment.

**2. Windows 11 Client**

- **Operating System:** Windows 11
- **Virtualization Platform:** Hyper-V
- **Planned Role:** Domain-joined workstation
- **Current Status:** Virtual machine created; domain integration pending

The Windows 11 virtual machine will be used to simulate an enterprise employee workstation. It will provide opportunities to practice domain authentication, Group Policy application, network connectivity troubleshooting, and endpoint administration.

### Virtual Networking

Hyper-V Virtual Switch Manager is used to create and manage networking between virtual machines.

**Virtual Switch Configuration:**

- **Switch Name:** CQ-LAB-SWITCH
- **Switch Type:** Internal
- **Purpose:** Provide a dedicated network for communication between lab virtual machines and the Hyper-V host
- **Status:** Created and verified in Hyper-V Virtual Switch Manager

An Internal virtual switch was selected to keep lab networking separate from the physical home network by default.

This is particularly important when configuring DHCP services. An improperly configured DHCP server connected to the home network could distribute incorrect IP addresses, DNS servers, or default gateways to other devices.

Using a dedicated lab network reduces this risk and allows network services to be configured and tested in a controlled environment.

### Planned Infrastructure Services

The following technologies will be implemented progressively:

- Active Directory Domain Services (AD DS)
- Domain Name System (DNS)
- Dynamic Host Configuration Protocol (DHCP)
- Group Policy Management
- Windows Server administration
- PowerShell automation
- Network troubleshooting and diagnostics
- Security and access management

Future exercises will expand into Microsoft 365, SharePoint Online, Microsoft Entra ID, Microsoft Azure, and AWS as suitable training resources become available.

### Current Lab Status

At the time of this documentation:

- [x] Hyper-V installed and operational
- [x] Windows Server 2022 virtual machine installed
- [x] Windows Server renamed to SRV-DC-01
- [x] Windows 11 client virtual machine created
- [x] Internal virtual switch CQ-LAB-SWITCH created
- [x] Connect both virtual machines to the internal switch
- [ ] Configure static IP addressing
- [ ] Install Active Directory Domain Services
- [ ] Configure DNS and DHCP

## Implementation

### 3.1 — Hyper-V Virtualization Environment

The enterprise IT lab was established using Microsoft Hyper-V on a Dell Precision 7670 workstation running Windows 11 Pro.

Hyper-V was selected because it provides a native Windows virtualization platform capable of running multiple virtual machines, managing virtual networking, and supporting enterprise infrastructure testing.

The virtualization environment will be used to simulate a small business network containing Windows servers, domain-joined workstations, and centralized network services.

**Implementation Summary:**

1. Enabled the Hyper-V virtualization platform on the Windows 11 Pro host.
2. Opened Hyper-V Manager to manage the virtual environment.
3. Created virtual machines for Windows Server 2022 and Windows 11.
4. Prepared the virtual environment for future networking and domain administration exercises.

**Result:** Hyper-V is operational, and both virtual machines have been created.

### 3.2 — Windows Server 2022 Installation

A Windows Server 2022 virtual machine was created to serve as the primary infrastructure server for the enterprise lab.

Windows Server 2022 was selected to provide hands-on experience with technologies commonly used in enterprise environments, including Active Directory Domain Services, DNS, DHCP, and Group Policy.

**Implementation Summary:**

1. Obtained the Windows Server 2022 installation media.
2. Created a Windows Server virtual machine in Hyper-V.
3. Attached the Windows Server installation media to the virtual machine.
4. Started the virtual machine and completed the Windows Server installation process.
5. Logged into Windows Server and confirmed that the operating system was accessible.

**Result:** Windows Server 2022 was successfully installed and is operational.

### 3.3 — Windows Server Computer Name Configuration

After installing Windows Server 2022, the server was renamed to follow a consistent enterprise naming convention.

**Configured Computer Name:** `SRV-DC-01`

**Naming Convention:**

- `SRV` — Identifies the system as a server.
- `DC` — Identifies the server's intended Domain Controller role.
- `01` — Identifies the first server of this type in the environment.

**Implementation Steps:**

1. Opened Server Manager.
2. Selected **Local Server**.
3. Located the computer name under the server properties.
4. Opened the computer name configuration settings.
5. Changed the computer name to `SRV-DC-01`.
6. Applied the configuration and restarted the server.
7. Confirmed the new computer name.

**Result:** The server was successfully renamed to `SRV-DC-01`.

**Technical Significance:**

Consistent naming conventions make systems easier to identify, administer, and troubleshoot. In larger environments, standardized computer names help administrators recognize system roles and distinguish between infrastructure resources.

The `DC` designation identifies the planned role of this server. Active Directory Domain Services has not yet been installed.

### 3.4 — Hyper-V Internal Virtual Switch Configuration

A dedicated virtual network was created to support communication between the Windows Server and Windows 11 virtual machines.

**Virtual Switch Configuration:**

- **Switch Name:** `CQ-LAB-SWITCH`
- **Switch Type:** Internal
- **Virtualization Platform:** Microsoft Hyper-V
- **Purpose:** Provide a controlled network for enterprise infrastructure training.

**Implementation Steps:**

1. Opened Hyper-V Manager on the host computer.
2. Selected **Virtual Switch Manager**.
3. Selected the **Internal** virtual switch type.
4. Created a new virtual switch.
5. Named the switch `CQ-LAB-SWITCH`.
6. Applied the configuration.
7. Reopened Virtual Switch Manager to verify the switch settings.

**Result:** The internal virtual switch was successfully created and verified.

**Technical Significance:**

An Internal virtual switch allows virtual machines connected to it to communicate with each other and the Hyper-V host without directly connecting the switch to the physical network.

This configuration provides a safer environment for experimenting with network infrastructure services such as DNS and DHCP.

During the exercise, I identified the risk of introducing a rogue DHCP server into an existing network. An improperly configured DHCP server could respond to client requests and distribute incorrect network settings, potentially disrupting connectivity.

Using a separate virtual network helps prevent these conflicts during lab exercises.

### 3.5 — Windows 11 Client Virtual Machine

A Windows 11 virtual machine was created to simulate a workstation in an enterprise environment.

The workstation will eventually be joined to the Active Directory domain hosted by `SRV-DC-01`.

**Planned Uses:**

- Testing domain user authentication.
- Applying and verifying Group Policy settings.
- Troubleshooting DNS and network connectivity.
- Testing user and group permissions.
- Practicing Windows endpoint administration.
- Simulating enterprise help desk incidents.

**Result:** The Windows 11 virtual machine has been created. Domain integration and internal lab networking remain pending.

### 3.6 — Current Implementation Status

The initial virtualization environment has been established successfully.

**Completed:**

- [x] Installed and enabled Hyper-V.
- [x] Created Windows Server 2022 virtual machine.
- [x] Installed Windows Server 2022.
- [x] Logged into Windows Server.
- [x] Renamed the server to `SRV-DC-01`.
- [x] Created the Windows 11 client virtual machine.
- [x] Created the `CQ-LAB-SWITCH` internal virtual switch.
- [x] Verified the virtual switch configuration.

**Pending:**

- [ ] Connect both virtual machines to `CQ-LAB-SWITCH`.
- [ ] Plan and configure IP addressing.
- [ ] Verify communication between virtual machines.
- [ ] Install Active Directory Domain Services.
- [ ] Configure DNS.
- [ ] Promote `

  ## 4. Verification

### 4.1 — Hyper-V Environment Verification

After establishing the virtualization environment, I verified that Microsoft Hyper-V was operational and capable of managing virtual machines.

**Verification performed:**

- Confirmed that Hyper-V Manager was accessible on the Windows 11 Pro host.
- Confirmed that the Windows Server 2022 virtual machine had been created.
- Successfully started and accessed the Windows Server virtual machine.
- Confirmed that the Windows 11 client virtual machine had been created.

**Result:** The Hyper-V virtualization environment is operational.

### 4.2 — Windows Server Verification

After installing Windows Server 2022, I successfully logged into the operating system and accessed its administrative interface.

I subsequently changed the computer name to `SRV-DC-01`.

**Verification performed:**

- Confirmed successful Windows Server installation.
- Confirmed that the server was accessible through the Hyper-V console.
- Completed the server renaming procedure.
- Confirmed that the server was accessible following the configuration change.

**Result:** Windows Server 2022 is installed, accessible, and renamed to `SRV-DC-01`.

### 4.3 — Virtual Network Verification

After creating the internal virtual switch, I reopened Hyper-V Virtual Switch Manager to verify its configuration.

**Verified configuration:**

| Setting | Value |
|---|---|
| Virtual Switch Name | CQ-LAB-SWITCH |
| Connection Type | Internal |
| Virtualization Platform | Microsoft Hyper-V |
| Existing Default Switch | Preserved |

A screenshot was captured showing the virtual switch configuration.

**Result:** The internal virtual switch was successfully created and verified.

### 4.4 — Verification Limitations

The initial virtualization environment is functional; however, full network connectivity testing has not yet been performed.

The following items remain pending:

- Connecting both virtual machines to the internal switch.
- Configuring IP addresses.
- Testing communication between the server and client.
- Installing Active Directory Domain Services.
- Configuring DNS and DHCP.
- Joining the Windows 11 client to the domain.

These components will be verified during subsequent lab exercises.

---

## 5. What I Learned

### 5.1 — Virtualization Fundamentals

This project provided practical experience with Microsoft Hyper-V and the process of creating a virtual enterprise environment.

I learned how a virtualization platform allows multiple operating systems to run on a single physical workstation while maintaining separate virtual hardware configurations.

This makes it possible to practice enterprise infrastructure administration without requiring multiple physical computers.

### 5.2 — Windows Server Administration

Installing and configuring Windows Server 2022 introduced me to the initial setup procedures used when preparing a server for an enterprise environment.

I gained experience accessing Windows Server through Hyper-V and changing its computer name.

I also learned why standardized naming conventions are important for identifying systems and their intended roles.

### 5.3 — Virtual Networking

Creating `CQ-LAB-SWITCH` introduced me to virtual networking concepts and the different connection types available in Hyper-V.

**External Switch:** Connects virtual machines to an external network through a physical network adapter.

**Internal Switch:** Allows communication between connected virtual machines and the Hyper-V host.

**Private Switch:** Allows communication between connected virtual machines without providing a connection to the Hyper-V host.

I selected an Internal switch because it provides a controlled environment for practicing enterprise networking while keeping the lab network separate from the physical home network by default.

### 5.4 — DHCP and Network Isolation

One important concept I learned was the potential impact of introducing an improperly configured DHCP server into an existing network.

My home network already has a DHCP service provided by my router.

If a second, unauthorized DHCP server were connected to the same network, it could respond to client requests and distribute incorrect network configuration information.

This could result in incorrect IP addressing, DNS configuration, default gateways, or intermittent connectivity.

Using a dedicated virtual network reduces the risk of accidentally disrupting the existing home network while experimenting with DHCP services.

### 5.5 — Importance of Verification

I learned that completing a configuration task is not the same as verifying that it works.

After creating the virtual switch, I returned to Hyper-V Virtual Switch Manager to confirm that the correct switch name and connection type were configured.

This reinforced the importance of verifying changes rather than assuming they were successful.

---

## 6. Challenges and Troubleshooting

### 6.1 — Learning an Unfamiliar Administration Environment

One challenge during the initial lab setup was becoming familiar with the configuration and administration tools available in Windows Server and Hyper-V.

Although I have professional experience supporting Windows environments, building and configuring a server infrastructure lab requires additional knowledge of virtualization, networking, and server administration.

**Approach:**

I divided the implementation into smaller tasks, completing and verifying each configuration before moving to the next.

This approach allows me to identify configuration errors more easily and understand the purpose of each change.

### 6.2 — Understanding Virtual Network Isolation

Another learning challenge was determining which Hyper-V virtual switch type would be appropriate for the lab.

Using an External switch could introduce unnecessary risks when experimenting with services such as DHCP.

**Approach:**

I reviewed the differences between External, Internal, and Private virtual switches.

I selected an Internal switch to allow communication between the lab virtual machines and the host without directly connecting the lab switch to the physical home network.

**Result:**

The internal virtual switch was created successfully.

### 6.3 — Understanding Rogue DHCP Servers

During the networking exercise, I considered the consequences of connecting an additional DHCP server to a network that already has an active DHCP service.

I identified that an improperly configured DHCP server could act as a rogue DHCP server and provide incorrect network settings to connected devices.

**Approach:**

I chose to isolate the lab network before configuring DHCP services.

**Result:**

The lab now has a dedicated internal virtual switch that can be used for future network configuration exercises.

### 6.4 — Outstanding Troubleshooting Tasks

No unresolved installation or virtual switch configuration failures have been documented at this stage.

The following tasks will provide opportunities for additional troubleshooting:

- Configuring network adapters and IP addresses.
- Testing communication between virtual machines.
- Diagnosing DNS resolution problems.
- Configuring Active Directory authentication.
- Investigating Group Policy application failures.

These exercises will be documented as the lab progresses.

---

## 7. Evidence

### 7.1 — Purpose of Evidence Collection

Screenshots and configuration records will be maintained to demonstrate completed lab exercises and support future troubleshooting.

Evidence will be used to verify that configuration changes were performed successfully and to provide a visual reference for technical documentation.

Screenshots will be organized within the GitHub repository and referenced from the appropriate documentation files.

### 7.2 — Evidence Inventory

| Evidence | Description | Status |
|---|---|---|
| Hyper-V Manager | Virtual machines created for the enterprise lab | To be captured |
| Windows Server 2022 | Successful installation and access to Server Manager | To be captured |
| Server Computer Name | SRV-DC-01 naming configuration | To be captured |
| Virtual Switch Manager | CQ-LAB-SWITCH configured as Internal | Captured |
| VM Network Adapters | Connection to the internal virtual switch | Pending |
| Network Connectivity | IP configuration and connectivity testing | Pending |

### 7.3 — Screenshot Storage

The following repository structure will be used to organize evidence:

```text
windows-enterprise-homelab/
├── README.md
├── docs/
│   ├── 001-lab-foundation.md
│   └── 002-hyperv-virtual-switch.md
└── images/
    ├── hyperv-manager.png
    ├── windows-server-2022.png
    ├── server-computer-name.png
    └── internal-virtual-switch.png
```

The filenames above represent the planned evidence structure. Screenshots will be added as they are collected.

### 7.4 — Evidence Handling

Before uploading screenshots, I will review them to ensure they do not expose sensitive information such as passwords, authentication tokens, personal information, or confidential network configurations.

### 7.5 — Project Status

**Mission 001: Enterprise IT Lab Foundation**

**Status:** Initial environment established.

The virtualization platform, Windows Server installation, server naming configuration, and internal virtual switch have been completed.

The next phase will focus on connecting the virtual machines to the lab network and configuring IP addressing.

Future exercises will expand the environment to include Active Directory, DNS, DHCP, Group Policy, and enterprise troubleshooting scenarios.****
- [ ] Promote Windows Server to Domain Controller
- [ ] Join Windows 11 client to the domain
- [ ] Configure and test Group Policy
