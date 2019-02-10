---
layout: post
title: Angular Deploy with firebase
subtitle: ''
categories: devlog
tags: angular
comments: true
---


## 1. Firebase란?

Firebase는 Google에서 제공하는 서비스 중 하나입니다. 호스팅부터, Database, Email인증 등 다양한 기능을 제공합니다. 특히 저같은 프론트엔드 개발자에게는 Database 및 인증서비스는 백엔드 없이도 간단한 프로젝트를 할 수 있는점에서 큰 도움이 됩니다. 프로젝트 4개와, 일정 트레픽 이하는 무료로 사용할 수 있습니다.

--- 

## 2. Angular with Firebase 


Firebase를 배포하는 방법은 여러가지 있습니다. 

우선 콘솔에서 파이어베이스 프로젝트를 생성합니다.

![console](https://user-images.githubusercontent.com/34129711/52530819-9f9bd600-2d4e-11e9-8077-d13847043854.png)

Analytics가 필요하시면 같이 체크해주시면 됩니다.

![create](https://user-images.githubusercontent.com/34129711/52530829-c22def00-2d4e-11e9-911a-bb1b16aafd3d.png)

완성되면, 가운데 웹 버튼을 눌러서 키를 발급받으면 됩니다.

![console1](https://user-images.githubusercontent.com/34129711/52530834-de319080-2d4e-11e9-8cfd-413eb67fab4c.png)

![key](https://user-images.githubusercontent.com/34129711/52530835-df62bd80-2d4e-11e9-97ea-f15583ca780a.png)


그 다음으로는 여러가지 방법이 있습니다.

Angular의 경우 index.html을 제외한 나머지 컴포넌트에서 `<script>` 태그를 불러올 수 없습니다.

공식문서에 설명된 것 처럼 index.html에 그대로 붙여도 됩니다만, 그렇게되면 고유 key가 노출이 되기 때문에 사용하지 않는 것을 권합니다.


--- 


## 3. Firebase (npm)

    - 첫번째 방법으로는 가장먼저 시작하는 app.component.ts에서 작업하는 방법이 있습니다.

    - app.component.ts에서 firebase를 initialize를 시키는 방법입니다.

---

$ npm install firebase

---

![firebase](https://user-images.githubusercontent.com/34129711/52530880-0372ce80-2d50-11e9-9266-2032046dd7b9.png)

firebase를 import한 뒤, firebase key에서 config를 복사합니다.
constroctor 다음으로 실행하는 ngOnInit안에서 firebase 자체 함수인 initializeApp을 실행시면됩니다.


## 4. angularFire2

    - 두번째 방법으로는 AngularFire2를 이용하는 방법입니다.

    - Angular에서 firebase를 쉽게 사용하기 위한 라이브러리 입니다.

[@angular/fire](https://www.npmjs.com/package/@angular/fire)

---

$ npm install angular/fire 

---

    - install 한 뒤, environment.ts에 firebase key를 붙입니다.

![env](https://user-images.githubusercontent.com/34129711/52530961-74ff4c80-2d51-11e9-8689-7c6d77dc28a7.png)


    - app.module.ts에서 불러오면 됩니다.

![app-module](https://user-images.githubusercontent.com/34129711/52530984-c0195f80-2d51-11e9-8045-71924ca50120.png)

    - 초기화 외에, 인증 등 필요한 기능은 app.module.ts에서 imports에 넣어서 쓰시면 됩니다.

## 5. Deploy

    - 초기화 한 뒤는 어렵지 않습니다.

---

$ npm install -g firebase-tools
$ firebase login
$ firebase init

---



    - 명령어를 실행시키면 firebase에 대해 로그인을 하고, 초기화를 합니다. 이때 firebase가 어떻게 사용할 것인지에 대해 묻는데, 맞게 키를 입력해주시면 정상적으로 초기화 셋팅이 됩니다.



---

$ firebase deploy

---


    - 마지막으로 deploy하면, deploy한 뒤 console창에 해당 url이 나옵니다.




참고자료
    - [Google Firebase 문서](https://firebase.google.com/docs/)

    - [angular/fire](https://www.npmjs.com/package/@angular/fire)
