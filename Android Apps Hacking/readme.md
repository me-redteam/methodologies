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
