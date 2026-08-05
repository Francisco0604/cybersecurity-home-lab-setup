# Cybersecurity Home Lab

[![Hypervisor](https://img.shields.io/badge/Hypervisor-VMware%20Workstation%20Pro%2017.6.3-blue)](docs/vmware-configuration.md)
[![Guest OS](https://img.shields.io/badge/Guest%20OS-Kali%20Linux%202026.2-blueviolet)](docs/installation.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Designed and documented by **Francisco Elroy Afonso**

---

## Overview

This repository documents the setup and configuration of my personal virtualized cybersecurity lab environment. 

The lab consists of a Kali Linux virtual machine running under VMware Workstation Pro on a Windows host. It provides a local, isolated environment for practicing Linux system administration, network utilities, shell environments, and security tools.

---

## Environment Specifications

### Host Environment
* **Operating System**: Windows 11 25H2

### Virtualization Platform
* **Hypervisor**: VMware Workstation Pro 17.6.3

### Guest Virtual Machine
* **Operating System**: Kali Linux 2026.2 (64-bit)
* **Desktop Environment**: XFCE
* **Memory (RAM)**: 3 GB (3072 MB)
* **Processors**: 1 CPU (2 vCPU Cores)
* **Storage**: 80 GB Virtual Disk (Thin Provisioned)
* **Networking**: NAT (`VMnet8`)
* **Display**: 3D Acceleration Disabled

---

## Purpose

I built this lab to establish a dedicated, controlled workspace to:

1. Perform manual operating system installations from ISO image files.
2. Configure virtual hardware resources and hypervisor networking.
3. Install and configure guest integration drivers (`open-vm-tools-desktop`).
4. Establish VM snapshot management procedures for baseline restoration.
5. Troubleshoot guest display rendering and input issues.

---

## Repository Structure

```text
cybersecurity-home-lab/
├── README.md
├── LICENSE
├── docs/
│   ├── installation.md
│   ├── vmware-configuration.md
│   └── troubleshooting.md
└── screenshots/
    ├── 01-vmware-library.png
    ├── 02-kali-desktop.png
    ├── 03-system-information.png
    ├── 04-vm-snapshot.png
    ├── 05-terminal-overview.png
    └── 06-network-information.png
```

---

## Documentation Index

* 📘 [**Installation Documentation (`docs/installation.md`)**](docs/installation.md): Details the ISO media selection, VM creation parameters, disk partitioning choices, post-installation package updates, and guest utility setup.
* ⚙️ [**VMware Configuration Documentation (`docs/vmware-configuration.md`)**](docs/vmware-configuration.md): Explains resource allocation choices (RAM, vCPU, storage), NAT network settings, and snapshot restoration procedures.
* 🛠️ [**Troubleshooting Documentation (`docs/troubleshooting.md`)**](docs/troubleshooting.md): Documents the root cause analysis and resolution of an invisible mouse cursor bug encountered during initial setup.

---

## Visual Verification

### Hypervisor Library & Virtual Machine Setup
![VMware Workstation Library](screenshots/01-vmware-library.png)

![VMware Snapshot Manager](screenshots/04-vm-snapshot.png)

### Guest Desktop & Terminal Verification
![Kali Linux Desktop Environment](screenshots/02-kali-desktop.png)

![Terminal Overview](screenshots/05-terminal-overview.png)

---

## Author

**Francisco Elroy Afonso**  
Aspiring Penetration Tester | Practical Ethical Hacker (PEH) | Google Cybersecurity Professional

---

## License

This repository is licensed under the [MIT License](LICENSE).
