# Security of the Skia Wallet

## Store the user's mnemonic key

Here you'll discover how Skia Wallet saves the mnemonic key of the persons on the browsers.

## Understand the security risks of the browsers

The company [Auth0](https://auth0.com/) is recognized for offering to the software developers an easy way to implement, adaptable authentication and authorization for their apps and websites.

They wrote an article "[Secure Browser Storage: The Facts](https://auth0.com/blog/secure-browser-storage-the-facts/)" in 2021 where they talk about the common security issues ([Cross Site Scripting](https://owasp.org/www-community/attacks/xss/) and [Cross Site Request Forgery](https://owasp.org/www-community/attacks/csrf) when storing data in next methodologies the currents browsers offers:

- Local Storage. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- Session Storage. Read about its technical details on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
- Cookies
- In memory

### Store data on a secure way
There's another technology that browsers supports called **Web Workers** that guarantee that the data remains inside a Web Worker and cannot be accessed from outside, like an attacker.