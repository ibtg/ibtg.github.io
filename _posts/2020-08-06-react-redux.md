---
layout: post
title: 'React redux'
subtitle: 'react redux'
categories: development
tags: react
comments: true
---

- 생활코딩 react redux 강의를 듣고 정리한 내용입니다

---

- `리액트`는 컴포넌트를 통해서 체계적으로 앱을 만든다

- `리덕스`는 상태를 중앙헤서 관리하는데 이를 통해서 데이터가 우리가 예측되지 않은 상태로 변형되는 가능성을 낮춰주는 역할을 한다. 즉, 복잡성을 낮춰준다

- 이러한 특징을 통해서 각각이 가진 장점을 취하게 된다

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-react-redux.jpg.jpg?raw=true">

- 왼쪽 그림에서는 데이터가 변경되면 모든 컴포넌트에 전달 되고 필요한 컴포넌트는 데이터를 사용하고 필요 없는 컴포넌트는 데이터를 사용하지 않는다

- 하지만 데이터가 필요한 없는 렌더 함수를 호출하기 때문에 효율성이 떨어진다는 단점이 있다

- 오른쪽 그림처럼 리덕스를 사용하게 되면, 변화가 있으면 리덕스의 스토어에 알려준다

- 그리고 정보가 필요한 컴포넌트들에만 정보를 전달하기 때문에 그 컴포넌트들만 렌더 함수를 실행한다

- 모든 정보는 리덕스가 가지고 있다. 리덕스는 빅브라더 같은 것이라고 할 수 있다

* 리액트와 리덕스를 연결해주는 것이 react-redux

---

### redux를 사용하지 않는 경우

- `without-redux 폴더의 코드`처럼, 만약 수 십 수백 개의 depth의 컴포넌트가 있다면 매우 복잡해 질 것이다

- 상위 컴포넌트 안에 컴포넌트가 있고, 또 그 아래 컴포넌트가 있고, 또 그 아래 컴포넌트가 있는 경우 맨아래서 10을 전달받기 위해서 맨 위부터 10을 전달해야 한다

- depth가 깊어 컴포넌트가 많다면 이렇게 너무 많은 props를 전달해주고 이벤트를 전달해주어야 한다

- 변화가 없으면 불필요하게 렌더함수 호출 하는 것을 막는 이러한 문제를 해하기 위해 redux를 사용한다

---

### redux를 사용하는 경우

- 리액트만으로 어플리케이션 구현하면 각각의 컴포넌트들은 props로 연결되어 있고 첫번째와 네번째 컴포넌트 연결하려고 하면 props를 계속 아래로 전달해주어야 한다

- `with-redux 폴더`코드처럼 리덕스를 이용하게 되면 스토어가 생기고 스토어 내부적으로는 스테이트 값이 저장되게 된다

- 그리고 각각의 필요한 컴포넌트들이 그 스토어에 구독을 하게게 되면 어떠한 컴포넌트에 변화가 생겼을 때 구독중인 컴포넌트들에게 변화 상태가 통보됨으로써 props로 길게 연결되는 복잡한 작업을 하지 않아도 된다

- 리덕스를 통해서 훨씬 더 편하게 어플리케이션의 상태를 관리할 수 있게 되었다

---

### redux 종속성 제거

- without-redux의 redux가 도입되지 않은 `AddNumber.js`는 코드는 부품으로써의 가치가 있다

- 이 부품은 어플리케이션 안에서도 여러군데 사용할 수 있고 다른 어플리케이션에서도 사용할 수 있다

- 왜냐하면 `AddNumber.js` 코드에서 onClick으로 버튼을 클릭하면, 이벤트 핸들러를 실행시켜주니까 필요한 곳에 사용할 수 있다

- 하지만 with-redux의 `AddNumber.js`는 스토어가 리덕스의 스토어이기 때문에 우리 애플리케이션에서 사용하고 있는 상태에 의존하고 있다. 그렇기 때문에 때문에 다른 곳에서 `AddNumber.js`를 사용할 수 없다

- 따라서 `AddNumber.js`가 재사용할 수 있는 컴포넌트가 되지 않는다

- 이를 해결하기 위한 방법으로 wrapping을 한다

- `AddNumber.js`를 감싸는 새로운 컴포넌트를 만들고 리덕스의 스토어를 핸들링 하는 컴포넌트를 만들어서 `AddNumber.js`는 리덕스와 무관하게 만드어준다

- 이 때, 기존의 `AddNumber.js`를 `presentational component`, `AddNumber.js` 감싸서 리덕스와 관련된 작업을 실질적어로 실행하는 `container component`라고 한다

---

### react redux 적용

- [github code](https://github.com/ibtg/react-redux-opentutorials) 참고

- 이전까지는 부품으로써 컴포넌트를 활용하기 위해서 `container`를 도입했기 때문에 코드의 양이 증가하게 되었다

- 그래서 리덕스를 리덕스 답게 리액트를 리액트 답게 해야할일이 많아짐에 따라 이러한 것들을 한번에 해결하기 위해 새로운 툴을 만들었는데 이를 `react-redux`라는 툴이다

- 리덕스 도입하기 위해 `container` 만들고 `src/containers`의 `AddNumber.js`처럼 `container`에 어떤 이벤트에 dispatch 해야 하는 경우 dispatch를 걸어주는 작업을 추가적으로 해야했다

- 그리고 `DisplayNumber.js` 처럼 props로 상태를 전달하는 경우에는 스토어를 구독해서 스토어에 데이터가 바뀔 때 마다 렌더링이 다시 되는 작업을 위한 코드를 작성했다

- 이렇게 되면 또 하나해야 하는 것이 `DisplayNumberRoot.js` 가 `container`의 `DisplayNumber.js`를 가져와서 components의 `DisplayNumber.js`를 제어하게 되는 것인데

- 이때 `DisplayNumberRoot.js`에서 unit="kg"라는 props를 주면 `presentational component`인 components에 `this.props.unit`이라는 값으로 값을 전달하려면 중간에 있는 `container`의 `DisplayNumber.js`가 props를 전달 받아서 또 다시 components의 `DisplayNumber.js`로 전달해주어야 한다

- 그렇기 때문에 중간에서`unit={}this.props.unit` 작업을 해주어야 한다.

- 중간에 컨테이니가 끼니까 복잡해 지는데 이러한 것들을 자동화해주는 것이 바로 react-redux이다

- src/components src/containers 코드 참조

---

## Reference

- [생활코딩 - react redux](https://opentutorials.org/module/4518)

- [github code](https://github.com/ibtg/react-redux-opentutorials)

- [React - Redux framework](https://www.slideshare.net/binhqdgmail/006-react-redux-framework)
