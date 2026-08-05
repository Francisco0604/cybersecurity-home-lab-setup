# Troubleshooting Log 🛠️

Documenting technical challenges and root cause analyses demonstrates practical problem-solving skills to recruiters and engineering teams.

---

## Issue #1: Invisible Mouse Cursor in Kali Linux VM

### 📌 Issue Description
After importing and booting the pre-built Kali Linux VMware image (`.ova` / `.vmx`), the mouse cursor became completely invisible inside the guest XFCE desktop environment. While mouse clicks registered, the pointer could not be visually tracked.

### 💻 Environment
* **Host OS**: Windows 11 (25H2)
* **Hypervisor**: VMware Workstation Pro 17.6.3
* **Guest OS**: Kali Linux (VMware Image vs. Installer ISO)

### 🧪 Attempted Fixes & Results

| Attempted Solution | Configuration Change | Result |
| :--- | :--- | :--- |
| **Reinstall VMware Tools** | Installed `open-vm-tools-desktop` via terminal | ❌ Failed (Cursor remained hidden) |
| **Disable 3D Acceleration** | Unchecked 3D Graphics in VM Display settings | ❌ Failed (Cursor remained hidden) |
| **VMX Configuration Edit** | Added `mks.inVMotion = "TRUE"` to `.vmx` file | ❌ Failed (No change in rendering) |

### ✅ Final Resolution
Installed Kali Linux from the official **Installer ISO (`kali-linux-installer-amd64.iso`)** instead of using the pre-built VMware appliance image.

### 🔍 Additional Workaround: VM Hardware Compatibility Upgrade
If using a VM image where the cursor is hidden, an alternative fix is:
1. Shut down the VM completely.
2. In VMware Workstation, right-click VM > **Manage** > **Upgrade Hardware Compatibility...**.
3. Upgrade to the latest hardware compatibility version (Workstation 17.x).
4. Power on VM; cursor renders properly.
