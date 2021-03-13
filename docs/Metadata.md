## Metadata service  
[TOC](https://github.com/ScaleSec/AWS101/blob/main/README.md#TOC)

The metadata service is an http endpoint available to all EC2 based services to get instance info and credentials. There are two versions:

### V1
* Default
* No authentication
* Provides all instance secrets such as role credentials, signature, etc

If you can get to the metadata, you can use the IAM role credentials to do stuff as the instance

ECS on EC2 can also access metadata

Fargate has a separate but almost identical service without credentials, so it is not a big deal

```shell
curl http://169.254.169.254/latest/meta-data/
```

### V2
V2 of the metadata service is authenticated. You have to request a token which has an expiry and is only valid for that instance. It also has a hop limit (TTL = 1 by default) to prevent SSRF. The token service will also reject a PUT request if it contains `X-Forwarded-For` which is automatically added by things such as an ELB. 

```shell
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
&& curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/
```

### Configuring metadata service
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html#configuring-instance-metadata-options

## User Data
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-add-user-data.html

User Data is base64 encoded arbitrary data available to the instance. It is set on instance creation. It usually contains scripts or cloud-init commands used during instance creation. This can sometimes contain secrets so its worth scraping. User data should not contain secrets, use an API call to a secrets manager instead of hard coding secrets. 