---
is_a: "[[software]]"
of: "[[hacking]]"
in: "[[hacking tools]]"
aliases: burp suite professional
---
# Notes
Burp is an awesome tool for webapp testing.

## Portswigger resources
- [Getting started with Burp Suite](https://portswigger.net/burp/documentation/desktop/getting-started)
- [Web Security Acadamy](https://portswigger.net/web-security/dashboard)
- [Support Center](https://portswigger.net/support)

## API Scanning
https://portswigger.net/burp/documentation/desktop/scanning/api-scanning

## Updating Burp on linux
chmod 755 ./burpsuite_pro_linux_v2020_12_1.sh
sudo ./burpsuite_pro_linux_v2020_12_1.sh

## Keep out-of-scope items out of Proxy history
Don't send items to Proxy history, if out of scope

## Favorite Extensions
Copy As Python-Requests

## Updating Burp Pro on Kali
Download update when prompted
su to root
run updater

## Burp Searches
### AWS
`.s3.amazonaws.com`

# Burp extensions

Compiling/creating a burp extension
Using java command line to compile a jar file
Based loosely on [https://www.vanstechelman.eu/content/creating-and-building-burp-suite-extention-using-java-command-line-tools](https://www.vanstechelman.eu/content/creating-and-building-burp-suite-extention-using-java-command-line-tools)

Download the burp extender api from [https://github.com/PortSwigger/burp-extender-api](https://github.com/PortSwigger/burp-extender-api)
Copy the burp folder into the directory with your extension source

mkdir build
mkdir bin

javac -d build *.java
jar cf bin/burpextender.jar -C build burp

Load the bin/burpextender.jar file
