---
tags: a/note
---
in:: [[hacking]]

# Notes
Rootabaga is a covert network hacking device to plug into internal networks, creating your own remote-access backdoor.

## Notes
* changed root password
* switching between minipwner and pineapple modes seems to reset wireless configuration
Minipwner mode http://192.168.1.1/cgi-bin/luci https://192.168.1.1/cgi-bin/luci - log in with root & pass
Pineapple mode
ifconfig eth0 172.16.1.1 netmask 255.255.255.0 echo ‘1’ > /proc/sys/net/ipv4/ip_forward iptables -A POSTROUTING -t nat -j MASQUERADE
