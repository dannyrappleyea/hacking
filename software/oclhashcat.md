---
is_a: "[[software]]"
topics:
  - "[[hacking]]"
  - "[[hacking tools]]"
---
# Notes
A password cracking tool

## WPA
Use wpa clean to reduce the file to just the WPA capture
**** NOTE **** - output file is FIRST, and it will erase your capture file if you don't get the order correct
wpaclean <out.cap> <in.cap>

## Convert the WPA capture into hashcat's HCCAP format
aircrack-ng <in.cap> -J <out.hccap>

For bruteforce, you have to run it once for each length password you want to try
- the "--custom-charset1" option sets what characters to use in the bruteforce
- the "?1" pattern at the end uses the custom character set, repeated for as many characters as you like

## upper, lower, number
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1?1?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u <file.hccap> ?1?1?1?1?1?1?1?1

## upper, lower, number, symbol
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1?1?1?1?1?1
./cudaHashcat-plus64.bin -m 2500 --attack-mode=3 --custom-charset1=?l?d?u?s <file.hccap> ?1?1?1?1?1?1?1?1

## Other notes
Showing hash types supported
./cudaHashcat-plus64.bin --help

MS Cached Credentials - CUDA bruteforce
- Must be in the format of hash:username

./cudaHashcat-plus64.bin -m 1100 --attack-mode=3 --custom-charset1=?l?d?u?s <file.txt> ?1
./cudaHashcat-plus64.bin -m 1100 --attack-mode=3 --custom-charset1=?l?d?u?s <file.txt> ?1?1
...
./cudaHashcat-plus64.bin -m 1100 --attack-mode=3 --custom-charset1=?l?d?u?s <file.txt> ?1?1?1?1?1?1?1?1



----------
oclhashcat-plus 0.9

## Markov Bruteforce attack mode - increments from 1 to 8 chars

./cudaHashcat-plus64.bin -m 1100 --attack-mode=3 -i --increment-min=1 --increment-max=8 --outfile=FILE <file.txt> --outfile-format=3 ?a?a?a?a?a?a?a?a
oclHashcat-plus64.bin -m 1100 --attack-mode=3 -i --increment-min=1 --increment-max=8 --outfile=FILE <file.txt> --outfile-format=3 ?a?a?a?a?a?a?a?a
---
## Lanman challenge response
./hashcat-cli64.app -m 5500 --attack-mode=3 --outfile=atlas-out.txt atlas_lm.txt --outfile-format=3 -1 ?u?d?s ?1?1?1?1?1?1 --pw-min 6

## Apache htaccess/htpasswd files
oclHashcat64.exe -m 1600 --attack-mode=3 -i --increment-min=6 --increment-max=8 --outfile=C:\Hacking\davout.txt --outfile-format=3 --custom-charset1=?l?u?d C:\Hacking\passwddav.txt ?1?1?1?1?1?1?1?1

Dictionary attack
oclHashcat64.exe -m 1600 c:\hacking\passwddav.txt C:\hacking\wordlists\*
