# Kali Linux Installation Guide 🚀

This document details the step-by-step installation choices, hypervisor resource allocation, and key lessons learned while deploying the Kali Linux offensive workstation.

---

## 📌 Installation Summary & Environment

| Property | Value |
| :--- | :--- |
| **Operating System** | Kali Linux (2024.x / 2026.x 64-bit) |
| **Download Source** | Official Kali Linux Website (`kali-linux-installer-amd64.iso`) |
| **Hypervisor** | VMware Workstation Pro 17.6.3 |
| **Host System** | Windows 11 (25H2) |
| **Allocated Memory** | 3 - 4 GB RAM |
| **Allocated Processors**| 1 Processor, 2 Cores |
| **Storage Provisioning**| 40 - 80 GB Thin Provisioned Virtual Disk |
| **Network Adapter** | NAT (`VMnet8`) |
| **Desktop Environment**| XFCE |

---

## 🛠️ Step-by-Step Installation Choices

### 1. VM Creation in VMware Workstation
1. Open **VMware Workstation Pro** and click **Create a New Virtual Machine**.
2. Select **Typical (recommended)** configuration.
3. Choose **Installer disc image file (iso)** and browse to the downloaded `kali-linux-installer-amd64.iso`.
4. Select Guest Operating System: **Linux** -> **Debian 11/12 64-bit**.
5. Set Virtual Machine Name to `KALI-ATTACK` and choose storage directory.
6. Allocate **3 GB RAM**, **2 vCPUs**, and **80 GB Disk** (store virtual disk as a single file).

### 2. Operating System Setup Wizard
1. Boot the VM and select **Graphical Install** from the boot menu.
2. Select Language (**English**), Location, and Keyboard Layout (**American English**).
3. Set Hostname: `kali-attack`. Leave domain name blank.
4. Configure user account:
   * **Full name:** `Kali User`
   * **Username:** `kali`
   * Set secure password.
5. Disk Partitioning: Select **Guided - use entire disk** (Ext4 filesystem withSwap).
6. Software Selection:
   * Keep default **XFCE** desktop environment.
   * Keep default top 10 tools & standard security utilities.
7. Install GRUB Boot Loader: Select `/dev/sda` for master boot record installation.
8. Complete installation and reboot into Kali Linux.

### 3. Post-Installation Configuration
Log in with created credentials and execute initial updates and guest utilities:
```bash
# 1. Update package lists and system tools
sudo apt update && sudo apt upgrade -y

# 2. Install open-vm-tools-desktop for display resolution & clipboard sharing
sudo apt install -y open-vm-tools-desktop

# 3. Restart system to activate guest drivers
sudo reboot
```

---

## 💡 Lessons Learned & Problems Encountered

### ❌ The Problem: Invisible Mouse Cursor in Pre-built VM Appliance
Initially, an attempt was made to import the pre-built Kali Linux VMware appliance (`.ova` / `.vmx`). However, upon booting into the XFCE desktop, the mouse cursor was completely invisible and failed to render inside the VM frame. Attempting to reinstall VMware Tools or adjust display acceleration settings did not resolve the cursor rendering bug.

### ✅ The Solution: Manual Installation via Installer ISO
Switching to the official **Installer ISO Image (`.iso`)** and performing a clean manual installation resolved the graphics and input rendering issues completely. Manual installation also provided deeper understanding of Linux partition structures and guest driver integration.

### 🎯 Key Takeaway for Security Engineering
* **Pre-packaged appliances** offer quick deployment, but driver conflicts with host GPUs or hypervisor versions can introduce unneeded bugs.
* **Manual ISO installations** provide total control over disk partitioning, system packages, and input drivers—ensuring environment stability.
