---
layout: post
title:  "Windows Docker 환경변수 확인"
search: true
category: docker
last_modified_at: 2020-09-15
---

docker windows container 를 사용하다 보면 docker 내 환경변수 값을 확인할 필요가 있다.
```bash
c:\>echo "$env:path"
```
처럼 커맨드를 입력하면 path 에 등록된 경로룰 출력할 수 있다.
