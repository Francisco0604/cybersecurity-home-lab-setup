# Cybersecurity Home Lab 🛡️

Designed & Documented by **Francisco Elroy Afonso**  
*Junior Penetration Tester | Practical Ethical Hacker (PEH) | Google Cybersecurity Professional*

---

## 📌 Overview

This repository documents the setup of my personal cybersecurity lab environment.

The lab is used to practice Linux administration, network scanning, penetration testing techniques, web application security, and offensive security methodologies in a safe, isolated virtual environment.

I will continue expanding this repository as I build additional target machines, labs, and security projects.

---

## 🎯 Objectives & Why I Built This

The goal of this lab is to establish a reliable local platform for practical hands-on learning:

* **Linux Administration & CLI**: System navigation, bash scripting, network commands, and package management.
* **Offensive Tooling**: Hands-on usage of Nmap, Burp Suite, Wireshark, Git, and security utilities.
* **Web Application Security**: Preparing for OWASP Top 10 vulnerabilities testing (Burp Suite, Juice Shop, DVWA).
* **Network Security & Isolation**: Configuring hypervisor networking, NAT, and host isolation.
* **Technical Problem Solving**: Documenting real setup challenges, root cause analyses, and verified fixes.

---

## 💻 Current Environment

| Component | Specification |
| :--- | :--- |
| **Host System** | Windows 11 (25H2) |
| **Hypervisor** | VMware Workstation Pro 17.6.3 |
| **Guest OS** | Kali Linux 2024.x / 2026.x |
| **Desktop Environment** | XFCE Desktop |
| **VM Hardware Specs** | 3 - 4 GB RAM \| 2 vCPUs \| 40 - 80 GB Storage |
| **Network Type** | NAT / Isolated Subnet |

---

## 📊 Current Progress

- [x] **Hypervisor Setup**: VMware Workstation installed & configured
- [x] **Kali Linux Deployment**: Manually installed Kali Linux from official Installer ISO
- [x] **Guest Integration & Utilities**: Configured `open-vm-tools-desktop` & display drivers
- [x] **Network Connectivity**: Verified NAT networking & IP assignment
- [x] **Clean Snapshot**: Created clean baseline VM snapshot in VMware
- [ ] **Burp Suite Setup & Intercept Configuration**
- [ ] **OWASP Juice Shop Target Deployment**
- [ ] **DVWA Target Deployment**
- [ ] **Active Directory Lab Build (Windows Server & Domain Controller)**

---

## 📂 Repository Structure & Documentation Index

```text
cybersecurity-home-lab/
├── README.md                           # Main repository overview & lab progress
├── screenshots/                        # Visual setup verification & evidence
│   ├── vmware-settings.png            # VMware Workstation network & VM settings
│   ├── kali-desktop.png               # Kali Linux XFCE desktop & terminal
│   ├── Chossing_Virtual_machines_instead_of_installer_images.png # VM vs Installer comparison
│   └── Download_recommended.png       # Appliance vs ISO image download options
└── docs/                               # Detailed lab documentation
    ├── installation.md                 # Kali Linux ISO installation steps & lessons learned
    ├── vmware-configuration.md         # Hardware allocation, NAT network & snapshot settings
    ├── troubleshooting.md             # Real-world problem resolution (Invisible mouse cursor fix)
    └── roadmap.md                      # Future project build plan & learning roadmap
```

### 📚 Documentation Links:
* 📘 [**Installation Guide (`docs/installation.md`)**](docs/installation.md): Step-by-step setup of Kali Linux via Installer ISO and key lessons learned.
* ⚙️ [**VMware Configuration Guide (`docs/vmware-configuration.md`)**](docs/vmware-configuration.md): Hypervisor resource allocation, NAT networking, and snapshot strategy.
* 🛠️ [**Troubleshooting Guide (`docs/troubleshooting.md`)**](docs/troubleshooting.md): Detailed root cause analysis and resolution for the invisible mouse cursor bug.
* 🗺️ [**Lab Roadmap (`docs/roadmap.md`)**](docs/roadmap.md): Multi-week roadmap for upcoming web targets and Active Directory labs.

---

## 📷 Visual Verification

### Virtual Machine Acquisition Options
![VM vs ISO Installer](screenshots/Chossing_Virtual_machines_instead_of_installer_images.png)

![Download Options](screenshots/Download_recommended.png)

### VMware Configuration & Kali Desktop
![VMware Settings](screenshots/vmware-settings.png)

![Kali Desktop](screenshots/kali-desktop.png)

---

## 📜 License & Usage
This repository is built for educational, research, and defensive security testing purposes only.
