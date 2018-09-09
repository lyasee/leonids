---
layout: post
title: Docker에 Jira 설치하기
excerpt: "Docker에 Jira 설치하기"
categories: 
- docker
- jira
comments: true
tags: 
- docker
- jira
---
Docker에 Jira 설치하기

지라 데이터 저장 공간 만들기
```
sudo mkdir -p /data/jira
```

지라 설치
```terminal
docker run --name jira -itd --restart always -u root \
    --env 'JVM_MAXIMUM_MEMORY=2G' \
    -p 18080:8080 \
    --volume /data/jira:/var/atlassian/jira \
    cptactionhank/atlassian-jira-software
```

확인
![2018-09-09 8 07 07](https://user-images.githubusercontent.com/18377818/45263817-f6ce0f80-b46b-11e8-90ea-78d1cb3618d8.png)

지라 서버 접속
주소:18080 (localhost:18080)

실행이 되고있나 확인
```
docker ps
```

이런 에러가 날 경우
![2018-09-09 8 08 50](https://user-images.githubusercontent.com/18377818/45263834-44e31300-b46c-11e8-919f-53ed2809ead7.png)

jira 이미지에서 Daemon(2) 권한 주기
```
docker exec -u root -it jira bash
chown -R 2:2 /var/atlassian/jira/
exit
```

직접 설정하기
![2018-09-09 8 46 59](https://user-images.githubusercontent.com/18377818/45264084-b2de0900-b471-11e8-803d-8c06c8b2da03.png)
![2018-09-09 8 48 06](https://user-images.githubusercontent.com/18377818/45264085-b2de0900-b471-11e8-9e54-f3fa30376e1a.png)

만약 연결할 디비가 없다면 도커에 설치해서 사용
````
docker run -d -p 13306:3306 \
--restart always \
-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
--name mysql \
mysql:5.6
````

#### MySQL 설정

디비 추가
```
CREATE DATABASE jiradb CHARACTER SET utf8 COLLATE utf8_bin;
```

계정 추가
```
GRANT ALL on jiradb.* TO 'jirauser'@'%' IDENTIFIED BY '비밀번호';
flush privileges;
```

MySQL 접속 정보 입력
![2018-09-09 8 52 35](https://user-images.githubusercontent.com/18377818/45264115-529b9700-b472-11e8-8718-e6de8c1a5a4b.png)

다음
![2018-09-09 8 56 32](https://user-images.githubusercontent.com/18377818/45264155-fedd7d80-b472-11e8-9cbd-f7b4c150be5e.png)

라이선스 키 입력
없는 경우 'MyAtlassian에서 Jira 평가판 라이선스를 생성'
![2018-09-09 9 03 58](https://user-images.githubusercontent.com/18377818/45264207-ecb00f00-b473-11e8-8f9b-997947696dba.png)

다음
![2018-09-09 9 08 11](https://user-images.githubusercontent.com/18377818/45264239-87a8e900-b474-11e8-9696-e5b538de7eb1.png)

다음
![2018-09-09 9 09 12](https://user-images.githubusercontent.com/18377818/45264252-ac04c580-b474-11e8-918f-54b9412575f1.png)

다음
![2018-09-09 9 10 10](https://user-images.githubusercontent.com/18377818/45264260-c343b300-b474-11e8-940a-23f0841e00da.png)

다음
![2018-09-09 9 10 36](https://user-images.githubusercontent.com/18377818/45264268-dbb3cd80-b474-11e8-85c6-9aa5638daf98.png)

완료
![2018-09-09 9 10 49](https://user-images.githubusercontent.com/18377818/45264269-dbb3cd80-b474-11e8-932f-63c242055b6a.png)
