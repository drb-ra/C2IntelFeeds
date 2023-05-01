# C2IntelFeeds
Automatically created C2 Feeds

<<<<<<< HEAD
=======

* Feeds ( Source/Raw Data courtesy of Censys - https://censys.io/ ) \
 **Search 2.0** has massively improved detection rates on non-standard ports. **Great job Censys Team!**

  By default C2s seen active in the last 7 days are added to the main feed files. There is also a 30 day feed for any C2 seen live in the last 30 days.

  * `C2 IPs` - Live C2 IP (no frontend or CDN IPs - All bad)
  * `C2 Domains` - All domain names extracted from implants, including domain fronting values and fake Host headers (High abuse of MS, Apple and Google).
  * `C2 Domains Filtered` - Excludes several domains abused in domain fronting, along with fake headers for popular sites. Current filter list see:  `exclusions.rex` file
  * `C2 Domains with URL` - Same as domains and domains filtered but including an extra column with the URI path of the C2
  * `C2 Domains with URL and IP` - Same as domains and domains filtered but including an extra column with the URI path of the C2 and another with the C2 IP 
  * `Unverified C2 IPs` - Live C2 IPs based simply on the Censys search/query no validation can easily be performed or further configuration extracted. Some however are extremely accurate. Details in table below.
  
    | Tool | ```Censys Search```|
    |------|:------------|
    | [Sliver](https://github.com/BishopFox/sliver) |`(services.tls.certificates.leaf_data.subject.common_name="multiplayer" and same_service(services.jarm.fingerprint= 00000000000000000043d43d00043de2a97eabb398317329f027c66e4c1b01 and NOT services.port=31337 )) OR (services.banner_hashes="sha256:1f25c454ae331c582fbdb7af8a9839785a795b06a6649d92484b79565f7174ae" and services.jarm.fingerprint=3fd21b20d00000021c43d21b21b43d41226dd5dfc615dd4a96265559485910) OR same_service(services.tls.certificates.leaf_data.pubkey_bit_size: 2048 and services.tls.certificates.leaf_data.subject.organization: /(ACME\|Partners\|Tech\|Cloud\|Synergy\|Test\|Debug)? ?(co\|llc\|inc\|corp\|ltd)?/ and services.jarm.fingerprint: 3fd21b20d00000021c43d21b21b43d41226dd5dfc615dd4a96265559485910 and services.tls.certificates.leaf_data.subject.country: US and services.tls.certificates.leaf_data.subject.postal_code: /<1001-9999>/)`|
    |[Covenant](https://github.com/cobbr/Covenant) |`same_service(services.tls.certificates.leaf_data.subject_dn="CN=Covenant" AND services.tls.certificates.leaf_data.issuer_dn="CN=Covenant") OR (services.software.product="Kestrel web server" AND services.http.response.html_title="Covenant")`|
    |[Brute Ratel C4](https://bruteratel.com) |`services.http.response.body_hash="sha1:1a279f5df4103743b823ec2a6a08436fdf63fe30" OR same_service(services.http.response.body_hash="sha1:bc3023b36063a7681db24681472b54fa11f0d4ec" and services.jarm.fingerprint="3fd21b20d00000021c43d21b21b43de0a012c76cf078b8d06f4620c2286f5e")`|
    |[Mythic](https://github.com/its-a-feature/Mythic) |`same_service(services.tls.certificates.leaf_data.subject_dn="O=Mythic" AND services.http.response.html_title="Mythic") OR services.banner_hashes="sha256:fb8b5d212f449a8ba61ab9ed9b44853315c33d12a07f8ce4642892750e251530"`|
    |Deimos|`services.jarm.fingerprint: "00000000000000000041d00000041d9535d5979f591ae8e547c5e5743e5b64"`|
    |[Nighthawk C2](https://www.mdsec.co.uk/nighthawk/) |`same_service(services.banner="HTTP/1.1 404 Not Found\r\nDate:  <REDACTED>\r\nX-Test: 2\r\nServer: Apache\r\nContent-Length: 20\r\n" and services.http.response.body_hashes="sha256:d872e8e4176213ea84ebc76d8fb621c31b4ca116fd0a51258813e804fe110ca4")`|
    |Bianlian Go Trojan |`[REDACTED]`|     
    |[Havoc](https://github.com/HavocFramework/Havoc) |`same_service(services.tls.certificates.leaf_data.issuer.organization=/(Acme\|ACME\|acme\|Partners\|PARTNERS\|partners\|Tech\|TECH\|tech\|Cloud\|CLOUD\|cloud\|Synergy\|SYNERGY\|synergy\|Test\|TEST\|test\|Debug\|DEBUG\|debug)? ?(Co\|CO\|co\|Llc\|LLC\|llc\|Inc\|INC\|inc\|Corp\|CORP\|corp\|Ltd\|LTD\|ltd)?/ AND services.tls.certificates.leaf_data.issuer.country=US AND services.tls.certificates.leaf_data.issuer.postal_code=/[0-9]{4}/) OR services.http.response.headers.unknown.name: "X-Havoc" OR services.banner_hashes="sha256:f5a45c4aa478a7ba9b44654a929bddc2f6453cd8d6f37cd893dda47220ad9870"`|
    |[Responder](https://github.com/lgandx/Responder) |`services.banner="HTTP/1.1 401 Unauthorized\r\nServer: Microsoft-IIS/7.5\r\nDate:  <REDACTED>\r\nContent-Type: text/html\r\nWWW-Authenticate: NTLM\r\nContent-Length: 0\r\n" OR services.banner_hashes="sha256:0fa31c8c34a370931d8ffe8097e998f778db63e2e036fbd7727a71a0dcf5d28c" OR services.smb.negotiation_log.server_guid="00000000000000000000000000000000ee85abf7eaf60c4f928192476deb76a9"`|
    
  The easiest files for most of you to use should be [C2 IPs](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/IPC2s.csv), [C2 Domains Filtered](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/domainC2s-filter-abused.csv) and [Unverified C2 IPs](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/unverified/IPC2s.csv) or their 30 day counterparts.  
  
>>>>>>> c5267961f (Update README.md)
* VPN 
  * Nord VPN Exit Nodes
* Feeds
  * `C2 IPs` - Live C2 IP (no frontend or CDN IPs - All bad)
  * `C2 Domains` - All domain names extracted from implants, including domain fronting values and fake Host headers (High abuse of MS, Apple and Google).
  * `C2 Domains Filtered` - Excludes several MS domains abused in domain fronting, along with fake headers for popular sites.
