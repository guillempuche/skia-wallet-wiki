# Security of the Skia Wallet


## Understand the security risks of the browsers

### List of security issues

Here you've a list of common attacks:
- Cross Site Scripting or XSS [link 1](https://owasp.org/www-community/attacks/xss/), [link 2](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- Cross Site Request Forgery or [CSRF](https://owasp.org/www-community/attacks/csrf)
- More attacks on OWASP [list](https://owasp.org/www-community/attacks/), [cheat sheets](https://cheatsheetseries.owasp.org)
https://github.com/cure53/DOMPurify/wiki/Security-Goals-&-Threat-Model

### Security issues when storing the user's mnemonic key on the browser

Here you'll discover how Skia Wallet saves the mnemonic key of the persons on the browsers.
The company [Auth0](https://auth0.com/) is recognized for offering to the software developers an easy way to implement, adaptable authentication and authorization for their apps and websites.

They wrote an article "[Secure Browser Storage: The Facts](https://auth0.com/blog/secure-browser-storage-the-facts/)" in 2021 storing data and attacking the code with the common security issues (). The next popular storage techniques were attacked:

- Local Storage. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).
- Session Storage. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage).
- Cookies.
- In memory.

## Protect against security risks

### General best practices
- Create a Content Security Policy or CSP. A [Cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html) from OWASP.

#### Prevent Cross Site Scripting (XSS)
As detailed in "[Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#xss-prevention-rules-summary)" by OWASP, "_For XSS attacks to be successful, an attacker needs to insert and execute malicious content in a webpage. Each variable in a web application needs to be protected. Ensuring that all variables go through validation and are then escaped or sanitized is known as perfect injection resistance. Any variable that does not go through this process is a potential weakness. Frameworks (like React) make it easy to ensure variables are correctly validated and escaped or sanitized. However, frameworks aren't perfect and security gaps still exist in popular frameworks like React. Output Encoding and HTML Sanitization help address those gaps._"

Solutions to XSS according to OWASP:
![image](https://user-images.githubusercontent.com/3519924/173188360-2d8ebc06-071a-4c58-966e-52f8e9c374cc.png)

### Secure the storage of the mnemonic key
There's another technology that browsers supports called **Web Workers** that guarantee that the data remains inside a Web Worker and cannot be accessed from outside, like an attacker.

#### Web Workers

Benefits over the previous Web techniques:
- Supported by all major mobile and desktop browsers. [Can I Use][can-i-use-web-workers] shows the compatibility table and supported by ~96% of browser's user in the world.
- It prevents third-party code from accessing mnemonic key and HTTP-request interceptions. Since third-party codes run on the main thread, they cannot intercept requests initiated by the web workers. Yes, when we store access tokens in web workers, API requests needing those access tokens should also be initiated from the web workers.

Some concerns of people (it contains technical jargon):
- [HTML5 Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html#web-workers) by OWASP
  - _Web Workers are allowed to use XMLHttpRequest object to perform in-domain and Cross Origin Resource Sharing requests. See relevant section of this Cheat Sheet to ensure CORS security._
  - _While Web Workers don't have access to DOM of the calling page, malicious Web Workers can use excessive CPU for computation, leading to Denial of Service condition or abuse Cross Origin Resource Sharing for further exploitation. Ensure code in all Web Workers scripts is not malevolent. Don't allow creating Web Worker scripts from user supplied input._
  - _Validate messages exchanged with a Web Worker. Do not try to exchange snippets of JavaScript for evaluation e.g. via eval() as that could introduce a [DOM Based XSS](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html) vulnerability._
- Questions about security risks on [Stack Overflow](https://stackoverflow.com/search?q=security+web+worker&searchOn=3)
  - [Answer](https://stackoverflow.com/a/67118606/9192954): "_the MessagePort interface is 100% client-side. A compromised machine could have a malicious script read directly in its RAM and derive the sensitive information from there, but once again, if they can do it... they can certainly access that token by means a lot simpler._".
- [Mozilla Security Review](https://wiki.mozilla.org/Firefox3.1/Web_Workers_Security_Review) in 2008:
  - _Workers execute in a tightly controlled sandbox._
  - _No access to Components or other global JS components._
  - _Only basic JS (Math, Date, etc.), timeouts, XHR, and importScripts._
  - _No pref dependencies yet, maybe will provide one to customize the number of OS threads allowed._
  - _Script loading is subject to the same restrictions as on the main thread (content policies, same origin restrictions, etc.)._
  - _XHR uses the same code as the main thread._

## Developer tools for security analyzes int

Skia Wallet uses in the development workflow: 
- [Snyk](https://snyk.io/) to "_find and fix security vulnerabilities in code and dependencies_".

## References

- [MDN][mdn-web-workers]
- [HTML Living Standard][whatwg-org]
- [Can I Use][can-i-use-web-workers]



[mdn-web-workers]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers
[whatwg-org]: https://html.spec.whatwg.org/multipage/workers.html
[can-i-use-web-workers]: https://caniuse.com/webworkers