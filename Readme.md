# Different attacks and sollutions to prevent them

### Env variables can be vulnerable. Npm vulnerable packages can have access to process and can theft the api keys and other data.  
And if we are running multiple apps on single instance then app can access the api keys and secret of each other.
* Prevention: On aws ssm parameter store-- free service. Create IAM user, who can use that policy, 
then inside app use aws-param-store sdk to get the params.
* Aws Secret Manager -- paid service.
* Docker: if using docker swarm then we can use built in Docker secret service from manager to workers to pass secrets on secured tls
https://blog.docker.com/2017/02/docker-secrets-management/
* HashiCorp Vault: enterprise solution for storing secret--- paid
* keyWhiz -- paid

### Injection Attacks - sending malicious code via forms search etc to manipulate, access the database both sql and noSql. 
prevention is same as xss.
Where as in xss the focus is mainly on client/end user, like redirecting stealing user data, downloading virus etc

## xss cross-site-scripting attack prevention
* validator, express-validator  for validating inbound data
* express-sanitizer for sanitation
* app.use(bodyParser.urlencoded({extended:false})); to prevent miss use of get request.
* chrome also have built-in xss-auditor that tries to sanitize the input data, so while testing sanitation use firefox
* inbound data, coming from client should be sanitize before storing to db
* outbound data, data from db, before sending to client should be sanitize too. To prevent persistence xss attack.
* CSP content-security-policy: is header set as server response.
It restricts browser from downloading content from untrusted sources.
* helmet: package for csp from server. https://www.npmjs.com/package/helmet
* helmet.xssFilter,helmet.contentSecurityPolicy({define policy here}), and report when attack occur are cool features
* On client side and server also: https://www.npmjs.com/package/dompurify
* On client: https://github.com/yahoo/xss-filters
* Reactjs: https://medium.com/javascript-security/avoiding-xss-in-react-is-still-hard-d2b5c7ad9412


## csrf cross site request frojery attack; prevention with react
* Get requests are stored in browser history, and can accidentally be invoked again.
* For transactions and data manipulation we should always use post, put, delete requests
* csrf protection using axios
// `xsrfCookieName` is the name of the cookie to use as a value for xsrf token
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default
* So in react we prevent the default form action of sending data.
and we manually send data as ajax request with cookie or token.
* So to prevent csrf attack with react, we should use another token say for ex. 'xsrf-token'.
* Now before sending data to backend, we should make a call to backend to get the 'xsrf-token', then save that token in redux store for further use, and at backend we should add another middleware, on routes which handle formData, to check for that 'xsrf-token' validity.
* Routes that handle form submit post req, we will have 2 middleware, one for general-purpose auth, second for 'xsrf-token' and its validation
* Also avoid sending 'xsrf-token' inside cookie, as attacker can also access this cookie :) and store it inside redux-store.
* Another solution is express-session + csrf package token middleware
* To prevent iframe miss use of our website we should set X-Frame-Option-deny.
This is done with the help of helmet middleware as app.use(helmet.frameGuard:{action:'deny'})
* Last ensure https encryption connection instead of http
* https://github.com/expressjs/csurf

## Securing Cookies:
*  if working with express-session package <br>
app.use(session({
  secret:'aabraKaDabra',<br>
  resave:false,<br>
  saveUninitialized:true,<br>
  cookie:{<br>
    secure: app.get('env') === 'development' ? false:true,<br>
    httpOnly: true,<br>
    domain: app.get('env') === 'development' ? 'localhost':'yourdomain.com',<br>
    maxAge:60*60*60*60,<br>
    sameSite:true<br>
  },<br>
  name:'mySecureCookie'<br>
}))<br>
* That's the max-security for session cookies.
* Cookies+jwt auth here security options are same ad above see https://expressjs.com/en/api.html#res.cookie
* jwt auth: now we dont use cookies we use Auth-headers to send token.

## npm packages vulnerabilities
* npm audit and go to additional resource link to learn about vulnerability
* npm audit fix to upgrade the versions of high vulnerable packages
* npm outdated to check the current and latest version of installed packages
* npm audit fix --f will update all packages
* Best tool is --- snyk -- npm i snyk -g, snyk test, snyk wizard
* Snyk provides comprehensive detail and remedies to fix them

## Brut-force, denial of service-DOS, distributed denial of service-DDOS
* aws cloudflare cdn network is designed to prevent servers from those millions of requests to take down the server, or to get access to server with trying millions of username and password combinations
* akamai-prolexic is another service to prevent ddos attacks, while sitting in front of app
* rate-limiting--- prevent ddos attack at the server at the app level say ex 10 req/sec from a given ip
* rate-limiter-flexible npm




# refs
* https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md
