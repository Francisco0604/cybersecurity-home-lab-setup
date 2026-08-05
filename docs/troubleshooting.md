# Lab Troubleshooting & Problem Resolution 🛠️

Documenting technical hurdles, root cause analyses, and verified resolutions demonstrates practical engineering and security problem-solving skills. This document details real-world issues encountered during the lab deployment and their fixes.

---

## Issue #1: Invisible Mouse Cursor in Kali Linux Virtual Machine

### 📌 Problem Description
After booting Kali Linux in VMware Workstation (or performing initial system package upgrades), the mouse cursor becomes completely invisible or fails to render inside the virtual machine display area. Mouse clicks may still register, but the pointer cannot be located visually.

### 🔍 Root Cause Analysis
* **Hypervisor Hardware Compatibility Mismatch**: Older default VM hardware profiles in VMware Workstation can fail to support modern X11/Wayland display server cursor rendering routines.
* **Pre-built OVA Appliance Display Driver Conflicts**: Pre-configured virtual machine appliances occasionally experience display driver mismatches with host GPU acceleration layers.

### 🛠️ Resolution Steps

#### Solution 1: Upgrade VM Hardware Compatibility (Recommended)
1. **Power off** the Kali Linux virtual machine completely.
2. Right-click `KALI-ATTACK` in the VMware Workstation library panel.
3. Select **Manage** > **Upgrade Hardware Compatibility...** (or click **Upgrade Virtual Machine**).
4. Select the latest available VMware Workstation hardware version (e.g. Workstation 17.x).
5. Complete the wizard and power on the VM. The cursor will render correctly.

#### Solution 2: Deploying via Official ISO Installer
If using a pre-built `.ova` appliance continues to display input glitches, install Kali Linux manually using the official **Installer ISO Image (`.iso`)**:
1. Attach `kali-linux-2024.x-installer-amd64.iso` to a new Debian 64-bit VM.
2. Complete the Graphical Installation wizard.
3. Install guest desktop utilities:
   ```bash
   sudo apt update && sudo apt install -y open-vm-tools-desktop
   sudo reboot
   ```

#### Solution 3: Keyboard Shortcuts & Display Settings
* Press `Ctrl + Alt` to release mouse capture back to the host, then click back into the guest screen.
* Go to **VM Settings > Display** and uncheck **Accelerate 3D graphics** if visual artifacts persist.

---

## Issue #2: Active Directory Domain Join DNS Resolution Failure

### 📌 Problem Description
When attempting to join the Windows 10 workstation (`WKSTN-01`) to the Active Directory domain (`CORP.LOCAL`), the system returns an error: *"An Active Directory Domain Controller (AD DC) for the domain CORP.LOCAL could not be contacted."*

### 🔍 Root Cause Analysis
The Windows 10 client VM was using default host DHCP settings or public DNS servers (e.g. `8.8.8.8`), preventing it from querying internal Active Directory SRV and A records managed by the Domain Controller (`DC-01`).

### 🛠️ Resolution Steps
1. On `WKSTN-01`, open **Network and Sharing Center** > **Change adapter settings**.
2. Right-click the network adapter > **Properties** > **Internet Protocol Version 4 (TCP/IPv4)**.
3. Set **Preferred DNS Server** explicitly to `192.168.10.10` (the Domain Controller's static IP).
4. Open Command Prompt and verify DNS resolution:
   ```cmd
   nslookup corp.local
   ping dc-01.corp.local
   ```
5. Retry domain join via `sysdm.cpl`. The domain join completes successfully.

---

## Issue #3: Web Target Host Unreachable from Attacker Workstation

### 📌 Problem Description
Kali Linux (`192.168.10.50`) cannot access the OWASP Juice Shop web target (`http://192.168.10.30:3000`).

### 🔍 Root Cause Analysis
The target VM network adapter was assigned to `VMnet1` (Host-Only) instead of `VMnet8` (Custom Subnet `192.168.10.0/24`), placing the target on a separate virtual broadcast domain.

### 🛠️ Resolution Steps
1. Shut down `WEB-TARGET`.
2. Open VM Settings > **Network Adapter**.
3. Change Network Connection to **Custom: Specific virtual network** -> Select `VMnet8` (or designated lab subnet).
4. Power on VM and confirm IP assignment (`192.168.10.30`).
5. Verify web reachability from Kali Linux:
   ```bash
   curl -I http://192.168.10.30:3000
   ```
