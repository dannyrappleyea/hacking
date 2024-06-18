---
is_a: "[[note]]"
of: "[[hacking]]"
---
# Notes
DNS spoofing

## Dnsmasq

apt-get install dnsmasq

touch _etc_dnsmasq.hosts

root@kali:_etc_dnsmasq.d# cat _etc_dnsmasq.conf
no-dhcp-interface=eth0
server=172.18.176.2
no-hosts
addn-hosts=_etc_dnsmasq.hosts
no-daemon
log-queries

dnsmasq

[https://blog.heckel.xyz/2013/07/18/how-to-dns-spoofing-with-a-simple-dns-server-using-dnsmasq/](https://blog.heckel.xyz/2013/07/18/how-to-dns-spoofing-with-a-simple-dns-server-using-dnsmasq/)
