---
layout: post
title:  "Jquery load/ready 차이점"
categories: AWS
tags: AWS
author: moai
---
출처 : [https://www.youtube.com/watch?v=JDLZY1xhzQs&list=WL&index=7&t=404s][youtube]
# 고려사항
## EC2
- 단 한대의 인스턴스를 사용하더라도 Auto Recovery 설정 혹은 Auto Scale Group을 통하여 장애 발생시 자동으로 인스턴스 교체
- Multi AZ(가용영역) 구현으로 AZ Failure시 대응

## S3
- 워크로드가 초당 100개 이상의 요청이 예상될 경우 순차적 키 이름 대신 랜덤키를 사용
 ```
exbucket/2019-07-25-15:00:00/ccut1124/photo1.jpg    ->    exbucket/11aa-2019-07-25-15:00:00/ccut1124/photo1.jpg   
exbucket/2019-07-25-15:00:00/ccut1123/photo1.jpg    ->    exbucket/13cd-2019-07-25-15:00:00/ccut1123/photo1.jpg   
exbucket/2019-07-25-15:00:00/ccut1241/photo1.jpg    ->    exbucket/93av-2019-07-25-15:00:00/ccut1241/photo1.jpg   
exbucket/2019-07-25-15:00:00/ccut3124/photo1.jpg    ->    exbucket/83dd-2019-07-25-15:00:00/ccut3124/photo1.jpg   
```
## CloudFormation
- 리소스를 코드로 관리해주는 서비스(ex.ec2인스턴스 생성 call!)
- 한 스택에 특정 종류의 리소스가 많을 경우 API 호출이 과다하게 일어나 리소스 생성 실패 가능
    - DynamoDB Table 동시 생성 개수 제한 
    - EC2 인스턴스 수십 대 동시 생성 
- DependsOn 속성을 사용하여 리소스 생성 순서를 정해주면 장애 방지 가능
## Elastic Beanstalk
- Elastic Beanstalk Health Check는 장애 발생 시 Elastic Beanstalk 환경 상태를 바꾸어 사용자에게 경고하지만 WAS 재시작/인스턴스 교체 등의 복구는 하지 않음
- Elastic Beanstalk 환경의 Auto Scaling Group Health Check를 EC2에서 ELB로 변경하여 ELB Health Check 실패 시 인스턴스를 교체하도록 설정하면 장애 발생 시 자동 복구 가능
## OpsWorks
- OpsWorks는 Auto Healing 기능을 통해 5분 이상 인스턴스가 OpsWorks Endpoint와 통신에 실패할 경우 인스턴스를 Stop 후 Start 함
- 따라서 Auto Healing이 Disable된 Layer에 인스턴스들을 배치할 경우 NAT Instance에 장애 발생 시 Private Subnet에 위치한 인스턴스들이 단체로 재부팅 되는 현상을 막을 수 있음


# 장애대응
## EC2
### 시스템 상태 확인
#### 유형1:상태 확인 이슈

#### 유형 1:시스템 상태 확인 이슈 시 해결방법

## ELB
[youtube]: https://www.youtube.com/watch?v=JDLZY1xhzQs&list=WL&index=7&t=404s