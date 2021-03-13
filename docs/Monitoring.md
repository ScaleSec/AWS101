# Monitoring
[TOC](https://github.com/ScaleSec/AWS101/blob/main/README.md#TOC)

## Cloudwatch
Cloudwatch is a large service which provides logging, monitoring, alerting, and events. The events part as been rebranded into a new service called EventBridge.

## Logging
CW is primarily used for logging. Many services integrate with CW to send logs without extra configuration (Lambda, ECS), other services can use the Cloudwatch Agent to ship logs.

## Monitoring
CW provides a ton of metrics to monitor and alert on. This can range from CPU usage to billing alerts.

## Cloudwatch Events
Service integrations work by sending JSON data, for the most part. CW can monitor events that are happening and then send that to a target. For example, an autoscaling event can be used to trigger an SNS topic. CW Events can also be used to trigger services similar to a cron job.

### Cloudwatch Event Examples
https://github.com/awsdocs/amazon-cloudwatch-events-user-guide/blob/master/doc_source/EventTypes.md

## CloudTrail
CloudTrail is an API logger. Almost every service will log an entry to a trail. It has a 90 day event viewer for free.

CloudTrail monitoring is essential for detecting attacks and forensics.