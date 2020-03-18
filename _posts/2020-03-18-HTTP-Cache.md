---
layout: post
title: "HTTP Cache에 대해서"
subtitle: ""
categories: devlog
tags: exp
comments: true
---

현재 재직중인 회사에서는 2주간 스프린트를 거쳐서 주기적으로 배포를 하고 있다.
문제는, 이번 스프린트에서 배포 후 CS인입이 갑자기 폭증하였는데, 주로 "사이트 접속을 하면 갑자기 아무것도 보이지 않는다" 는 이슈였다.

- 해당 이슈는 "나의 비밀번호 찾기 페이지 url를 들어가면 로그인 창으로 가는 문제"였다.

해당 건을 처리하면저 React Route 설정을 수정했는데, 자동로그인 기능을 통해서 바로 접속하는 고객들이 빈화면으로 가게 되는 이슈인데, 다른 개발자 분들과 이야기 해보니 웹브라우저 캐싱 문제일 수도 있겠다는 의견이 있어서, 웹 캐싱에 관련해서 찾아보았다.

---

### 웹 캐시 (Web Cache)

웹 캐싱 또는 HTTP 캐싱은 서버에서 내려주는 정보를 임시 저장하는 정보 기술이다. (위키피디아)
이전에 가저온 성능을 잠시 저장했다가, 재사용한다면 리소스의 표현에 필요로 하는 시간을 줄일 수 있다. 하지만 잘못 사용할 경우, 고객들에게 실시간으로 정보를 올바르게 주지 못하게 될 수 있다.

---

### 참고자료

[Mozila Web Cache](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching)
[mozila Cache Control](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Cache-Control)
[웹 캐시](https://goddaehee.tistory.com/171)
[Google Developer : http Caching](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=ko)
[알아둬야 할 HTTP 쿠키 & 캐시 헤더](https://www.zerocho.com/category/HTTP/post/5b594dd3c06fa2001b89feb9)
