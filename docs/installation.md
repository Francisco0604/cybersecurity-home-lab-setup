# Detailed Deployment & Installation Guide 🚀

This document details the step-by-step installation instructions for building each node in the cybersecurity home lab, covering both **Pre-built Virtual Machine Appliances** and **ISO Installer Images**.

---

## 📌 Virtual Machine Deployment Methods Overview

When building your virtualized home lab, you can choose between two main VM deployment methods:

| Feature | Method A: Pre-built VM Appliances (.OVA / .VMDK) | Method B: ISO Installer Images (.ISO) |
| :--- | :--- | :--- |
| **Setup Time** | ⚡ Very Fast (5-10 minutes) | ⏱️ Moderate (20-30 minutes) |
| **Ease of Configuration** | Import & run out-of-the-box | Requires step-by-step OS installation wizard |
| **Use Cases** | Quick penetration testing setup (e.g. Kali Linux) | Standard enterprise deployment (Windows Server, Win 10, Custom Linux) |
| **Control** | Standard default partition & package layout | Complete control over disk partitions, user accounts, & packages |

![VM Deployment Options](../screenshots/Chossing_Virtual_machines_instead_of_installer_images.png)
*Figure 1: Choosing between Pre-built Virtual Machines and ISO Installer Images.*

![Download Recommended](../screenshots/Download_recommended.png)
*Figure 2: Selecting the recommended pre-built appliance image or ISO installer download.*

---

## 1. Domain Controller Installation (Windows Server 2019 / 2022)

Windows Server is deployed using an **ISO Installer Image**.

### Step 1.1: VM Creation & ISO Mounting
1. Launch **VMware Workstation** and select **File > New Virtual Machine** (Custom/Typical).
2. Choose **Installer disc image file (iso)** and browse to your `Windows_Server_2019.iso`.
3. Select Guest Operating System: **Windows Server 2019** (or 2022).
4. Specify VM Name: `DC-01` and storage location.
5. Hardware Allocation:
   * **RAM:** 4096 MB (4 GB)
   * **Processors:** 2 Cores
   * **Disk Size:** 60 GB (Store virtual disk as a single file, Thin Provisioned)
   * **Network Adapter:** `VMnet8` / Custom Isolated Subnet (`192.168.10.0/24`)

### Step 1.2: OS Installation Wizard
1. Power on the VM and press any key to boot from CD/DVD.
2. Select Language, Time, and Keyboard preferences.
3. Select **Windows Server 2019 Standard/Datacenter (Desktop Experience)**.
4. Accept license terms and select **Custom: Install Windows only (advanced)**.
5. Select the unallocated 60 GB drive and click **Next** to proceed with installation.
6. Set the local Administrator account password upon reboot.
7. Install **VMware Tools** (`VM > Install VMware Tools...`) and restart.

### Step 1.3: Active Directory DS & DNS Promotion
1. Set Static IP Address (`192.168.10.10`), Subnet Mask (`255.255.255.0`), DNS (`127.0.0.1`).
2. Open **Server Manager** > **Add Roles and Features**:
   * Select **Active Directory Domain Services (AD DS)**
   * Select **DNS Server**
3. Click **Promote this server to a domain controller**:
   * Select **Add a new forest**
   * Specify Root Domain Name: `CORP.LOCAL`
   * Set DSRM password and complete installation.
4. Reboot `DC-01`.

---

## 2. Active Directory User & SPN Setup

1. Open **Active Directory Users and Computers** (`dsa.msc`).
2. Create Organizational Units (OUs):
   * `CORP_Employees`
   * `IT_Admins`
   * `Service_Accounts`
3. Create test domain user accounts: `jsmith`, `mrossi`, `sql_service`.
4. Configure a Service Principal Name (SPN) for Kerberoasting practice:
   ```cmd
   setspn -a MSSQLSvc/sql01.corp.local:1433 CORP\sql_service
   ```

---

## 3. Windows 10 Endpoint Deployment & Domain Join

Windows 10 Enterprise is deployed via **ISO Installer Image**.

### Step 3.1: ISO Installation
1. Create a new VM in VMware Workstation attached to `Windows_10_Enterprise.iso`.
2. Allocate **4 GB RAM**, **2 vCPUs**, and **50 GB Storage**.
3. Complete the standard Windows 10 installation wizard (Name host: `WKSTN-01`).
4. Install **VMware Tools** for display and input drivers.

