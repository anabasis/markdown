# ANGULAR

교육 : <https://www.youtube.com/watch?v=BpLYqHJHAdU&list=PLlSZlNj22M7SC6zsFLaMplXe2RrO-R9S5>, <http://coded.kr/>

## ATOM Angular

atom-typescript

<https://medium.com/witinweb/angular-2-cli-%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-11a188e17223>

## 설치

```bash
npm install ionic cordova -save
```

ionic 모바일 프레임워크
cordova smart폰 장치 제어 라이브러리

```bash
npm install -g @angular/cli

# 프로젝트 생성
ng new my-angular
cd my-angular

# 서버기동
ng serve
```

angular-cli.json
package.json

Component
.html .css .ts

컴포넌트 생성
ng generate component about
ng g c about

|

```js
[index.html]
<app-root></app-root>
```

```js
[main.ts]
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

// bootstrapModule
```

```js
[./app/app.module]
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { AboutComponent } from './about/about.component';
```

BrowserModule : <https://angular.io/api/platform-browser/BrowserModule>
NgModule : <http://poiemaweb.com/angular-module>