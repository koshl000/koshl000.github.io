---
layout: post
title:  "IP 제한 버킷 정책"
categories: aws
tags: aws,s3
author: moai
---
출처:https://s3tools.org/kb/item10.htm  

How to restrict access to a bucket to specific IP addresses
To secure our files on Amazon S3, we can restrict access to a S3 bucket to specific IP addresses.

The following bucket policy grants permissions to any user to perform any S3 action on objects in the specified bucket. However, the request must originate from the range of IP addresses specified in the condition. The condition in this statement identifies 192.168.143.* range of allowed IP addresses with one exception, 192.168.143.188.
```json
{
    "Version": "2012-10-17",
    "Id": "S3PolicyIPRestrict",
    "Statement": [
        {
            "Sid": "IPAllow",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*" 
            },
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::bucket/*",
            "Condition" : {
                "IpAddress" : {
                    "aws:SourceIp": "192.168.143.0/24" 
                },
                "NotIpAddress" : {
                    "aws:SourceIp": "192.168.143.188/32" 
                } 
            } 
        } 
    ]
}
```
