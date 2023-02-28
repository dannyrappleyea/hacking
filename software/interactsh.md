is:: [[software]]
from:: [[hacking]]
in:: [[hacking tools]]

# Notes
https://github.com/projectdiscovery/interactsh
- **Interactsh** is an open-source tool for detecting out-of-band interactions. It is a tool designed to detect vulnerabilities that cause external interactions.

## Installation
```
sudo yum install golang
go install -v github.com/projectdiscovery/interactsh/cmd/interactsh-server@latest
```

## Run client
With Docker
```
docker run projectdiscovery/interactsh-client:latest
```

With Docker and custom server
```
docker run projectdiscovery/interactsh-client:latest -server myserver.com
```


## Run server
### As non-root user
```
interactsh-server -d domain.name -https-port 8443 -http-port 8080 -dns-port 8053 -smtp-port 8025 -smtps-port 8587 -smtp-autotls-port 8465 -ldap-port 8389
```