---
is: "[[software]]"
of: "[[hacking]]"
in: "[[hacking tools]]"
---
# Notes
SSL/TLS testing tool

## Links
https://testssl.sh/
https://github.com/drwetter/testssl.sh

## Using
`testssl.sh <url>`

## Limiting to one IP
If the dns resolves to multiple IPs, this only tests one of them

`testssl.sh --ip one <url>`

## Log data
Log data in text and json formats
`testssl.sh --log --json <url>`
