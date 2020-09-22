# C2IntelFeeds
Automatically created C2 Feeds

* VPN 
  * Nord VPN Exit Nodes

* Feeds

  By default C2s seen active in the last 7 days are added to the main feed files.

  * `C2 IPs` - Live C2 IP (no frontend or CDN IPs - All bad)
  * `C2 Domains` - All domain names extracted from implants, including domain fronting values and fake Host headers (High abuse of MS, Apple and Google).
  * `C2 Domains Filtered` - Excludes several MS domains abused in domain fronting, along with fake headers for popular sites.

  Additionally a new 30 day set of feed files was added for any C2 seen live in the last 30 days.
