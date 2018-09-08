---
layout: post
title: Docker에 postgreSQL 설치하기
excerpt: "Docker에 postgreSQL 설치하기"
categories: 
- docker
- postgresSQL
comments: true
tags: 
- docker
- postgresSQL
---
Docker에 postgreSQL 설치하기

```
docker run --rm --name postgres96 \ 
-e PGDATA=/data/pgdata \  
-e POSTGRES_INITDB_ARGS="--data-checksums -E utf8 --no-locale" \  
-e POSTGRES_DB=디비명 \  
-e POSTGRES_USER=계정명 \  
-e POSTGRES_PASSWORD=비밀번호 \  
--publish 15432:5432 \  
postgres:9.6.6
```
