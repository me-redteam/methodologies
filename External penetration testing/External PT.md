# External penetration testing
> An external network pen test is designed to test the effectiveness of perimeter security controls to prevent and detect attacks as well as identifying weaknesses in internet-facing assets such as web, mail and FTP servers,
> A penetration testing methodology defines a roadmap with practical ideas and proven practices that can be followed to assess the true security posture of a network, application, system, or any combination thereof.

- Testing Methodology 
  + Collecting Inscope Domains 
    +   Recon of a target's SSL certificates.
        -  To expand Our attack surface we want to extract hostname from certificate 
        -  Tools : 
           - Cero  
             - URL : https://github.com/glebarez/cero
              ```
              Scan for one host or one ip
              echo ip|cero -p $(ports) -c $threads
              echo domain|cero -p $(ports) -c $threads
              ```
              ```
              Scan For CIDR OR List of Domains
              prips CIDR | cero -p $(ports) -c $threads
              cat list | cero -p $(ports) -c $threads
              ```
            - hakip2host
              - URL : https://github.com/hakluke/hakip2host
              ```
              Command 
               echo ip | hakip2host
               echo domain| hakip2host
               prips CIDR | hakip2host
              ```
        
    +   Subdomain Enumeration
        -  The goal is to enumerate subdomains from various resources to increase the attack surface to the maximum
        -  Tools : 
           - Subfinder 
             - URL   https://github.com/projectdiscovery/subfinder
                - Subfinder's default config file location is at $HOME/.config/subfinder/config.yaml .
                - For subfinder you can obtain  API keys by signing up on 18 Passive DNS sources .
                - Sample for config file : https://gist.github.com/sidxparab/e981c813f4ad057ed080a75a7fe00f4e
                - Basic command : ```subfinder -d apple.com -all -config config.yaml -o output.txt```
             
           - Amass 
             - URL https://github.com/OWASP/Amass
                - By default, amass config file is located at $HOME/.config/amass/config.ini .
                - Sample for config file : https://gist.github.com/sidxparab/4cd40d6e2f9422a005b06f19919200d0 .
                - Basic Command : ```amass enum -passive -d example.com -config config.ini -o output.txt```
                - Detailed article : https://hackbotone.com/blog/amass-osint-reconnaissance-tool/
             
           - Sublist3r 
             - URL https://github.com/aboul3la/Sublist3r
                - Sublist3r is widely popular to enumerate subdomains of a website and to gather subdomain it uses many popular search engines such as Google, Yahoo, Bing, Baidu.
                - Basic Command : ```python sublist3r.py -d apple.com```
             
           - Assetfinder 
             - URL https://github.com/tomnomnom/assetfinder
                - Basic Command : ```assetfinder --subs-only apple.com```
             
    +   Subdomain Permutations
        - generate combinations/permutations of the already known subdomains. 
           - ripgen
             - URL https://github.com/resyncgg/ripgen/
               - Ripgen is designed to take a list of known-good subdomains and generate different permutations. It does this by breaking apart subdomain names into individual words, and then generating thousands upon thousands of different possible domains for you using those words and some other common subdomains prefixes and suffixes.
               - Steps :
                 - The tool takes result from any subdomain enumeration tool
                 - ```subfinder -d apple.com -o apple-subdomains.txt```
                 - ```cat apple-subdomains.txt|ripgen > apple-ripgen.txt```
                 - ```cat apple-ripgen.txt | dnsx -t 1000 -silent -o apple-ripgen-resolve.txt```
                 - The last step we used dnsx (tool to check for resorvable domains ) to make sure that all domains we got from ripgen is resorvable 
           - gotator
             - URL https://github.com/Josue87/gotator
               - Detailed Steps : https://sidxparab.gitbook.io/subdomain-enumeration-guide/active-enumeration/permutation-alterations
     
    +   Subdomain Bruteforce 
        - Sometimes tools miss out on multiple subdomains that may or may not be listed on various sources where these tools go and look for. Subdomain Bruteforcing helps to brute force the target's main domain against a wordlist to see if a valid subdomain can be found
        - tools : 
           - puredns 
             - URL  https://github.com/d3mondev/puredns
               - puredns is a fast domain resolver and subdomain bruteforcing tool that can accurately filter out wildcard subdomains and DNS poisoned entries.
               - Steps : 
                 - Generating list of open public resolvers using dnsvalidator : https://github.com/vortexau/dnsvalidator
                 - ```puredns bruteforce wordlist.txt apple.com -r resolvers.txt -w output.txt```
       
    +  Extracting Subdomains From Wayback History
       - Checking for Wayback History is always a great idea
       - tools : 
         - gau 
           - URL https://github.com/lc/gau
             - fetches known URLs from AlienVault's Open Threat Exchange, the Wayback Machine, Common Crawl, and URLScan for any given domain
             - basic command : ```gau --subs apple.com```
         - gauplus
           - URL https://github.com/bp0lr/gauplus
             - basic command : gauplus apple.com
             - After Collecting URLS from gau & gauplus We can use unfurl tool : https://github.com/tomnomnom/hacks/tree/master/unfurl to extract only domains
             - ```gau --subs apple.com|unfurl domains```

    +  Trademark and Copyright Recon
       - When approaching a target, the scope is important.Using trademark and copyright recon we can increase our attack service 
       - for example apple.com
         - At the end of the page you will find "Copyright © 2022 Apple Inc" as a trademark
         - Searching on google using "Copyright © 2022 Apple Inc" will get us another domains not only apple.com like "applemediaservices.com" & "shazam.com" whiich also owned by apple  
         - Also you can Search for old years , you may find : 
           - Old sites
           - Outdated installs of software
           - Build tools
         - We can use same techniqu with shodan ---> Org:"apple Inc."
         - Also on crt.sh ----> apple Inc.

    +  Virtual Host 
       - Many web servers are more like apartment buildings than houses: They host several domain names, and so the IP address alone is not enough to indicate which domain a user is trying to reach. This can result in the server showing the wrong SSL certificate, which prevents or terminates an HTTPS connection
       - For Example we have ip 10.10.10.10 with *.apple.com in the certificate we will brute force on hosts using tool like ffuf as below 
           ```ffuf -u http://10.10.10.10 -w subdomain-wordlist -H "FUZZ.apple.com"```
       - For subdomain wordlist we can use :
         - https://wordlists-cdn.assetnote.io/data/manual/2m-subdomains.txt
         - https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS
     
  + Probing 
    - Once we are done enumerating and gathering subdomains from all the possible ways, the next step is to use this list of subdomains/domains and probe for working HTTP and HTTPS servers in order to filter out interesting endpoints out of the rest.
    - To perform the probing task we will use :
      - ports we got from port scan
      - group of well known ports
      - tools : 
        - httpx 
          - URL https://github.com/projectdiscovery/httpx
            - Command to scan well known ports 
              ```
              httpx -sc -tech-detect -cl -server -ct -title -nf -t 500 -tls-grab -tls-probe -vhost -fr -timeout 15 -p 1000,10000,10001,10002,1001,1002,1003,1004,1005,10079,10080,10081,2000,20000,20001,2001,2002,2003,2004,2005,20079,20080,20081,201,2011,2012,2013,2019,2020,2021,20442,20443,20444,2048,2049,2050,2079,2080,2081,2082,2083,2084,2085,2086,2087,2088,2094,2095,2096,2120,2121,21211,21212,21213,2122,221,222,2221,2222,22221,22222,22223,2223,223,2322,2323,2324,2344,2345,2346,2374,2375,2376,2377,2379,24,2479,2480,2481,263,264,265,27016,27017,27018,28016,28017,28018,28079,28080,28081,28442,28443,28444,29016,29017,29018,2999,3000,3001,3002,3003,3004,3005,30079,30080,30081,30442,30443,30444,3047,3048,3049,3050,3079,3080,3081,3099,310,3100,3101,311,312,31419,3305,3306,3307,3332,3333,33332,33333,33334,3334,3351,3388,3389,3390,3455,3456,3457,3540,3541,3542,3543,3688,3689,3690,3748,3749,3750,3779,3780,3781,3789,3790,3791,38079,38080,38081,38442,38443,38444,388,389,390,3938,3999,39999,4000,40000,40001,4001,4002,4003,4004,4005,40079,40080,40081,4039,4040,4041,40442,40443,40444,4171,4172,4173,4199,4200,4201,4242,4243,4244,4369,442,443,444,4442,4443,4444,44443,44444,44445,4505,4506,451,452,453,4566,4567,4568,4781,4782,4783,48079,48080,48081,48442,48443,48444,4847,4848,4849,4949,4999,49999,500,5000,50000,50001,5001,5002,5003,50030,5004,50049,5005,50050,50051,5006,50060,50069,5007,50070,50071,50075,50079,5008,50080,50081,5009,50090,501,5010,50105,5011,502,503,50442,50443,50444,5049,5050,5051,5079,5080,5081,514,523,5399,5400,5401,5432,5499,5500,5501,554,55442,55443,55444,555,5554,5555,55554,55555,55556,5556,556,5598,5599,5600,5601,5602,5666,5671,5672,5673,5674,5677,5678,5679,5799,5800,5801,5802,58079,58080,58081,58442,58443,58444,5899,5900,5901,5902,5983,5984,5985,5986,5987,5999,59999,6000,60000,60001,60002,6001,6002,6003,6004,6005,60079,60080,60081,60442,60443,60444,6079,6080,6081,61616,61621,623,630,631,632,636,6378,6379,6380,6432,6443,6587,6588,6589,6788,6789,6790,684,685,686,6984,6999,7000,7001,7002,7003,7004,7005,7006,7009,7010,7011,7013,7014,7015,7069,7070,7071,7072,7079,7080,7081,7082,7170,7171,7172,7199,7282,7283,7284,7289,7290,7291,7442,7443,7444,7473,7474,7475,7546,7547,7548,7549,7575,7653,7654,7655,7656,7657,7658,7675,7676,7677,776,777,7776,7777,7778,7779,778,7780,7787,7788,7789,7889,7890,7891,79,7978,7979,7980,799,7998,7999,80,800,8000,8001,8002,8003,8004,8005,8006,8007,8008,8009,801,8010,8011,8012,8013,8014,8015,8016,8017,8018,8019,802,8020,8021,8022,8023,8024,8025,8026,8027,8028,8029,8030,8031,8032,8033,8034,8035,8036,8037,8039,8040,8041,8042,8043,8044,8045,8046,8047,8048,8049,8050,8051,8052,8053,8054,8055,8056,8057,8065,8066,8067,8068,8069,807,8070,8071,8072,8073,8079,808,8080,8081,8082,8083,8084,8085,8086,8087,8088,8089,809,8090,8091,8092,8093,8094,8095,8096,8097,8098,8099,81,8100,8101,8102,8103,8104,8105,8106,8107,8108,8109,8110,8111,8112,8113,8117,8118,8119,8122,8123,8124,8125,8126,8127,8138,8139,8140,8141,8179,8180,8181,8182,8183,8184,8185,8189,8190,8191,8199,82,8200,8201,8221,8222,8223,8281,8282,8283,83,8333,8334,8335,8382,8383,8384,84,8400,8401,8402,842,843,844,8442,8443,8444,8445,85,8552,8553,8554,8584,8585,8586,8587,8589,8590,8591,86,8601,8602,8603,8665,8666,8667,8685,8686,8687,8688,8689,8699,87,8700,8701,873,8764,8765,8766,8767,8786,8787,8788,8789,879,8790,8799,88,880,8800,8801,8802,8803,8804,8805,8806,8807,8808,8809,881,8810,8811,8812,8813,8814,8815,8816,8817,8818,8819,8820,8821,8822,8823,8824,8825,8826,8827,8829,8830,8831,8832,8833,8834,8835,8837,8838,8839,8840,8841,8842,8843,8844,8845,8847,8848,8849,8850,8851,8852,8853,8854,8855,8856,8857,8858,8859,8860,8861,8865,8866,8867,8868,8869,887,8870,8871,8872,8873,8874,8875,8876,8877,8878,8879,888,8880,8881,8882,8883,8884,8885,8886,8887,8888,8889,889,8890,8891,8892,8898,8899,89,8900,8983,8987,8988,8989,8990,8991,8992,8993,8994,8998,8999,90,9000,9001,9002,9003,9004,9005,9006,9007,9008,9009,9010,9011,9012,9013,9014,9015,9016,9019,9020,9021,9022,9023,9024,9025,9026,9027,9028,9029,9030,9031,9032,9033,9034,9035,9036,9037,9038,9039,9040,9041,9042,9043,9044,9045,9046,9047,9048,9049,9050,9051,9069,9070,9071,9079,9080,9081,9082,9083,9084,9085,9087,9088,9089,9090,9091,9092,9093,9094,9095,9096,9097,9098,9099,91,9100,9101,9102,9103,9104,9105,9106,9107,9108,9109,9110,9111,9112,9118,9119,9120,9135,9136,9137,9150,9188,9189,9190,9191,9192,9198,9199,92,9200,9201,9202,9203,9204,9205,9206,9207,9208,9209,9210,9211,9212,9213,9214,9215,9216,9217,9218,9219,9220,9221,9222,9223,9250,9251,9252,9294,9295,9296,9298,9299,93,9300,9301,9302,9303,9304,9305,9306,9307,9308,9309,9310,9311,9312,9388,9389,9390,94,9442,9443,9444,9445,9446,9499,95,9500,9501,9502,9508,9514,9526,9527,9528,9549,9550,9551,9594,9595,9596,9599,96,9600,9601,9605,9606,9607,9662,9663,9664,9681,9682,9683,9689,9690,9691,97,9703,9704,9705,9742,9743,9744,9764,9765,9766,98,9860,9861,9862,9868,9869,9870,9875,9876,9877,989,99,990,9942,9943,9944,9945,9949,9950,9951,9965,9966,9967,998,9980,9981,9982,9987,9988,9989,999,9990,9991,9992,9993,9994,9995,9996,9997,9998,9999
              ```
        - httprobe 
          - URL https://github.com/tomnomnom/httprobe

  + ScreenShots
    - Now we have discovered all the live web servers present in the scope , we want to take screen shots for the live hosts
    - tools : 
      - aquatone 
        - URL https://github.com/michenriksen/aquatone
        - Basic Commands : ```cat live-hosts|aquatone```
      - EyeWitness 
        - URL https://github.com/FortyNorthSecurity/EyeWitness
        - basic commands : ```EyeWitness --file live-hosts```
        
  + Template Based Scanning
    - Template-based scanners allow you to create a template to identify a vulnerability and re-run it on any number of targets. It works on regex and other matching patterns to check if the conditions are satisfied and if yes, it flags for that specific vulnerability. Most of the remote and unauthenticated CVEs are automated using Nuclei & Jaeles templates.
    - tools : 
      - nuclei 
        - URL  https://github.com/projectdiscovery/nuclei
        - Nuclei is a Fast and Customizable Vulnerability Scanner. Nuclei tool is Golang Language-based tool used to send requests across multiple targets based on nuclei templates leading to zero false positive or irrelevant results and provides fast scanning on various hosts.
        - basic command : ```nuclei -l <target-list> -t <template-path>```
        - detailed article for using nuclei : 
          - https://blog.intigriti.com/2021/05/10/hacker-tools-nuclei/
          - https://github.com/projectdiscovery/nuclei
      - jaeles 
        - URL  https://github.com/jaeles-project/jaeles
  
  
  + cloud enumeration 
    - Here we want to Enumerate possible cloud assets belonging to our target
    - Tools : 
      - cloud_enum
        - URL  https://github.com/initstring/cloud_enum
        - Basic Command : ```cloud_enum.py -k apple -k apple.com```
      - cloudlist  
        - URL  https://github.com/projectdiscovery/cloudlist
      - MicroBurst
        - Tool to enumerate azure services
        - detailed article : https://www.netspi.com/blog/technical/cloud-penetration-testing/enumerating-azure-services/
  
  
