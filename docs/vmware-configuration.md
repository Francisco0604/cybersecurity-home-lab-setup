# VMware Workstation Configuration Documentation

This document explains the hypervisor settings, virtual hardware resource allocation, network parameters, display settings, and snapshot strategy configured in VMware Workstation Pro 17.6.3.

---

## Resource Allocation & Rationale

| Component | Configuration | Technical Rationale |
| :--- | :--- | :--- |
| **Hypervisor** | VMware Workstation Pro 17.6.3 | Provides reliable hardware virtualization, virtual network isolation controls, and snapshot management. |
| **Memory (RAM)** | 3 GB (3072 MB) | Allocates sufficient memory for the XFCE desktop and CLI tools while leaving resource headroom for the Windows host OS. |
| **Processors** | 1 CPU (2 Cores) | Provides 2 virtual CPU cores for parallel execution of system tasks and multi-threaded terminal tools. |
| **Disk Storage** | 80 GB (Thin Provisioned) | Reserves virtual storage capacity for package updates and capture files without immediately allocating 80 GB of host physical storage. |
| **Network Adapter** | NAT (`VMnet8`) | Routes guest traffic through host IP translation, providing outbound network access while isolating the guest from the external physical LAN. |
| **Display Settings** | 3D Acceleration Disabled | Disabling 3D acceleration prevents GPU driver rendering conflicts and display glitches within the virtualized XFCE environment. |

---

## Network Configuration Details

* **Adapter Type**: NAT (`VMnet8`)
* **Subnet Addressing**: Managed dynamically by VMware virtual DHCP/NAT engine.
* **Gateway & DNS**: Resolved through VMware host network adapter interface.
* **Security Control**: Prevents external inbound traffic from directly reaching the guest operating system interface.

---

## Snapshot Strategy

VMware snapshots capture the complete virtual disk state and memory state of a virtual machine at a specific point in time.

### Configuration
* **Baseline Snapshot**: `Baseline Clean Setup`
* **Condition**: Created immediately after guest OS installation, package updates, and `open-vm-tools-desktop` verification.

### Purpose
Taking a clean baseline snapshot ensures that if guest configuration files become corrupted or packages fail during operational practice, the virtual machine can be reverted to a working state within seconds without requiring a full OS re-installation.
