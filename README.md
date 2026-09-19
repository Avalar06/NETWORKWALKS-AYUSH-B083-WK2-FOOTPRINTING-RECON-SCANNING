# NETWORKWALKS WEEK 02 — Footprinting, Reconnaissance & Network Scanning

![Cybersecurity](https://img.shields.io/badge/Focus-Cybersecurity-blue)
![Week](https://img.shields.io/badge/NetworkWalks-Week%2002-informational)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux%20%7C%20Windows-lightgrey)

## Overview

This repository contains my **NetworkWalks Cybersecurity & Ethical Hacking Internship — Week 02** practical work on:

> **Footprinting, Reconnaissance & Network Scanning**

The practical work covers passive/light reconnaissance of the authorized target **networkwalks.com** and controlled host discovery on my own local network **192.168.0.0/24**.

The work is organized into five project modules:

1. **PM1 — Footprinting with Multiple Kali Linux Tools**
2. **PM2 — Footprinting with Google Hacking Database (GHDB)**
3. **PM3 — Footprinting with Maltego**
4. **PM4 — Footprinting with theHarvester**
5. **PM5 — Network Scanning with Zenmap**

This README documents the actual commands, observations, limitations, and evidence associated with the practical work. It does not use sample-report results as substitutes for my own execution results.

---

## Scope and Authorization

The practical work was performed within the supplied authorization scope.

### Authorized targets

- **External target:** `networkwalks.com`
- **Local network:** `192.168.0.0/24`

### Activities permitted within scope

- Passive/light-footprinting and reconnaissance
- DNS and domain information gathering
- Public search-engine based OSINT
- Controlled local host discovery

### Activities not performed / outside scope

- Exploitation
- Unauthorized access
- Privilege escalation
- Denial-of-service activity
- Brute-force or password attacks
- Social engineering
- Unauthorized data access, modification, or deletion
- Testing systems outside the authorized scope

> **Note:** The authorization letter is intentionally not included as a public repository artifact unless explicitly required.

---

## Project Structure

The intended repository structure is:

```text
NETWORKWALKS-AYUSH-B083-WK2-FOOTPRINTING-RECON-SCANNING/
├── README.md
├── CHANGELOG.md
├── REPORT/
│   └── NetworkWalks-Week2-Final-Report.pdf
├── PM1-Footprinting/
├── PM2-GHDB/
├── PM3-Maltego/
├── PM4-theHarvester/
└── PM5-Zenmap/
```

> The folder/file tree above describes the planned organization. Individual filenames should only be treated as present after they have been uploaded and verified.

---

# PM1 — Footprinting with Multiple Kali Linux Tools

## 1. WHOIS

### Command

```bash
whois networkwalks.com
```

### Saved output

```text
~/NetworkWalks-Week2/PM1/Task1-WHOIS/pm1_task1_whois.txt
```

### Observed name servers

- `ns6135.hostgator.com`
- `ns6136.hostgator.com`

The output size was previously recorded as approximately **6750 bytes**.

---

## 2. WhatWeb

### Command

```bash
whatweb networkwalks.com
```

### Result

No usable technology-fingerprinting output was obtained.

### Troubleshooting performed

```bash
whatweb --version
timeout 30 whatweb networkwalks.com
timeout 15 whatweb --debug https://example.com
whatweb --help
```

### Environment details

- WhatWeb: **0.6.4**
- Ruby: **3.3.8**
- RubyGems: **3.6.7**
- Executable: `/usr/bin/whatweb`

### Saved troubleshooting record

```text
~/NetworkWalks-Week2/PM1/Task2-WhatWeb/whatweb_troubleshooting.txt
```

### Important limitation

No unsupported WordPress, plugin, or version claims are made from the WhatWeb execution because the captured output did not provide usable technology fingerprinting evidence.

---

## 3. Nslookup

### Command

```bash
nslookup networkwalks.com
```

### Observed address

```text
192.232.216.135
```

### Working directory

```text
~/NetworkWalks-Week2/PM1/Task3-Nslookup
```

> The exact saved output filename was not verified in the continuity record, so it is deliberately not guessed here.

---

## 4. Curl

### Command

```bash
curl -I https://networkwalks.com | tee ~/NetworkWalks-Week2/PM1/Task4-Curl/pm1_task4_curl.txt
```

### Observed response information

```text
HTTP/2 200
server: Apache
x-nginx-cache: WordPress
content-type: text/html; charset=UTF-8
```

A WordPress REST API reference was also observed during the inspection.

---

## 5. Wafw00f

### Command

```bash
wafw00f https://networkwalks.com
```

### Observed result

The site was identified as being behind:

```text
ModSecurity (SpiderLabs) WAF
```

The tool reported **2 requests**.

### Saved output

```text
~/NetworkWalks-Week2/PM1/Task5-Wafw00f/pm1_task5_wafw00f.txt
```

---

## 6. DNSRecon

### Command

```bash
dnsrecon -d networkwalks.com 2>&1 | tee ~/NetworkWalks-Week2/PM1/Task6-DNSRecon/pm1_task6_dnsrecon.txt
```

### Observed DNS information

| Record | Observation |
|---|---|
| SOA | `ns6135.hostgator.com / 50.87.144.87` |
| NS | `ns6135.hostgator.com / 50.87.144.87` |
| NS | `ns6136.hostgator.com / 192.232.216.131` |
| MX | `mail.networkwalks.com / 192.232.216.135` |
| A | `networkwalks.com / 192.232.216.135` |
| TXT | SPF-related information and Google site-verification TXT |
| SRV | `_autodiscover._tcp.networkwalks.com -> cpanelemaildiscovery.cpanel.net` |

**8 DNS records were reported.**

Additional output included:

- No answer for the DNSSEC query
- Recursion enabled

---

# PM2 — Footprinting with Google Hacking Database (GHDB)

## 1. Security Camera Search

### Search term

```text
cam
```

### Visible result count

```text
1–15 of 100 entries
```

### Documented GHDB examples

1. `intitle:"Webcam" inurl:WebCam.htm`
2. `intitle:"Toshiba Network Camera"`
3. `intitle:"Index of /cam/"`
4. `intitle:"Index of /webcam/"`
5. `intitle:"Device(IP CAMERA)" "language" -com|net`
6. `intitle:"NoVus IP camera" -com`
7. `intitle:"Network Camera" inurl:main.cgi`
8. `inurl:webcam site:skylinewebcams.com inurl:roma`
9. `intitle:"Login" intext:"cam"`
10. `intext:"Real-time IP Camera Monitoring System" intext:"ActiveX Mode (For IE Browser)"`
11. `intitle:"Login" intext:"camera"`
12. `intitle:"webcamXP" inurl:8080`
13. `intitle:"Index of" "DCIM/camera"`
14. `intitle:ip camera login page`
15. `intitle:"Microseven M70CAM IP Camera"`

### Saved result

```text
~/NetworkWalks-Week2/PM2/Task1-Security-Cameras/pm2_task1_ghdb_camera_results.txt
```

### Scope note

No live third-party camera endpoints were accessed.

---

## 2. Mathematics / PDF Search

The following GHDB-oriented searches were documented:

```text
intitle:index.of "parent directory" mathematics pdf
intitle:index.of "parent directory" "mathematics" filetype:pdf
intitle:index.of "mathematics" "pdf" "parent directory"
intitle:index.of "mathematics" "books" pdf
intitle:index.of "math" "books" pdf
intitle:index.of "mathematics" "textbook" pdf
intitle:index.of "mathematics" "lecture notes" pdf
site:edu "index of" mathematics pdf
```

### 10 documented listings

1. `http://erwhon.superkuh.com/library/Math/`
2. `https://www.sajairamcollege.ac.in/pdf/MATHEMATICS/`
3. `https://theswissbay.ch/pdf/Books/Mathematics/`
4. `https://isidore.co/misc/Physics%20papers%20and%20books/Mathematics/`
5. `https://discrete.openmathbooks.org/pdfs/`
6. `https://zaco.au/lib/math/hs/`
7. `https://www.math.mcgill.ca/barr/`
8. `http://www.unm.edu/~megrad/Math/`
9. `https://www.math.dartmouth.edu/~carlp/PDF/`
10. `https://docs.bartonccc.edu/syllabus/Master/MATH/`

### Saved result

```text
~/NetworkWalks-Week2/PM2/Task2-Math-PDF/pm2_task2_math_pdf_results.txt
```

No copyrighted PDF collection was downloaded.

---

# PM3 — Footprinting with Maltego

## Environment

- Maltego Graph: **4.13.0**
- Bundled JRE
- Windows

Utilities data source was configured. No local transform server was used.

---

## Entity configuration

The correct Maltego entity was:

```text
Domain
networkwalks.com
```

An initially incorrect entity type was corrected during troubleshooting.

---

## Available transforms

Relevant Utilities transforms included:

- To Email address [From whois info]
- To Email Addresses [PGP]
- To Email Addresses [Search Engine]
- To Emails @domain [Search Engine]

---

## Google Custom Search configuration

The initial Search Engine transform failed because Google API configuration had not yet been completed.

Configuration subsequently used:

- Search Engine name: **NetworkWalks OSINT**
- Target: `networkwalks.com`
- Search Engine ID: `263f22ab80a7344f3`
- Google Cloud project: **AYUSHDUTTA832**
- Custom Search API: enabled
- Maltego Transform Manager: configured with Search Engine ID and API key
- Extract URLs: unchecked

> **Security:** The Google API key is private. It must not be placed in this README, GitHub, screenshots, reports, or other public artifacts.

---

## Successful Search Engine Transform

Observed:

- **31 credits consumed**
- **169 credits remaining**
- **2 entities returned**
- Approximately **16.065 seconds**

Returned email entities:

- `sales@networkwalks.com`
- `info@networkwalks.com`

---

## WHOIS Transform

Returned:

```text
abuse@godaddy.com
```

This was interpreted as the registrar/WHOIS abuse contact and **not** as a confirmed NetworkWalks organizational mailbox.

---

## PGP Transform

No additional entities were returned.

Observed credit usage:

```text
11 credits
```

---

## Maltego evidence

Graph file:

```text
NetworkWalks-Week2\PM3-Maltego\PM3-NetworkWalks-Maltego-Email-Footprinting.mtgx
```

Evidence filenames recorded in the continuity log:

```text
PM3-Maltego-01-Installation.png
PM3-Maltego-02-Domain-Entity.png
PM3-Maltego-03-Email-Transforms.png
PM3-Maltego-04-WHOIS-Email.png
PM3-Maltego-05-Google-Email-Results.png
PM3-Maltego-Results.txt
```

These filenames should only be treated as uploaded repository files after verification.

---

# PM4 — Footprinting with theHarvester

## Version

```text
theHarvester 4.10.1
```

The instructional PDF referenced Microsoft, but Microsoft was outside the authorized scope. The same methodology was therefore applied to the authorized target **networkwalks.com**.

---

## 1. Baidu

### Command

```bash
theHarvester -d networkwalks.com -l 1000 -b baidu 2>&1 | tee ~/NetworkWalks-Week2/PM4-theHarvester/Task1-Baidu/pm4_task1_baidu.txt
```

### Observed result

- No IPs found
- No emails found
- No people found
- No hosts found

---

## 2. All Sources

### Command

```bash
theHarvester -d networkwalks.com -l 50 -b all 2>&1 | tee ~/NetworkWalks-Week2/PM4-theHarvester/Task2-All-Sources/pm4_task2_all_sources.txt
```

### Email observed

```text
info@networkwalks.com
```

### Visible IPs

```text
172.67.198.228
192.232.216.135
```

### Hosts

```text
32
```

### Representative hosts

- `*.networkwalks.com`
- `autodiscover.networkwalks.com`
- `autodiscoverp.networkwalks.com`
- `autodiscovers.networkwalks.com`
- `cpanel.networkwalks.com`
- `cpanelr.networkwalks.com`
- `cpcalendars.networkwalks.com`
- `cpcalendarsm.networkwalks.com`
- `cpcalendarsr.networkwalks.com`
- `cpcontacts.networkwalks.com`
- `cpcontactsk.networkwalks.com`
- `cpcontactss3.networkwalks.com`
- `ftp.networkwalks.com`
- `mail.networkwalks.com`
- `webdisk.networkwalks.com`
- `webmail.networkwalks.com`

### ASNs observed

- **AS13335**
- **AS31898**
- **AS46606**

### URLs observed

- `http://networkwalks.com/`
- `https://networkwalks.com/`

### Example integrated sources

The output included information from sources such as:

Chaos, Certspotter, CRTsh, DuckDuckGo, Baidu, GitLab, Commoncrawl, Hackertarget, Leaklookup, Hudsonrock, OTX, RapidDNS, LeakIX, Subdomaincenter, THC, Threatcrowd, Robtex, URLScan, Yahoo, Waybackarchive, and others.

Several integrated sources reported missing API-key configuration, including examples such as Censys, GitHub, Hunter, Shodan, VirusTotal, SecurityScorecard, and BuiltWith.

> Missing API-key warnings are configuration messages and should **not** be interpreted as vulnerabilities.

---

## Hudson Rock output

The source reported:

- **101 total compromised**
- **0 employees**
- **101 users**
- **0 hosts/IPs/emails in that source output**

### Interpretation

This is **third-party/source-reported OSINT information only**.

It is **not** treated as confirmation that NetworkWalks itself was compromised.

---

# PM5 — Network Scanning with Zenmap

## Objective

The Zenmap module covered:

1. Installing Zenmap
2. Identifying the local IP/subnet
3. Performing a Ping Scan of the local subnet
4. Counting live hosts
5. Recording observed IP addresses
6. Recording observed MAC addresses
7. Generating a topology PDF

---

## Local Network Identification

### Command

```text
ipconfig
```

### Actual practical network

| Parameter | Value |
|---|---|
| IPv4 | `192.168.0.184` |
| Subnet mask | `255.255.255.0` |
| Network | `192.168.0.0/24` |
| Gateway | `192.168.0.1` |

An additional interface, **Ethernet 2**, had:

```text
192.168.56.1
```

This interface was **not used for the practical scan**.

---

## Ping Scan

### Zenmap target

```text
192.168.0.0/24
```

### Profile

```text
Ping Scan
```

Equivalent Nmap command:

```bash
nmap -sn 192.168.0.0/24
```

Approximate duration:

```text
3.34 seconds
```

### Live hosts discovered

**3 live hosts** were observed.

| IP address | MAC address | Vendor / description |
|---|---|---|
| `192.168.0.1` | `98:03:8E:0E:59:79` | TP-Link Systems |
| `192.168.0.193` | `2E:22:C5:96:6D:65` | Unknown |
| `192.168.0.184` | Not displayed in Ping Scan output | Local Windows host |

> The MAC address for **192.168.0.184** is intentionally not supplied because it was not displayed in the recorded Ping Scan output.

---

## Zenmap topology

Topology screenshot:

```text
PM5-03-topology.png
```

The topology PDF was generated as part of the practical.

Previously available local evidence paths:

```text
/mnt/data/PM5-01-ipconfig.png
/mnt/data/PM5-02-ping-scan.png
/mnt/data/PM5-03-topology.png
```

---

# Risk Analysis

The following are **observations from reconnaissance and scanning**, not confirmed vulnerabilities.

| Observation | Relative assessment |
|---|---|
| Public DNS infrastructure discoverable | Medium |
| Public server IP identifiable | Low |
| HTTP technical information exposed | Low |
| WAF technology identifiable | Low |
| Multiple public hosts/subdomains | Medium |
| Public organizational email addresses | Low |
| Multiple live LAN hosts | Medium |
| Public DNS service records | Medium |

These observations describe information exposure and attack-surface visibility identified during the authorized practical exercise. They do not by themselves establish exploitable vulnerabilities.

---

# Recommendations

Based on the observations documented during the practical:

- Review public DNS records regularly.
- Remove obsolete DNS records and unnecessary public subdomains.
- Maintain an external attack-surface inventory.
- Review HTTP response headers and minimize unnecessary information disclosure.
- Maintain and tune WAF controls.
- Monitor public infrastructure exposure.
- Review publicly exposed organizational email addresses.
- Maintain SPF, DKIM, and DMARC where applicable.
- Perform periodic authorized external reconnaissance.
- Perform periodic authorized internal host discovery.
- Investigate unexpected devices on the local network.
- Maintain network diagrams and asset inventories.
- Protect credentials and API keys.
- Keep authorization letters and other private evidence files outside public repositories where appropriate.

---

# Evidence and Outputs

The practical work generated tool outputs, screenshots, and supporting evidence for the five modules.

### Recorded evidence locations

```text
~/NetworkWalks-Week2/PM1/
~/NetworkWalks-Week2/PM2/
~/NetworkWalks-Week2/PM4-theHarvester/

NetworkWalks-Week2\PM3-Maltego\PM3-NetworkWalks-Maltego-Email-Footprinting.mtgx

/mnt/data/PM5-01-ipconfig.png
/mnt/data/PM5-02-ping-scan.png
/mnt/data/PM5-03-topology.png
```

The exact repository file list should always be verified before treating these paths as public GitHub paths.

---

# Final Report

The practical report was prepared manually.

Documented report structure:

1. Cover Page
2. Introduction
3. Objectives
4. Tools Used
5. Activities Performed
   - 5.1 PM1
   - 5.2 PM2
   - 5.3 PM3
   - 5.4 PM4
   - 5.5 PM5
6. Risk Analysis / Impact
7. Recommendations
8. Conclusion
9. Evidence / Screenshot Index

### Formatting used for the report

- **Font:** Times New Roman
- **Major/chapter headings:** 14 pt, bold
- **Body text:** 12 pt
- Centered screenshots
- Captions below screenshots
- Page numbers in footer

The final report PDF should be added under:

```text
REPORT/NetworkWalks-Week2-Final-Report.pdf
```

only after the actual submission file has been verified.

---

# Tools Used

| Module | Tools |
|---|---|
| PM1 | WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, DNSRecon |
| PM2 | Google Hacking Database |
| PM3 | Maltego Graph 4.13.0, Google Custom Search |
| PM4 | theHarvester 4.10.1 |
| PM5 | Zenmap / Nmap |

---

# Practical Status

| Area | Status |
|---|---|
| Week 02 practical execution | Complete |
| Manual report | Complete |
| GitHub repository setup | In progress |
| Master README | In progress / to be committed |
| Evidence upload | Pending verification |
| Final report upload | Pending verification |
| Repository quality check | Pending |
| Final submission/share link | Pending |

---

# Security and Responsible Use

This repository is intended for educational and authorized cybersecurity work.

Do not use the commands, techniques, or findings documented here against systems without explicit authorization.

The following information must remain private and must never be committed to the repository:

- API keys
- Passwords
- Access tokens
- Private credentials
- Authorization letters, unless explicitly required
- Other confidential documents or secrets

---

# Important Interpretation Rules

To keep the project technically accurate:

- The sample report must not be treated as a source of my actual results.
- WhatWeb is not credited with WordPress or plugin versions that were not observed.
- No live third-party camera endpoint was accessed.
- Hudson Rock output is not treated as proof of a NetworkWalks compromise.
- `abuse@godaddy.com` is not treated as a confirmed NetworkWalks organizational mailbox.
- No MAC address is invented for `192.168.0.184`.
- The practical scan network remains exactly `192.168.0.0/24`.
- Missing API-key warnings from theHarvester sources are not treated as vulnerabilities.
- Evidence filenames are not assumed to exist until verified.

---

# Author

**Ayush Dutta**

NetworkWalks Cybersecurity & Ethical Hacking Internship  
**Week 02 — Footprinting, Reconnaissance & Network Scanning**

---

## Disclaimer

All activities documented in this repository were performed within the authorized scope of the internship practical. The findings represent reconnaissance and controlled network-discovery observations collected during the exercise and should not be interpreted as proof of exploitable vulnerabilities unless separately validated through authorized security testing.
