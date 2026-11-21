# Mission 002 — Hyper-V Internal Virtual Switch Configuration

**Project:** CodeQuacken Windows Enterprise Home Lab  
**Lab Host:** AEGIS  
**Virtualization Platform:** Microsoft Hyper-V  
**Status:** Completed — Screenshot Collection Pending  
**Documentation Date:** November 20, 2026

---

## 1. Objective

The objective of this exercise is to configure a dedicated virtual network within Microsoft Hyper-V to support an enterprise IT training environment.

The virtual network will provide communication between Windows Server 2022 and Windows 11 virtual machines while maintaining separation from the physical home network.

This configuration will establish the networking foundation required for future exercises involving Active Directory Domain Services (AD DS), DNS, DHCP, Group Policy, and enterprise network troubleshooting.

An additional objective is to develop an understanding of Hyper-V virtual switch types, network isolation, and the potential risks associated with introducing improperly configured network services into an existing environment.

---

## 2. Lab Environment

### 2.1 — Host System

The virtual network was configured on the following system:

| Component | Configuration |
|---|---|
| Host Computer | AEGIS |
| Manufacturer | Dell |
| Model | Precision 7670 |
| Operating System | Windows 11 Pro |
| Virtualization Platform | Microsoft Hyper-V |
| Network Management Tool | Hyper-V Virtual Switch Manager |

### 2.2 — Virtual Machines

The following virtual machines are intended to use the dedicated lab network:

**Windows Server 2022**

- **Computer Name:** `SRV-DC-01`
- **Operating System:** Windows Server 2022
- **Planned Role:** Active Directory Domain Controller
- **Network Configuration:** Pending

**Windows 11 Client**

- **Operating System:** Windows 11
- **Planned Role:** Domain-joined workstation
- **Network Configuration:** Pending

Both virtual machines will eventually communicate through the internal virtual switch.

### 2.3 — Virtual Switch Configuration

| Setting | Value |
|---|---|
| Virtual Switch Name | CQ-LAB-SWITCH |
| Switch Type | Internal |
| Host Access | Enabled by switch type |
| Direct Physical Network Connection | None |
| IP Addressing | Not yet configured |
| DHCP Services | Not yet configured |
| Active Directory Integration | Pending |

The switch provides a dedicated virtual network segment for infrastructure testing.

---

## 3. Implementation

### 3.1 — Virtual Switch Planning

Before creating the virtual switch, I reviewed the available Hyper-V networking options.

Microsoft Hyper-V provides three primary virtual switch types:

**External Virtual Switch**

An External switch connects virtual machines to an external network through a physical network adapter.

This configuration can allow virtual machines to communicate with other devices on the physical network, depending on the network configuration.

**Internal Virtual Switch**

An Internal switch allows communication between virtual machines connected to the switch and the Hyper-V host operating system.

It does not directly connect the virtual network to the physical network.

**Private Virtual Switch**

A Private switch allows communication between virtual machines connected to the same switch without providing network connectivity to the Hyper-V host.

### 3.2 — Network Design Decision

An Internal virtual switch was selected for the enterprise lab.

This configuration was chosen for the following reasons:

1. It allows the Windows Server and Windows 11 virtual machines to communicate.
2. It provides connectivity between the virtual machines and the Hyper-V host.
3. It keeps the lab network separate from the physical home network by default.
4. It provides a controlled environment for testing DHCP and DNS services.
5. It reduces the risk of accidentally affecting other devices on the home network.

The Internal switch does not automatically provide internet connectivity. Internet access, routing, or network address translation would require additional configuration.

### 3.3 — Virtual Switch Creation

The following steps were performed on the Windows 11 Pro host:

1. Opened **Hyper-V Manager**.
2. Selected **Virtual Switch Manager** from the Actions panel.
3. Selected the **Internal** virtual switch type.
4. Clicked **Create Virtual Switch**.
5. Entered `CQ-LAB-SWITCH` as the switch name.
6. Confirmed that the connection type was set to **Internal network**.
7. Applied the configuration.
8. Reopened Virtual Switch Manager to verify the new switch.

