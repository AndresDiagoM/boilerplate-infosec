# Information Security with HelmetJS

This is the boilerplate for the Information Security lessons. Instructions for completing these lessons start at https://www.freecodecamp.org/learn/information-security/information-security-with-helmetjs/


# Using helmet.hidePoweredBy()
Hackers can exploit known vulnerabilities in Express/Node if they see that your site is powered by Express. X-Powered-By: Express is sent in every request coming from Express by default. Use the helmet.hidePoweredBy() middleware to remove the X-Powered-By header. Chaining app.use(helmet.hidePoweredBy()) will remove the header.
    ```javascript
    app.use(helmet.hidePoweredBy());
    ```

# Using helmet.frameguard()
Your page could be put in a <frame> or <iframe> without your consent. This can result in clickjacking attacks, among other things. Clickjacking is a technique of tricking a user into interacting with a page different from what the user thinks it is. This can be obtained by putting your page in a <frame> with a different page in it. To avoid your page to be iframed, you can use the frameguard middleware. This middleware sets the X-Frame-Options header. It restricts who can put your site in a frame. It has three settings: DENY, SAMEORIGIN, and ALLOW-FROM.
    ```javascript
    app.use(helmet.frameguard({action: 'deny'}));
    ```

# Using helmet.xssFilter()
X-XSS-Protection is a feature of Internet Explorer, Chrome and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS) attacks. Although these protections are largely unnecessary in modern browsers when sites implement a strong Content-Security-Policy that disables the use of inline JavaScript ('unsafe-inline'), they can still provide protections for users of older web browsers. This middleware sets the X-XSS-Protection header. This header is used by Internet Explorer, and Chrome.
    ```javascript
    app.use(helmet.xssFilter());
    ```
