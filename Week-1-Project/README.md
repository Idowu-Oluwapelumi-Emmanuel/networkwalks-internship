# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux on Linux Mint.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Install/import Kali Linux as a virtual machine.
- Create a private **NAT Network** for the cybersecurity lab.
- Configure network connectivity for Kali Linux.
- Assign a consistent IP address to the Kali VM.
- Verify network connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture

┌─────────────────────────────────────────────────────────────┐
│ HOST MACHINE │
│ Linux Mint │
│ 8 GB RAM │
│ Intel Core i7 │
└────────────────────────┬────────────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ VIRTUALBOX 7.2 │
│ │
│ ┌───────────────────────────────────────────────────┐ │
│ │ NAT Network │ │
│ │ 10.0.0.0/24 │ │
│ │ │ │
│ │ ┌─────────────────────────────────────┐ │ │
│ │ │ KALI LINUX VM │ │ │
│ │ │ RAM: 2048 MB │ │ │
│ │ │ IP: 10.0.0.2/24 │ │ │
│ │ │ Gateway: 10.0.0.1 │ │ │
│ │ │ DNS: 8.8.8.8 │ │ │
│ │ └─────────────────────────────────────┘ │ │
│ │ │ │
│ │ Future target VMs: 10.0.0.3 – 10.0.0.99 │ │
│ └───────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
text


---

## ⚙️ Lab Configuration

| Component | Configuration |
| :--- | :--- |
| **Host OS** | Linux Mint |
| **Host RAM** | 8 GB |
| **Processor** | Intel Core i7 |
| **Hypervisor** | VirtualBox 7.2 |
| **Security OS** | Kali Linux 2026.2 |
| **Kali RAM** | 2048 MB |
| **Virtual Network** | NAT Network |
| **Network Address** | 10.0.0.0/24 |
| **Kali IP Address** | 10.0.0.2/24 |
| **Default Gateway** | 10.0.0.1 |
| **DNS Server** | 8.8.8.8 |
| **Future VM Range** | 10.0.0.3–10.0.0.99 |

---

## 🪜 Lab Setup Procedure

### Step 1: Install VirtualBox

VirtualBox was installed as the hypervisor on Linux Mint.

```bash
sudo apt update
sudo apt install virtualbox virtualbox-ext-pack -y

Step 2: Create the NAT Network

A dedicated NAT Network was created in VirtualBox.

Configuration:
Setting	Value
Network Name	NatNetwork
IPv4 Prefix	10.0.0.0/24
DHCP	Enabled
IPv6	Disabled

A NAT Network was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity. This will allow future attacker and target VMs to communicate within the lab.
Step 3: Import Kali Linux

The Kali Linux virtual machine was downloaded from the official Kali Linux website and imported into VirtualBox.

Network Configuration:
Setting	Value
Adapter 1	Attached to: NAT Network
Network	NatNetwork
Adapter Type	Intel PRO/1000 MT Desktop

RAM Allocated: 2048 MB

A shared folder was also configured for transferring required files between the host operating system and the Kali VM.
Step 4: Configure the Kali Linux Network

The Kali Linux network configuration was checked and configured with a consistent IPv4 address.

Configuration:
Setting	Value
IP Address	10.0.0.2
Subnet Mask	255.255.255.0
Gateway	10.0.0.1
DNS	8.8.8.8

A consistent IP address makes it easier to document the lab and reference the Kali machine in future exercises.
Step 5: Create a Clean VM Snapshot

After completing the initial configuration, a VirtualBox snapshot was created.

Snapshot Name: Fresh Kali Setup (10.0.0.2)

The snapshot represents the clean baseline of the laboratory. If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.
🔎 Lab Verification
Test Results
Test	Command	Expected Result	Result
Check IP address	ip a	Correct Kali IP displayed	✅ Passed
Test gateway	ping 10.0.0.1	Successful replies	✅ Passed
Test Internet	ping 8.8.8.8	Successful replies	✅ Passed
DNS resolution	nslookup networkwalks.com	Domain resolves	✅ Passed
Verify Nmap	nmap --version	Nmap version displayed	✅ Passed
Example Results
text

IP Address:     10.0.0.2/24
Gateway:        10.0.0.1
DNS:            8.8.8.8
Internet:       ✅ Working

https://screenshots/kali_ip_ping.png

Figure 1: Kali Linux IP address and internet connectivity verification
🐞 Problems Encountered & Solutions

No major issues were encountered during the setup process. The Kali Linux VM was successfully imported, configured, and verified without errors.

Network connectivity was established immediately after configuring the NAT Network, and the IP address 10.0.0.2 was assigned without conflict.
💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice.
1. NAT vs NAT Network

A standard NAT configuration and a NAT Network serve different purposes. A NAT Network allows multiple VMs connected to the same virtual network to communicate with one another while providing network address translation for external connectivity. This makes it useful for building a multi-machine cybersecurity laboratory.
2. Virtual Machine Networking

I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks and how network configuration affects communication between machines.
3. Static IP Configuration

I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux.
4. VM Snapshots

I learned that a clean snapshot should be created before performing risky or experimental activities. This provides a known-good recovery point for future cybersecurity exercises.
5. Documentation

I learned that documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.
🔐 Security & Ethical Use

This laboratory is intended strictly for education purposes only.

⚠️ Do not use the tools or techniques learned in this lab against systems you do not own or have explicit written permission to test. Unauthorized access to computer systems is illegal.
🔗 Tools & Resources
Tool	URL
VirtualBox	https://virtualbox.org/wiki/Downloads
Kali Linux	https://kali.org/get-kali
👤 Author

Idowu Oluwapelumi
Cybersecurity Intern at Networkwalks

LinkedIn: https://www.linkedin.com/in/idowu-oluwapelumi-64b976307/

Documentation completed: September 2026