**Result:** The `CQ-LAB-SWITCH` virtual switch was successfully created.

### 3.4 — Existing Network Configuration

During verification, I confirmed that the existing Hyper-V Default Switch remained present.

The Default Switch was not deleted or modified as part of this exercise.

The new Internal switch was created as a separate network resource specifically for the enterprise lab.

### 3.5 — Current Configuration Boundaries

This exercise was limited to creating and verifying the virtual switch.

The following tasks were intentionally deferred:

- Connecting virtual machine network adapters.
- Assigning static IP addresses.
- Configuring a DHCP server.
- Configuring DNS services.
- Testing network connectivity between virtual machines.
- Installing Active Directory Domain Services.

These tasks will be completed in subsequent lab exercises.

---

## 4. Verification

### 4.1 — Virtual Switch Verification

After creating the switch, I reopened Hyper-V Virtual Switch Manager to confirm that the configuration was saved successfully.

**Verification Results:**

| Verification Item | Result |
|---|---|
| CQ-LAB-SWITCH appears in Virtual Switch Manager | Passed |
| Switch connection type is Internal | Passed |
| Existing Default Switch remains available | Passed |
| Virtual machine connectivity tested | Pending |
| Static IP addressing configured | Pending |
| DHCP functionality tested | Pending |

### 4.2 — Screenshot Verification

A screenshot was captured showing `CQ-LAB-SWITCH` selected in Hyper-V Virtual Switch Manager.

The screenshot confirms that:

- The virtual switch exists.
- The switch is configured as an Internal network.
- The existing Default Switch remains present.

This screenshot will be added to the repository as supporting evidence.

### 4.3 — Verification Limitations

Creating a virtual switch does not automatically establish network connectivity between virtual machines.

Each virtual machine must be connected to the appropriate virtual switch through its network adapter settings.

The virtual machines must also have compatible IP addressing before network communication can be tested.

These tasks will be addressed in the next networking exercise.

---

## 5. What I Learned

### 5.1 — Virtual Switches and Network Adapters

A virtual switch functions similarly to a physical network switch.

It provides a connection point through which multiple virtual machines can exchange network traffic.

A virtual network adapter is the virtual hardware component that connects an individual virtual machine to the virtual switch.

Understanding this distinction is important when troubleshooting virtual network connectivity.

For example, a virtual switch may be functioning correctly while a virtual machine remains disconnected because its network adapter is attached to the wrong switch.

### 5.2 — Network Isolation

This exercise reinforced the importance of separating testing environments from production networks.

An isolated lab environment allows administrators to experiment with infrastructure services while reducing the risk of affecting unrelated systems.

An Internal Hyper-V switch provides separation from the physical network by default. However, additional host configurations such as routing, network bridging, or Internet Connection Sharing could introduce connectivity between networks.

Maintaining isolation therefore requires consideration of both virtual switch configuration and host networking settings.

### 5.3 — DHCP and Rogue DHCP Servers

A significant learning point during this exercise was understanding the risk of connecting an unauthorized DHCP server to an existing network.

My home network already uses a DHCP service provided through the ISP router.

If an additional DHCP server were introduced without proper planning, devices could receive network configuration information from an unintended source.

Potential consequences include:

- Incorrect IP address assignments.
- Incorrect subnet masks.
- Incorrect default gateways.
- Incorrect DNS server assignments.
- Intermittent connectivity problems.
- Difficulty accessing local or external network resources.

An unauthorized DHCP server providing network configurations is commonly referred to as a **rogue DHCP server**.

Multiple DHCP servers are not inherently problematic when they are intentionally designed and coordinated. The risk comes from unauthorized or conflicting configurations.

### 5.4 — Importance of Network Planning

I learned that network design decisions should be made before deploying infrastructure services.

Services such as Active Directory, DNS, and DHCP depend on reliable network connectivity and correct IP addressing.

Establishing a controlled virtual network first helps reduce configuration problems during later stages of deployment.

### 5.5 — Configuration Verification

