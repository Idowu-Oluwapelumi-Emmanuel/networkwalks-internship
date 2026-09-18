
# Week 2 Project — Cybersecurity & Ethical Hacking

## Internship Program

**Networkwalks Cybersecurity Program — Week 2**

---

## Modules Completed

| Module | Tool | OS | Status |
|---|---|---|---|
| PM1 | Footprinting with 6 Kali Tools | Kali Linux | ✅ Completed |
| PM4 | Footprinting with theHarvester | Kali Linux | ✅ Completed |
| PM5 | Network Scanning with Zenmap | Windows | ✅ Completed |

---

## Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Find domain registration details |
| WhatWeb | Fingerprint web technologies |
| Nslookup | Resolve domain names to IP addresses |
| Curl | Read HTTP response headers |
| Wafw00f | Detect Web Application Firewalls |
| DNSRecon | Enumerate DNS records |
| theHarvester | Harvest emails and sub-domains |
| Zenmap | Scan a network for live hosts |

---

# Module 1 — Footprinting with Multiple Kali Tools

**Target:** `networkwalks.com`

### Commands Run

```bash
whois networkwalks.com
whatweb networkwalks.com
nslookup networkwalks.com
curl -I https://networkwalks.com
wafw00f networkwalks.com
dnsrecon -d networkwalks.com
````

### Key Findings

| Tool     | Finding                                      |
| -------- | -------------------------------------------- |
| WHOIS    | Registrar: GoDaddy; Name Servers: HostGator  |
| WhatWeb  | WordPress 7.0.4; WP Download Manager 3.3.58  |
| Nslookup | IP Address: `192.232.216.135`                |
| Curl     | Server: Apache; Endpoint: `/wp-json/`        |
| Wafw00f  | WAF: ModSecurity (SpiderLabs)                |
| DNSRecon | DNS Software: BIND 9.16.23; MX records found |

---

# Module 4 — Footprinting with theHarvester

**Target:** `microsoft.com`

### Commands Run

```bash
theHarvester -d microsoft.com -l 1000 -b baidu
theHarvester -d microsoft.com -l 50 -b all
```

### Key Findings

| Task   | Command    | Emails | Hosts |
| ------ | ---------- | -----: | ----: |
| Task 1 | `-b baidu` |      0 |    17 |
| Task 2 | `-b all`   |      0 |   752 |

### Notable Finding

Hudson Rock reported **602,371 compromised credentials** related to `microsoft.com`, including **16,140 employee accounts** and **581,627 user accounts**.

> **Note:** Breach-intelligence results should be treated as third-party findings and independently verified before being considered confirmed organizational exposure.

### Sample Sub-domains Found — Task 1 (Baidu)

* `2Flearn.microsoft.com`
* `accountguard.microsoft.com`
* `adoption.microsoft.com`
* `appsource.microsoft.com`
* `careers.microsoft.com`
* `cla.opensource.microsoft.com`
* `code.msdn.microsoft.com`
* `developer.microsoft.com`
* `infomails.microsoft.com`
* `jobs.careers.microsoft.com`
* `learn.microsoft.com`
* `news.microsoft.com`
* `prod.support.services.microsoft.com`
* `s.windows.microsoft.com`
* `spf-c.microsoft.com`
* `support.microsoft.com`
* `wcpstatic.microsoft.com`

### Sample Sub-domains Found — Task 2 (All Sources)

* `copilot.microsoft.com`
* `azure.microsoft.com`
* `learn.microsoft.com`
* `support.microsoft.com`
* `developer.microsoft.com`
* `careers.microsoft.com`
* `account.microsoft.com`
* `powerapps.microsoft.com`
* `visualstudio.microsoft.com`
* `minecraft.microsoft.com`
* `webmail.microsoft.com`
* `techcommunity.microsoft.com`

---

# Module 5 — Network Scanning with Zenmap

**Local Subnet:** `10.0.0.0/24`

### Commands Run

```cmd
ipconfig
nmap -sn 10.0.0.0/24
```

### Key Findings

| Item           | Result                                              |
| -------------- | --------------------------------------------------- |
| Subnet Scanned | `10.0.0.0/24`                                       |
| Scan Type      | Ping scan (`nmap -sn`)                              |
| Live Hosts     | 2                                                   |
| Host 1         | `10.0.0.10` — MAC: `08:00:27:69:6D:57`              |
| Host 2         | `10.0.0.3` — VirtualBox NAT router; MAC not visible |
| Scan Duration  | 11.80 seconds                                       |
| IPs Scanned    | 256                                                 |

### Network Topology

* `10.0.0.10` — Windows VM (yellow node)
* `10.0.0.3` — VirtualBox virtual router (green node)
* `localhost` — Scanner machine (black node)

---

# Risk Analysis Summary

|  # | Risk                       | Evidence                               | Impact                                               | Level    |
| -: | -------------------------- | -------------------------------------- | ---------------------------------------------------- | -------- |
|  1 | Web technology exposed     | WordPress 7.0.4 identified             | May expose the system to vulnerabilities if outdated | Medium   |
|  2 | Server IP exposed          | `192.232.216.135` identified           | Reveals the server's network location                | Low      |
|  3 | HTTP information exposed   | `/wp-json/` endpoint identified        | May assist further enumeration                       | Low      |
|  4 | WAF identifiable           | ModSecurity detected                   | Reveals part of the security stack                   | Low      |
|  5 | DNS information exposed    | BIND 9.16.23 and MX records identified | Provides information about the infrastructure        | Medium   |
|  6 | 752 sub-domains discovered | theHarvester output                    | Increases the externally visible attack surface      | High     |
|  7 | 602K credentials reported  | Hudson Rock finding                    | Potential credential-stuffing risk                   | Critical |
|  8 | Live hosts detected        | 2 live hosts identified                | Unknown devices may require investigation            | Medium   |

**Risk Level Key:** Critical > High > Medium > Low

---

# Recommendations

1. Keep CMS platforms and plugins updated.
2. Review publicly exposed technology information.
3. Review HTTP response headers for unnecessary information disclosure.
4. Regularly audit DNS records.
5. Monitor publicly indexed information about organizational domains.
6. Secure, monitor, or remove forgotten and unused sub-domains.
7. Monitor breach-intelligence sources for potentially exposed credentials.
8. Perform regular internal network discovery within authorized environments.
9. Investigate unknown or unauthorized devices on the network.
10. Perform reconnaissance activities only with proper authorization and within the defined scope.

---

# What I Learned

* **Footprinting is an important early phase of security assessment** because it involves gathering information about a target before conducting further testing.
* **Different tools reveal different types of information.** WHOIS, WhatWeb, Nslookup, Wafw00f, and DNSRecon each provide different perspectives on a target's infrastructure.
* **theHarvester can help map an organization's external footprint** by identifying publicly available sub-domains, hosts, and other information.
* **Breach intelligence can reveal potential security risks**, although third-party findings should be independently verified.
* **Network scanning can reveal live hosts** on an authorized network, even when using a basic ping scan.
* **Documentation matters.** Clear screenshots, commands, findings, and organized results make a technical report easier to understand and evaluate.
* **Authorization is essential.** Security testing must be conducted only on systems and networks where permission has been obtained and within the agreed scope.

---

# Liability Disclaimer

I performed these activities only on systems and devices where I had secured written permission, or on devices and systems that I own or control for authorized laboratory purposes. All materials are intended for educational and research purposes only.

Unauthorized access to computer systems or networks may be illegal and can result in criminal or civil consequences. Security tools should therefore be used responsibly and only within an authorized scope.

Hacking and security testing are appropriate when:

* You are testing a device or network that you own or are authorized to test.
* You have obtained written and documented permission from the owner.
* You are working as a security professional under a signed agreement with a clearly defined scope.

Activities outside these conditions may constitute unauthorized access and should not be performed.

---

# Author

**Idowu Oluwapelumi**

**Week 2 Project — Networkwalks Cybersecurity Program**

```
```
