---
layout: post
title: '이미지 맵 태그'
subtitle: '쉽고 빠르게 이메일 광고 버튼만들기'
categories: devlog
tags: javascript
comments: true
---
 

## Display: none과 visibility: hidden의 차이


6/28일 인터뷰때 받은 질문입니다.

html의 요소를 JS의 상태에 따라 가리기위해 display:none을 주로 사용하기에 visibility:none  속성에 대해서는 잘 알지 못했다. 이번 기회에 둘을 써보고 비교해보겠다.


--- 

### CSS Visibility

CSS의 Visibility 속성은 레이아웃을 변경하지 않고, 요소를 보이거나 숨기는 역할을 한다.

visible: 요소가 보임
hidden: 요소가 숨겨진체 보이지 않게 됨. 숨겨진 요소는 탭 인덱스로 탐색을 할 수 없다.
collapse: table의 행, 열 등의 요소에 지정할 수 있으며, 적용하게 되면 display: none처럼 요소를 숨기고 차지한 공간을 제거함. 다른 요소에서는 hidden과 동일하다. 


### CSS Display

CSS의 Display는 다양한 방법으로 사용하고 있다. MDN문서에 따르면, CSS 속성은 요소를 블록과 인라인 요소 중 어느 쪽으로 처리할지, 그리고 그리드나 플렉스처럼 자식 요소를 배치할 때 사용할 방식을 설정합니다.

display: none을 설정하게 되면, 해당 요소를 보이거나 숨기는 것 뿐만 아니라, 차지한 공간까지도 제거할 수 있다. 

![1](https://user-images.githubusercontent.com/34129711/60671525-3e67d100-9eae-11e9-946d-3b3d85075b2a.png)

- Chrome으로 HTML 요소를 살펴보면, "<div>"태그에 하나는 display: none, 다른 하나는 visibility: hidden이 적용되어있으며, 자식요소로 "<p>"태그를 가지고 있다. 

![2](https://user-images.githubusercontent.com/34129711/60671523-3dcf3a80-9eae-11e9-98a1-196849fc64b9.png)

-  display: none의 경우 하위 자식요소들까지 전부 제거한 반면, visibility: hidden은 width와 height를 지정함으로써 가려지지만, 영역은 차지하고 있다.

![3](https://user-images.githubusercontent.com/34129711/60671526-3e67d100-9eae-11e9-838d-beda68b92599.png)

-   Visibility: hidden은 자식요소는 가리지 않고 보여주고 있다.


---

### 결론

1. display: none을 설정하면, 자식 요소들까지도 보이지 않는다.
2. visibility: hidden을 설정하면, 해당 요소는 보이지 않지만, visibility를 visible로 설정한 자손은 화면에 보인다. 

접근성을 고려하였을 때, 
1. visibility: hidden은 접근성 트리에서 제거된다. 따라서 해당 요소와 자식요소는 스크린리더가 읽을 수 없다. 
2. 마찬가지로 display: none을 설정하면, 접근성 트리에서 제거되며 해당 요소 및 자식요소를 스크린리더가 읽을 수 없게된다. 


참고 : [MDN: CSS - Visibility](https://developer.mozilla.org/ko/docs/Web/CSS/visibility)

[MDN: CSS - Display](https://developer.mozilla.org/ko/docs/Web/CSS/display)