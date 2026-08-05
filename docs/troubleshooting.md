# Troubleshooting Documentation

This document records technical issues encountered during the lab deployment, including environment details, attempted remediation steps, root cause analysis, and the final verified solution.

---

## Issue #1: Invisible Mouse Cursor in Kali Linux Guest

### Problem Description
Upon booting a pre-built Kali Linux VMware virtual machine appliance (`.ova` / `.vmx`), the mouse cursor became completely invisible within the XFCE desktop environment. Keyboard inputs and mouse clicks functioned, but the mouse pointer could not be visually tracked on screen.

### Environment Details
* **Host OS**: Windows 11 25H2
* **Hypervisor**: VMware Workstation Pro 17.6.3
* **Guest OS**: Kali Linux (Pre-built VMware Appliance vs. Official Installer ISO)
* **Desktop Environment**: XFCE

---

### Attempted Solutions & Results

| Attempted Fix | Procedure | Result |
| :--- | :--- | :--- |
| **Reinstall Guest Tools** | Ran `sudo apt install --reinstall open-vm-tools-desktop` via terminal | ❌ Unresolved (Cursor remained invisible) |
| **Disable 3D Acceleration** | Unchecked **Accelerate 3D graphics** in VM Display settings | ❌ Unresolved (Cursor remained invisible) |
| **VMX Configuration Edit** | Added `mks.inVMotion = "TRUE"` parameter to the VM configuration file | ❌ Unresolved (Cursor remained invisible) |

---

### Root Cause Analysis
Pre-packaged virtual machine appliance images contain pre-configured X11 display driver settings and kernel modules that can experience rendering mismatches with certain host GPU display drivers and hypervisor hardware compatibility profiles.

---

### Final Verified Solution
Replaced the pre-built VM appliance by creating a new Virtual Machine and performing a clean, manual operating system installation using the official **Kali Linux Installer ISO (`kali-linux-2026.2-installer-amd64.iso`)**. 

Installing directly from the installer ISO allowed the operating system to detect the virtual display hardware during setup and configure the correct display drivers natively.

#### Alternative Hardware Upgrade Workaround
For pre-built VM appliances exhibiting hidden cursor artifacts, an alternative fix is:
1. Shut down the virtual machine.
2. In VMware Workstation, navigate to **VM** > **Manage** > **Upgrade Hardware Compatibility...**.
3. Select the latest hardware compatibility profile (Workstation 17.x).
4. Power on the VM; mouse cursor rendering returns to normal.

---

## Lessons Learned
1. **Pre-built Appliances vs. Manual ISO Installs**: Pre-configured VM appliances offer fast deployment, but manual ISO installations provide control over input driver detection and partition layouts.
2. **Systematic Troubleshooting**: Documenting failed attempts prevents repetitive troubleshooting cycles and isolates root cause variables.
