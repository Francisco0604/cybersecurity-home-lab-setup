# Detailed Deployment & Installation Guide 🚀

This document details the step-by-step installation instructions for building each node in the cybersecurity home lab.

---

## 1. Domain Controller Installation (Windows Server)

1. Launch **VMware Workstation** and create a new Virtual Machine.
2. Select **Windows Server 2019/2022 ISO**.
3. Allocate **4 GB RAM**, **2 vCPUs**, and **60 GB Disk** (Store virtual disk as single file).
4. Complete standard Windows Server installation (Desktop Experience).
5. Open **Server Manager** > **Add Roles and Features**:
   * Select **Active Directory Domain Services (AD DS)**
   * Select **DNS Server**
6. Click **Promote this server to a domain controller**:
   * Select **Add a new forest**
   * Specify Root Domain Name: `CORP.LOCAL`
   * Set DSRM password and click Install.
7. Reboot the Domain Controller.

---

## 2. Active Directory User & SPN Setup

1. Open **Active Directory Users and Computers** (`dsa.msc`).
2. Create Organizational Units (OUs):
   * `CORP_Employees`
   * `IT_Admins`
   * `Service_Accounts`
3. Create test accounts: `jsmith`, `mrossi`, `sql_service`.
4. Configure a Service Principal Name (SPN) for Kerberoasting tests:
   ```cmd
   setspn -a MSSQLSvc/sql01.corp.local:1433 CORP\sql_service
   ```

---

## 3. Windows 10 Endpoint Deployment & Domain Join

1. Install **Windows 10 Enterprise** VM on VMware Workstation.
2. Set IPv4 Preferred DNS Server to `192.168.10.10` (DC IP).
3. Open **System Properties** > **Rename this PC (advanced)** > **Change...**.
4. Select **Member of Domain**: `CORP.LOCAL`.
5. Enter Domain Admin credentials (`CORP\Administrator`).
6. Reboot to complete domain join.

---

## 4. Kali Linux Attacker Workstation Setup

1. Import or install **Kali Linux 2024.x ISO/OVA** into VMware Workstation.
2. Update package repositories and verify security tools:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install nmap responder impacket-scripts burpsuite ffuf sqlmap -y
   ```
3. Test network connectivity to the lab subnet:
   ```bash
   ping -c 4 192.168.10.10
   ```

---

## 5. Vulnerable Web App (Docker OWASP Juice Shop)

1. Deploy an **Ubuntu Linux VM** or configure Docker on Kali.
2. Run OWASP Juice Shop in Docker:
   ```bash
   docker run -d -p 3000:3000 bkimminich/juice-shop
   ```
3. Access Juice Shop at `http://192.168.10.30:3000`.