### Step 3.2: Domain Join (`CORP.LOCAL`)
1. Open IPv4 Settings and set Preferred DNS Server to `192.168.10.10` (DC IP).
2. Open **System Properties** (`sysdm.cpl`) > **Computer Name** > **Change...**.
3. Change domain membership to **Domain**: `CORP.LOCAL`.
4. Enter Domain Admin credentials (`CORP\Administrator`).
5. Reboot `WKSTN-01` to finish domain join.

---

## 4. Kali Linux Attacker Workstation Setup

Kali Linux can be deployed using **either** method:

### Option A: Installing via ISO Installer Image (.ISO) - Recommended for Custom Builds
1. Download `kali-linux-2024.x-installer-amd64.iso`.
2. Create a New Virtual Machine:
   * **OS Type:** Linux > Debian 11/12 64-bit
   * **RAM:** 4096 MB | **vCPU:** 2 Cores | **Disk:** 40 GB
3. Boot VM with ISO mounted and select **Graphical Install**.
4. Configure installation settings:
   * **Hostname:** `kali-attack`
   * **Domain:** `corp.local` (or leave blank)
   * **User Creation:** Create non-root admin user (`kali`).
   * **Partitioning:** Guided - use entire disk (Ext4 & Swap).
   * **Software Selection:** Keep default XFCE desktop environment + default toolset (`top 10` & `default`).
   * **GRUB Boot Loader:** Select `/dev/sda` for master boot record installation.
5. Finish installation, reboot, and log in.
6. Install guest utilities and update:
   ```bash
   sudo apt update && sudo apt install -y open-vm-tools-desktop
   sudo reboot
   ```

### Option B: Importing Pre-built Virtual Machine (.OVA / .VMDK) - Quick Setup
1. Download pre-built Kali VMware image (`.7z` or `.ova`).
2. Extract archive and open `.vmx` or import `.ova` directly into VMware Workstation.
3. Power on VM and log in with default credentials (`kali` / `kali`).
4. Change default credentials immediately:
   ```bash
   passwd
   ```

---

## 5. Vulnerable Web App Deployment (OWASP Juice Shop / DVWA)

### Option A: Ubuntu ISO Installation + Docker
1. Create a VM using `ubuntu-22.04-live-server.iso` (2 GB RAM, 1 vCPU, 30 GB Disk).
2. Complete server setup and install Docker:
   ```bash
   sudo apt update && sudo apt install -y docker.io docker-compose
   sudo systemctl enable --now docker
   ```
3. Run OWASP Juice Shop in container:
   ```bash
   docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
   ```
4. Access web application at `http://192.168.10.30:3000`.

### Option B: Local Container on Kali Linux
If resource-constrained, run Juice Shop directly inside Kali Linux:
```bash
sudo docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
```

---

## 💡 Troubleshooting: Invisible or Missing Mouse Cursor

If the mouse cursor becomes invisible, disappears, or fails to render inside the Virtual Machine (a common issue in Linux ISO installs such as Kali Linux or after software updates):

### Primary Solution: Upgrade Virtual Machine Hardware Compatibility
1. **Power off** the virtual machine completely.
2. In VMware Workstation, right-click the virtual machine name in the left Library sidebar (or navigate to the **VM** menu).
3. Select **Manage** > **Upgrade Hardware Compatibility...** (or click **Upgrade Virtual Machine**).
4. Follow the wizard prompts and select the latest compatible VMware hardware version (e.g. Workstation 17.x).
5. Click **Finish** to complete the upgrade and power on the VM.

### Alternative Quick Fixes
* **Install / Reinstall Guest Tools**: Ensure `open-vm-tools-desktop` is installed inside the Linux guest:
  ```bash
  sudo apt update && sudo apt install -y open-vm-tools-desktop
  ```
* **Release & Recapture Mouse Input**: Press `Ctrl + Alt` to release mouse focus back to the host machine, then click inside the VM display window to recapture cursor focus.
* **Toggle Display Acceleration**: Go to **VM Settings > Display** and uncheck **Accelerate 3D graphics** if cursor rendering artifacts persist.

---

## 🛠️ Verification & Next Steps

Once all VMs are deployed via ISO or pre-built images:
1. Ensure all network adapters are set to `VMnet8` (Custom Host-Only / NAT Subnet).
2. Verify static IP assignments (`192.168.10.10`, `.20`, `.30`, `.50`).
3. Proceed to the [**VMware Configuration Guide**](vmware-configuration.md) to set up baseline snapshots.


