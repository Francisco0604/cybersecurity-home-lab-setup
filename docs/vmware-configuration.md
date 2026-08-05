# VMware Workstation Configuration ⚙️

This document details the hypervisor configuration, virtual network settings, resource allocation, and snapshot strategy used in VMware Workstation Pro.

---

## 💻 Resource Allocation

| Hardware Component | Configuration | Rationale |
| :--- | :--- | :--- |
| **RAM / Memory** | 3072 MB (3 GB) | Optimal balance for host performance and smooth XFCE desktop execution |
| **Processors** | 1 CPU, 2 vCPU Cores | Enables efficient multi-threading for tool execution (Nmap, Burp Suite) |
| **Storage Disk** | 80 GB Thin Provisioned | Sufficient space for toolsets, logs, and capture files without consuming host storage immediately |
| **Network Adapter** | NAT (`VMnet8`) | Provides guest internet access while maintaining isolated subnet routing |
| **Display Acceleration**| 3D Graphics Disabled | Prevents graphics driver stuttering and cursor rendering bugs |

---

## 🌐 Network Configuration (NAT)

1. Open **VMware Workstation Pro**.
2. Go to **Edit** > **Virtual Network Editor** and click **Change Settings**.
3. Select `VMnet8` (NAT):
   * **Subnet IP:** Assigned dynamically via hypervisor NAT engine (e.g. `192.168.x.0`).
   * **Subnet Mask:** `255.255.255.0`
   * **DHCP:** Enabled for automatic guest network configuration.

---

## 📸 VM Snapshot Strategy

Creating clean baseline snapshots allows restoring the VM to a pristine state whenever software breaks or testing offensive tools.

### Taking a Baseline Snapshot
1. Power off or pause the Kali Linux Virtual Machine.
2. Navigate to **VM** > **Snapshot** > **Take Snapshot...**.
3. **Name**: `Baseline Clean Setup - Kali Linux Installed`.
4. **Description**: *Clean installation from ISO with open-vm-tools-desktop, updated packages, and NAT networking verified.*
