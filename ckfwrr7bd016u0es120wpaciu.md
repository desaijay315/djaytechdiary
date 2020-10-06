## How to secure Node.Js apps: The ultimate guide

NodeJs is the popular server-side scripting language and its one of the widely used in the javascript world. Perhaps you might be using it in your projects.

In this article, we will focus on some of the best practices for securing your web application.

**Here’s the quick rundown with some express/node js modules which help secure your Node.js app:
**

### Crypto

[_Schneier’s Law_](https://www.schneier.com/blog/archives/2011/04/schneiers_law.html) _stated:_

> _Anyone, from the most clueless amateur to the best cryptographer, can create an algorithm that he himself can’t break._

We might not know exactly what your secret algorithm is, and the fact that you yourself cannot break the cypher secret code is irrelevant. You should focus on generating or using the algorithm in such a way that others can't break it.

An attacker may use common tools like [substitution cyphers](https://en.wikipedia.org/wiki/Substitution_cipher) or [polyalphabetic cyphers](https://en.wikipedia.org/wiki/Polyalphabetic_cipher) to recover the plaintext from the ciphertext. Do not use your own crypto, use standard one instead.


[**Bcryp**](https://en.wikipedia.org/wiki/Bcrypt)**t** is one of the widely used and it has been around for quite some time and remains unbroken till date. It is a password hashing function. 

** How Dropbox securely stores your passwords?
**

%[https://blogs.dropbox.com/tech/2016/09/how-dropbox-securely-stores-your-passwords]


### **Keep your dependencies up to date**

Keeping third-party libraries up-to-date and keeping the track of it is quite important, as these libraries may risk your entire application.

### HTTP response headers


```

HTTP/1.1 200 OK
Accept-Ranges: bytes
Access-Control-Allow-Origin: http://www.xyz12.com/api/users
Allow: GET, POST, HEAD
Age: 3600
Cache-Control: public, max-age=86400
Expires: Sat, 01 Aug 2020 08:26:52 GMT
Last-Modified: Sat, 01 Aug 2020 07:26:52 GMT
Server: Apache/2.4.1 (Unix)
Set-Cookie: __cfduid=ddcweqwertyasdfghjkil12mgrdd; expires=Mon, 31-DEC-20 00:00:00 GMT; path=/; domain=xyz12.com; HttpOnly; SameSite=Lax
Vary: *
Content-Length: 2265
Connection: keep-alive


``` 

- The HTTP 200 OK success status response code means that the request has succeeded.
- The Accept-Ranges HTTP header is used by the server to inform about the unit that can be used to define the range.
- The Access-Control-Allow-Origin header indicates whether the response can be shared with requesting code from the given origin.
- The Age header contains the time the object has been in a proxy cache.
- The Cache-Control header holds directives (instructions) for caching in both requests and responses.
- The Expires header contains the date, after which the response is considered stale. If there is Cache-Control header, then the Expires header is ignored
- The Connection header controls if the network connection stays open after the current transaction finishes.

**- Strict-Transport-Security
**

The strict transport security ensures all requests after the first request made to the application are https. We also need to ensure that even the first request is secure and hence we would be using **Helmet package** to configure the HSTS. Even though we secured our session id cookie, we need to ensure all requests go over https even if we add another cookie and forget to set it to Secure, we'll still not be transmitting it in cleartext over an HTTP connection.


**- X-Content-Type-Options
**

X-Content-Type-Options needs to be set as nosniff,  this will protect from MIME sniffing attack and indicate the browser that content-type header should not be changed


[**helmet**](https://github.com/helmetjs/helmet) — It is an express module to set secure HTTP headers.

To install helmet-


```
npm install --save helmet
```

Using helmet middleware in an express app


%[https://gist.github.com/desaijay315/ef7654da39e6999714b22c1bc197e252]



### Use cookies securely

HTTP Cookies are widely used throughout the web to identify the user’s session, store some data on the user's browsers, allowing the webserver to recognize the user as they navigate through the site, and generally contain sensitive data.

Some important points to remember while setting up the cookie - 

- Don’t store sensitive data in a cookie, unless it's required
- Use HttpOnly to mitigate XSS attacks
- Use SameSite to mitigate CSRF attacks
- Use Secure to mitigate MITM attacks. It will let you forbid a cookie to be ever transmitted over simple HTTP.

Install Express session: **express-session** stores session data on the server and id in the cookie itself, and not the session data

```
npm install --save express-session
```

Using express-session middleware 


%[https://gist.github.com/desaijay315/26e7dfc734f757cbc90148810bc861cc]


Using cookie-session middleware

%[https://gist.github.com/desaijay315/f8a355221de3a99b36e5e15406734360]


## Cross-Origin Resource Sharing

The CORS mechanism describes new HTTP headers which provide browsers with a way to request remote URLs only when they have permission. This will be useful when trying to make API Call to a different domain, it will be blocked by the browser feature called same-origin policy.


![cors.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601914664776/Q_k7XRNd5.png)


Using cors middleware

```
npm install --save cors
```


%[https://gist.github.com/desaijay315/fc53fb2f90f79ab1e34608862d1e1814]



## Cross-Site Request Forgery

CSRF is a technique which allows the attacker to do any malicious tasks on behalf of the victim. It exploits a vulnerability in the application where the actual request and the forged request cannot be differentiated. Think, what if the attacker changes the password of your bank account and transfer all your money. Such attacks are as session-hijacking in which a user session is taken over by the attacker. 

Install csurf:

```
npm install --save csurf
```


Using csrf middleware


%[https://gist.github.com/desaijay315/fc9a5709ed6ce27c42d9ede70efb260d]


_Sending the token value back to the view_


%[https://gist.github.com/desaijay315/cec6510df703d9663d6ea884a05cec9f]



### Error Handling

Recommendations in the [node.js](http://nodejs.org/) community are that you pass errors around in callbacks (Because errors only occur for asynchronous operations) as the first argument

Using `async`/`await` you can now get asynchronous control flow as you want:

Handling asynchronous error


%[https://gist.github.com/desaijay315/b4ded4815a1043b57a4b461bbbd91d69]



### Nodejs API Authentication of JWT Tokens

REST(Representational state transfer) is the widely used web architecture as it is flexible and simple to use. Generating jwt tokens for authenticating users to obtain access to resources is one of the secured approaches.

This will enable the use of a token instead of username and password for accessing each resource.

If you want to learn more on authentication, here you go - 

%[https://blog.usejournal.com/handling-authentication-with-nodejs-24fc29265e0f]

### Tools to check the security risk of open-source dependencies

[**Sqreen**](https://www.sqreen.com/) **—** It protects your application from cross-site scripting, SQL injection, MongoDB injection, checks vulnerabilities against all broken files and 3rd party libraries, etc..

[**Snyk**](https://snyk.io/) **—** It helps you find and fix known vulnerabilities in your dependencies

[**Acunetix**](https://www.acunetix.com/) — It helps in the vulnerability of web applications and frameworks like Angular, React, Vue, Ember.

**OWASP** provides the basis for testing the web application. To know more about it and more about web security, kindly visit -

**Learn OWASP and Web Application Security 
**

%[https://djaytechdiary.com/javascript-web-application-security-guide]





If you liked it please leave some likes to show your support. Also, leave your responses below and reach out to me if you face any issues.

Follow me on [Twitter](http://www.twitter.com/beingjaydesai) | Check out my [LinkedIn](https://www.linkedin.com/in/iamjaydesai/) | See my [GitHub](https://github.com/desaijay315)