# Security of the Skia Wallet


## Understand the security risks of the browsers

### List of security issues

Here you've a list of common attacks:
- Cross-Site Scripting or XSS [link 1](https://owasp.org/www-community/attacks/xss/), [link 2](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- Cross-Site Request Forgery or [CSRF](https://owasp.org/www-community/attacks/csrf)
- More attacks on OWASP [list](https://owasp.org/www-community/attacks/), [cheat sheets](https://cheatsheetseries.owasp.org)


### Security issues when storing the user's mnemonic key on the browser

Here you'll discover how Skia Wallet saves the mnemonic key of the persons on the browsers.
The company [Auth0](https://auth0.com/) is recognized for offering to the software developers an easy way to implement, adaptable authentication and authorization for their apps and websites.

They wrote an article "[Secure Browser Storage: The Facts][auth0-storage]" in 2021 storing data and attacking the code with the common security issues (). The next popular storage techniques were attacked:

- Local Storage. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).
- Session Storage. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage).
- Cookies.
- In memory.

## Protect against security risks

### Basic best practices

A long list by [OWASP][owasp-cheatsheets] of the most common attacks and how to prevent them.

Skia Wallet team has to work on the next areas:
- [ ] **[To do]** Cross-Site Scripting or [XSS](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#xss-prevention-rules-summary).
- [ ] **[To do]** Cross-Site Request Forgery or [CSRF](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [ ] **[To do]** [Sanitize the inputs](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) with the Javascript library [DOMPurify](https://github.com/cure53/DOMPurify) (its goals [here](https://github.com/cure53/DOMPurify/wiki/Security-Goals-&-Threat-Model)). It's a common issue on multiple type of attacks.
- [ ] **[To do]** Set up the [HTML headers](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html): `X-Frame-Options`, `X-XSS-Protection`, `X-Content-Type-Options`, `Referrer-Policy`, `Content-Type`, `Strict-Transport-Security`, `Content-Security-Policy`, `Access-Control-Allow-Origin`, `Cross-Origin-Opener-Policy`, `Cross-Origin-Resource-Policy`, `Cross-Origin-Embedder-Policy`, remove `X-Powered-By`.
- [ ] **[To do]** Use Two-Factor Authentication (2FA) and verifiers for Skia Wallet team members with access to key feature: the codebase (2FA on [Github](https://docs.github.com/en/enterprise-cloud@latest/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication), GPG on [Github](https://docs.github.com/en/enterprise-cloud@latest/authentication/managing-commit-signature-verification/about-commit-signature-verification))

Secondary:
- [ ] [CSS](https://cheatsheetseries.owasp.org/cheatsheets/Securing_Cascading_Style_Sheets_Cheat_Sheet.html)
- [ ] Javascript package manager [NPM](https://cheatsheetseries.owasp.org/cheatsheets/NPM_Security_Cheat_Sheet.html)
- [ ] [NodeJS](https://cheatsheetseries.owasp.org/cheatsheets/Nodejs_Security_Cheat_Sheet.html)

### Secure the storage of the mnemonic key
There's another technology that browsers supports called **Web Workers** that guarantee that the data remains inside a Web Worker and cannot be accessed from outside, like an attacker.

#### Web Workers

"_Web workers enable developers to benefit from parallel programming in JavaScript. Parallel programming allows applications to run different computations at the same time._" according to [Auth0](https://auth0.com/blog/speedy-introduction-to-web-workers/). "_Web Workers are a simple means for web content to run scripts in background threads. The worker thread can perform tasks without interfering with the user interface._" by [MDN][mdn-web-workers]

![image](https://user-images.githubusercontent.com/3519924/173195144-02b344ce-52c8-45ff-ad47-95c3395cd92f.png)
_Image from [Auth0]_

"_The Worker interface spawns real OS-level threads, and mindful programmers may be concerned that concurrency can cause effects in your code if you aren't careful. However, since web workers have carefully controlled communication points with other threads, it's actually very hard to cause concurrency problems. There's no access to non-threadsafe components or the DOM. And you have to pass specific data in and out of a thread through serialized objects. So you have to work really hard to cause problems in your code._ by [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers#about_thread_safety)

"_Web Workers can run JavaScript code in a background thread separate from the main execution thread of the JS frontend application. They communicate with the frontend application via a channel called MessageChannel. Specifically, the application can send a message to the Web Worker to perform some action via the MessageChannel. The Web Worker will perform the action and send back to the application the needed information._" by [Auth0][auth0-storage]. "_With Web Workers, the secret is available in the isolated JavaScript code of the Web Worker. If JS frontend code needs access to the secret, the Web Worker implementation is the only one satisfying the requirement while preserving the secret confidentiality._".

[Bruce Wilson](https://blog.logrocket.com/using-webworkers-for-safe-concurrent-javascript-3f33da4eb0b2/) says "_any data that is passed via `postMessage()`_" (the method used by Web Worker to communicate with the main thread) "_is copied before it is passed, so changing the data in the main window thread does not result in changes to the data in the worker thread. This provides inherent protection from conflicting concurrent changes to data that’s passed between main thread and worker thread_".

Benefits of Web Workers over the previous storing web techniques:

- Supported by all major mobile and desktop browsers. [Can I Use][can-i-use-web-workers] shows the compatibility table and supported by ~96% of browser's user in the world.
- It prevents third-party code from accessing mnemonic key and HTTP-request interceptions. Since third-party codes run on the main thread, they cannot intercept requests initiated by the web workers. Yes, when we store access tokens in web workers, API requests needing those access tokens should also be initiated from the web workers.

Some concerns found on the documentation and Internet:

- [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers#content_security_policy) "_Workers are considered to have their own execution context, distinct from the document that created them. For this reason they are, in general, not governed by the content security policy of the document (or parent worker) that created them. [...] To specify a content security policy for the worker, set a [Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) response header for the request which delivered the worker script itself._"
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


##### References

- [List of attacks and their solutions][owasp-cheatsheet] by OWASP
- [MDN][mdn-web-workers]
- [HTML Living Standard][whatwg-org]
- [Can I Use][can-i-use-web-workers]

## Developer tools for security analyzes

Skia Wallet uses in the development workflow: 
- [Snyk](https://snyk.io/) to "_find and fix security vulnerabilities in code and dependencies_".

[owasp-cheatsheet]: https://cheatsheetseries.owasp.org/Glossary.html
[auth0-storage]: https://auth0.com/blog/secure-browser-storage-the-facts/
[mdn-web-workers]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers
[whatwg-org]: https://html.spec.whatwg.org/multipage/workers.html
[can-i-use-web-workers]: https://caniuse.com/webworkers