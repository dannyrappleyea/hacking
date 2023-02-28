is:: [[note]]
from:: [[hacking aws]]

# Notes
AWS Organizations are used to manage large numbers of AWS accounts grouped in a heirarchy.
* Root: Parent account and container
* Organizational Unit (OU): Containers within the root account to attach accounts and policies to.
* Account: AWS account

## Recon
Find your account:
* ```aws sts get-caller-identity```
* Look in the account field

Describe the organization
* ```aws organizations describe-organization```
* Should return an ```Organization``` object if part of an organization
* Can be called from any account
* Gives master account ID and email address

Other things to try
* A normal user account likely doesn't have access, but worth trying
* ```aws organizations list-accounts```
