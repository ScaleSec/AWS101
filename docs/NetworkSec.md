# Network Security
[TOC](https://github.com/ScaleSec/AWS101/blob/main/README.md#TOC)

## Regions vs Zones

### Regions
A geographical area containing many data centers

### Availability Zone 
Basically an isolated data center, but your zone assignments will not be the same as another AWS account i.e. your us-east-1a is not always the same as my us-east-1a. Not all accounts or regions will have the same zones.

## VPC
A Virtual Private Network is the core of AWS networking. VPCs are regional as a whole e.g us-east-1. Almost all AWS services will live inside a VPC.

VPCs have a set CIDR but a secondary CIDR can be added. Primary CIDRs cannot be changed.

### Subnets
Subnets are part of a VPC. Their ranges must be part of the VPC CIDR. Subnets are zonal e.g. us-east-1a

It is recommended for production to have 3 zones, so 3 subnets per VPC. 

Subnets can be public or private depending on their routing table.

### Route Tables
A route table is associated with a subnet. It contains routing info for the subnet e.g `10.0.0.0/16 -> local`

Most of your network headaches will be discovered within a route table

## ACL / NACL
* Defined at the VPC:subnet level
* Stateless - you need a matching inbound/outbound rule
* Can deny traffic
* Usually set up for minimal filtering
  * Well known ports: 22, 80, 443, 3306, etc
  * Ephemeral ports depending on OS and cloud

## Security Group
* Defined per resource
* Stateful
* Each SG lives inside a VPC
* No deny rules
* Configured for least privilege
* Specific to a use case - 1 instance, 1 database, 1 environment, etc

## Internet Gateway
A managed appliance which can replace the need for a public IP. If you have multiple instances and don't want them to have public IPs, you will need an internet gateway in a public subnet and corresponding route table entries to the IG and Nat Gateway.

## Nat Gateway
A Nat Gateway resides in a public subnet and provides an egress only mechanism for traffic to flow to the internet. It connects to an IG. Egress-only gateways are the same thing but for IPv6 traffic.

## Privatelink/Endpoints
Privatelink is a service for traffic to stay inside the AWS backbone. All traffic to supported services will not touch the public internet. S3 and Dynamo use gateway endpoints and are free. All other services use interface endpoints which are not free but cheaper than NG + IG in select cases. If you need access to only a few APIs in a production environment, it will be cheaper. 

## Misc Info

### Firewalls
* Filtered - Silently dropped. Useful if you don't want to give away that something is inaccessible. Do this at the perimeter. 

* Closed - Send a TCP RST or Destination Unreachable. Better for internal networks where you are comfortable giving away information about listening/non-listening ports.

A packet isn't getting to your server either way, but filtering is just one tiny extra step to make a hacker's job harder.

### HTTP Codes
* 401 - You are authenticated but not authorized
* 403 - You are not authenticated but also misconfigured apps will send this instead of a 401
* 404 - Not found, keep scanning
* 429 - please stop

* 500 - The request made the server break
* 501 - Not implemented: I don't know what you are asking 
* 502 - Bad gateway: Origin did not respond correctly
* 503 - Origin overloaded or dead
* 504 - Origin server timeout
