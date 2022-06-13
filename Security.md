# Security of the Skia Wallet

You can learn the security risks of the browsers, security issues when storing the user's private keys and the measures Skia Wallet team is taking into account.

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

- Local Storage or `localStorage`. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).
- Session Storage or `sessionStorage`. Read about it on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage).
- Cookies. Read about it on [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
- In memory. It stores the data temporary, this means if you refresh the page, it'll go away.

## How Metamask is saving the user's data?

MetaMask is the most famous software cryptocurrency wallet used to interact with the Ethereum blockchain. It allows users to access their Ethereum wallet through a browser extension or mobile app, which can then be used to interact with decentralized applications. MetaMask is developed by ConsenSys Software Inc., a blockchain software company focusing on Ethereum-based tools and infrastructure.

Then it's a good source to understand how they save the user's data.

First, let's check the its code architecture [here](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/docs/architecture.png)

> ℹ️ Some of the links below point to lines of code at one moment in time. If you want to read the latest code, switch to the related project's default branch (e.g. `main`, `develop`...)

Metamask extension uses [LavaMoat](https://github.com/LavaMoat/lavamoat) to package all the code ([here is the line](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/package.json#L16) in a secure way to prevent software supply chain attacks. Microsoft explains [here](https://docs.microsoft.com/en-us/microsoft-365/security/intelligence/supply-chain-malware) that _"software vendors are likely unaware that their apps or updates are infected with malicious code when they're released to the public. The malicious code then runs with the same trust and permissions as the app. The number of potential victims is significant, given the popularity of some apps. A case occurred where a free file compression app was poisoned and deployed to customers in a country where it was the top utility app."_

LavaMoat's creator did a [talk in Speakeasy JS](https://youtu.be/iaqe6F4S2tA) in 2021 about an attack dono to a Javacript library used by multiple projects in the blockchain.

List of ways Metamask saves the user's data:

- Browser extensions needs to write a [manifest file](https://developer.chrome.com/docs/extensions/mv3/manifest/) to tell to the browsers the permissions, icons... they gonna use. [Metamask's manifest](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/manifest/v3/_base.json) has a permission for storage (here is the [documentation from Chrome team](https://developer.chrome.com/docs/extensions/reference/storage/) and from [MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/storage), it's very similar as `localStorage` but with some differences commented in the previous link. At some point (2018), it switched from `localStorage` to `chrome.storage.local` ([reference 1](https://github.com/MetaMask/metamask-extension/issues/3076#issuecomment-359969327),), [reference 2](https://github.com/MetaMask/metamask-extension/issues/2749#issuecomment-390328007)), find therYou can open the Developer Tools for the Metamask Extension of the browser where it's installed, then on the console, run the next command (read more clicking the previous documentation link):
```js
chrome.storage.local.get('data', result => {
    var vault = result.data.KeyringController.vault
    console.log(vault)
})
```
- The file `local-store.js`(https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/lib/local-store.js) is where Metamask connect to the browser's extension storage. It uses `browser.storage.local` to do so and it is provided by the Javascript library [webextension-polyfill](https://github.com/mozilla/webextension-polyfill).
- The file `background.js`(https://github.com/MetaMask/metamask-extension/blob/b170211700/app/scripts/background.js) contains big part of the data management of the Metamask extension.
  - [`initApp`](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/background.js#L90) is where big part of `background` code is initialized.
  - [`persistData`](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/background.js#L369) is runned during the initialization `initApp` in the [line #167](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/background.js#L167). Here, it uses the file `local-store.js`.
- [`MetamaskController`](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/metamask-controller.js#L161): used to store the user's state related to the Metamask extension. Where is it called? in file [background](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/background.js#L314)
- User's preferences (locale, features that user wants to see...) are saved in the field called [`store`](https://github.com/MetaMask/metamask-extension/blob/b170211700d78655eacc76feda0ea3b393366e1c/app/scripts/controllers/preferences.js#L19) in `PreferencesController`.
- A common Javascript library used by Metamask is [@metamask/obs-store](https://github.com/MetaMask/obs-store), also called `ObservableStore` or _vault_.  The main code is [here](https://github.com/MetaMask/obs-store/blob/main/src/ObservableStore.ts).
- For creating, deleting... the private keys, the Javascript library used is [Keyring Controller](https://github.com/MetaMask/KeyringController/). The library uses the library `ObservableStore` to store data. The code is [here](https://github.com/MetaMask/KeyringController/blob/main/index.js). Some of its methods:
  - [`createNewVaultAndRestore`](https://github.com/MetaMask/KeyringController/blob/0a92a4b2ef80b665ae8240881b3a1f2d7715d514/index.js#L100)). It _"destroys any old encrypted storage, creates a new encrypted store with the given password, creates a new HD wallet from the given seed with 1 account."_
  - [`persistAllKeyrings`](https://github.com/MetaMask/KeyringController/blob/0a92a4b2ef80b665ae8240881b3a1f2d7715d514/index.js#L549) is runned inside `createNewVaultAndRestore`. It encrypts (with [browser-passworder](https://github.com/MetaMask/browser-passworder) and stores (with `ObservableStore`) the user's mnemonic key (also called _seed phrase_).

## Protect against security risks

### Common best practices

A long list by [OWASP][owasp-cheatsheets] of the most common attacks and how to prevent them.

Skia Wallet team has to work on the next areas:
- [ ] **[To do]** Cross-Site Scripting or [XSS](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#xss-prevention-rules-summary).
- [ ] **[To do]** Cross-Site Request Forgery or [CSRF](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [ ] **[To do]** [Sanitize the inputs](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) with the Javascript library [DOMPurify](https://github.com/cure53/DOMPurify) (its goals [here](https://github.com/cure53/DOMPurify/wiki/Security-Goals-&-Threat-Model)). It's a common issue on multiple type of attacks.
- [ ] **[To do]** Set up the [HTML headers](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html): `X-Frame-Options`, `X-XSS-Protection`, `X-Content-Type-Options`, `Referrer-Policy`, `Content-Type`, `Strict-Transport-Security`, `Content-Security-Policy`, `Access-Control-Allow-Origin`, `Cross-Origin-Opener-Policy`, `Cross-Origin-Resource-Policy`, `Cross-Origin-Embedder-Policy`, remove `X-Powered-By`.
- [ ] **[To do]** Require multi-factor authentication for admins. In places like the codebase (2FA on [Github](https://docs.github.com/en/enterprise-cloud@latest/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication), GPG on [Github](https://docs.github.com/en/enterprise-cloud@latest/authentication/managing-commit-signature-verification/about-commit-signature-verification))

Secondary:
- [ ] Good practices for software developers according to:
  - Google: [security for websites](https://developers.google.com/search/docs/advanced/security/overview)
  - Microsoft: [Security Development Lifecycle](https://www.microsoft.com/en-us/securityengineering/sdl), [mitigate supply chain attacks](https://docs.microsoft.com/en-us/microsoft-365/security/intelligence/supply-chain-malware#for-software-vendors-and-developers)
- [ ] [CSS](https://cheatsheetseries.owasp.org/cheatsheets/Securing_Cascading_Style_Sheets_Cheat_Sheet.html)
- [ ] Javascript package manager [NPM](https://cheatsheetseries.owasp.org/cheatsheets/NPM_Security_Cheat_Sheet.html)
- [ ] [NodeJS](https://cheatsheetseries.owasp.org/cheatsheets/Nodejs_Security_Cheat_Sheet.html)

### Secure the storage of the mnemonic key and private keys
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

## Tools for security analyzes

Skia Wallet uses in the development workflow: 
- [Snyk](https://snyk.io/) to "_find and fix security vulnerabilities in code and dependencies_".

[owasp-cheatsheet]: https://cheatsheetseries.owasp.org/Glossary.html
[auth0-storage]: https://auth0.com/blog/secure-browser-storage-the-facts/
[mdn-web-workers]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers
[whatwg-org]: https://html.spec.whatwg.org/multipage/workers.html
[can-i-use-web-workers]: https://caniuse.com/webworkers