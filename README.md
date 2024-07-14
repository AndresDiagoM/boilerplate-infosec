# Information Security with HelmetJS

This is the boilerplate for the Information Security lessons. Instructions for completing these lessons start at https://www.freecodecamp.org/learn/information-security/information-security-with-helmetjs/

# Using helmet.hidePoweredBy(): Hide "X-Powered-By" Header

Hackers can exploit known vulnerabilities in Express/Node if they see that your site is powered by Express. X-Powered-By: Express is sent in every request coming from Express by default. Use the helmet.hidePoweredBy() middleware to remove the X-Powered-By header. Chaining app.use(helmet.hidePoweredBy()) will remove the header.

```javascript
app.use(helmet.hidePoweredBy());
```

# Using helmet.frameguard(): Mitigate Clickjacking with the "X-Frame-Options" Header

Your page could be put in a `<frame>` or `<iframe>` without your consent. This can result in clickjacking attacks, among other things. Clickjacking is a technique of tricking a user into interacting with a page different from what the user thinks it is.
This can be obtained by putting your page in a `<frame>` with a different page in it. To avoid your page to be iframed, you can use the frameguard middleware. This middleware sets the X-Frame-Options header. It restricts who can put your site in a frame. It has three settings: DENY, SAMEORIGIN, and ALLOW-FROM.

```javascript
app.use(helmet.frameguard({ action: "deny" }));
```

# Using helmet.xssFilter(): Mitigate the Risk of Cross Site Scripting (XSS) Attacks with helmet.xssFilter()

X-XSS-Protection is a feature of Internet Explorer, Chrome and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS) attacks. Although these protections are largely unnecessary in modern browsers when sites implement a strong Content-Security-Policy that disables the use of inline JavaScript ('unsafe-inline'), they can still provide protections for users of older web browsers. This middleware sets the X-XSS-Protection header. This header is used by Internet Explorer, and Chrome.
The basic rule to lower the risk of an XSS attack is simple: "Never trust user's input". As a developer you should always sanitize all the input coming from the outside. This includes data coming from forms, GET query urls, and even from POST bodies. Sanitizing means that you should find and encode the characters that may be dangerous e.g. <, >.
The X-XSS-Protection HTTP header is a basic protection. The browser detects a potential injected script using a heuristic filter. If the header is enabled, the browser changes the script code, neutralizing it. It still has limited support.

```javascript
app.use(helmet.xssFilter());
```

# Using helmet.noSniff(): Avoid Inferring the Response MIME Type

Browsers can use content or MIME sniffing to adapt to different kinds of content. There are some cases in which the browser will try to determine the content type of a response by inspecting the content-type header. If, for example, you serve a response with a content-type of text/plain, but the content includes `<script>`, the browser will execute the script.
To prevent this, you can set the **X-Content-Type-Options** header to nosniff. This will prevent the browser from interpreting files as something else than declared by the content-type in the response header.

```javascript
app.use(helmet.noSniff());
```

# Using helmet.ieNoOpen(): Prevent IE from Opening Untrusted HTML

Some web applications will serve untrusted HTML for download. Some versions of Internet Explorer by default open those HTML files in the context of your site. This means that an untrusted HTML page could start doing bad things in the context of your pages. This middleware sets the **X-Download-Options** header to noopen. This will prevent IE users from executing downloads in the trusted site's context.

```javascript
app.use(helmet.ieNoOpen());
```

# Using helmet.hsts(): HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) is a web security policy which helps to protect websites against protocol downgrade attacks and cookie hijacking. If your website can be accessed via HTTPS you can ask user’s browsers to avoid using insecure HTTP. By setting the header Strict-Transport-Security, you tell the browsers to use HTTPS for the future requests in a specified amount of time. This will work for the requests coming after the initial request.

Configure helmet.hsts() to use HTTPS for the next 90 days. Pass the config object {maxAge: timeInSeconds, force: true}. You can create a variable ninetyDaysInSeconds = 90*24*60\*60; to use for the timeInSeconds. Testing the configuration, make a request to the / endpoint and inspect the response headers.

Note: Configuring HTTPS on a custom website requires the acquisition of a domain, and an SSL/TLS Certificate.

```javascript
const ninetyDaysInSeconds = 90 * 24 * 60 * 60;
app.use(helmet.hsts({ maxAge: ninetyDaysInSeconds, force: true }));
// force is set to true, which tells browsers to use HTTPS for the next 90 days
```

