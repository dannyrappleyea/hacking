---
is_a: "[[note]]"
topics: "[[hacking]]"
---
# Notes
## Linux / Unix
Identifying unix-based systems
```
uname -a cat _etc_issue
cat _etc_redhat-release
```

## Windows
Windows Loops

Windows - try to log into every machine

for /L %i in (1,1,254) do net use \\10.0.0.%i

Windows - search for users on many machines

WMIC /Node:”@targets.txt" ComputerSystem Get UserName

for /L %i in (1,1,9) do WMIC /Node:000%iCPU ComputerSystem Get UserName

Find groups for a list of users
FOR /F %i in (users.txt) do DSQUERY USER -samid %i | DSGET USER -memberof -expand >%i.txt
