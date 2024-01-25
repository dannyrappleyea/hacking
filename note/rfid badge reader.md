---
is: "[[note]]"
of: "[[hacking]]"
---
# Notes
Testing HID RFID reader

* HID ProxPro II Wall Switch reader, model 5455BGN00
* Arduino UNO
Used Tastic RFID Thief code. Commented out all code related to SD card or LCD. Run HID sensor at 5V

* Can read card at distance between 7.75" and 8"
Test card data:

26 bit card. FacilityCode = 113, CardCode = 6274, 44bit HEX = 2006E23104

Test card data (proxmark3)
root@bt:~_proxmark3_pm3/client# ._proxmark3 /dev_ttyACM0 ＃db＃ Stand-alone mode! No PC necessary.
＃db＃ Starting recording
＃db＃ TAG ID: 2006e23104 (6274)
＃db＃ Recorded 0 20 6e23104
proxmark3>

Proxmark 3
reading HID cards
lf hid fskdemod
emulating HID cards
lf hid sim <card id>
