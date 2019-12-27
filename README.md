# cyber_security_base

This repository is for Cyber Security Base 2019 https://cybersecuritybase.mooc.fi/module-3.1 

In the first course project, your task is to create a web application that has at least five different flaws from the OWASP top ten list (https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf). Starter code for the project is provided on Github at https://github.com/cybersecuritybase/cybersecuritybase-project.

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

Application shows everyone's name and address who is signed up to the event.

Not every user should know who is signed up to the event. Even if that is necessary information for everyone to know, the address should be hidden.

