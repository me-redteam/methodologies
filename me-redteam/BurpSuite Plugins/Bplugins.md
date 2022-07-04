# Burp Suite Extensions 
---
 BurpSuite Plugins 
---
  + Content Type Converter

    + Introduction : 
      - This extension converts data submitted within requests between various common formats:
        - JSON To XML
        - XML to JSON
        - Body parameters to JSON
        - Body parameters to XML
      - This is useful for discovering vulnerabilities that can only be found by converting the content type of a request. For example, if an original request submits data using JSON, we can attempt to convert the data to XML, to see if the application accepts data in XML form. If so, we can then look for vulnerabilities like XXE injection which would not arise in the context of the original JSON endpoint. It might also be possible to find vulnerabilities behind web application firewalls or other filters that assume the incoming data is in a specific format, while the application tolerates data in other formats.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Right Click on the required request 
      - Extensions >>> Content type converter
---
  + BurpJSLinkFinder
  
    + Introduction : 
      - Burp Extension for a passive scanning JS files for endpoint links.
        - Export results the text file
        - Exclude specific 'js' files e.g. jquery, google-analytics
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Once you've loaded the plugin , it will	work in the background on the identified scope 

---  
  + Js Miner
  
    + Introduction : 
      - This tool tries to find interesting stuff inside static files; mainly JavaScript and JSON files.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - The extension includes two differnet modes : 
        - Passive : Once you've loaded the plugin , it will	work in the background on the identified scope \
        - Active : 
          - Right Click on the required request 
          - Extensions >>> Js Miner >>> Run JS Auto-Mine
 ---
  + Param Miner
 
    + Introduction : 
      - This extension identifies hidden, unlinked parameters. It's particularly useful for finding web cache poisoning vulnerabilities.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - The extension includes two differnet modes : 
        - Passive : Once you've loaded the plugin , it will	work in the background on the identified scope \
        - Active : 
          - Right Click on the required request
          - Extensions >>> Param Miner >>> Guess (cookies|headers|params)
