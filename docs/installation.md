# Kali Linux Installation Documentation

This document records the installation steps, configuration choices, system updates, and initial driver setup for the Kali Linux virtual machine.

---

## Technical Specifications

| Parameter | Configuration |
| :--- | :--- |
| **Operating System** | Kali Linux 2026.2 (64-bit) |
| **Media Source** | `kali-linux-2026.2-installer-amd64.iso` |
| **Hypervisor** | VMware Workstation Pro 17.6.3 |
| **Host System** | Windows 11 25H2 |
| **Allocated Memory** | 3 GB (3072 MB) |
| **Allocated Processors**| 1 CPU (2 vCPU Cores) |
| **Storage Provisioning**| 80 GB Thin Provisioned Virtual Disk |
| **Network Mode** | NAT (`VMnet8`) |
| **Desktop Environment**| XFCE |

---

## Installation Process

### 1. Virtual Machine Creation
1. Opened VMware Workstation Pro 17.6.3 and selected **Create a New Virtual Machine**.
2. Selected **Typical (recommended)** setup.
3. Specified the installer ISO file path (`kali-linux-2026.2-installer-amd64.iso`).
4. Set the guest operating system type to **Linux** and version to **Debian 11/12 64-bit**.
5. Named the virtual machine `KALI-ATTACK`.
6. Allocated 80 GB virtual disk capacity stored as a single file.
7. Customized hardware parameters to allocate 3 GB RAM and 2 vCPU cores.

### 2. Operating System Installation
1. Booted the VM and selected **Graphical Install** from the GRUB boot menu.
2. Selected language (**English**), territory (**United States**), and keymap (**American English**).
3. Set the system hostname to `kali-attack` and left the domain name blank.
4. Created a non-root administrative user account (`kali`).
5. Configured disk partitioning using **Guided - use entire disk** with the default single-partition scheme (Ext4 filesystem and Swap space).
6. Confirmed partition changes and wrote modifications to disk.
7. Accepted default software selection choices: **XFCE Desktop Environment**, **top 10 tools**, and **default toolset**.
8. Installed the GRUB boot loader to the primary virtual disk drive (`/dev/sda`).
9. Finished installation and rebooted the system into the installed OS.

### 3. Post-Installation Commands & Guest Driver Setup
Logged into the system and executed system updates and driver installation commands in the terminal:

```bash
# 1. Update package repository indexes and upgrade existing packages
sudo apt update && sudo apt upgrade -y

# 2. Install open-vm-tools-desktop for dynamic display resizing and clipboard sharing
sudo apt install -y open-vm-tools-desktop

# 3. Reboot the system to initialize updated drivers and kernel modules
sudo reboot
```

System installation and configuration were verified post-reboot using standard system utility commands:

```bash
whoami
hostnamectl
uname -a
ip a
```

![Kali Linux Desktop](screenshots/02-kali-desktop.png)
*Figure 1: Kali Linux XFCE desktop environment after installation.*

![System Information](screenshots/03-system-information.png)
*Figure 2: System information verification showing kernel and OS details (`hostnamectl`, `uname -a`).*

![Terminal Overview](screenshots/05-terminal-overview.png)
*Figure 3: Terminal execution and user account verification (`whoami`).*

![Network Information](screenshots/06-network-information.png)
*Figure 4: Network interface configuration verification (`ip a`).*

---

## Snapshot Creation

After verifying successful installation, network access, and display driver integration, a baseline snapshot was created in VMware Workstation:

* **Snapshot Name**: `Fresh Kali`
* **State**: System powered off.
* **Purpose**: Provides a known clean restoration point for guest system recovery.
