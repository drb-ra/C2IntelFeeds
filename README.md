# C2IntelFeeds
Automatically created C2 Feeds | Also posted via [@drb_ra](https://twitter.com/drb_ra) 

**This would not be possible without Source/Raw Data courtesy of Censys** - https://censys.io/ 


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

| Tool | ```Censys Search```|
|------|:------------|
|[Sliver](https://github.com/BishopFox/sliver) |`(services.tls.certificates.leaf_data.subject.common_name="multiplayer" and same_service(services.jarm.fingerprint= 00000000000000000043d43d00043de2a97eabb398317329f027c66e4c1b01 and NOT services.port=31337 )) OR (services.banner_hashes="sha256:1f25c454ae331c582fbdb7af8a9839785a795b06a6649d92484b79565f7174ae" and services.jarm.fingerprint=3fd21b20d00000021c43d21b21b43d41226dd5dfc615dd4a96265559485910) OR same_service(services.tls.certificates.leaf_data.pubkey_bit_size: 2048 and services.tls.certificates.leaf_data.subject.organization: /(ACME\|Partners\|Tech\|Cloud\|Synergy\|Test\|Debug)? ?(co\|llc\|inc\|corp\|ltd)?/ and services.jarm.fingerprint: 3fd21b20d00000021c43d21b21b43d41226dd5dfc615dd4a96265559485910 and services.tls.certificates.leaf_data.subject.country: US and services.tls.certificates.leaf_data.subject.postal_code: /<1001-9999>/)`|
|[Covenant](https://github.com/cobbr/Covenant) |`same_service(services.tls.certificates.leaf_data.subject_dn="CN=Covenant" AND services.tls.certificates.leaf_data.issuer_dn="CN=Covenant") OR (services.software.product="Kestrel web server" AND services.http.response.html_title="Covenant")`|
|[Brute Ratel C4](https://bruteratel.com) |`services.http.response.body_hash="sha1:1a279f5df4103743b823ec2a6a08436fdf63fe30" OR same_service(services.http.response.body_hash="sha1:bc3023b36063a7681db24681472b54fa11f0d4ec" and services.jarm.fingerprint="3fd21b20d00000021c43d21b21b43de0a012c76cf078b8d06f4620c2286f5e")`|
|[Mythic](https://github.com/its-a-feature/Mythic) |`same_service(services.tls.certificates.leaf_data.subject_dn="O=Mythic" AND services.http.response.html_title="Mythic") OR services.banner_hashes="sha256:fb8b5d212f449a8ba61ab9ed9b44853315c33d12a07f8ce4642892750e251530" OR services.http.response.favicons.md5_hash="6be63470c32ef458926abb198356006c"`|
|[Deimos](https://github.com/DeimosC2/DeimosC2)|`services.jarm.fingerprint: "00000000000000000041d00000041d9535d5979f591ae8e547c5e5743e5b64" OR same_service(services.banner_hashes="sha256:38ea755e162c55ef70f9506dddfd01641fc838926af9c43eda652da63c67058b" and services.http.response.body_hashes="sha1:04ca7e137e1e9feead96a7df45bb67d5ab3de190" and services.tls.certificates.leaf_data.subject_dn="O=Acme Co" and services.tls.certificates.leaf_data.issuer_dn="O=Acme Co" and not services.tls.certificates.leaf_data.names="127.0.0.1:3000")`|
|[Nighthawk C2](https://www.mdsec.co.uk/nighthawk/) |`same_service(services.banner="HTTP/1.1 404 Not Found\r\nDate:  <REDACTED>\r\nX-Test: 2\r\nServer: Apache\r\nContent-Length: 20\r\n" and services.http.response.body_hashes="sha256:d872e8e4176213ea84ebc76d8fb621c31b4ca116fd0a51258813e804fe110ca4")`|
|Bianlian Go Trojan |`same_service(services.tls.certificates.leaf_data.subject_dn=/C=[0-9a-zA-Z]{16}, O=[0-9a-zA-Z]{16}, OU=[0-9a-zA-Z]{16}/ AND services.tls.certificates.leaf_data.issuer_dn=/C=[0-9a-zA-Z]{16}, O=[0-9a-zA-Z]{16}, OU=[0-9a-zA-Z]{16}/)`|     
|[Havoc](https://github.com/HavocFramework/Havoc) |`same_service(services.tls.certificates.leaf_data.issuer.organization=/(Acme\|ACME\|acme\|Partners\|PARTNERS\|partners\|Tech\|TECH\|tech\|Cloud\|CLOUD\|cloud\|Synergy\|SYNERGY\|synergy\|Test\|TEST\|test\|Debug\|DEBUG\|debug)? ?(Co\|CO\|co\|Llc\|LLC\|llc\|Inc\|INC\|inc\|Corp\|CORP\|corp\|Ltd\|LTD\|ltd)?/ AND services.tls.certificates.leaf_data.issuer.country=US AND services.tls.certificates.leaf_data.issuer.postal_code=/[0-9]{4}/) OR services.http.response.headers.unknown.name: "X-Havoc" OR services.banner_hashes="sha256:f5a45c4aa478a7ba9b44654a929bddc2f6453cd8d6f37cd893dda47220ad9870"`|
|[Responder](https://github.com/lgandx/Responder) |`services.banner="HTTP/1.1 401 Unauthorized\r\nServer: Microsoft-IIS/7.5\r\nDate:  <REDACTED>\r\nContent-Type: text/html\r\nWWW-Authenticate: NTLM\r\nContent-Length: 0\r\n" OR services.banner_hashes="sha256:0fa31c8c34a370931d8ffe8097e998f778db63e2e036fbd7727a71a0dcf5d28c" OR services.smb.negotiation_log.server_guid="00000000000000000000000000000000ee85abf7eaf60c4f928192476deb76a9"`|
|[Pupy RAT](https://github.com/n1nj4sec/pupy)|`same_service(services.http.response.headers.Etag:"aa3939fc357723135870d5036b12a67097b03309" AND services.http.response.headers.Server="nginx/1.13.8") OR same_service(services.tls.certificates.leaf_data.issuer.organization:/[a-zA-Z]{10}/ AND  services.tls.certificates.leaf_data.subject.organization:/[a-zA-Z]{10}/ AND services.tls.certificates.leaf_data.subject.organizational_unit="CONTROL")`|
|Qakbot|`same_service(services.jarm.fingerprint={"21d14d00021d21d21c42d43d0000007abc6200da92c2a1b69c0a56366cbe21","04d02d00004d04d04c04d02d04d04d9674c6b4e623ae36cc2d998e99e2262e"} AND services.http.response.body_hash="sha1:22e5446e82b3e46da34b5ebce6de5751664fb867") OR same_service(services.banner_hashes="sha256:5234096d7003929ad67037af6f5816933cab9e85f9b286468249ac9ab9bfb861" AND services.http.response.body_hash="sha1:22e5446e82b3e46da34b5ebce6de5751664fb867") OR (services.tls.certificates.leaf_data.subject_dn: /C=[A-Z]{2}, OU=([A-Z][a-z]{3,})( [A-Z][a-z]{3,}){0,2}, CN=[a-z]{4,12}\.[a-z]{2,4}/ and not services.tls.certificates.leaf_data.subject_dn:"OU=Domain Control Validated")`|
|[DcRat](https://github.com/qwqdanchun/DcRat)|`services.tls.certificates.leaf_data.issuer_dn="CN=DcRat Server, OU=qwqdanchun, O=DcRat By qwqdanchun, L=SH, C=CN"`|
|Viper|`services.http.response.body_hashes="sha1:cd40dbcdae84b1c8606f29342066547069ed5a33" OR services.http.response.favicons.md5_hash="a7469955bff5e489d2270d9b389064e1"`|
|[Supershell](https://github.com/tdragon6/Supershell/)|`services.http.response.html_title="Supershell - 登录" OR services.http.response.body_hashes="sha256:21ec9c71669486c5b874b1be3b9c341133e83939fdbeefa2080df1b1703c4928"`|
|Pikabot|`services: (tls.certificates.leaf_data.signature.self_signed: true and http.response.headers: (key: "Etag" and value.headers: '"3147526947+gzip"') and not tls.certificate.parsed.subject_dn: "emailAddress=") or services: (tls.certificates.leaf_data.signature.self_signed: true and tls.cipher_selected="TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256" and tls.certificates.leaf_data.pubkey_bit_size=4096 and tls.certificates.leaf_data.issuer_dn: /C=[A-Z]{2}, ST=[A-Z]{2}, O=([A-Z][a-z]{2,})( [A-Z][a-z\.]{2,}){0,5}, OU=([A-Z][a-z]{2,})( [A-Z][a-z\.]{2,}){0,5}, L=([A-Z][a-z]{2,})( [A-Z][a-z]{2,}){0,2}, CN=.*/)`|
|Meduza Stealer|`services.http.response.html_title="Meduza Stealer" OR services.http.response.favicons.md5_hash="e7a2bb050f7ec5ec2ba405400170a27d"`|
|[Evilginx/EvilGoPhish](https://help.evilginx.com)|`services.software.product: {Evilginx, EvilGoPhish}`|
|Hookbot/Pegasus|`services.http.response.html_title="HOOKBOT PANEL" OR services.http.response.favicons.hashes="sha256:b13b77f0b3d95c1146394ea855d915f189d3ea374179755cfb2ac47bfc8f306c"`|
|[AsyncRAT](https://github.com/NYAN-x-CAT/AsyncRAT-C-Sharp)|`same_service(services.tls.certificates.leaf_data.issuer_dn="CN=AsyncRAT Server" and services.tls.certificates.leaf_data.subject_dn="CN=AsyncRAT Server")`|
|[Remcos](https://breakingsecurity.net/remcos/)|`same_service(services.tls.versions.ja4s="t130200_1301_234ea6891581" and services.tls.ja3s="eb1d94daa7e0344597e756a1fb6e7054" and services.tls.cipher_selected="TLS_AES_128_GCM_SHA256" and services.jarm.fingerprint: 00000000000000000041d41d0000001798d6156df422564fb9b667b7418e4c and services.service_name="UNKNOWN" and services.tls.certificates.leaf_data.issuer_dn="" and services.tls.certificates.leaf_data.subject_dn="")`|
|DanaBot|`[REDACTED]`|
|Rhysida Trojan|`[REDACTED]`|
|[Oyster Backdoor](https://www.rapid7.com/blog/post/2024/06/17/malvertising-campaign-leads-to-execution-of-oyster-backdoor/)|`[REDACTED]`|
|SocGholish|`[REDACTED]`|
|[NetSupport Manager RAT](https://www.netsupportmanager.com)|`services.http.response.headers.Server="NetSupport Gateway/*"`|
|[Geacon_Pro](https://github.com/testxxxzzz/geacon_pro)|`same_service(services.tls.certificates.leaf_data.subject_dn="C=KZ, ST=KZ, L=, O=NN Fern Sub, OU=NN Fern, CN=foren.zik" AND  services.tls.certificates.leaf_data.issuer_dn="C=KZ, ST=KZ, L=, O=NN Fern Sub, OU=NN Fern, CN=foren.zik")`|
|[Hak5 Cloud C2](https://shop.hak5.org/products/c2)|`services.software.product: "cloud c2" and services.software.vendor="Hak5"`|
|[CHAOS](https://github.com/tiagorlampert/CHAOS)|`services.software.uniform_resource_identifier: "cpe:2.3:a:chaos:chaos:*:*:*:*:*:*:*:*"`|
|[Interactsh](https://github.com/projectdiscovery/interactsh)|`services.software.uniform_resource_identifier: "cpe:2.3:a:interactsh:interactsh:*:*:*:*:*:*:*:*"`|
|[Reverse SSH](https://github.com/NHAS/reverse_ssh)|`[REDACTED]`|
|[wstunnel](https://github.com/erebe/wstunnel)|`[REDACTED]`|
|[Ligolo-ng](https://github.com/nicocha30/ligolo-ng)|`[REDACTED]`|
|Ransomhub Python C2|`[REDACTED]`|
|[Pyramid](https://github.com/naksyn/Pyramid)|`[REDACTED]`|
|VPN Themed Phishing|`[REDACTED]`|
|StealC v2|`services.http.response.body_hashes="sha256:067b25c7c2e27041dc47a0a4564b56a6bbfdc41e5dd630dbf070fdada4dbff71"`|
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

