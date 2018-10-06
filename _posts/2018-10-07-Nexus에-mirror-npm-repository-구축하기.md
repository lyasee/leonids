---
layout: post
title: Nexus에 Mirror NPM Repository 구축하기
excerpt: "Create Mirror NPM Repository"
categories: 
- npm
- nexus
comments: true
tags: 
- npm
- nexus
---

## 1. Create Repository  
![2018-10-07 12 25 39](https://user-images.githubusercontent.com/18377818/46572967-8a422400-c9c9-11e8-95be-9cdcb4bc01dd.png)  
npm (hosted) > npm (proxy) > npm (group) 를 이용하여 만들겠습니다.
![2018-10-07 12 26 08](https://user-images.githubusercontent.com/18377818/46572977-a776f280-c9c9-11e8-9024-977456177e8a.png)  

## 2. Private Repository  
Repository > npm (hosted) 를 선택하고 아래와 같이 입력해주세요.  
![2018-10-07 12 31 14](https://user-images.githubusercontent.com/18377818/46572980-ac3ba680-c9c9-11e8-88b0-73069c8fc31d.png)  

## 3. Proxy Repository  
Repository > npm (proxy) 를 선택하고 아래와 같이 입력해주세요.  
![2018-10-07 12 28 38](https://user-images.githubusercontent.com/18377818/46572979-ac3ba680-c9c9-11e8-8bed-0c56326a817a.png)  
Remote storage에 공식 npm resitry 주소를 입력해주세요.  

## 4. Group Repository  
Repository > npm (group) 를 선택하고 아래와 같이 입력해주세요.  
![2018-10-07 12 32 07](https://user-images.githubusercontent.com/18377818/46572981-ac3ba680-c9c9-11e8-8f92-479f27b649e6.png)  
Private, Proxy를 그룹화하여 단일 URL로 관리할수 있게 합니다.  

## 5. 위에서 만든 Repositroy를 사용하도록 설정
'계정:비밀번호' 의 해시를 생성합니다.  
```
echo -n 'admin:admin123' | openssl base64
```
생성된 해시: **_auth=YWRtaW46YWRtaW4xMjM=**  
  이제 .npmrc 에 아래와 같이 입력해주세요.  
```
registry=http://아이피:8081/repository/npm-group/
_auth=YWRtaW46YWRtaW4xMjM=
```  
**Window**  
 C:\Users\게정명\AppData\Roaming\npm\etc\npmrc  
**Mac**  
 vi ~/.npmrc  

## 6. 테스트  
```
npm init
npm i axios
```  
배포하려면 package.json에 아래와 같이 추가해주세요.
```
{
  ...

  "publishConfig": {
    "registry": "http://your-host:8081/repository/npm-private/"
  }
}  

npm publish
```