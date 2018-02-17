---
layout: post
title: How to create npm package with angular
excerpt: "간단한 프로젝트를 만들고, npm에 배포하는 과정을 설명합니다."
categories: angular
comments: true
---
간단한 프로젝트를 만들고, npm에 배포하는 과정을 설명합니다.

1. Angular-cli를 이용하여 프로젝트를 만듭니다.  
	``` ng new test-package```

2. pipes 폴더*(app/pipes)* 생성, pipes 폴더에서 highlight 모듈 파일 생성  
	``` ng g module highlight ```
    
3. highlight 폴더*(app/pipes/highlight)* 안에 pipe파일 생성  
	``` ng g pipe highlight```

4. pipe가 잘 작동하는지 확인해봅시다!

	*아래 소스를 해당 파일에 복사하면 됩니다.*
    
    **highlight.pipe.ts** *(test-package/src/app/pipes/highlight/highlight.pipe.ts)*
    ```typescript
    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({
      name: 'highlight'
    })
    export class HighlightPipe implements PipeTransform {

      transform(text: string, search): string {
        if(search == "" || search == undefined || search == null) return text;
        var pattern = search.replace(/[\-\[\]\/\{\}\(\)\*\+\?\.\\\^\$\|]/g, "\\$&");
        pattern = pattern.split(' ').filter((t) => {
          return t.length > 0;
        }).join('|');
        var regex = new RegExp(pattern, 'gi');

        return search ? text.replace(regex, (match) => `<strong class="highlight">${match}</strong>`) : text;
      }

    }
    ```
    
    **highlight.module.ts** *(test-package/src/app/pipes/highlight/highlight.module.ts)*
    ```typescript
    import { NgModule } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { HighlightPipe } from './highlight.pipe';

    @NgModule({
      imports: [
        CommonModule
      ],
      declarations: [HighlightPipe]
    })
    export class HighlightModule { }
    ```
    
	**app.module.ts** *(test-package/src/app/app.module.ts)*
    ```typescript
	import { BrowserModule } from '@angular/platform-browser';
	import { NgModule } from '@angular/core';

	import { AppComponent } from './app.component';
    import { HighlightModule } from './pipes/highlight/highlight.module';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        HighlightModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```
    
    **app.component.ts** *(test-package/src/app/app.component.ts)*
    ```typescript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      title = 'angular npm package';
      search = 'package'
    }
    ```
    **app.component.html** *(test-package/src/app/app.component.html)*
    ```html
    <div style="text-align:center">
      <p [innerHtml]="title | highlight : search"></p>
    </div>
    ```
    
    `ng serve`
    
    localhost:4200 결과 확인  
    angular npm **package**
    
5. 이제 만든 모듈을 배포할 준비 단계

	[ng-packagr](https://www.npmjs.com/package/ng-packagr)를 이용합니다.  
    ```yarn add ng-packagr ```
    
    package.json와 같은 경로에 public_api.ts, ng-package.json 파일 생성!  
    해당 파일에 소스를 복사!
    
    **public_api.ts** *(test-package/public_api.ts)*
    ```typescript
    export * from './src/app/pipes/highlight/highlight.pipe';
    export * from './src/app/pipes/highlight/highlight.module';
    ```
    
    **ng-package.json** *(test-package/ng-package.json)*
	```json
	{
		"$schema": "./node_modules/ng-packagr/ng-package.schema.json",
		"lib": {
			"entryFile": "public_api.ts"
		}
	}
    ```
    
    **package.json** *(test-package/package.json)*
    ```json
{
	"name": "test-package",
	"version": "0.0.0",
	"license": "MIT",
	"scripts": {
		"ng": "ng",
		"start": "ng serve",
        "build": "ng build --prod",
        "test": "ng test",
        "lint": "ng lint",
        "e2e": "ng e2e",
        "packagr": "ng-packagr -p ng-package.json" // 추가
	},
	"private": false, // 배포 허용
    ...
}
	```

6. 배포!

	터미널에 `yarn packagr` 명령어 입력!  
    dist파일이 생성됩니다.
    
    **npm-test-package**라는 새로운 폴더를 만듭니다.  
    (test-package폴더 밖에)
    dist폴더 안에 내용을 **npm-test-package**폴더로 전체 복사
    
    터미널에 `npm adduser`로 npm 계정을 추가해주세요.  
    이제 **npm-test-package**폴더 안에서 `npm publish` 명령어를 입력하는 순간 바로 npm사이트에 업로드 됩니다!  
    (package.json안에 "name"부분이 패키지 이름이 됩니다, "version"도 수정해서 올려주세요~)
    