This exercise reinforced the importance of verifying infrastructure changes after implementation.

Rather than assuming the virtual switch was created successfully, I reopened the management interface and confirmed the saved configuration.

This verification process is an important part of professional systems administration and troubleshooting.

---

## 6. Challenges and Troubleshooting

### 6.1 — Selecting the Appropriate Virtual Switch Type

**Challenge:**

Determine which Hyper-V virtual switch type would provide the required lab connectivity without directly exposing the lab network to the physical home network.

**Investigation:**

Reviewed the functionality and intended use of External, Internal, and Private virtual switches.

**Resolution:**

Selected an Internal virtual switch to support communication between the Hyper-V host and lab virtual machines.

**Result:**

The switch was created successfully and verified.

### 6.2 — Avoiding DHCP Conflicts

**Challenge:**

Prevent a future lab DHCP server from interfering with the DHCP service already operating on the home network.

**Investigation:**

Considered the consequences of introducing a second, improperly configured DHCP server onto the same network segment.

**Resolution:**

Created a dedicated Internal virtual switch for the lab instead of directly connecting the virtual machines to the home network.

**Result:**

The lab has a separate virtual network segment for future DHCP testing.

### 6.3 — Outstanding Troubleshooting Tasks

No virtual switch creation errors were reported during this exercise.

Future networking exercises will investigate:

- Virtual network adapter configuration.
- Static IP addressing.
- Subnet masks and network boundaries.
- DNS server configuration.
- ICMP connectivity testing.
- DHCP address allocation.
- Domain authentication dependencies.

---

## 7. Evidence

### 7.1 — Evidence Overview

Screenshots are collected to demonstrate successful configuration and provide visual references for future troubleshooting.

The primary evidence for this exercise is the Hyper-V Virtual Switch Manager configuration screen.

### 7.2 — Evidence Inventory

| Evidence | Description | Status |
|---|---|---|
| Internal Virtual Switch | CQ-LAB-SWITCH configured as Internal | Captured; upload pending |
| Default Switch | Existing Default Switch preserved | Visible in captured screenshot |
| Server Network Adapter | Connection to CQ-LAB-SWITCH | Future exercise |
| Client Network Adapter | Connection to CQ-LAB-SWITCH | Future exercise |
| IP Configuration | Assigned addresses and subnet information | Future exercise |
| Connectivity Test | Server-to-client network communication | Future exercise |

### 7.3 — Screenshot Storage

The screenshot will be stored in the following repository location:

`images/internal-virtual-switch.png`

Once the image has been uploaded, it can be referenced using the following Markdown:

```markdown
![Hyper-V Internal Virtual Switch Configuration](../images/internal-virtual-switch.png)
```

The image will show the `CQ-LAB-SWITCH` configuration in Hyper-V Virtual Switch Manager.

### 7.4 — Evidence Security

Before uploading screenshots to GitHub, I will verify that they do not expose:

- Passwords or authentication credentials.
- Authentication tokens or recovery keys.
- Confidential organizational information.
- Personal information.
- Sensitive network configurations.

All evidence will be reviewed before being published.

---

## 8. Conclusion

The Hyper-V internal virtual switch configuration was completed successfully.

The `CQ-LAB-SWITCH` virtual switch was created and verified as an Internal network, providing a dedicated network segment for the enterprise IT lab.

This exercise introduced important concepts related to virtual networking, network isolation, DHCP conflicts, and configuration verification.

The completed configuration establishes the networking foundation required for future Windows Server and Active Directory exercises.

### Next Exercise — Mission 003: Virtual Machine Network Configuration

The next exercise will involve:

1. Connecting the Windows Server 2022 virtual machine to `CQ-LAB-SWITCH`.
2. Connecting the Windows 11 client virtual machine to the same switch.
3. Designing a dedicated lab IP addressing scheme.
4. Assigning appropriate IP configurations.
5. Testing network connectivity between the virtual machines.
6. Investigating and resolving any connectivity problems.

**Mission Status:** Completed

**Evidence Status:** Screenshot upload pending
