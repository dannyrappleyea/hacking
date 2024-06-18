---
is_a: "[[note]]"
of: "[[hacking web services]]"
---
# Notes
Postmessage

## Links
https://labs.detectify.com/2016/12/08/the-pitfalls-of-postmessage/

## Logging postmessage events
```
window.onmessage = (event) => {
    console.log(`POSTMESSAGE:`);
    console.log(event);
};
```
