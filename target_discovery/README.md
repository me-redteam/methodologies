# Passive Target Discovery 

Recon in general Information Gathering, just collecting info about some organization or personal we can say it as recon.
Recon in general Will be described as information collection/OSINT/Discovery etc.


Two types of recon in general:

Active
------------- | 
passive 


## **Passive recon**

Gathering information without alerting the subject of the surveillance is passive reconnaissance. This is the natural start of any reconnaissance because, once alerted, a target will likely react by drastically increasing security in anticipation of an attack.



**Note: there are various of tools and ways to discover each method.**
## Methods

   * [<strong>1- Obtains important DNS records that commonly used types:</strong>](#1--obtains-important-dns-records-that-commonly-used-types)
   * [<strong>2- Reverse DNS Lookup - PTR Record for main domain</strong>](#2--reverse-dns-lookup---ptr-record-for-main-domain)
   * [<strong>3- Whois lookup domain name</strong>](#3--whois-lookup-domain-name)
   * [<strong>4- Whois lookup IP address</strong>](#4--whois-lookup-ip-address)
   * [<strong>5- ASN blocks discovery</strong>](#5--asn-blocks-discovery)
   * [<strong>6- Scrape Certificate Transparency Logs</strong>](#6--scrape-certificate-transparency-logs)
   * [<strong>7- Subdomains Enmuration:</strong>](#7--subdomains-enmuration)
   * [<strong>8- Forwards DNS Bruteforcing on subdomain name:</strong>](#8--forwards-dns-bruteforcing-on-subdomain-name)
   * [<strong>9- Reverse DNS lookups for identified subnets (PTR Record)</strong>](#9--reverse-dns-lookups-for-identified-subnets-ptr-record)
   * [<strong>10- Search Engine Scraping / Dorking (Google/Bing/Yandex)</strong>](#10--search-engine-scraping--dorking-googlebingyandex)
   * [<strong>11- Shodan Enumeration:</strong>](#11--shodan-enumeration)
   * [<strong>12- Wayback Machine</strong>](#12--wayback-machine)
   * [<strong>13- Metadata Scraping</strong>](#13--metadata-scraping)
   * [<strong>14- Static analysis of associated mobile applications</strong>](#14--static-analysis-of-associated-mobile-applications)
   * [<strong>15- Social Media Scraping</strong>](#15--social-media-scraping)
   * [<strong>16- Passive DNS Recon</strong>](#16--passive-dns-recon)
   * [<strong>17- Github/Gitlab Enumeration</strong>](#17--githubgitlab-enumeration)
   * [<strong>18- Virtual Host Enumeration</strong>](#18--virtual-host-enumeration)
   * [<strong>19- Alterations &amp; permutations of already known subdomains</strong>](#19--alterations--permutations-of-already-known-subdomains)
   * [<strong>20- TLD expansion</strong>](#20--tld-expansion)



### Methods details:


## **1- Obtains important DNS records that commonly used types:**

- A (Host address)
- AAAA (IPv6 host address)
- MX (Mail eXchange)
- PTR (Pointer)
- TXT (Descriptive text)
- SPF it’s the record that specify which IP addresses and/or hostnames are authorized to send email from the specific domain and this help to discover new IP addresses.


Tool:

**Dig**

- Description:
Zone transfer, DNS lookups & reverse lookups

- Installation:
Installed by default in Kali, otherwise:

-	Usage:

```
dig (DNS Record) target.com +noall +answer
```




## **2- Reverse DNS Lookup - PTR Record for main domain**

The Reverse Lookup tool will do a reverse IP lookup. If you type in an IP address, we will attempt to locate a DNS PTR record for that IP address.

Tool: 


**Dig**

- Usage:

```
dig +noall +answer -x target IP
```

GUI tool:

https://mxtoolbox.com/



## **3- Whois lookup domain name**

WHOIS allows you to look up the name and contact information of whoever operates any website's generic domain name.

Tool:


**Whois**

- Installation
 
Installed by default on Kali

- Usage 

```
Whois target.com
```

GUI tool:
http://whois.aeda.ae/
https://www.whois.com/whois/




## **4- Whois lookup IP address**

IP WHOIS Lookup Tool offers a free IP Lookup Service to check who owns an IP address. 

Tools: 


**ASN**

- Description:

ASN / RPKI validity / BGP stats / IPv4v6 / Prefix / ASPath / Organization / IP reputation & geolocation lookup tool / Web traceroute server.

https://github.com/nitefood/asn

- Installation

```
git clone https://github.com/nitefood/asn.git
```
```
sudo apt -y install curl whois bind9-host mtr-tiny jq ipcalc grepcidr ncat aha
```

- Usage

```
./asn -n target IP
```

- GUI Tool

https://bgp.he.net/
https://bgpview.io




## **5- ASN blocks discovery**
Finding ASN will help us identify netblocks of the domains.

Tools:

**Amass**

- Description

Brute force, Google, VirusTotal, alt names, ASN discovery
- Installation

```
sudo apt-get install amass
```

- Usage

```
amass intel -asn "ASN"
```

GUI tool:

http://bgp.he.net/




## **6- Scrape Certificate Transparency Logs**

Enumerate subdomains using CT logs

Tool:

**CTFR**

https://github.com/UnaPibaGeek/ctfr

- Installation

```
git clone https://github.com/UnaPibaGeek/ctfr.git
```

- Usage
```
python3 ctfr.py -d target.com
```

GUI tool:
https://crt.sh/

## **7- Subdomains Enmuration:**
Sub-domain enumeration is the process of finding sub-domains for one or more domains. It helps to broader the attack surface, find hidden applications, and forgotten subdomains

**Note that there are varios of tools to use for the subdomains enum**

Tools: 
**amass**

- Installation

```
apt-get install amass
```

- Usage

```
amass enum -passive -d target.com
```

**assetfinder**

- Installation

```
apt-get install assetfinder
```

- Usage

```
assetfinder -subs-only target.com
```

**subfinder**

- Installation
```
apt-get install subfinder
```

- Usage
```
subfinder -d target.com
```

## **8- Forwards DNS Bruteforcing For the Subdomains:**
To find the discovered subdomains IP addresses.

Tool:

**resolveDomains**
- Description

Resolve live subdomain and get the IP addresses.

https://github.com/Josue87/resolveDomains

- Installation

```
git clone https://github.com/Josue87/resolveDomains.git
```

- Usage

```
./resolveDomains -d targetsubdomian.txt
```




## **9- Reverse DNS lookups for identified subnets (PTR Record)**

Tool:

**Fierce** 
- Usage:

```
fierce --range x.x.x.x/xx
```




## **10- Search Engine Scraping / Dorking (Google/Bing/Yandex)**

Tool:

**Sublist3r**

- Description

Baidu, Yahoo, Google, Bing, Ask, Netcraft, DNSdumpster, VirusTotal, Threat Crowd, SSL Certificates, PassiveDNS

https://github.com/aboul3la/Sublist3r

- Installation

```
git clone https://github.com/aboul3la/Sublist3r.git
```
```
pip install -r requirements.txt
```

- Usage

```
python3 sublist3r.py -d targets.com
```
-d flag in the **Sublist3r** command-line means for the scraping.
and if you want to brute-force you can add the flag -b.

**Google**
Search engine
- Usage

`Find subsomains: site:*.target.com
Find subdomains & exclude specific ones: site:*.target.com -site:www.target.com -site:help.target.com`




## **11- Shodan Enumeration:**

The below link is helpful for the Shodan enumeration
https://thedarksource.com/shodan-cheat-sheet 





## **12- Wayback Machine**
Find forgotten domains/URLs


GUI tool:

https://archive.org/web/




## **13- Metadata Scraping** 
Try to find any metadata of the founded/leaked files of the target.




## **14- Static analysis of associated mobile applications**
Search for the target if they have mobile app from all the stores. 
Tool:

**Mobile Security Framework (MobSF)**

https://github.com/MobSF/Mobile-Security-Framework-MobSF




## **15- Social Media Scraping**

Social Media Scraping help to get the targets emails to use it for the next phase. 

From the DNS lookup you might find emails for the owner or any contact of the target. Such as from the pervious tools or from the GUI.




## **16- Passive DNS Recon** 

Passive DNS helps to discover hosts related to a domain. Finding visible hosts from the attackers perspective is an important part of the security assessment process.

GUI tool:

https://dnsdumpster.com/




## **17- Github/Gitlab Enumeration**

You can find leaked secrets of the target by using the GitHub/GitLab dorking form the below tool:

https://github.com/hisxo/gitGraber |
https://github.com/eth0izzle/shhgit |
https://github.com/techgaun/githubdorks |
https://github.com/michenriksen/gitrob |
https://github.com/anshumanbh/git-all-secrets |
https://github.com/awslabs/git-secrets |
https://github.com/kootenpv/gittyleaks |
https://github.com/dxa4481/truffleHog |
https://github.com/obheda12/GitDorker |

**All you need is to generate an API key from your GitHub account and store it in a specific file depend on the tool you choice.**

For example we will use the **gitGraber** tool,
- Installation
```
git clone https://github.com/hisxo/gitGraber.git
```

```
pip install -r requirements.txt
```

Add your Github API key where you can find instruction from  GitHub  [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) 

Then you need to add your API key in the tool file **config.py**:


```
GITHUB_TOKENS = ['**API_KEY**']
GITHUB_URL_FILE = 'rawGitUrls.txt'
GITHUB_API_URL = 'https://api.github.com/search/code?q='
GITHUB_API_COMMIT_URL = 'https://api.github.com/repos/'
GITHUB_SEARCH_PARAMS = '&sort=indexed&o=desc'
GITHUB_BASE_URL = 'https://github.com'
GITHUB_MAX_RETRY = 10
DISCORD_WEBHOOKURL = 'https://discordapp.com/api/webhooks/7XXXXXXXXXXXXXX/XXXXXXXXXXXXXXXXXXXXXX'
SLACK_WEBHOOKURL = 'https://hooks.slack.com/services/TXXXXXXXX/BXXXXXXXX/XXXXXXXXXXXXXXXXXXXXXXX'
TELEGRAM_CONFIG = {
    "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "chat_id": -999999999999999
}
```


Then save the file.


- Usage

```
python3 gitGraber.py -k wordlists/keywords.txt -q "target.com"
```


## **18- Virtual Host Enumeration** 
Tool:

**Virtual-host-discovery**
- Description
Expand the target by detecting old or deprecated code. It may also reveal hidden hosts that are statically mapped in the developer's `/etc/hosts` file.

- Installation

```
apt-get install hosthunter
```


- Usage

```
hosthunter targetsIP.txt --output result.txt
```




## **19- Alterations & permutations of already known subdomains**

Tool:

**AltDNS**

- Description
Subdomain discovery through alterations and permutations

https://github.com/infosec-au/altdns

- Installation
```
git clone https://github.com/infosec-au/altdns.git
```

Python 2:

```
pip install py-altdns==1.0.0
```

Python 3:

```
pip3 install py-altdns==1.0.2
```

- Usage

```
altdns -i subdomains.txt -o data_output -w words.txt 
```




## **20- TLD expansion**

Tool:

**Dnscan**

- Description

AXFR, brute force

https://github.com/rbsec/dnscan

- Installation

```
git clone https://github.com/rbsec/dnscan.git
```
- Usage

```
./dnscan.py -l target.txt -T
```







