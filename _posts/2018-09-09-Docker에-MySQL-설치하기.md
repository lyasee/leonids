---
layout: post
title: Docker에 MySQL 설치하기
excerpt: "Docker에 MySQL 설치하기"
categories: 
- docker
- mysql
comments: true
tags: 
- docker
- mysql
---
Docker에 MySQL 설치하기

```
docker run -d -p 13306:3306 \
	-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
    --name mysql \
	mysql:5.6
```

옵션 바꾸기
```
docker update --restart always mysql
```