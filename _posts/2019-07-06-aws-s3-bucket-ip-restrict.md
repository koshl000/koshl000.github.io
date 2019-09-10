---
layout: post
title:  "IP 제한 버킷 정책"
categories: AWS
tags: AWS,S3
author: moai
---

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




다수의 ip일 경우는 다음과 같이 설정한다.
```json
"Condition" : {
        "IpAddress" : {
            "aws:SourceIp": [
                             "18.208.0.0/13",
                             "52.95.245.0/24"
                            ] 
        },
        "NotIpAddress" : {
            "aws:SourceIp": "192.168.143.188/32" 
        } 
    } 
 
```