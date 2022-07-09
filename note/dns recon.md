---
tags: a/note
---
in:: [[hacking]]

# Notes
DNS Recon

Zone Transfers
dig @server dnszone.com AXFR dig @server dnszone.com IXFR=1

theharvester
cd _pentest_enumeration/theharvester
./theHarvester.py -d [domain.com](http://relateiq.com) -b all >~/PentestShared_Clients_domain_recon_/harvester.txt

on kali:

theharvester -d [domain.com](http://relateiq.com) -b all >~_PentestShared_Clients_domain_reconharvester.txt

fierce
cd _pentest_enumeration_dns_fierce
perl [fierce.pl](http://fierce.pl) -dns [examplecompany.com](http://examplecompany.com) -wordlist _root_PentestShared_tools_fierce_hosts.txt -wide >~_PentestShared_Clients_domain_recon_fierce.txt

on kali:

fierce ...
fierce -dns company.com -wordlist _usr_share_fierce_hosts.txt -wide

Manual testing
dig domain.com TXT dig domain.com SRV dig _ldap._tcp.dc._msdcs.domain.com SRV
