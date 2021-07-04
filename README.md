# C2IntelFeeds
Automatically created C2 Feeds


* Feeds ( Source/Raw Data courtesy of Censys - https://censys.io/ )

  By default C2s seen active in the last 7 days are added to the main feed files.

  * `C2 IPs` - Live C2 IP (no frontend or CDN IPs - All bad)
  * `C2 Domains` - All domain names extracted from implants, including domain fronting values and fake Host headers (High abuse of MS, Apple and Google).
  * `C2 Domains Filtered` - Excludes several domains abused in domain fronting, along with fake headers for popular sites. Current filter list:  `(^microsoft\.com|\.microsoft\.com|\.windowsupdate\.com|^apple\.com|^amazon\.com|\.apple\.com|\.amazon\.com|\.mzstatic\.com|^gmail\.com|^jquery\.com|^github\.com|\.live\.com|\.bing\.com|^google\.com|\.bloomberg\.com|\.oracle\.com|\.lenovo\.com|^a0\.awsstatic\.com|^ajax\.aspnetcd\.com|^baidu\.com|^cnn\.com|^www\.gmail\.com|^www\.bankrate\.com|^tencent\.com|^ajax\.aspnetcdn\.com|^dist\.nuget\.org|^www\.msnbc\.com|^www\.forbes\.com|^bbc\.com|^bing\.com|^nytimes\.com|\.nytimes\.com|^cdn\.vsassets\.io)`
  * `C2 Domains with URL` - Same as domains and domains filtered but including an extra column with the URI path of the C2
  * `C2 Domains with URL and IP` - Same as domains and domains filtered but including an extra column with the URI path of the C2 and another with the C2 IP 

  Additionally a new 30 day set of feed files was added for any C2 seen live in the last 30 days.
  
* VPN 
  * Nord VPN Exit Nodes

* C2_configs 
  * Detailed CobaltStrike Configuration in CSV and JSON including the following fields:  `FirstSeen,ip,ASN,BeaconType,C2Server,Port,SleepTime,Jitter,Proxy_Behavior,HostHeader,CertificateNames,HttpGet_Metadata,HttpPostUri,HttpPost_Metadata,KillDate,PipeName,UserAgent,Watermark,DNS_Idle,DNS_Sleep` IP reflects the true C2 IP not the one provided in the configuration of the beacon.


<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
