# Week 2 Project — Cybersecurity & Ethical Hacking

## Internship Program
Networkwalks Cybersecurity Program — Week 2

---

## Modules Completed

| Module | Tool | OS | Status |
|---|---|---|---|
| PM1 | Footprinting with 6 Kali Tools | Kali Linux | ✅ |
| PM4 | Footprinting with theHarvester | Kali Linux | ✅ |
| PM5 | Network Scanning with Zenmap | Windows | ✅ |

---

## Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Find domain registration details |
| WhatWeb | Fingerprint web technologies |
| Nslookup | Resolve domain to IP address |
| Curl | Read HTTP response headers |
| Wafw00f | Detect Web Application Firewall |
| DNSRecon | Enumerate all DNS records |
| theHarvester | Harvest emails & sub-domains |
| Zenmap | Scan network for live hosts |

---

## Module 1 — Footprinting with Multiple Kali Tools

**Target:** `networkwalks.com`

### Commands Run

```bash
whois networkwalks.com
whatweb networkwalks.com
nslookup networkwalks.com
curl -I https://networkwalks.com
wafw00f networkwalks.com
dnsrecon -d networkwalks.com

### Key Findings

| Tool | Finding |
|---|---|
| WHOIS | Registrar: GoDaddy, Name Servers: HostGator |
| WhatWeb | WordPress 7.0.4, WP Download Manager 3.3.58 |
| Nslookup | IP Address: 192.232.216.135 |
| Curl | Server: Apache, Endpoint: /wp-json/ |
| Wafw00f | WAF: ModSecurity (SpiderLabs) |
| DNSRecon | DNS Software: BIND 9.16.23, MX records found |

---

## Module 4 — Footprinting with theHarvester

**Target:** `microsoft.com`

### Commands Run

```bash
theHarvester -d microsoft.com -l 1000 -b baidu
theHarvester -d microsoft.com -l 50 -b all

#### Key Findings

| Task | Command | Emails | Hosts |
|---|---|---|---|
| Task 1 | `-b baidu` | 0 | 17 |
| Task 2 | `-b all` | 0 | 752 |

**Notable Finding:** Hudson Rock reported **602,371 compromised credentials** related to microsoft.com, including **16,140 employee accounts** and **581,627 user accounts**.

#### Sample Sub-domains Found (Task 1 — Baidu)

- 2Flearn.microsoft.com
- accountguard.microsoft.com
- adoption.microsoft.com
- appsource.microsoft.com
- careers.microsoft.com
- cla.opensource.microsoft.com
- code.msdn.microsoft.com
- developer.microsoft.com
- infomails.microsoft.com
- jobs.careers.microsoft.com
- learn.microsoft.com
- news.microsoft.com
- prod.support.services.microsoft.com
- s.windows.microsoft.com
- spf-c.microsoft.com
- support.microsoft.com
- wcpstatic.microsoft.com

#### Sample Sub-domains Found (Task 2 — All Sources)

- copilot.microsoft.com
- azure.microsoft.com
- learn.microsoft.com
- support.microsoft.com
- developer.microsoft.com
- careers.microsoft.com
- account.microsoft.com
- powerapps.microsoft.com
- visualstudio.microsoft.com
- minecraft.microsoft.com
- webmail.microsoft.com
- techcommunity.microsoft.com

---

## Module 5 — Network Scanning with Zenmap

**Local Subnet:** `10.0.0.0/24`

### Commands Run

```cmd
ipconfig
nmap -sn 10.0.0.0/24

#### Key Findings

| Item | Result |
|---|---|
| Subnet Scanned | 10.0.0.0/24 |
| Scan Type | Ping scan (`nmap -sn`) |
| Live Hosts | 2 |
| Host 1 | 10.0.0.10 (MAC: 08:00:27:69:6D:57) |
| Host 2 | 10.0.0.3 (VirtualBox NAT router, MAC not visible) |
| Scan Duration | 11.80 seconds |
| IPs Scanned | 256 |

#### Network Topology

- `10.0.0.10` — Windows VM (yellow node)
- `10.0.0.3` — VirtualBox virtual router (green node)
- `localhost` — Scanner machine (black node)
---

## Risk Analysis Summary

| # | Risk | Evidence | Impact | Level |
|---|---|---|---|---|
| 1 | Web technology exposed | WordPress 7.0.4 found | May have known exploits | Medium |
| 2 | Server IP exposed | 192.232.216.135 | Network location revealed | Low |
| 3 | HTTP info exposed | /wp-json/ endpoint | Aids enumeration | Low |
| 4 | WAF identifiable | ModSecurity detected | Reveals security stack | Low |
| 5 | DNS info exposed | BIND 9.16.23, MX records | Infrastructure profile | Medium |
| 6 | 752 sub-domains found | theHarvester output | Huge attack surface | High |
| 7 | 602K credentials leaked | Hudson Rock finding | Credential stuffing risk | Critical |
| 8 | Live hosts visible | 2 hosts on network | Unauthorized devices possible | Medium |

**Risk Level Key:** Critical > High > Medium > Low

---

## Recommendations

1. Keep CMS platforms and plugins updated
2. Review publicly exposed technology information
3. Review HTTP headers for unnecessary info leakage
4. Regularly audit DNS records
5. Monitor what Google indexes about your domain
6. Secure or remove forgotten sub-domains
7. Monitor breach intelligence for leaked credentials
8. Perform regular internal network discovery
9. Investigate unknown devices on the network
10. Only perform reconnaissance with proper authorization
---

## What I Learned

- **Footprinting is the first phase of any attack** — gathering public information before touching the target
- **Different tools reveal different information** — WHOIS, WhatWeb, DNSRecon each expose unique data
- **theHarvester can map an entire organization's external footprint** — 752 sub-domains from one command
- **Breach intelligence is powerful** — 602K leaked credentials is a massive security finding
- **Network scanning reveals live hosts** — even a simple Ping scan maps the network
- **Documentation matters** — clear screenshots and organized files make the report professional
- **Authorization is essential** — all activities must be within legal scope

---

## Liability Disclaimer
I have performed these activities only on systems and devices where I had secured written permission, or the devices/systems that I own myself. All materials are for educational and research purposes only.

Do not use anything from here to break the law. Unauthorized access is a crime in most countries, even when nothing is damaged. Misuse can lead to criminal charges, heavy fines, loss of your job, and a permanent record.

Hacking is only legal when:
- You test a device or network that you own or your lab environment
- You have written and documented permission from the owner
- You are working as a security professional under a signed agreement with an agreed scope

Everything outside these cases is illegal.

---

## Author

**Idowu Oluwapelumi**
Week 2 Project — Networkwalks Cybersecurity Program

---
