---
is_a: "[[software]]"
of: "[[hacking]]"
in: "[[hacking tools]]"
aliases: nc
---
# Notes
Basic Netcat listener

Server
```
nc -l -p 4000
```

Client
```
nc {ip} 4000
```

# Installation
Redhat-based systems
```
sudo yum install nc
```