# Information Security with HelmetJS

This is the boilerplate for the Information Security lessons. Instructions for completing these lessons start at https://www.freecodecamp.org/learn/information-security/information-security-with-helmetjs/


# Using helmet.hidePoweredBy()
Hackers can exploit known vulnerabilities in Express/Node if they see that your site is powered by Express. X-Powered-By: Express is sent in every request coming from Express by default. Use the helmet.hidePoweredBy() middleware to remove the X-Powered-By header. Chaining app.use(helmet.hidePoweredBy()) will remove the header.
```javascript
app.use(helmet.hidePoweredBy());
```

# Using helmet.frameguard()
Your page could be put in a `<frame>` or `<iframe>` without your consent. This can result in clickjacking attacks, among other things. Clickjacking is a technique of tricking a user into interacting with a page different from what the user thinks it is. 
This can be obtained by putting your page in a `<frame>` with a different page in it. To avoid your page to be iframed, you can use the frameguard middleware. This middleware sets the X-Frame-Options header. It restricts who can put your site in a frame. It has three settings: DENY, SAMEORIGIN, and ALLOW-FROM.
```javascript
app.use(helmet.frameguard({action: 'deny'}));
```

# Using helmet.xssFilter()
X-XSS-Protection is a feature of Internet Explorer, Chrome and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS) attacks. Although these protections are largely unnecessary in modern browsers when sites implement a strong Content-Security-Policy that disables the use of inline JavaScript ('unsafe-inline'), they can still provide protections for users of older web browsers. This middleware sets the X-XSS-Protection header. This header is used by Internet Explorer, and Chrome.
The basic rule to lower the risk of an XSS attack is simple: "Never trust user's input". As a developer you should always sanitize all the input coming from the outside. This includes data coming from forms, GET query urls, and even from POST bodies. Sanitizing means that you should find and encode the characters that may be dangerous e.g. <, >.
The X-XSS-Protection HTTP header is a basic protection. The browser detects a potential injected script using a heuristic filter. If the header is enabled, the browser changes the script code, neutralizing it. It still has limited support.


```javascript
app.use(helmet.xssFilter());
```

# Using helmet.noSniff()
Browsers can use content or MIME sniffing to adapt to different kinds of content. There are some cases in which the browser will try to determine the content type of a response by inspecting the content-type header. If, for example, you serve a response with a content-type of text/plain, but the content includes `<script>`, the browser will execute the script. 
To prevent this, you can set the **X-Content-Type-Options** header to nosniff. This will prevent the browser from interpreting files as something else than declared by the content-type in the response header.

```javascript
app.use(helmet.noSniff());
```

# Using helmet.ieNoOpen()
Some web applications will serve untrusted HTML for download. Some versions of Internet Explorer by default open those HTML files in the context of your site. This means that an untrusted HTML page could start doing bad things in the context of your pages. This middleware sets the **X-Download-Options** header to noopen. This will prevent IE users from executing downloads in the trusted site's context.

```javascript
app.use(helmet.ieNoOpen());
```

