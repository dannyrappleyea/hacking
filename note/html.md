---
is_a: "[[note]]"
of: "[[hacking web services]]"
---
# Notes
- Can test html snippets at https://htmledit.squarefree.com/

Writing Into HTML DOM

```
<!DOCTYPE html>
<html>
  <head>
    <title>Lorem Ipsum</title>
    <script>
    function myFunction() {
      document.getElementById("location").innerHTML = document.location;
      document.getElementById("cookies").innerHTML = document.cookie;
      document.getElementById("localstorage").innerHTML = JSON.stringify(window.localStorage);
      document.getElementById("sessionstorage").innerHTML = JSON.stringify(sessionStorage);
    }
    </script>
  </head>
<body onload="myFunction()"">
  <h1>Location</h1>
  <div id="location"></div>
  <h1>Cookie</h1>
  <div id="cookies"></div>
  <h1>localStorage</h1>
  <div id="localstorage"></div>
  <h1>sessionStorage</h1>
  <div id="sessionstorage"></div>
</body>
</html>
```
