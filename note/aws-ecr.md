---
is_a: "[[note]]"
of: "[[hacking aws]]"
---
# Notes
AWS [ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) is Amazon's Docker repository service. Getting access could allow for:
* Downloading docker images to view source code or look for secrets
* Uploading docker images with malware, getting code execution if run in the target environments

## Links
* [User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)
* [CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ecr/index.html#cli-aws-ecr)

## Prerequisites
This assume you:
* Have an AWS account, configured for CLI access
* Have Docker installed

## Recon
Recon will be hard. To get access to a repository, you need the target AWS account number and repository name.

# Hacking ECR
When using the AWS CLI, ```registry-id``` is the target AWS account number. 

## Finding repositories
Unless the target is really misconfigured, you probably don't have permissions.

```aws ecr describe-repositories --registry-id <aws-account-number>```

If the repository is public, or misconfigured, you can get information about a specific repository. This gives the repository region.

```aws ecr describe-repositories --registry-id <aws-account-number> --repository-name <repository-name>```

## Listing images in a repository
If a repository is either Public, or Private with misconfigured permissions, you may be able to get the docker images.

```aws ecr list-images --registry-id <aws-account-number> --repository-name <repository-name>```

## Log into docker
This appears to login to the entire registry for the target account. Perhaps this could leak more information about repositories?
```
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```

## Pulling an image
```docker pull <aws-account-number>.dkr.ecr.<region>.amazonaws.com/<repository-name>:latest```

## Inspecting an image
After pulling an image, inspecting it might give hints on where to look for interesting things

```docker inspect <aws-account-number>.dkr.ecr.<region>.amazonaws.com/<repository-name>```

## Hacking an image
If the ECR repository allows, hack the docker image and upload into the repository. See [docker](shared/hacking/docker.md) for instructions. When pushing the image back into the repository, use the AWS repository format.

```
docker image push <aws-account-number>.dkr.ecr.<region>.amazonaws.com/<repository-name>:latest
```

Verify that the image pushed, by checking SHA hashes

```aws ecr list-images --registry-id <aws-account-number> --repository-name <repository-name>```

