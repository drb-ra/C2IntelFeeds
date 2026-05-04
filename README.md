# C2IntelFeeds
Automatically created C2 Feeds | Also posted via [@drb_ra](https://twitter.com/drb_ra) 

**From May 1st, 2026 raw data is provided courtesy of Modat** -- https://modat.io/ 

*A special thank you not to Censys for the support provided over the last few years.*

---

**C2IntelFeeds** is a collection of automatically generated **Command-and-Control (C2) threat intelligence feeds** derived from large-scale internet scanning data (primarily Censys).

These feeds are intended for **defenders** and are suitable for:
- Threat hunting
- Network monitoring
- Detection engineering
- IOC enrichment
- Defensive blocking or alerting

The project focuses on identifying **real C2 infrastructure**, not malware samples.

---

## 🔍 What This Repository Provides

This repository contains multiple **plain-text, CSV, and JSON feeds** listing suspected or confirmed C2 infrastructure, including:

- C2 IP addresses
- C2 domains and hostnames
- Domains with C2 URL paths
- IP + port combinations
- C2 configuration metadata (when available)

Feeds are updated automatically and primarily reflect **recent activity**.

---

## ⏱️ Time Windows

Most feeds are available in two time ranges:

- **7-day feeds** (default)
- **30-day feeds** (historical context)
- **90-day feeds** (long-term context)

The time window refers to **last observed activity**, not creation date.

---

## 📁 Feed Types

### ✅ Verified Feeds (Preferred)

These feeds have undergone additional validation and **exclude known benign infrastructure**.

| Feed | Description |
|----|----|
| **C2 IPs** | Validated C2 server IP addresses |
| **C2 Domains** | Domains extracted from known C2 implants |
| **C2 Domains (Filtered)** | Same as above, with high-false-positive domains removed |
| **C2 Domains + URL** | Domains with specific C2 URI paths |
| **C2 Domains + URL + IP** | Domains, paths, and resolved IPs |

---

### ⚠️ Unverified Feeds

These feeds are generated from fingerprint matches but **may contain false positives**.

| Feed | Description |
|----|----|
| **Unverified C2 IPs** | Potential C2 IPs based on scan artifacts |
| **Unverified C2 Domains** | Potential C2 domains |
| **IP + Port Pairs** | Destination IP and port combinations |

> ⚠️ **Use unverified feeds cautiously**. Local validation is strongly recommended.

---

## 🧬 C2 Configuration Data

Where possible, extracted **C2 configuration metadata** is included in CSV and JSON formats.

Typical fields may include:

- First seen timestamp
- True C2 IP (actual listener)
- Port, jitter, sleep time
- ASN and network information
- HTTP host headers
- TLS certificate data
- User-agent strings
- Optional public keys (JSON)

Both **standard** and **30-day** variants may be available.

---

## 🛰️ How the Data Is Generated

Feeds are built using **Censys search queries** designed to detect known C2 frameworks by fingerprinting:

- TLS certificate fields
- JARM fingerprints
- HTTP response headers and titles
- Body hashes
- Service banners
- Known implant artifacts

# Censys Searches

| Tool | ```Modat Search```|
|------|:------------|
|[Sliver](https://github.com/BishopFox/sliver) |`same_service(service="unknown" transport="tcp" cert.issuer.cn="operators" cert.subject.cn="multiplayer")  and same_service(service="http" transport="tcp" banner.sha256=dba60000613d7556f0b129e77a9451d6ce5a83f52f2d4830314d2e6b52b7928c)`|
|[Covenant](https://github.com/cobbr/Covenant) |`same_service(cert.issuer.cn="Covenant" cert.subject.cn="Covenant" ) OR same_service(web.title="Covenant"service="http" transport="tcp" technology="Kestrel" technology="Bootstrap")`|
|[Brute Ratel C4](https://bruteratel.com) |`product="Brute Ratel C4"`|
|[Mythic](https://github.com/its-a-feature/Mythic) |`same_service(service="http" transport="tcp" cert.issuer.org="Mythic" cert.subject.org="Mythic" ) or same_service(banner.sha256=bc7e468313dcdc814784a20b5676188d19c033a1a3d9e3ebe1ff92f006522216 product!="Mythic C2") or product="Mythic C2"`|
|[Deimos](https://github.com/DeimosC2/DeimosC2)|`same_service(service="http" transport="tcp" banner.sha256=613dfd23a3e10f890cee3032b088cfcdf1bf53ed0851e192c0c93ee59b53821a web.html.sha256=99eb12f2ab3c4866a353e098ffa3cb7a967e617c49b98480394ec5d8ea92b094 cert.issuer.org="Acme Co" cert.subject.org="Acme Co" )`|
|[Nighthawk C2](https://www.mdsec.co.uk/nighthawk/) |`[TBC]`|
|Bianlian Go Trojan |`[TBC]`|     
|[Havoc](https://github.com/HavocFramework/Havoc) |`[TBC]`|
|[Responder](https://github.com/lgandx/Responder) |`same_service(service="smb" transport="tcp" banner~"server_guid: AAAAAAAAAAAAAAAAAAAAAO6Fq/fq9gxPkoGSR23rdqk")`|
|[Pupy RAT](https://github.com/n1nj4sec/pupy)|`same_service(service="unknown" transport="tcp" cert.subject.ou="CONTROL" cert.subject.org=~"^[a-zA-Z]{10}$" cert.issuer.org=~"^[a-zA-Z]{10}$") OR product="Pupy RAT"`|
|Qakbot|`[TBC]`|
|[DcRat](https://github.com/qwqdanchun/DcRat)|`[TBC]`|
|Viper|`same_service(web.title="VIPER" web.html.sha256=771fb5f8203ca3b8c3a184ebc4347d5308fd75ad57895bc8fddebe7f355ef20a) OR same_service(service="http" transport="tcp" product="Viper RAT")`|
|[Supershell](https://github.com/tdragon6/Supershell/)|`same_service(service="http" transport="tcp" product="Supershell C2")`|
|Pikabot|`[TBC]`|
|Meduza Stealer|`[TBC]`|
|[Evilginx/EvilGoPhish](https://help.evilginx.com)|`same_service(service="http" transport="tcp" (web.title~"Evilginx" or web.title~"evilgophish" or cert.issuer.org="Evilginx API")) or product="Evilginx"`|
|Hookbot/Pegasus|`[TBC]`|
|[AsyncRAT](https://github.com/NYAN-x-CAT/AsyncRAT-C-Sharp)|`[TBC]`|
|[Remcos](https://breakingsecurity.net/remcos/)|`[TBC]`|
|DanaBot|`[REDACTED]`|
|Rhysida Trojan|`[REDACTED]`|
|[Oyster Backdoor](https://www.rapid7.com/blog/post/2024/06/17/malvertising-campaign-leads-to-execution-of-oyster-backdoor/)|`[REDACTED]`|
|SocGholish|`[REDACTED]`|
|[NetSupport Manager RAT](https://www.netsupportmanager.com)|`[TBC]`|
|[Geacon_Pro](https://github.com/testxxxzzz/geacon_pro)|`[TBC]`|
|[Hak5 Cloud C2](https://shop.hak5.org/products/c2)|`[TBC]`|
|[CHAOS](https://github.com/tiagorlampert/CHAOS)|`[TBC]`|
|[Interactsh](https://github.com/projectdiscovery/interactsh)|`[TBC]`|
|[Reverse SSH](https://github.com/NHAS/reverse_ssh)|`[REDACTED]`|
|[wstunnel](https://github.com/erebe/wstunnel)|`[REDACTED]`|
|[Ligolo-ng](https://github.com/nicocha30/ligolo-ng)|`[REDACTED]`|
|Ransomhub Python C2|`[REDACTED]`|
|[Pyramid](https://github.com/naksyn/Pyramid)|`[REDACTED]`|
|VPN Themed Phishing|`[REDACTED]`|
|StealC v2|`[TBC]`|
|[AdaptixC2](https://github.com/Adaptix-Framework/AdaptixC2)|`[REDACTED]`|
|Matanbuchus|`[REDACTED]`|
|[Pywssocks](https://pypi.org/project/pywssocks/)|`[REDACTED]`|

## 🧹 False-Positive Reduction

The repository includes an exclusion file: **exclusions.rex**

This file removes:
- Known CDN/domain-fronting services
- Common shared hosting providers
- Frequently benign infrastructure

Filtered feeds apply these exclusions automatically.

---

## 🧠 How to Use These Feeds

These feeds are suitable for:

- **SIEM ingestion** (Splunk, Sentinel, Elastic, etc.)
- **EDR enrichment**
- **Threat hunting queries**
- **Network detections**
- **Firewall / proxy monitoring**
- **IOC correlation pipelines**

They are intentionally provided in **simple formats** to ease automation.

The easiest files for most of you to use should be [C2 IPs](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/IPC2s.csv), [C2 Domains Filtered](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/domainC2s-filter-abused.csv) and [Unverified C2 IPs](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/unverified/IPC2s.csv) or their 30 day counterparts.  


---

## 🌐 VPN & Proxy Lists

Separate feeds include known:
- VPN exit nodes
- Residential proxy networks

These can help:
- Reduce noise in detections
- Add context to outbound traffic
- Identify infrastructure abuse

---

## 📜 License

This project is licensed under:

**Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**

- Attribution required
- Non-commercial use only
- Share alike for derivatives

---

## ⚠️ Disclaimer

These feeds are provided **as-is** for defensive and research purposes.

- No guarantee of accuracy or completeness
- Infrastructure may be compromised, misattributed, or reused
- Always validate before taking action

---

**If you find this project useful, attribution is appreciated.**


<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

