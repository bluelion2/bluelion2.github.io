---
title: 'Mobx가 어떻게 상태관리를 할 수 있을까 '
date: 2021-07-03
category: 'development'
draft: false
---

## 개요

현재 진행중인 프로젝트에서 React의 상태관리 라이브러리로 Mobx를 쓰고 있는데, 어떻게 Mobx가 상태관리를 하는지 알아보고 내부 공유용으로 발표하기 위해 정리해보았다.

- 6버전을 바탕으로 정리합니다. 5버전 이상부터는 Proxy를 쓰기 때문에 IE에서 지원이 되지 않습니다. 단 6버전에서 옵션을 통해 IE를 지원할 수 있습니다.

  ```
    import { configure } from "mobx"

    configure({ useProxies: "never" }} // Proxy를 사용하지 않음
  ```

---

### 1. 상태관리

- 위키에 검색을 해보면 상태관리의 뜻으로 `하나 이상의 사용자 인터페이스 컨트롤의 상태를 관리한다.`라고 정의를 하고 있다. 사용자가 어떠한 행위를 통해 파생되어 값이 바뀌게 될텐데, 이러한 값을 관리하거나 바뀐 것을 인지하고 뷰에 보여주는 모든 것을 관리하는 것이라 생각한다.

- React에서는 기본적으로 제공하는 상태를 관리하는 방법이 있지만, 대규모 프로젝트를 개발하는 입장에서는 보다 더 (접근하기 또는 조작하기) 쉬운 (편한) 상태관리 라이브러리를 쓰기도 한다. 대표적으로 Redux, Mobx, Recoil등이 있으며 Redux 같은 경우 비동기 처리를 위해 미들웨어가 더 추가되곤 한다. (thunk, saga 등등)

<br>

---

### 2. Mobx의 Flow

