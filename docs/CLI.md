# CLI
[TOC](https://github.com/ScaleSec/AWS101/blob/main/README.md#TOC)

## Install
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

Mac:
`brew install awscli`

jq is recommended to parse JSON
`brew install jq`

## Configuration
Set up credentials, default region, and output type
```shell
export AWS_PROFILE=xyz
aws configure --profile $AWS_PROFILE
```

You need to export AWS_PROFILE in any terminal from now on.

https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html

## Useful Commands

## Misc
`aws xyz subcommand --generate-cli-skeleton`

generate-cli-skeleton is supported by most commands. Will generate a JSON schema for the command to be passed in with `--cli-input-json`

## List all resources
`aws resourcegroupstaggingapi get-resources | jq`

### List users
`aws iam list-users --output json | jq -r '.Users[].Arn'`
`aws iam list-users --output table`

### List and Describe Instances
`aws ec2 describe-instances | jq`

`aws ec2 describe-volumes | jq`

Get console output
`aws ec2 get-console-output --instance-id i-12345 | jq`

Find stopped instances
`aws ec2 describe-instances --filters Name=instance-state-name,Values=stopped --output json | jq`

## S3
* List all buckets. This will only work if you have permission to list all buckets
`aws s3 ls`

* List a bucket
`aws s3 ls s3://mybucket`
