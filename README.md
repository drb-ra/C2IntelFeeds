# C2IntelFeeds
Automatically created Command and Control (C2) Feeds | Also posted via [@drb_ra](https://twitter.com/drb_ra)


## Data Source
- All data is courtesy of Censys - https://censys.io/ - A vendor providing excellent Internet visibility.
  * Their **Search 2.0** product has massively improved detection rates on non-standard ports. **Great job Censys Team!**

## Folder Contents

### `C2_configs` - long-term results, containing detailed Cobaltstrike C2 Configuration
  - Format: CSV & JSON
  - Fields (CSV Headings):  `FirstSeen, ip, ASN, BeaconType, C2Server, Port, SleepTime, Jitter, Proxy_Behavior, HostHeader, CertificateNames, HttpGet_Metadata, HttpPostUri, HttpPost_Metadata, KillDate, PipeName, UserAgent, Watermark, DNS_Idle, DNS_Sleep`
  - _Note: `ip` reflects the true C2 IP not the one provided in the configuration of the beacon._

### `feeds` - summaries of the above dataset, primarily trimmed by time and ioc type _(ip, domain, or URL.)_ 
- General Data Formats: _by suffix/extension_ 
  * `*.csv` (main field files) - contains data from the last 7 days
  * `*-30day.csv` - contains data from the last 30 days
  * `*-filter-abused.csv` - Excludes several domains abused in domain fronting, along with fake headers for popular sites. The current filter list is in this repo as `exclusions.rex`

- Specific Feed Contents:
  * C2 IPs (`IPC2s...`) - Live C2 IP (no frontend or content delivery network (CDN) IPs - All bad)
  * C2 Domains (`domainC2s...`) - All domain names extracted from implants, including domain fronting values and fake Host headers (High abuse of MS, Apple and Google).
  * C2 Domains with URL (`domainC2swithURL...`) - Same as above, but includes an extra column with the URI path of the C2
  * C2 Domains with URL and IP (`domainC2swithURLwithIP...`) - Same as above, but includes an extra column with the C2 IP 
  
### `vpn` - identified commercial VPN hosts
  * `NordVPNIPs.csv` - NordVPN Exit Node IP addresses



<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
