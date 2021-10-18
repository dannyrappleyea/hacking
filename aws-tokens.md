# AWS Tokens
AWS access tokens are the keys to hacking AWS-based organizations. It is rare and difficult to get a username and password, and those are typically guarded by MFA. But access tokens are used EVERYWHERE.

# Formats
## Environment variables
export AWS_ACCESS_KEY_ID="ASIAFOOFOOFOO"
export AWS_SECRET_ACCESS_KEY="Yx8h8JjPRVWBzS5gvFooFooFoo"
export AWS_SESSION_TOKEN="IQotOQHN1n90bTIspuB2di/ReallyLongFoo"

## Credentials File
[AwsProfile]
aws_access_key_id=ASIAFOOFOOFOO
aws_secret_access_key=Yx8h8JjPRVWBzS5gvFooFooFoo
aws_session_token=IQotOQHN1n90bTIspuB2di/ReallyLongFoo

## S3 Presigned Url
https://mybucket.s3.amazonaws.com/foo?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAFOOFOOFOO%2F20210101%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210101T121659Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Security-Token=IQotOQHN1n90bTIspuB2di%2BReallyLongFoo&X-Amz-Signature=00f6f11954e99fa9c89c9912ecf918f193d09

# References
[Configuration basics - AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
