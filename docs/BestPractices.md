# Best Practices
[TOC](https://github.com/ScaleSec/AWS101/blob/main/README.md#TOC)

## Shared Responsibility
AWS is responsible for most of the hardware and physical security. They are not responsible for anything else except for with some managed services.

## Least Privilege

### Policies
* Use restrictive policies
* Use permission boundaries
* Use SCPs

### Root Account
The root account should be secured:
* MFA
* No access keys generated
* Break glass, in general

### Misc IAM
* Use roles when possible instead of access keys
* MFA for all users
* Strong password policy
* Cross account access should use assume role with external ID
* Use policy conditions

## Separation of Duties
* Different roles for every resource
* Don't share users
* IAM policies based on needs and duties
  * Separate key admins and key users
  * Separate daily users and super admins
## Defense in Depth

Here are some practical examples of Defense in Depth:

Network Level:
* VPN -> NACL -> WAF -> SG -> iptables -> OS hardening and patching
* Instance -> iptables -> SG -> NACL -> NAT Gateway -> Internet Gateway

API Level:
* User/Group/Role -> SCP -> Resource Policy -> Permission Boundary -> Restrictive IAM policy

Service Level:
* EC2 Instance -> Unique Role -> SCP -> Resource Policy -> Permission Boundary -> Restrictive IAM policy

## Inventory

You need to know all your assets before you can defend them. 

`aws resourcegroupstaggingapi get-resources | jq`

## S3 Security
* Set up server access logging and CloudTrail to log S3 events to important buckets
* Set up AWS Config to check your policies are in place and alert on them

### IAM Policies
Don't put `s3:*` in your policies. Restrict access to buckets by creating policies that allow access based on demonstrated need.

### Bucket Policies
Bucket policies take precedence over other IAM policies. Set these first and keep them restrictive.

### Block Public Access
AWS has a new feature to block all public access as well as ACLs. Turn that on.

### Encryption
Enforce encryption at the bucket level. There is no downside to transparent encryption. For AWS-KMS encryption, the only thing to worry about is KMS API limits. You get plenty of calls in the free tier before you will have to pay.

## AWS Config
* Track changes for production resources
* Alert on configuration drifts or unexpected changes
* Monitor and alert on common misconfigurations such as public S3 buckets/objects

## CloudTrail
* Enable CloudTrail in all regions
* Turn on log file validation in S3 so you can ensure delivery integrity 
* Enable access logging for the CloudTrail S3 bucket
* Turn on MFA delete for the CloudTrail bucket
* Enable bucket level encryption





