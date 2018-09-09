---
layout: post
title: Docker에 Confluence 설치하기
excerpt: "Docker에 Confluence 설치하기"
categories: 
- docker
- confluence
comments: true
tags: 
- docker
- confluence
---
Docker에 Confluence 설치하기
```
docker run --name confluence -itd --restart always \
  --env 'JVM_MAXIMUM_MEMORY=2G' \
  --publish 18090:8090 \
  --volume /data/confluence:/var/atlassian/confluence \
  cptactionhank/atlassian-confluence
```

확인
```
docker ps
```

주소:18090 접속
  
---
##### 아래와 같은 오류가 날때
![2018-09-09 10 14 26](https://user-images.githubusercontent.com/18377818/45264857-1a01ba80-b47e-11e8-8845-95f1c83bdbc0.png)
Daemon(2) 계정에게 권한을 주면 됩니다.
```
docker exec -it -u root confluence bash
chown -R 2:2 /var/atlassian/confluence/
exit
```
---

접속후 설정화면  
외부 DB 연결시 '운영환경 설치'를 눌러주세요.  
![2018-09-09 10 18 32](https://user-images.githubusercontent.com/18377818/45264873-546b5780-b47e-11e8-9678-ad86575db06f.png)

에드온 설정
![2018-09-09 10 19 27](https://user-images.githubusercontent.com/18377818/45264880-6fd66280-b47e-11e8-87f1-37fbb3757f06.png)

라이선스 키 입력  
먼저 평가판으로 구축하셔도 나중에 구입후 라이선스를 변경할 수 있습니다.  
![2018-09-09 10 21 30](https://user-images.githubusercontent.com/18377818/45264901-ba57df00-b47e-11e8-9021-7de07c9ec24d.png)

다음
![2018-09-09 10 23 11](https://user-images.githubusercontent.com/18377818/45264987-e889ee80-b47f-11e8-9f31-e212d6751962.png)

MySQL 설정
>[Docker에 MySQL 설치하기](http://lyasee.com/articles/2018-09/Docker%EC%97%90-MySQL-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

디비 생성
```
CREATE DATABASE confluencedb CHARACTER SET utf8 COLLATE utf8_bin;
```

유저 생성
```
GRANT ALL PRIVILEGES ON confluencedb.* TO 'confluenceuser'@'%' IDENTIFIED BY '비밀번호';
flush privileges;
```

데이터베이스 설정
![2018-09-09 10 28 43](https://user-images.githubusercontent.com/18377818/45264986-e889ee80-b47f-11e8-9cba-f9f3c75f9d98.png)

데이터베이스 URL (READ-COMMITTED 옵션)
```
jdbc:mysql://아이피주소:포트번호/confluencedb?sessionVariables=tx_isolation='READ-COMMITTED'
```

구축된 jira가 있다면 jira와 연동하시면 됩니다.