---
  + GAP
  
    + Introduction : 
      - This is a python extension that runs in Portswigger's Burp Suite and parses an already crawled sitemap to build a custom parameter list. It also adds common parameter names that could be useful in the final list used for fuzzing ,also finds potential links to try these parameters on.
    + Installation Steps :
      - Get GAP.py (https://github.com/xnl-h4ck3r/burp-extensions/blob/main/GAP.py)
      - Point Burp Suite to the Jython .jar file in Extender > Options > Python Environment
      - On Extensions tab, click Add
      - Set Extension type to Python and select the GAP.py file
    + How To Use : 
      - On the Target >>> Site map >>> right click on the request or on all the target you want enumerate
      - Extensions >>> GAP
    + for more details about the extension modes kindly check : 
      https://github.com/xnl-h4ck3r/burp-extensions/blob/main/GAP%20Help.md
---
  + Wsdler

    + Introduction : 
      - This extension takes a WSDL request, parses out the operations that are associated with the targeted web service, and generates SOAP requests that can then be sent to the SOAP endpoints.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Right Click on the wsdl file request >>> Extender >>> Parse-wsdl
---
  + WAF Detect

    + Introduction : 
      - This extension passively detects the presence of a web application firewall (WAF) from HTTP responses.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Once you've loaded the plugin , it will	work in the background on the identified scope 
---
  + HTTP Request Smuggler
 
    + Introduction : 
      - This is an extension for Burp Suite designed to help you launch HTTP Request Smuggling attacks. It supports scanning for Request Smuggling vulnerabilities, and also aids exploitation by handling cumbersome offset-tweaking for you.

    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Right Click on the request >>> Extensions >>> HTTP Request Smuggler >>> HTTP Request Smuggler >>> Smuggle probe
--- 
  + Autorize

    + Introduction :
      - Autorize is an extension aimed at helping the penetration tester to detect authorization vulnerabilities, one of the more time-consuming tasks in a web application penetration test.
        It is sufficient to give to the extension the cookies of a low privileged user and navigate the website with a high privileged user. The extension automatically repeats every request with the session of the low privileged user and detects authorization vulnerabilities.
        It is also possible to repeat every request without any cookies in order to detect authentication vulnerabilities in addition to authorization ones.

    + Installation Steps : 
      - You can intsall it from burp extende

    + How to Use :
      - After installation, the Autorize tab will be added to Burp.
      - Open the configuration tab (Autorize -> Configuration).
      - Get your low-privileged user authorization token header (Cookie / Authorization) and copy it into the textbox containing the text "Insert injected header here". Note: Headers inserted here will be replaced if present or added if not.
      - Uncheck "Check unauthenticated" if the authentication test is not required (request without any cookies, to check for authentication enforcement in addition to authorization enforcement with the cookies of low-privileged user)
      - Check "Intercept requests from Repeater" to also intercept the requests that are sent through the Repeater.
      - Click on "Intercept is off" to start intercepting the traffic in order to allow Autorize to check for authorization enforcement.
      - Open a browser and configure the proxy settings so the traffic will be passed to Burp.
      - Browse to the application you want to test with a high privileged user. 
      - The Autorize table will show you the request's URL and enforcement status.
      - It is possible to click on a specific URL and see the original/modified/unauthenticated request/response in order to investigate the differences.
      - Also check below URL : 
        - https://blog.yeswehack.com/yeswerhackers/pimpmyburp-pwnfox-autorize-find-idor/
  ---
  + Burp Bounty

    + Introduction : 
      - Burp Suite extension allows you, in a quick and simple way, to improve the active and passive burpsuite scanner by means of personalized rules through a very intuitive graphical interface. Through an advanced search of patterns and an improvement of the payload to send, we can create our own issue profiles both in the active scanner and in the passive.
    + Installation Steps :
      - You can intsall it from burp extender
      - Also there is another PRO version from this extension , check from : https://burpbounty.net/burp-bounty-pro-vs-free/
    + How To Use : 
      - For detailed Usage kindly check :
        https://github.com/wagiro/BurpBounty/wiki/usage
---
  + Turbo Intruder

    + Introduction : 
      - Turbo Intruder is a Burp Suite extension for sending large numbers of HTTP requests and analyzing the results. It's intended to complement Burp Intruder by handling attacks that require extreme speed or complexity.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - In the request highlight the area you want to inject over
      - Right click >>> Extensions >>> Send to Turbo Intruder
      - New window will be opened containing a Python snippet which you can customise before launching the attack.
      - Also there are differnet scripts in drop down menu could be used for differnet types of attack , also you can create your own scripts 
---
  + JSON Web Tokens

    + Introduction : 
      - Extension lets you decode and manipulate JSON web tokens on the fly, check their validity and automate common attacks.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - If the request contains jwt token you will find new tab calles "JWS" and "JSON WEB TOKENS"
      - Press on "JSON WEB TOKENS" you will find the decoded token , also you can edit the token or launch some of predefined attacks 
---
  + 403 Bypasser

    + Introduction : 
      - A Burp Suite extension made to automate the process of bypassing 403 pages
    + Installation Steps :
      - Download the extension from "https://github.com/Gilzy/403Bypasser/tree/8a821dcf11c7f2fb9974361cbb16072b51b7cf15"
      - Point Burp Suite to the Jython .jar file in Extender > Options > Python Environment
      - On Extensions tab, click Add
      - Set Extension type to Python and select the 403Bypasser.py file
      Kindly note there is another version availabe on burp extender doesn't support auto detection
    + How To Use : 
      - 1-Once you've loaded the plugin , it will	work in the background on the identified scope 
      - Payloads are supplied by the user under query payloads.txt and header payloads.txt 
  ---
  + HackBar

    + Introduction : 
      - Hackbar is a plugin designed for the penetration tester in order to help them to speed their manual testing procedures.  which contains a number of dictionaries according to the vulnerability type whether its SQL Injection, Cross-Site Scripting, or URL Redirections. 
        The Burp’s Hack Bar is a Java-based Burpsuite Plugin which helps the pen-testers to insert any payload by opting from a variety of different dropdown lists.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Right-click on a request and then Send to Repeater/Intruder
      - Find the location you want to insert the payload
      - Right click >>> Extensions >>> HackBar ,Payload Bucket
      - Select Attack >>> Select Payload 
      - Modify the payload if you need to and Send your request
  ---
  + J2EEScan 

    + Introduction : 
      - The goal of this extension is to improve the test coverage during web application penetration tests on J2EE applications. J2EEScan adds more than 80+ unique security test cases and new strategies to discover different kind of J2EE vulnerabilities.
    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - Just run normal active scan
   ---
  + HTML 5 Auditor

    + Introduction : 
      - This extension checks for usage of HTML5 features that have potential security risks, including:
        - client side storage
        - client geo-location
        - HTML5 client caches
        - web sockets
    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - Once you've loaded the plugin , it will	work in the background on the identified scope 
   ---
  + PHP Object Injection 

    + Introduction : 
      - This extension adds an active scan check to find PHP object injection vulnerabilities.
    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - Once you've loaded the plugin , Run active scan
   ---
  + NGINX alias traversal
 
    + Introduction : 
      - This extension detects NGINX alias traversal due to misconfiguration. For more information kindly check : https://i.blackhat.com/us-18/Wed-August-8/us-18-Orange-Tsai-Breaking-Parser-Logic-Take-Your-Path-Normalization-Off-And-Pop-0days-Out-2.pdf
    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - Once you've loaded the plugin , Run active scan
  ---
  + OAUTHScan

    + Introduction :
      - OAUTHScan is a Burp Suite Extension written in Java with the aim to provide some automatic security checks, which could be useful during penetration testing on applications implementing OAUTHv2 and OpenID standards.
    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - OAUTHScan is fully integrated with the Burp Scanner, after installed on Burp you have only to launch Passive or Active scans on your targeted request.
  ---
  + taborator

    + Introduction :
      - A Burp extension to show the Collaborator client in a tab along with the number of interactions in the tab name.
    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - right click in a repeater >>> Extension >>> Taborator >>> Insert Collaborator payload.
  
  ---
  
  + Upload Scanner

    + Introduction :
      - A Burp Suite Pro extension to do security tests for HTTP file uploads.

    + Installation Steps : 
      - You can intsall it from burp extender
    + How to Use :
      - Below are some videos that explain different use cases for using the plugin :
        - https://www.modzero.com/share/uploadscanner/UploadScanner_101_Basics.mp4
        - https://www.modzero.com/share/uploadscanner/UploadScanner_102_FlexiInjector.mp4
        - https://www.modzero.com/share/uploadscanner/UploadScanner_103_context_menu.mp4
        - https://www.modzero.com/share/uploadscanner/UploadScanner_106_Fingerping_and_DoS.mp4
        - https://www.modzero.com/share/uploadscanner/UploadScanner_104_recursive_upload_and_fuzzer.mp4
        - https://www.modzero.com/share/uploadscanner/UploadScanner_105_ReDownloader.mp4

  ---

  + Active Scan ++

    + Introduction : 
      - ActiveScan++ extends Burp Suite's active and passive scanning capabilities. Designed to add minimal network overhead,.
    + Installation Steps :
      - You can intsall it from burp extender
    + How To Use : 
      - Just Run normal active scan 
  ---
  + reflected parameters

    + Introduction :
      - This extension monitors traffic and looks for request parameter values (longer than 3 characters) that are reflected in the response
    + Installation Steps : 
      - You can intsall it from burp extende
    + How to Use :
      - Once you've loaded the plugin , it will	work in the background on the identified scope 
 
  ---
