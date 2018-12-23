---
layout: post
title: 'JS new Date()'
subtitle: 'Safari 브라우져의 예상못한 오류'
categories: devlog
tags: javascript
comments: true
---

201807/26 작성한 포스트를 옮겨 옵니다. [원문 link](https://github.com/bluelion2/bluelion2.github/blob/master/_posts/2018-07-26-JS%20new%20Date().md)

# JS new Date()함수때문에 일어난 삽질


현재 한 여행사의 프론트엔드 개발자로 입사해서 열심히 일하고 있는 개발자 입니다.

날짜 비교를 통한 연령구분 함수를 만들어서 테스팅 중,

Chrome 에서는 날짜계산을 잘 해서 정확한 결과값을 도출하는데,

Safari로 테스트를 할 때에는 날짜계산을 못해서 이상한 값을 반환하는 상황이 발생하였습니다.


Safari에서 테스트를 안해봤지만, 같은 코드이길래 당연히 똑같은 결과를 반환할 줄 알았기 때문에,

그 부분을 담당했던 저로써는, 당황할 수 밖에 없었습니다.

또한 가장 중요한 부분중 하나기 때문에 즉각 처리를 해야 했죠.



---


## 1. 우선 중간중간마다 결과값을 찍어보기로 했습니다.


    '중간의 결과값이 잘못나와서 잘못 되었을 것이다.'

    라는 전제를 깔고, console과 모바일에서 볼 수 있게끔 alert에 값을 담아서 확인을 하였습니다.

    테스트 결과 크롬 및 안드로이드 폰에서는 잘 나오고, 모바일 및 맥북 사파리에서는 결과값을 제대로 못한다는 것을 알게 되었습니다.


---


## 2. 코드에 문제가 아니다.


    Safari에서만 결과값을 제대로 반환하지 못하기 때문에, 브라우저에 문제가 있을거라 판단하고,

    구글신께 빌면서 찾기 시작했습니다.

    다행히, 이런경우가 많은지 쉽게 찾을 수 있었습니다.

    참고 : https://stackoverflow.com/questions/4310953/invalid-date-in-safari


---


    원인은 ECMA2015부터는, ISO 8601 날짜 전용 문자열은 UTC로 구문 한다고 합니다.

    하지만, Safari, Explorer에서는 인식을 못해서, 결과값을 NaN으로만 반환할 뿐이죠.

    Ex) "201-07-26"

    참고 : https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/parse


---


## 3. 해결하기

    결과적으로는 정규식을 이용하는 꼼수를 사용해서 "-"를 "/"로 교체하거나, 처음부터 /를 사용해야 했습니다.

    Ex) console.log (new Date('2018-07-26'.replace(/-/g, "/")));

    참고하여 다시 코드를 짜니, Safari에서도 정상 작동하면서, 무사히 퇴근할 수 있었습니다.... ㅋㅋㅋ;;