# Using helmet.dnsPrefetchControl(): Disable DNS Prefetching

To improve the performance of web applications, browsers prefetch DNS records for the links in a page. This is a technique called DNS prefetching. It may have privacy implications, but it can help to improve the performance of a web application. If you have a security concern, you can disable DNS prefetching. This can be done by setting the **X-DNS-Prefetch-Control** header. This header will disable the DNS prefetching for the links in your web application.

```javascript
app.use(helmet.dnsPrefetchControl());
```

# Using helmet.noCache(): Disable Client-Side Caching

Caching can be a powerful tool to increase the speed of a web application. However, it can also lead to problems if the content of the application changes. This can cause the user to not receive the latest content. You can set the **Cache-Control** and **Pragma** headers to disable caching. **Cache-Control** handles more cases than **Pragma**, so it is best to use **Cache-Control**.

```javascript
app.use(helmet.noCache());
```

# Set a Content Security Policy with helmet.contentSecurityPolicy()

This challenge highlights one promising new defense that can significantly reduce the risk and impact of many type of attacks in modern browsers. By setting and configuring a Content Security Policy you can prevent the injection of anything unintended into your page. This will protect your app from XSS vulnerabilities, undesired tracking, malicious frames, and much more. CSP works by defining where resources can be loaded from, preventing browsers from loading data from any other locations. This makes it more difficult for an attacker to inject malicious scripts.

There are five directives in a CSP: defaultSrc, scriptSrc, styleSrc, imgSrc, and connectSrc. These are the most basic ones. Using these directives, you can allow or disallow the browser from loading specific resource types.

Helmet supports both defaultSrc and default-src naming styles.

In this exercise, use helmet.contentSecurityPolicy(). Configure it by adding a directives object. In the object, set the defaultSrc to ["'self'"] (the list of allowed sources must be in an array), in order to trust only your website address by default. Also set the scriptSrc directive so that you only allow scripts to be downloaded from your website ('self'), and from the domain 'trusted-cdn.com'.

Hint: in the 'self' keyword, the single quotes are part of the keyword itself, so it needs to be enclosed in double quotes to be working.

```javascript
app.use(
	helmet.contentSecurityPolicy({
		directives: {
			defaultSrc: ["'self'"],
			scriptSrc: ["'self'", "trusted-cdn.com"],
		},
	})
);
```

# Configure Helmet Using the ‘parent’ helmet() Middleware

app.use(helmet()) will automatically include all the middleware introduced above, except noCache(), and contentSecurityPolicy(), but these can be enabled if necessary. You can also disable or configure any other middleware individually, using a configuration object.

```javascript
app.use(
	helmet({
		frameguard: {
			// configure
			action: "deny",
		},
		contentSecurityPolicy: {
			// enable and configure
			directives: {
				defaultSrc: ["'self'"],
				styleSrc: ["style.com"],
			},
		},
		referrerPolicy: {
			// enable
			policy: "same-origin",
		},
		dnsPrefetchControl: false, // disable
	})
);
```

# Understand BCrypt Hashes
BCrypt hashes are very secure. A hash is basically a fingerprint of the original data- always unique. This is accomplished by feeding the original data into an algorithm and returning a fixed length result. To further complicate this process and make it more secure, you can also salt your hash. Salting your hash involves adding random data to the original data before the hashing process which makes it even harder to crack the hash.

BCrypt hashes will always look like:

```javascript
$2a$12$ZyprE5MRw2Q3WpNOGZWGbeG7ADUre1Q8QO.uUUtcbqloU0yvzavOm
```

which does have a structure. The first small bit of data $2a is defining what kind of hash algorithm was used. The next portion $13 defines the cost. Cost is about how much power it takes to compute the hash. It is on a logarithmic scale of 2^cost and determines how many times the data is put through the hashing algorithm. For example, at a cost of 10 you are able to hash 10 passwords a second on an average computer, however at a cost of 15 it takes 3 seconds per hash... and to take it further, at a cost of 31 it would take multiple days to complete a hash. 

A cost of 12 is considered very secure at this time. The last portion of your hash $ZyprE5MRw2Q3WpNOGZWGbeG7ADUre1Q8QO.uUUtcbqloU0yvzavOm, looks like one large string of numbers, periods, and letters but it is actually two separate pieces of information. The first 22 characters is the salt in plain text, and the rest is the hashed password!