As we have list of live hosts  , we should start now to pick one of the hosts to test 
  + Technology Fingerprinting
    - Check for the services running on the application. Often the servers, libraries and third-party components used might be outdated and vulnerable to known security issues.
    - tools : 
      - Wappalyzer Plugin
      - Whatweb Plugin 
  
  + Directory Enumeration 
    - Performing a recursive directory enumeration both with and without session is useful. Always use a good wordlist to maximum results .
    - tools : 
      - ffuf 
        - URL  https://github.com/ffuf/ffuf
        - basic commands : ```ffuf  -u http://host -w wordlist -e extension -X method -recursion -recursion-depth 2```
      - feroxbuster 
        - URL  https://github.com/epi052/feroxbuster
        - basic command : ```feroxbuster -u http://host -x js,html```
      - kiterunner
        - URL  https://github.com/assetnote/kiterunner
        - Kiterunner is a tool that is capable of not only performing traditional content discovery at lightning fast speeds, but also bruteforcing routes/endpoints in modern applications.
        - basic command : ```kr scan hosts.txt -A=apiroutes-210328:20000 -x 5 -j 100 --fail-status-codes 400,401,404,403,501,502,426,411```
        - Detailed articl : https://blog.assetnote.io/2021/04/05/contextual-content-discovery/
      - Source2URl
        - URL https://github.com/danielmiessler/Source2URL
        - Parse source code directories and output list of URLs that are then sent through a proxy.
      - Scavenger 
        - URL https://github.com/0xDexter0us/Scavenger
        - Burp extension to create target specific and tailored wordlist from burp history.
        - Detailed article for creating custome wordlists : 
          - https://blog.dexter0us.com/posts/art-of-fuzzing-and-tailored-wordlist/
          - https://bendtheory.medium.com/finding-and-exploiting-unintended-functionality-in-main-web-app-apis-6eca3ef000af
      - wordlistgen
        - URL  https://github.com/ameenmaali/wordlistgen
        - wordlistgen is a tool to pass a list of URLs and get back a list of relevant words for your wordlists.
        - for example creating wordlist from wayback urls : ```gau apple.com|wordlistgen|sort -u```
      - apkleaks
        - URL  https://github.com/dwisiswant0/apkleaks
        - By Downloading apk files related to our target we can extract endpoints/URLs from this apk files 
        - basic command : ```apkleaks -f file.apk```
      - Diggy 
        - URL https://github.com/s0md3v/Diggy
        - Diggy can extract endpoints/URLs from apk files
        - basic command : ```./diggy.sh file.apk```
      - Also Directory enumeration should based on the used technology , for example iis servers have its own wordlist , asp applications have its own wordlists 
      - Suggested word list for Directory enumeration : 
        - Assetnote wordlist : https://wordlists.assetnote.io/
        - Seclist : https://github.com/danielmiessler/SecLists
        - leakypaths : https://github.com/ayoubfathi/leaky-paths
        - onelistforall: https://github.com/six2dez/OneListForAll
       
  + Wayback History
    - Checking for Wayback History is always a great idea. You can often find URLs that may no longer be available through application workflow but are still accessible.
    - tools and websites : 
      - Wayback Machine  https://archive.org/web/
      - gau https://github.com/lc/gau
      - gauplus https://github.com/bp0lr/gauplus
  
  + Spidering 
    - Crawling Web application for gathering URLs and JavaSript file locations.
    - tools : 
      - gospider 
        - URL  https://github.com/jaeles-project/gospider
        - basic command : ```gospider -s "https://apple.com/" -o output -c 10 -d 1 ```
      - hakrawler 
        - https://github.com/hakluke/hakrawler
        - basic command :  ```echo https://apple.com | hakrawler```
      - burpsuite Crawler 
        - ``` Scan ----> scan type : crawler ----> scan configuration : Crawler Strategy : fastet , never stop crawl due app errors  ----> Resource pool :create new resource pool```
      
  + JavaScript Files 
    - JavaScript files often contain interesting information. Digging into each and every JavaScript file you can get a bunch of useful information from Hardcoded APIs, Secrets (such as AWS Credentials), Information about S3 Buckets, More Subdomains, PII Information, Endpoints, Interesting Parameters, Logics to Bypass certain restrictions like for XSS and Open Redirection, you can do a lot.
    - tools : 
      - JS Link Finder
        - URL https://github.com/PortSwigger/js-link-finder
        - Burp Extension for a passive scanning JS files for endpoint links.
      - xnLinkFinder
        - URL  https://github.com/xnl-h4ck3r/xnLinkFinder
        - This is a tool used to discover endpoints for a given target. It can find them by crawling a target, or get them from a Burp project.
      - subjs
        - URL  https://github.com/lc/subjs
        - subjs fetches javascript files from a list of URLS or subdomain 
        - basic command : ```cat urls.txt | subjs``` 
      - Linkfinder 
        - LinkFinder is a python script written to discover endpoints and their parameters in JavaScript files.
        - basic command : ```python linkfinder.py -i https://example.com/1.js```
      - SecretFinder
        - URL  https://github.com/m4ll0k/SecretFinder   
        - SecretFinder is a python script based on LinkFinder, written to discover sensitive data like apikeys, accesstoken, authorizations, jwt,..etc in JavaScript files. 
        - basic command : ```python3 SecretFinder.py -i https://example.com/1.js -o results.html```
      - Detailed article for Java script Recon https://gist.github.com/m4ll0k/31ce0505270e0a022410a50c8b6311ff-   
        
  + Parameter Discovery
    - Often we will be able to discover parameters that might not be directly visible through the application navigation but are still processed on the server-side. These parameters are often vulnerable to attacks such as SSRF, Open Redirection, XSS, SQLi, IDORs
    - tools :
      - Arjun 
        - URL  https://github.com/s0md3v/Arjun
        - Arjun can find query parameters for URL endpoints.
        - basic command : ```python arjun.py -u https://apple.com -m method ```
      - x8
        - URl https://github.com/Sh1Yo/x8
        - The tool helps to find hidden parameters that can be vulnerable or can reveal interesting functionality
        - basic command : ```x8 -u "https://apple.com/" -w <wordlist>```
      - GAP
        - URL https://github.com/xnl-h4ck3r/burp-extensions
        - burp extension to find more hidden parameter 
        - detailed guide  : ```https://github.com/xnl-h4ck3r/burp-extensions/blob/main/GAP%20Help.md```
      - param-miner
        - URL https://github.com/PortSwigger/param-miner
        - This extension requires Burp Suite 2021.9 or later. To install it, simply use the BApps tab in Burp.
      - ParamSpider
        - URL https://github.com/devanshbatham/ParamSpider
        - Finds parameters from web archives of the entered domain,Finds parameters from subdomains as well.
      - gf 
        - URl https://github.com/tomnomnom/gf
        -  A wrapper around grep to avoid typing common patterns
        -  Sample of gf patterns  :
           - https://github.com/1ndianl33t/Gf-Patterns
           - https://github.com/mrofisr/gf-patterns
       
           
  + Dorking (to be added)        
  + User Enumeration (enumerate emails from linkedin and other resources) (to be added)
  + password spraying (attacking owa & exchange) (to be added)

  + Port Scan
    - First we need to find all the open ports using fast tool like masscan 
      - command : ```masscan -p 0-65535 --rate 5000 --open $IP -oG TARGET_SUBNET.gnmap```
    - get the list of hosts from Masscan’s output, the following command was used
      - command : ```grep "Host:" MASSCAN_OUTPUT.gnmap | cut -d " " -f2 | sort -V | uniq > HOSTS```
    - The command below was used to get the combined list of all open ports detected by Masscan.
      - command : ```grep "Ports:" MASSCAN_OUTPUT.gnmap | cut -d " " -f4 | cut -d "/" -f1 | sort -n | uniq | paste -sd, > OPEN_PORT```
    - run nmap 
      - command : ```sudo nmap -sSV -p OPEN_PORTS -v --open -Pn -n --randomize-hosts -T4 -iL HOSTS -oA OUTPUT```
    - Other tools to perform port scanning :
      - naabu : https://github.com/projectdiscovery/naabu
      - RustScan : https://github.com/RustScan/RustScan
   
