---
layout: post
title: MongoDB Sort operation used more than the maximum 33554432 bytes of RAM
excerpt: "Sort operation used more than the maximum 33554432 bytes of RAM"
categories: 
- mongodb
comments: true
tags: 
- mongodb
- mongodb error
---
mongodb sort 작업이 32MB 메모리 이상을 사용하는 경우 나는 오류입니다.  
  
![스크린샷 2019-09-04 오전 10 59 17](https://user-images.githubusercontent.com/18377818/64220931-07418a80-cf06-11e9-9b65-6ac5995755c6.png)
  
해결 방법  
1. 인덱스 추가 (unique)
2. 더 작게 limit 걸기
3. *internalQueryExecMaxBlockingSortBytes* 를 이용하여 버퍼 크기 조정
  
3번 방법을 이용하여 128MB 로 설정하기
```
> db.adminCommand({"setParameter": 1, "internalQueryExecMaxBlockingSortBytes" : 134217728})
```