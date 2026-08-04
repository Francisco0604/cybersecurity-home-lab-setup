# Lab Network Architecture & Routing Guide 🌐

This document details the network topology, IP address mapping, DNS routing, and host isolation rules configured for the cybersecurity home lab.

---

## 1. IP Address & Subnet Allocation Matrix

* **Subnet CIDR:** `192.168.10.0/24`
* **Subnet Mask:** `255.255.255.0`
* **Gateway IP:** `192.168.10.2`
* **Broadcast IP:** `192.168.10.255`

### Host Mapping Table

| IP Address | Hostname | Operating System | Network Role | DNS Config |
| :--- | :--- | :--- | :--- | :--- |
| `192.168.10.10` | **DC-01** | Windows Server 2019 | Domain Controller & DNS | `127.0.0.1` |
| `192.168.10.20` | **WKSTN-01** | Windows 10 Enterprise | Domain Client Endpoint | `192.168.10.10` |
| `192.168.10.30` | **WEB-TARGET** | Ubuntu Linux / Docker | OWASP Web Vulnerable Host | `192.168.10.10` |
| `192.168.10.50` | **KALI-ATTACK** | Kali Linux 2024.x | Offensive Workstation | `192.168.10.10` |

---

## 2. DNS & Name Resolution

* **Active Directory Domain:** `CORP.LOCAL`
* All Windows clients and target servers point to `192.168.10.10` as their primary DNS server to resolve internal active directory records (`dc-01.corp.local`, `sql01.corp.local`).

---

## 3. Network Verification & Recon Commands

Run these verification commands from Kali Linux (`192.168.10.50`):

```bash
# 1. Verify gateway and Domain Controller reachability
ping -c 3 192.168.10.10

# 2. Perform Nmap Host Discovery Ping Sweep
nmap -sn 192.168.10.0/24

# 3. Perform Full TCP Service & OS Scan on Domain Controller
nmap -A -p 53,88,135,139,389,445,3268 -T4 192.168.10.10
```