![mobx flow](https://mobx.js.org/assets/getting-started-assets/overview.png)

- Mobx의 공식 문서에서 설명하는 동작 원리이다. 행위(actions)를 통해 상태(state)를 변화시키고, 상태가 변경시 파생(derivvation), 재행동(Reaction)을 일으킴으로써 해야 할 행위들을 하게 된다.

- 파생 및 state가 변경되는것을 중간에 가로체서 처리하거나, 하나의 state가 바뀌면 다른 행위 및 state가 연쇄적으로 바뀌는 것 또한 가능하다.

- [Mobx - getting start](https://mobx.js.org/getting-started)

<br>

---

### 3. Mobx 사용하기

<br>

![increase app](https://user-images.githubusercontent.com/34129711/124348056-c8c0a800-dc22-11eb-9e5e-646f76d6a745.png)

- 버튼을 클릭하면 value가 증가하는 간단한 앱이다. button을 클릭하는 이벤트, 즉 action이 발생하면 해당 state의 값이 바뀌고, update를 감지함에 따라 rerender를 일으키게 된다.
- 상태관리 스토어가 `Class` 일 필요는 없다.

<br>

---

### 4. Mobx Observable Deep Dive

- Mobx에서 가장 중요한 어노테이션은 세가지로 정의할 수 있다.

  - `observable`: 상태를 저장, 추적 가능한 값(필드)를 스토어에 정의 할 수 있다.
  - `action`: 해당 메소드는 상태 값을 바꿀 수 있다는 것을 정의함.
  - `computed`: 파생되어서 새롭게 만들어지는 값을 getter할수 있게 정의함.

<br>

이 중에서 `observable`이 어떻게 값을 바꾼것을 알고 update, rerender 하는지 깊게 알아보자.

#### 1) Store

- `Store` Class를 생성시 Contructor에서 `makeObservable()` 을 통해 해당 속성이 관찰 해야하는 값인지를 명시해준다. extends 하는 값이 없다면 `makeAutoObsevable()`을 사용해도 편하다.

  - 클레스 필드인 `value`는 observable, 메서드인 `increase()`는 action으로 구분짓는다.

  - `observable`을 등록하는 방법은 세가지가 있다.

    - `makeObservable` / `makeAutoObservable` / `observable`

      - `make(Auto)Observable`은 인자로 들어온 객체를 바로 변경하지만, `observable`의 경우 객체를 복제본을 만든다.
      - Mobx 공식 가이드에서는 proxy보다 비proxy 관찰 값이 더 성능이 좋고, 디버깅이 편하기 때문에 `observable` 대신 `makeObservable` 을 쓰는것을 권고한다. (객체 observable은 Proxy 쓰면서..)

      ![use_proxy](https://user-images.githubusercontent.com/34129711/124352226-26142380-dc3a-11eb-9bcd-7cba2177253d.png)

- Proxy

  - ES6에 추가되었으며, 객체의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 새로운 행동을 정의할 때 사용한다.

  ```
  var obj = {}
  var newObj = {
    get: function (obj, name) {
       return name in obj ? obj[name] : undefined
    },
    set: function(obj, property, value) {
        obj[property] = value
    }
  }
  var proxy = new Proxy(obj, newObj)
  proxy.a = 10
  proxy.b = 11
  console.log(proxy.a) // 10
  console.log(proxy.dd) // undefined
  console.log(obj.a) // 10
  console.log(obj == proxy) // false
  console.log(typeof proxy) // object
  ```

- makeObservable()을 통해 관찰할 대상을 명시한다. 내부 함수를 보자.

  ![code](https://user-images.githubusercontent.com/34129711/123287305-9d1a2f80-d549-11eb-905b-0fc9799f158d.png)

  ```
  var $mobx = /*#__PURE__*/Symbol("mobx administration");
  ```

  makeObservable은 첫번째 인자로 this를 받으며(class 자신) 두번째 인자로 객체를 받는데, 관찰할 대상의 속성, 메서드, 클래스 필드명은 키로 가지면서 값으로는 mobx 내장 함수를 받는다.

  ![code](https://user-images.githubusercontent.com/34129711/123287654-d9e62680-d549-11eb-9022-640c03a0af03.png)

  ```
  var keys = ownKeys({ a: 10, b: false })
  console.log('keys', keys) // ['a', 'b']
  ```

  - `Reflect`가 있으면 Reflect.ownKeys를, 없으면 Object 내장함수를 조합해서, 해당 객체에서 가지고 있는 속성을 리턴해주는 함수이다.

    - Reflect API : 중간에서 가로챌 수 있는 JavaScript 작업에 대한 메서드를 제공하는 내장 객체이다.
    - Object.getOwnPropertyNames : 지정된 객체에서 직접 찾은 모든 속성 (Symbol을 사용하는 속성을 제외하고 열거 할 수없는 속성 포함)의 배열을 리턴하는 함수.
    - Object.getOwnPropertySymbol : 객체에서 직접 찾은 모든 심볼 속성의 배열을 반환

  - `console.log(store)`를 보면 `makeObservable()` 함수를 통해서 받은 값을 관찰 할수 있는 option들이 달린`ObservableObjectAdministration` symbol이 추가되어 있다.

  ![스크린샷 2021-06-24 오후 11 58 17](https://user-images.githubusercontent.com/34129711/124349411-cc0b6200-dc29-11eb-8355-d298277fedc3.png)

<br>

#### 2) observable

- 자바스크립트의 함수는 객체이다 따라서, 아래 이미지와 같이 합성이 가능하다.
  ![observable function](https://user-images.githubusercontent.com/34129711/123531751-19776300-d742-11eb-9e0c-1f409d64ea3b.png)

- Mobx의 observable은 `createObservable()`과 `observableFactory`가 합쳐진 객체이다.

  ![create observable](https://user-images.githubusercontent.com/34129711/124349540-84d1a100-dc2a-11eb-9e1c-0541dc37d377.png)
  ![observable factory](https://user-images.githubusercontent.com/34129711/123531611-f1d3cb00-d740-11eb-8b89-d01d2e977e45.png)

- observable()에서는 내부적으로 이 값이 어떤 타입인지 체크하고, observable.box를 통해 관찰할 수 있는 값으로 리턴해준다.

- observable.box에서는 ObservableValue로 리턴을 해주는데, 해당 observableValue의 중요한 Set만을 보자.

  - Set : 새로운 값이 바뀌었는지 비교하고, 바뀌었다면 새로운 값을 할당하려 한다.

    ![ObservableValue](https://user-images.githubusercontent.com/34129711/123627047-79f2c700-d84c-11eb-98ec-2d2018eea514.png)

  - 할당하는 중, 값이 바뀌었다면 globalState에 값이 바뀐 것을 등록을 한다.

    ![prepareNewValue](https://user-images.githubusercontent.com/34129711/124350483-ec3e1f80-dc2f-11eb-99b0-9bfd8d74772e.png)

  - 새로운 값을 Set 하면서, 변경되었음을 globalState에 알려준다.

    ![setNewValue](https://user-images.githubusercontent.com/34129711/123627491-0a310c00-d84d-11eb-9fde-e3c91763e6a5.png)

#### 3) observer()

- Mobx 내부에서 값이 바뀌었다고 해서, React가 다시 화면을 그리진 않는다.

  ![mobx-state-change](https://user-images.githubusercontent.com/34129711/124350667-e5fc7300-dc30-11eb-96f9-93c30fd9a5f6.gif)

- Mobx를 React에서 쓸 때, `mobx-react`에서 제공하는 `observer(Component)`로 컴포넌트를 래핑함으로써

- (내부적으로 쓰고 있기 때문에), 공식문서에서도 memo를 쓸 필요가 없다고 한다. memo를 통해 해당 컴포넌트를 리렌더 할지 결정된다.

  ![observer](https://user-images.githubusercontent.com/34129711/124350889-33c5ab00-dc32-11eb-83d6-b4a9cb670518.png)

---

### 5. 결론

- `observable` 한 값을 만들기 위해 `makeObservable()`을 통해 생성, 등록한다.
- `makeObservable()`을 통해 만들어진 스토어는 내부에 관찰할 대상 객체의 키, 밸류 뿐만 아니라 `ObservableObjectAdministration` Symbol 값을 가지고 있다. 고유한 이름 부터, 관찰 등록할 naming, 안에서 스토어의 값을 `get`, `set` 할때 `intercept` 하는 로직이 있다.
- 생성된 값이 바뀌면 내부적으로 set 하기 전에, 기존 값과 비교를 해서 변경이 되었다면 바뀌었다고 해당 스토어에 알려준다.
- `mobx-react`의 `observer()`로 래핑한 컴포넌트가 값이 바뀌었다는 것을 인지하고 다시 그려준다.
- `computed`,`action` 부분도 비슷한 원리겠지만 깊게 한번 봐야겠다.

### 6. 참고자료

- [Mobx](https://mobx.js.org/)
