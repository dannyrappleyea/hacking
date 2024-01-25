---
is: "[[software]]"
of: "[[hacking]]"
in: "[[hacking tools]]"
---
# Notes
Recon-ng is a pentest reconnaissance tool for a blackbox approach.

## Links
https://hackertarget.com/recon-ng-tutorial/
https://tremblinguterus.blogspot.com/2019/11/reconnaissance-with-recon-ng-part-iv.html
https://www.blackhillsinfosec.com/whats-changed-in-recon-ng-5x/
https://www.blackhillsinfosec.com/wp-content/uploads/2019/11/recon-ng-5.x-cheat-sheet-Sheet1-1.pdf
https://medium.com/hacker-toolbelt/recon-ng-how-to-iii-bf42b6fbc82b


# Older stuff

python [recon-ng.py](http://recon-ng.py/) -w client
set COMPANY [id.me](http://id.me/) set DOMAIN [id.me](http://id.me/)

keys list
query select * from hosts
query select * from contacts
query delete from hosts where host like 'm%’
query delete from hosts where ip_address is null

—
Finding hosts

use recon_hosts_gather_http_api/google_site
run
use recon_hosts_gather_http_web/bing_domain
run

use discovery_info_disclosure_http/google_ids
set URL …
run

—
other info

use recon_hosts_gather_http_api/bing_ip
run

—
geo
use recon_hosts_geo_http_api/ipinfodb
run
—
**Repetitive tasks**

use recon_hosts_enum_dns_resolve
run

—
export

use csv_file
set FILENAME _root_PentestShared_clients_client_recon_recon-ng-export.txt
run

—
Import hosts
(for i in ` cat hosts.txt ` ; do echo use recon_hosts_support/add_host; echo set HOST $i; echo run; echo exit; done) >import_hosts.rc

in recon-ng
resource _root_clients_client_recon/import_hosts.rc
