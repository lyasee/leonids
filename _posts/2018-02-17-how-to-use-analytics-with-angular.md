---
layout: post
title: How to use analytics with angular
excerpt: "앵귤러에 구글 아날리틱스를 적용하는 방법을 설명합니다."
categories: angular
comments: true
tags: 
- angular
- google analytics
---
앵귤러에 구글 아날리틱스를 적용하는 방법을 설명합니다.

기존 방법
```html
<script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
    m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

    ga('create', 'XXXXX', 'auto');
    ga('send', 'pageview');
</script>
```

이렇게 하게되면 [SPA(single-page application)](https://en.wikipedia.org/wiki/Single-page_application) 에서 경로를 구분하지 못함


- - -
변경  
1. **index.html** 수정 (project/index.html)
```html
<script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
    m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

    ga('create', 'XXXXX', 'auto'); // xxxx 개별코드
	// ga('send', 'pageview');
</script>
```

2. **google-analytics.service.ts 생성** *(project/app/services/google/google-analytics/google-analytics.service.ts)*

	`ng g service google-analytics`
    
    ```typescript
    import { Injectable } from '@angular/core';

    declare var ga: Function; // 구글 아날릭티스 메소드

    @Injectable()
    export class GoogleAnalyticsService {

        constructor() {
        }

        /**
         * 구글 아날리틱스에서 사용중 페이지 설정
         * @param url 사용 중인 페이지
         */
        pageView(url: string) {
            ga('send', 'pageview', url);
        }
    }
    ```
	
3. **app.component.ts 수정** *(project/app/app.component.ts)*

    ```typescript
	import { Component, OnInit } from '@angular/core';
    import { Location } from '@angular/common';
    import { Router } from "@angular/router";
    import { GoogleAnalyticsService } from "./services/google/google-analytics/google-analytics.service";

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent implements OnInit {

      currentRoute: any

      constructor(
        private router: Router,
        private location: Location,
        private googleAnalyticsService: GoogleAnalyticsService
      ){
        // 구글 아날리틱스 SPA 페이지 구분
        this.router.events.subscribe(() => {
          let newRoute = this.location.path() || '/';
          // 라우터가 변경될때 아날리틱스로 전송
          if (this.currentRoute != newRoute) {
              this.googleAnalyticsService.pageView(newRoute);
              this.currentRoute = newRoute;
          }
        });
      }

      ngOnInit(){
      }
    }
    ```
    
- - -
이벤트 추가 하는 방법(event)

1. **google-analytics.service.ts 수정** (project/app/services/google/google-analytics/google-analytics.service.ts)*

    ```typescript
    import { Injectable } from '@angular/core';

    declare var ga: Function; // 구글 아날릭티스 메소드

    @Injectable()
    export class GoogleAnalyticsService {

        constructor() {
        }

        /**
         * 구글 아날리틱스에서 사용중 페이지 설정
         * @param url 사용 중인 페이지
         */
        pageView(url: string) {
            ga('send', 'pageview', url);
        }

        /**
         * 구글 아날리틱스에서 이벤트 설정
         * 참고 URL http://analyticsmarketing.co.kr/digital-analytics/google-analytics/537/
         * @param category 이벤트 유형을 나타내는 기본단위, ex)모바일전화연결
         * @param action 이벤트 카테고리에 대한 설명, ex)버튼클릭
         * @param label 추가 세분화 용도로 사용, ex)오피스
         */
        pageEvent(category: string, action: string, label: string){
            ga('send', 'event', category, action, label);
        }
    }
    ```

2. **test.ts** 테스트용

	```typescript
    import { Component, OnInit } from '@angular/core';
    import { GoogleAnalyticsService } from '../../../services/google/google-analytics/google-analytics.service';

    @Component({
      selector: 'app-test',
      templateUrl: './test.html',
      styleUrls: ['./test.css']
    })
    export class TestComponent implements OnInit {

      constructor(
        private googleAnalyticsService: GoogleAnalyticsService
      ) { }

      ngOnInit() {
      }

      submit(){
        this.googleAnalyticsService.pageEvent('검색', '검색요청', '검색완료')
      }


    }
    ```