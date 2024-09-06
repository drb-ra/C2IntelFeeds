# C2IntelFeeds
Automatically created C2 Feeds | Also posted via [@drb_ra](https://twitter.com/drb_ra)


* Feeds ( Source/Raw Data courtesy of Censys - https://censys.io/ ) \
 **Search 2.0** has massively improved detection rates on non-standard ports. **Great job Censys Team!**

  By default C2s seen active in the last 7 days are added to the main feed files. There is also a 30 day feed for any C2 seen live in the last 30 days.

  * `C2 IPs` - Live C2 IP (no frontend or CDN IPs - All bad)
  * `C2 Domains` - All domain names extracted from implants, including domain fronting values and fake Host headers (High abuse of MS, Apple and Google).
  * `C2 Domains Filtered` - Excludes several domains abused in domain fronting, along with fake headers for popular sites. Current filter list see:  `exclusions.rex` file
  * `C2 Domains with URL` - Same as domains and domains filtered but including an extra column with the URI path of the C2
  * `C2 Domains with URL and IP` - Same as domains and domains filtered but including an extra column with the URI path of the C2 and another with the C2 IP 
  * `Unverified C2 IPs` - Live C2 IPs based simply on the Censys search/query no validation can easily be performed or further configuration extracted. Some however are extremely accurate. Details in table below.

**NOTE:** Given the issue with Threatfox submissions we have now published 4 additional files. These files contain IP and destination port pairs for all IP based feeds (IPs and Unverified IPs both for 7 and 30 days).

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
|[Remcos](https://breakingsecurity.net/remcos/)|`[REDACTED]`|
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

**NOTE:** The last 3 entries are tools that may also be used for legitimate purposes. Please validate your hits or filter these locally for your own needs.

# Additional Details

  The easiest files for most of you to use should be [C2 IPs](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/IPC2s.csv), [C2 Domains Filtered](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/domainC2s-filter-abused.csv) and [Unverified C2 IPs](https://github.com/drb-ra/C2IntelFeeds/blob/master/feeds/unverified/IPC2s.csv) or their 30 day counterparts.  
  
* VPN 
  * Nord VPN Exit Nodes
  * Proton VPN Entry & Exit Nodes

* C2_configs 
  * Detailed CobaltStrike Configuration in CSV and JSON including the following fields:  `FirstSeen,ip,ASN,BeaconType,C2Server,Port,SleepTime,Jitter,Proxy_Behavior,HostHeader,CertificateNames,HttpGet_Metadata,HttpPostUri,HttpPost_Metadata,KillDate,PipeName,UserAgent,Watermark,DNS_Idle,DNS_Sleep` IP reflects the true C2 IP not the one provided in the configuration of the beacon.
  * Version 2 includes 3 additional fields `SpawnToX86,SpawnToX64,PublicKey`
  * There's also a 30 day JSON only version that includes First and Last Seen dates within the last 30 days. 
  * Powershell Empire and PoSHC2 are also avaliable in JSON format.


<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
