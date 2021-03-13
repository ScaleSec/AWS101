# IAM Overview
[TOC](../README.MD#TOC)

## User
A user is an IAM identity which can be used to log in and generate access keys. IAM users have passwords and MFA.

## Role
Roles are identities that users and services can assume to perform some action. They cannot have access keys.

### Instance Profiles
EC2 uses an instance profile as a container for a role. A role is not directly attached to an instance, per se. 

## Group
Groups can contain users and have permissions applied to them

## Policy
Policies are JSON documents that define permission sets. Each service has granular permissions which can be independently granted. Actions that are not explicitly granted are denied.
