# Burp extensions

Compiling/creating a burp extension
Using java command line to compile a jar file
Based loosely on [https://www.vanstechelman.eu/content/creating-and-building-burp-suite-extention-using-java-command-line-tools](https://www.vanstechelman.eu/content/creating-and-building-burp-suite-extention-using-java-command-line-tools)

Download the burp extender api from [https://github.com/PortSwigger/burp-extender-api](https://github.com/PortSwigger/burp-extender-api)
Copy the burp folder into the directory with your extension source

mkdir build
mkdir bin

javac -d build *.java
jar cf bin/burpextender.jar -C build burp

Load the bin/burpextender.jar file
