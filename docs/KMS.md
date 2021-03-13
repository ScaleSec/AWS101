# KMS Overview
[TOC](../README.MD#TOC)

AWS Key Management Service manages all encryption key material. 

KMS uses envelope encryption to provide encryption at scale.

## AWS Managed Keys
AWS automatically generates KMS keys that are specific to an account, region, and service. These can be used in place of your own generated keys.

## Customer Managed Keys
You can generate your own keys and control the rotation and access. Access is managed with a key policy and IAM access.

Access to a key should be split into a key administrator (someone who can manage but not use the key) and a key user (someone who can decrypt/encrypt but not manage the key).

### Key Policies
A key policy designates who is allowed to access the key. It is regional and only one policy exists per key. The entire account has access to a new key by default.

### IAM
IAM policies can be used to grant access to a key as long as the key policy allows it. 

