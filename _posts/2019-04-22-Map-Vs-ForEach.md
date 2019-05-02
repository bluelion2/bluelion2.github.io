---
layout: post
title: 'Map vs forEach'
subtitle: ''
categories: daily_read
tags: daily_read
comments: true
---

# 19.04.22 오늘의 글


[Map vs forEach](https://codeburst.io/javascript-map-vs-foreach-f38111822c0f)

--- 

배열을 순회해서, 그 안의 요소를 가지고 작업을 해야하는 상황에서, 보통 es5의 map을 주로 사용했습니다.

forEach보다, map이 더 짧아서기도 하고, map이 forEach 보다 성능상 더 좋다고 해서, 더 쓰기도 했습니다.

```
document.addEventListener('DOMContentLoaded', async (): Promise<void> => {
    const container = createHtmlElem('section');
    document.body.appendChild(document.body, container);
    await getImageList('개발자').then(result => {
        result.map(item => {
            const imgTag = createHtmlElem('img', {
                src: 'http src',
                'data-srcset': item.imageUrl,
                type: item.metadata.type,
                height: item.metadata.height,
                width: item.metadata.width,
            }, 'lazy')
            document.body.appendChild(container, imgTag);                
        })
    })
})
```

해당 코드는 최초 HTML Code가 완전히 로드 및 파싱 되었을 때 발생하는 이벤트 입니다. [DOMContentLoaded](https://developer.mozilla.org/ko/docs/Web/Events/DOMContentLoaded)

실행하면, getImageList('개발자') 라는 함수를 통해 개발자와 관련된 이미지를 배열로 가져오게 됩니다.
가지고 온 이미지리스트를 img tag를 만들어서 body에 추가하는 함수 입니다.

그러던중, 프로그래머스 자바스크립트 스터디에서 리더님께서 저 코드 내부중에서 result를 가지고 map을 쓰는 이유에 대해 물어보셨고, 

별 이유없이 map이 더 짧아서 쓴다고 했습니다.

--- 

MDN의 정의를 보면, 

- [map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

- [forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

map은 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환하고, 메서드는 주어진 함수를 배열 요소 각각에 대해 실행합니다. undefined를 return 합니다.

다시 위의 함수를 보면, result를 보았을 때, 새로운 배열을 return 하는게 아니라 기존의 배열을 가지고 지지고 볶는 함수기 때문에,

다른 개발자들이 볼 때, 오해를 안 할 수 있다고 이야기 하셨습니다. 

즉, 둘다 배열을 순회하는 것은 맞지만, 목적에 맞게끔 사용하는 것이 좋다는 것을 깨닫게 되었습니다.

같이 근무하는 개발자가 내 코드를 보고, 무엇을 하는 함수인지 주석없이 파악할 수 있는게, 협업을 쉽게하는 지름길이 아닐까 생각하게 되었습니다.
