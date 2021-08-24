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
* Business Logic Testing
* Client-side Testing
* Error Handling

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

2.14. Check if `OPTIONS` HTTP method has a risky HTTP method enabled such as `PUT or DELETE`:

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

## 5. Session Management Testing
5.1. Identify actual session cookie out of bulk cookies in the application, as most of the application has many cookies, so you need to identify which one is the session cookie.

5.2 Decode cookies using some standard decoding algorithms such as `Base64`, `hex`, `URL`, etc, you can use Burpsuite decoder tool.

5.3 Modify cookie session token value by 1 bit/byte. Then resubmit and do the same for all tokens. Reduce the amount of work you need to perform in order to identify which part of the token is actually being used and which is not

5.4. Check for session cookies and cookie expiration date/time.

5.5. Check for `HttpOnly` and `Secure` (if the application is over SSL) flags in cookie.

5.6. Check if any user information is stored in cookie value or not If yes, tamper it with other user's data so you can try to escalate your privileges.

5.7. Test the session Fixation, to avoid seal user session (session Hijacking), this can be done to check if you get a new cookie session after you login successfully and its different from the one you had before login. 


## 6. Input Validation Testing


## 7. Business Logic Testing

## 8. Error Handling


## 9. Other Test Cases (All Categories)




