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

### Injection Attacks - sending malicious code via forms search etc to manipulate access the database both sql and noSql. 
prevention is same as xss.
Where as in xss the focus is mainly on client/end user, like redirecting stealing user data, downloading virus etc

## xss cross-site-scripting attack prevention
* validator, express-validator  for validating inbound data
* express-sanitizer for sanitation
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
* So in react we prevent the default form action of sending data.
and we manually send data as ajax request with cookie or token.
* So to prevent csrf attack with react, we should use another token say for ex. 'xsrf-token'.
* Now before sending data to backend, we should make a call to backend to get the 'xsrf-token', then save that token in redux store for further use, and at backend we should add another middleware, on routes which handle formData, to check for that 'xsrf-token' validity.
* Routes that handle form submit post req, we will have 2 middleware, one for general-purpose auth, second for 'xsrf-token' and its validation
* Also avoid sending 'xsrf-token' inside cookie, as attacker can also access this cookie :) and store it inside redux-store.










# refs
* https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md