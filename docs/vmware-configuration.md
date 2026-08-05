# VMware Workstation Hypervisor Configuration ⚙️

While virtualization engines such as Oracle VM VirtualBox can also be utilized, **VMware Workstation Pro / Player** is used throughout this write-up for hypervisor management, custom virtual networking, and snapshot trees.

---

## 1. Virtual Network Editor Setup (VMnet)

1. Open **VMware Workstation as Administrator**.
2. Go to **Edit** > **Virtual Network Editor**.
3. Click **Change Settings** to elevate permissions.
4. Add or edit a custom VMnet adapter (e.g., `VMnet8` or `VMnet2`):
   * **Network Type:** NAT or Host-Only (Isolated from external LAN).
   * **Subnet IP:** `192.168.10.0`
   * **Subnet Mask:** `255.255.255.0`
5. Ensure DHCP settings are set to lease addresses within `192.168.10.100 - 192.168.10.200` to prevent collisions with static IPs.

![VMware Settings](../screenshots/vmware-settings.png)

---

## 2. VM Creation & Storage Types

When creating Virtual Machines in VMware Workstation:
* **For ISO Installer Images (.ISO):**
  1. Select **File > New Virtual Machine > Typical / Custom**.
  2. Select **Installer disc image file (iso)** and point to the downloaded `.iso` file.
  3. Ensure **VMware Tools / Open VM Tools** are installed post-OS wizard for optimal resolution, guest isolation, and clipboard sharing.
* **For Pre-built VM Appliances (.OVA / .VMDK):**
  1. Extract the downloaded `.7z` / `.zip` archive.
  2. Double-click the `.vmx` file or select **File > Open** in VMware to import directly.

---

## 3. Resource Allocation Guidelines

| Machine | Recommended RAM | vCPU Cores | Disk Provisioning |
| :--- | :--- | :--- | :--- |
| **Domain Controller** | 4096 MB | 2 Cores | Thin Provision, 60 GB |
| **Windows 10 Client** | 4096 MB | 2 Cores | Thin Provision, 50 GB |
| **Kali Linux** | 4096 MB | 2 Cores | Thin Provision, 40 GB |
| **Ubuntu Target** | 2048 MB | 1 Core | Thin Provision, 30 GB |

---

## 4. VM Snapshot Strategy

Always take a **Clean Baseline Snapshot** before running security assessments:
1. Shut down all lab virtual machines.
2. Select VM > **VM** > **Snapshot** > **Take Snapshot...**.
3. Title: `Baseline Clean State - AD & Web Target Configured`.
4. Restore to this snapshot whenever testing destructive exploits or malware payloads.

---

## 5. 💡 Troubleshooting: Missing or Invisible Mouse Cursor

If your mouse cursor disappears or becomes invisible when working inside a virtual machine (common after OS updates or fresh Kali Linux installs):

1. **Upgrade Virtual Machine Hardware Compatibility**:
   * Fully **Power off** the virtual machine.
   * Right-click the virtual machine in the VMware Library pane.
   * Select **Manage** > **Upgrade Hardware Compatibility...** (or click **Upgrade Virtual Machine**).
   * Follow the wizard to upgrade to the latest supported VMware hardware version.
   * Power on the virtual machine; the mouse cursor will render properly.
2. **Re-capture / Release Mouse Input**: Use the `Ctrl + Alt` shortcut to release the cursor from the VM frame if trapped.
3. **Ensure Guest Utilities are Installed**: Install `open-vm-tools-desktop` on Linux guests.

