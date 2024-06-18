---
is_a: "[[note]]"
of: "[[hacking]]"
---
# Notes
Wirelesshacking

## Aircrack
Wireless adapter issues
If issues, unplug and replug wireless usb cable

airmon-ng start wlan0
airodump-ng mon0
airodump-ng -c 4 --bssid 58:35:D9:3B:66:50 mon1

—
power settings
iwconfig wlan0 txpower 30
- may need to ifcon
iw reg set BO

fig wlan0 down before iwreg

Starting on specific channel
airmon-ng start wlan0 1
