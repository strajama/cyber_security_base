# cyber_security_base

This repository is for Cyber Security Base 2019 https://cybersecuritybase.mooc.fi/module-3.1 

## FLAW 1

### Cross-Site Scripting (XSS)

#### Description

According to OWASP, XSS flaws occur whenever an application includes untrusted data in a new web page without proper validation or escaping, or updates an existing web page with user-supplied data using a browser API that can create HTML or JavaScript. XSS allows attackers to execute scripts in the victim’s browser which can hijack user sessions, deface web sites, or redirect the user to malicious sites.


The HttpOnly flag directs compatible browsers to prevent client-side script from accessing cookies. Including the HttpOnly flag in the Set-Cookie HTTP response header helps mitigate the risk associated with Cross-Site Scripting (XSS) where an attacker's script code might attempt to read the contents of a cookie and exfiltrate information obtained. When set, browsers that support the flag will not reveal the contents of the cookie to a third party via client-side script executed via XSS. 


#### Where the flaw is and how to fix it

CyberSecurityBaseProjectApplication-class has customize-method with the line **cntxt.setUseHttpOnly(false);**

Remove that or set *false* to *true* so HttpOnly -cookie will be used.
