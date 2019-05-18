# Different attacks and sollutions to prevent them

### Env variables can be vulnirable. Npm vulnirable packages can have access to process and can theft the api keys and other data.  
And if we are running multiple apps on single instance then app can access the api keys and secret of each other.
* Prevention: On aws ssm parameter store-- free service. Create IAM user, who can use that policy, 
then inside app use aws-param-store sdk to get the params.
* Aws Secret Manager -- paid service.
* Docker: if using docker swarm then we can use built in Docker secret service from manager to workers to pass secrets on secured tls
https://blog.docker.com/2017/02/docker-secrets-management/
* HashiCorp Vault: enterprise solution for storing secret--- paid
* keyWhiz -- paid

### Injection Attacks - sending malicious code via forms search etc.
