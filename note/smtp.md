---
is_a: "[[note]]"
of: "[[hacking]]"
---
# Notes
Talking to a smtp server

```
EHLO [acme.com](http://acme.com/)

MAIL FROM: < [someuser@acme.com](mailto:someuser@acme.com) >
RCPT TO: < [someuser@acme.com](mailto:someuser@acme.com) >
DATA
From: < [someuser@acme.com](mailto:someuser@acme.com) >
To: < [someuser@acme.com](mailto:someuser@acme.com) >
Date: Sun, 27 July 2014 05:53:36 -0700
Subject: test message

test message
.
```
