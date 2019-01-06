---
layout: post
title: Angular Universal 적용기1
subtitle: ''
categories: devlog
tags: angular
comments: true
---

---


## 1. Angular Universal 적용기


안녕하세요. 강승훈 입니다.

Angular universal에 대해 알아보고, 어떻게 적용하려 했는지에 대해 기록해보기 위해 작성합니다.

Universal에 대해 한글로 번역된 글이 그다지 없는 것 같더군요. Universal이 그만큼 쓸 일이 없어서 일 수도 있겠지만요.


---


### 적용하려는 이유


- 1. 첫 페이지 로딩 속도 향상


현재 운영하고 있는 사이트의 lazy loading 및 로직 향상을 통해 첫 페이지 로딩속도를 많이 줄였지만, 그래도 더 빨리 로드시키는 것을 목표로 했기 때문에, Angular Universal을 이용한 서버사이드 렌더링을 적용하려 하였습니다.


- 2. SEO 문제


SPA의 단점 중 하나인 SEO에 대해 해결하기 위해서는 서버 사이드 렌더링이 필요하다 판단되었습니다.


- 3. SSR(Server Side Rendering)과 CSR(Client Side rendering)

    -   SSR은 서버에서 렌더링 후 내려줌 / CSR은 HTML, CSS를 브라우저에 내려줌으로써 브라우저가 렌더링함.

    -   결국은 어디에서 렌더링을 하냐의 차이

    -   각각의 장단점이 있음. (서버비용, 크롤링, 첫 페이지 로딩...)

    -   [서버사이드 렌더링 그리고 클라이언트 사이드 렌더링](http://asfirstalways.tistory.com/244)


---


### Angular Universal 개요


- Angular Universal 이란

    -   Angular를 서버사이드 렌더링을 하기 위해 개발.

    -   [Angular Universal](https://angular.io/guide/universal)

    -   build시 dist안에 기존의 빌드파일 외의 별도로 server와 관련된 server.js가 새로 생성됨.

    -

