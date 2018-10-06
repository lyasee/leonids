---
layout: post
title: Docker에 Nexus 설치하기
excerpt: "Docker에 Nexus 설치하기"
categories: 
- docker
- nexus
comments: true
tags: 
- docker
- nexus
---

Docker Nexus install 

Nexus 데이터를 저장할 디렉토리
Nexus 는 user 를 200 으로 사용
```
sudo mkdir /data/nexus
sudo chown -R 200 /data/nexus
``` 

실행
```
docker run -itd \
-p 8081:8081 \
--restart always \
--name nexus \
--volume /data/nexus:/nexus-data \
sonatype/nexus3
``` 

접속
localhost:8081 (아이피:8081) 

초기 아이디, 비밀번호
admin
admin123