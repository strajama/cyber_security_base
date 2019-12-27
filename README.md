# cyber_security_base

This repository is for [Cyber Security Base 2019](https://cybersecuritybase.mooc.fi/module-3.1) 

In the first course project, your task is to create a web application that has at least five different flaws from the [OWASP top ten list](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf). Starter code for the project is provided on [Github at](https://github.com/cybersecuritybase/cybersecuritybase-project).

For login, use username 'ted' and password 'ted'.

## FLAW 1

### Cross-Site Scripting (XSS)

#### Description

According to OWASP: *XSS flaws occur whenever an application includes untrusted data in a new web page without proper validation or escaping, or updates an existing web page with user-supplied data using a browser API that can create HTML or JavaScript. XSS allows attackers to execute scripts in the victim’s browser which can hijack user sessions, deface web sites, or redirect the user to malicious sites.*

The HttpOnly flag directs compatible browsers to prevent client-side script from accessing cookies. Including the HttpOnly flag in the Set-Cookie HTTP response header helps mitigate the risk associated with Cross-Site Scripting (XSS) where an attacker's script code might attempt to read the contents of a cookie and exfiltrate information obtained. When set, browsers that support the flag will not reveal the contents of the cookie to a third party via client-side script executed via XSS. 


#### Where the flaw is and how to fix it

CyberSecurityBaseProjectApplication-class has customize-method with the line: 

*cntxt.setUseHttpOnly(false);*

Remove that or set **false** to **true** so HttpOnly-cookie will be used. When you HttpOnly-cookie is used, it tells the browser that this particular cookie should only be accessed by the server. Any attempt to access the cookie from client script is strictly forbidden. An HttpOnly cookie cannot be accessed by client-side APIs, such as JavaScript. This restriction eliminates the threat of cookie theft via cross-site scripting (XSS).

Also, in done.html there is name and address shown with unescaped text with tag "th:utext". This should be changed to "th:text" or you can inject JavaScript to the page.


## FLAW 2

### Broken Authentication

#### Description

According to OWASP: *Application functions related to authentication and session management are often implemented incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities temporarily or permanently.*

#### Where the flaw is and how to fix it

Application has many broken authentication flaws, for example:
1) There is no multi-factor authentication.
2) Application permits default, weak, or well-known passwords.
3) Application permits brute force or other automated attacks.
4) Application uses plain text password.

First three need more work to fix so only the last one is fixed here.

In SecurityConfiguration-class, uncomment next line in configureGlobal-method:

*auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());*

Now the password isn't shown as plain text.


## FLAW 3

### Sensitive Data Exposure

#### Description

According to OWASP: *Many web applications and APIs do not properly protect sensitive data, such as financial, healthcare, and PII. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity theft, or other crimes. Sensitive data may be compromised without extra protection, such as encryption at rest or in transit, and requires special precautions when exchanged with the browser.*

#### Where the flaw is and how to fix it

Application shows everyone's name, address and birthday who is signed up to the event.

Not every user should know who is signed up to the event. This information is something that only users with admin roles can access. 

Address and birthday are sensitive data that shouldn't be asked and stored if there isn't special need for that.

List in done.html should be removed and Sign in -info should be changed so that address and birthday aren't needed.


## FLAW 4

### Security Misconfiguration

#### Description

According to OWASP: *Security misconfiguration is the most commonly seen issue. This is commonly a result of insecure default configurations, incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing sensitive information. Not only must all operating systems, frameworks, libraries, and applications be securely configured, but they must be patched and upgraded in a timely fashion.*

#### Where the flaw is and how to fix it

All the Security Headers in Spring are disabled in SecurityConfiguration-class. Also CSRF-token is disabled which makes it ppossible to do CSRF-attack (Cross-Site Request Forgery).


## FLAW 5

### Broken Access Control

#### Description

According to OWASP: *Restrictions on what authenticated users are allowed to do are often not properly enforced. Attackers can exploit these flaws to access unauthorized functionality and/or data, such as access other users' accounts, view sensitive files, modify other users’ data, change access rights, etc.*

#### Where the flaw is and how to fix it

In CustomUserDetailsService-class you can see that there are two different user roles (USER and ADMIN), but in SecurityConfiguration-class you can see that after authentication user can do anything and roles aren't used. 

At least uncomment *.antMatchers("/h2-console/*").hasAnyAuthority("ADMIN")* so database is not accessed by every user.


## FLAW 6

### Insufficient Logging & Monitoring

#### Description

According to OWASP: *Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response, allows attackers to further attack systems, maintain persistence, pivot to more systems, and tamper, extract, or destroy data. Most breach studies show time to detect a breach is over 200 days, typically detected by external parties rather than internal processes or monitoring. *

#### Where the flaw is and how to fix it

This flaw is quite obvious, because there is no logging or monitoring implemented in application to detect any kind of attack. There should be at least some monitoring and alerting for logins and access control failures.

