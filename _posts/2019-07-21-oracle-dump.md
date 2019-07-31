---
layout: post
title:  "Oracle Dump Export/Import"
categories: Oracle
tags: Oracle
author: moai
---

Export
```bash
exp userid=계정명/비밀번호 file=[file_path] tables=테이블명
```

Import
```bash
imp userid=계정명/비밀번호 file=[file_path]
```

`exp help=yes`를 통해 export에 사용 가능한 옵션 조회가 가능하다.  
export 한 서버와 import할 서버의 버전이 같지 않을 경우 import 시 통계 관련 에러가 발생 할 수 있다. 이 때는 `statistics=n` 옵션을 추가해
통계 임포트는 건너 뛸 수 있다.