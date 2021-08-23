## 1. Introduction
> A web application security test focuses only on evaluating the security of a web application. The process involves an active analysis of the application for any weaknesses, technical flaws, or vulnerabilities. Any security issues that are found will be presented to the system owner, together with an assessment of the impact, a proposal for mitigation or a technical solution.

### Testing options:
1. Black-box (Zero Knowledge)
2. Grey-box (Credentials)
3. White-box (Source Code Review)

### Assessments prerequists:
1. VPN Access (if needed)
2. Mulitple user account from each available user role
3. Whitelising from security devices

### Testing Methodology
#### Passive Testing
* Understand the application's logic and explores the application as a user (Manually via the browser).
* Use HTTP proxy such as BurpSuite to check all the HTTP requests and response to know more about (HTTP headers, parameters, cookies, APIs, technology usage/patterns, etc).
* Chack the authentication mechanism implemented (if exist).

#### Active Testing
* Information Gathering
* Configuration and Deployment Management Testing
* Identity Management Testing
* Authentication Testing
* Authorization Testing
* Session Management Testing
* Input Validation Testing
* Error Handling
* Cryptography
* Business Logic Testing
* Client-side Testing
* API Testing

## 2. Information Gathering / Enumeration / Scanning
2.1. Use a search engine to search (Google Dorks) for potentially sensitive information (In external assessment).
```
site:target.com
```
```
site: will limit the search to the provided domain.
inurl: will only return results that include the keyword in the URL.
intitle: will only return results that have the keyword in the page title.
intext: or inbody: will only search for the keyword in the body of pages.
filetype: will match only a specific filetype, i.e. png, or php.
```
2.2. Use Github enumeration
```
"target.com" password
"target.com" secret
"target.com" credentials
"target.com" token
"target.com" config
"target.com" key
"target.com" pass
"target.com" login
"target.com" ftp
"target.com" pwd
"target.com" security_credentials
"target.com" connectionstring
"target.com" JDBC
"target.com" ssh2_auth_password
"target.com" send_keys
"target.com" send,keys
```
2.3. Setup HTTP proxy on the browser for the target application using Burpsuite to identify the application entry points and add the target to the scope.

2.4. Retrieve and Analyze the `robot.txt` files (can be found in target.com/robot.txt). 

2.5. Use Fingerprint Tool such as Nmap perform port scan and service Fingerprinting ```nmap -sV -Pn target.com -oA results ```.

2.6. On the application pages do mouse Right-click and check the HTML pages code and look for any useful information such as framework version, credentials, endpoints or any forgoton comments.

2.7. Check the application HTTP response's headers and look for the technology being used such as ```operating system, Web server, versions..etc``` such as the below HTTP response example.
```
HTTP/1.1 200 OK
Server: nginx/1.17.3
Date: Thu, 05 Sep 2019 17:50:24 GMT
Content-Type: text/html
Content-Length: 117
Last-Modified: Thu, 05 Sep 2019 17:40:42 GMT
Connection: close
ETag: "5d71489a-75"
Accept-Ranges: bytes
```
2.8. Check the application HTTP response if important security headers are missing such as:
* ```Strict-Transport-Security```
* ```Content-Security-Policy```
* ```X-XSS-Protection```

2.9. On Burpsuite perform Content Discovery on the target application to crawl the application contents ```right-click on the target > Engagement tools > Discover content```:

![image](https://user-images.githubusercontent.com/48615614/130413453-a6219224-0561-4a80-a6fd-d1061ad3b218.png)

2.10. On burpsuite right-click on the target and click on `actively scan this host` to launch a web application scanning on the target:

![image](https://user-images.githubusercontent.com/48615614/130415447-c759ca86-98a3-4a54-9497-2bb3c4772aa5.png)

2.11. Perform Directory Brute force to identify more/hiddent endpoints, you can use Burpsuite intruder or other open-source tools, as example:
* ```ffuf -w Documents/wordlist/Common.txt -u https://target.com/FUZZ -c -recursion -recursion-depth 5```
* ```dirb target.com```
