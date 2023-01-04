---
tags: a/note
---
in:: [[hacking tools]]

# Notes
Nmap is a network reconnaisance tool to discover accessible hosts and ports.

Scan top 1000 TCP
```
nmap -sSU -Pn --top-ports 1000 -oA {service}-top1000tcp {hosts}
```

Scan bottom 64535
```
nmap -sSU -Pn --top-ports 1000 -oA {service}-bottom-64000tcp {hosts}
```

Scan top 1000 UDP
```
nmap -sSU -Pn --top-ports 1000 -oA {service}-top1000udp {hosts}
```

Scan protocols

# Categories
For script scanning, try: auth,default,discovery,exploit,external,intrusive,safe,version,vuln

# Older notes
# Nmap

PING
     nmap -n -v --reason -sP -oA pingscan
SMB w/vulns
     nmap -n -v --reason -Pn -sT -p 139,445 --script=smb-check-vulns --script-args=unsafe=1 -oA nmap_smb-check-vulns
     nmap -n -v --reason -Pn -sT -p 139,445 --script=smb-enum-users -oA nmap_smb-enum-users

MSSQL
     nmap -n -v --reason -Pn -sV -sTU -p T:1433,U:1434 --script ms-sql-empty-password --script-args mssql.instance-all -oA nmap_mssql

SSH
     nmap -n -v --reason -Pn -sT -p 22 -T 5 --script ssh-hostkey --script-args ssh_hostkey=all -oA nmap_ssh_hostkey
All TCP
     nmap -n -v --reason -Pn -sT -p1- -oA nmap_fulltcp

Quick Scan
     nmap -n -v --reason -Pn -T4 -A -sSU --top-ports 100 -oA nmap_top100a

SSL Certificates
     nmap -v --reason -Pn -sT -p 443 --script ssl-cert -oA ssl_certs

Get robots.txt
for i in `grep 'open/tcp' http_services.gnmap | cut -d ' ' -f 2`; do curl --connect-timeout 3 -f -w "%{url_effective}\n" -s -o robots.txt_$i http://${i}/robots.txt; done

Parsing UP systems
grep "Status: Up" 10.0_ping.gnmap | cut -d ' ' -f 2 >targets.txt

Custom Nmap datadir
export NMAPDIR=/root/.nmap
mkdir ~/.nmap
cp /usr/share/nmap/nmap-services ~/.nmap

Scriptscan
auth,default,discovery,exploit,intrusive,malware,safe,version,vuln

Getting the top 100 ports
nmap -F -oG - -v -sU --top-ports 100 0.0.0.1

Ping
nmap -sP -v --reason -oA ping -iL ip.txt

Web Recon
nmap -Pn -n -sT -p 80,443,8080 -v --reason -sV --script=banner,ssl-cert -oA webrecon -iL ip.txt

TCP Recon
nmap -Pn -sT --top-ports 100 -v --reason -sV --script=banner -oA tcprecon -iL iprange.txt

Windows Recon
nmap -n -Pn -sT -p 139,445 -v --reason -oA windowsrecon -iL targets.txt

Windows Recon with ms08-067
nmap -n -Pn -sT -p 139,445 -v --reason -oA windowsrecon --script smb-check-vulns --script-args=unsafe=1  -iL targets.txt


Script scanning
nmap -v --reason -oA <filename> -Pn -A -sSU --script "not dos" -p T:1-65535,U:53,67-69,111,123,135,137-139,161-162,445,500,514,520,631,998,1434,1701,1900,4500,5353 --host-timeout 60m -iL <targets
