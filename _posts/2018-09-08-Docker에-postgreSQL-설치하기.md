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
####Docker에 postgreSQL 설치하기

공홈
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

또는

```
run --name postgresql -itd --restart always \
  --env 'DB_USER=계정명' --env 'DB_PASS=비밀번호' \
  --env 'DB_NAME=디비명' \
  --publish 포트:5432 \
  --volume 경로(postgresql 정보를 저장할):/var/lib/postgresql \
  sameersbn/postgresql:9.3
```