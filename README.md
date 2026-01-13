# Detecting-Cross-Site-Scripting-XSS-Attacks-


## Objective

Cross-Site Scripting (XSS) is a client-side attack that occurs when a web application fails to sanitize user input, allowing attackers to inject and execute malicious JavaScript in a victim’s browser. As a SOC analyst, my job will be to detect XSS by identifying suspicious JavaScript payloads in HTTP requests, recognizing URL-encoded attack patterns, and correlating request frequency and user-agent behavior to distinguish automated scanning from legitimate user activity. Tools like Wireshark help analyze raw network traffic to identify injection attempts.


What is XSS Really? 
Cross-site Scripting (XSS) happens when 
- Website trusts user inputs
- then sends it back to the browser without cleaning it
- This allows JacaScript to run in the victim's browser.

  XSS does not attack the server directly. It attacks users through the website. When the website becomes a delivery truck for the attackers' JavaScript.

Developers sometimes 
- Don't sanitize input correctly
- Disable built-in protections
- Ise Outdated libraries
- Or legacy or custom-built apps often skip protections entirely

  3 Types of XSS
  1) Reflected XSS (Most Common)
     -Payloas is in the request
     - Server reflects it immediately in the response
     - Usually delivered via link
    
       EXAMPLE: https://site.com/search?query=<script>alert(1)</script>


  2) Stored XSS (Dangerous)
     - Payload is stored in the database
     - Affects every user who loads the page
       EXAMPLES: Malicious script stored in
       - Blog comments
       - User Profiles
       - Forum posts
      
    3) DOM-Based XSS
       - Happens entirely in the browser
       - JavaScript modifies the page in unsafe ways
       - Server logs may show nothing suspicious


    How does it work? 
- <img width="386" height="84" alt="XSS-vulnerable-code" src="https://github.com/user-attachments/assets/e556d8e1-3ab3-490a-a339-184a9f234527" />


This displays whatever is entered in the 'user' parameter. If we enter 'LetsDefend' as the 'user' parameter, we will see the words "hello LetsDefend". 
<img width="656" height="108" alt="XSS-image-1" src="https://github.com/user-attachments/assets/dd0df2f3-68a0-442e-8be4-1b2c5d77291f" />

The 'user' parameter is included directly in the HTTP response. The JavaScript code we wrote worked. 


What if the threat actor wants to redirect the user to a malicious site?

'https://letsdefend.io/xss_example.php?user=%3Cscript%3Ewindow.location=%27https://google.com%27%3C/script%3E'

The victim will click on the link and not realize they might have been redirected to a malicious website that appears to be Google, but requests login credentials, allowing the threat actor to obtain their information. 


As a SOC Analyst, I might have to question myself, "Is someone trying to inject JavaScript through user input?"

Red Flags
- JavaScript keywords in URLS or form data
- URL-encoded payloads, for example, (%3Cscript%3E)
- Repeated requests with slightly different payloads
- Automated tools (User-agent clues)
- High request frequency (every few seconds)


For Example: 
"<script>
alert(
prompt(
confirm(
onerror=
onload=
document.cookie
window.location
  < > " ' ( ) ;
%3C  %3E  %22  %27"
  
While learning Filter Keywords in wireshark while investiagting I can start with "http." 

I can search for Script keywords
"http.request.uri contains "script"
"http.request.uri contains "alert"
"http.request.uri contains "onerror"

Reading about common URL endoing I can also look for 
"http.request.uri contains "%3C"
"http.request.uri contains "%3E"
"http.request.method == "POST"




