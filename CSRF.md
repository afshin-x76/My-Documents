# CSRF

Cross-site request forgery, also known as one-click attack or session riding, is a type of malicious exploit of a website where unauthorized commands are submitted from a user that the web application trusts. all those commands can work without the user's interaction or even knowledge.

In CSRF attack, attacker goal is to cause a victim to submit a web request to a website that the victim has privileged access to. This web request can be crafted to include URL parameters, cookies and other data that appear normal to the web server processing the request. At risk are web applications that perform actions based on input from trusted and authenticated users without requiring the user to authorize the specific action.at risk are applications that perform actions base on request from trusted users and without requiring user to authorize some actions.

those users that are authorized to website by a cookie saved in web browsers could unknowingly send HttpRequests to that sites.

web browsers automatically include cookies in any request to those sites that generate and sent cookies for us. this property is exploited by CSRF attacks.

to make a CSRF attack work, attacker must identify a reproducible web request that execute specific action. Once such a request is identified, a link can be created that generates this malicious request and that link can be embedded on a page within the attacker's control.

CSRF commonly has the following characteristics:

- It involves sites that rely on a user's identity.
- It exploits the site's trust in that identity.
- It tricks the user's browser into sending HTTP requests to a target site.
- It involves HTTP requests that have side effects.

### History

CSRF vulnerabilities have been known and in some cases exploited since 2001. Because it is carried out from the user's IP address, some website logs might not have evidence of CSRF. there were few well-documented examples:

- The Netflix website in 2006 had numerous vulnerabilities to CSRF, which could have allowed an attacker to perform actions such as adding a DVD to the victim's rental queue, changing the shipping address on the account, or altering the victim's login credentials to fully compromise the account.
- The online banking web application of ING Direct was vulnerable to a CSRF attack that allowed illicit money transfers.
- Popular video website YouTube was also vulnerable to CSRF in 2008 and this allowed any attacker to perform nearly all actions of any user.

### Forging Login Request

An attacker may forge a request to log the victim into a target website using the attacker's credentials; this is known as login CSRF. Login CSRF makes various novel attacks possible; for instance, an attacker can later log into the site with his legitimate credentials and view private information like activity history that has been saved in the account.

### HTTP verbs and CSRF

the protective measures against an attack depend on the method of the HTTP request.

- Http GET should be used as a safe method, that is, not significantly changing user's state in the application. Applications using GET for such operations should switch to HTTP POST or use anti-CSRF protection.
- In simplest form of POST with data encoded as a query string (field1=value1&field2=value2) CSRF attack is easily implemented using a simple HTML form and anti-CSRF measures must be applied.
- If data is sent in any other format (JSON, XML) a standard method is to issue a POST request using XMLHttpRequest with CSRF attacks prevented by Same-origin policy (SOP) and Cross-origin resource sharing (CORS).
- other HTTP methods (PUT, DELETE etc.) can only be issued using XMLHttpRequest with Same-origin policy (SOP) and Cross-origin resource sharing (CORS) and preventing CSRF.

> In particular, the convention has been established that the GET and HEAD methods SHOULD NOT have the significance of taking an action other than retrieval. These methods ought to be considered "safe". This allows user agents to represent other methods, such as POST, PUT and DELETE, in a special way, so that the user is made aware of the fact that a possibly unsafe action is being requested.

### Limitation

Several things have to happen for cross-site request forgery to succeed:

1. The attacker must target a site that doesn't check the referrer header or a victim with a browser or plugin that allows referer spoofing.
2. The attacker must find a form submission at the target site, or a URL that has side effects, that does something.
3. The attacker must determine the right values for all the forms or URL inputs.
4. The attacker must lure the victim to a web page with malicious code while the victim is logged into the target site.

### Prevention

#### Synchronizer token pattern

STP is a technique where a token, secret and unique value for each request, is embedded by the web application in all HTML forms and verified on the server side.

![image](https://miro.medium.com/max/1000/1*FaeYWDkLDYLDbclSijvGyA.jpeg)

#### Cookie-to-header Token

- On an initial visit without an associated server session, the web application sets a cookie which is scoped appropriately so that it should not be provided during cross-origin requests.
- JavaScript operating on the client side reads its value and copies it into a custom HTTP header sent with each transactional request
- The server validates presence and integrity of the token
- Security of this technique is based on the assumption that only JavaScript running on the client side of an HTTPS connection to the server that initially set the cookie will be able to read the cookie's value. 

#### Double Submit Cookie

![image](https://miro.medium.com/max/700/1*XeUrcIMGQKEaLbGNruqODA.png)

a site can set a CSRF token as a cookie, and also insert it as a hidden field in each HTML form. When the form is submitted, the site can check that the cookie token matches the form token. The same-origin policy prevents an attacker from reading or setting cookies on the target domain, so they cannot put a valid token in their crafted form.

#### SameSite cookie attribute

An additional "SameSite" attribute can be included when the server sets a cookie, instructing the browser on whether to attach the cookie to cross-site requests. If this attribute is set to "strict", then the cookie will only be sent on same-site requests, making CSRF ineffective.
















