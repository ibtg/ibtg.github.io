---
layout: post
title: 'React의 훅(Hooks)'
subtitle: 'react hooks'
categories: development
tags: react
comments: true
---

- React의 훅(Hooks)에 대해서 정리한 글입니다.

---

- 훅(Hooks)는 리액트 버전 16.8에 새로 추가된 기능으로 함수형 컴포넌트(Functional Component)에서도 상태 관리 및 LifeCycle API를 사용할 수 있게 되었습니다.

---

### useState

- 훅(Hooks) 도입 이전에는 함수형 컴포넌트에서는 상태관리를 할 수 없었지만, `useState` 함수를 통해서 상태 관리를 할 수 있게 되었습니다

- 아래 두 코드는 버튼을 누르면 숫자가 1씩 증가하는 기능을 클래스형 컴포넌트와 함수형 컴포넌트를 사용해서 작성한 코드 입니다.

- 첫번째 클래스형 컴포넌트에서는 state 객체를 통해서 상태 관리를 하고, setState 함수를 사용해서 state 값을 업데이트 할 수 있습니다

```jsx

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  }

  modify = (n) => {
    this.setState((current) => {
      return {
        count: n,
      };
    });
  };

  //클래스 형 컴포넌트에서는 render 함수 필요
  render() {
    const { count } = this.state;
    return (
      <div>
        {count}
        <button onClick={() => this.modify(count + 1)}>Increment</button>
      </div>
    );
  }
}

export default App;
```

- 하지만 함수형 컴포넌트에서는 `useState`를 사용해서 state를 관리할 수 있도록 해줍니다.

- `useState`는 배열을 반환하는데, 인자로 전달되는 값은 반환되는 첫번째 값의 state의 값이 되고, 반환되는 두번째 값은 state를 관리할 수 있는 함수 입니다.

- 따라서 아래 코드의 setCount 함수의 인자 값이 변경되면 count 값이 해당 값으로 변경이 됩니다.

- 함수형 컴포넌트에서는 this 같은 문장 규칙과 `render`를 고려해야 했지만 훅을 사용하는 함수형 컴포넌트는 이러한 것들을 신경을 쓰지 않아도 되고 코드가 더 간결해지는 장점이 있습니다.

```jsx
import React, { useState } from 'react';

const App = () => {
  const [count, SetCount] = useState(0);

  return (
    <div>
      {count}
      <button onClick={() => SetCount(count + 1)}>Increment</button>
    </div>
  );
};

export default App;
```

---

### useEffect

- `useEffet`를 사용하면 함수형 컴포넌트에서도 LifeCycle API를 사용할 수 있습니다.

- 아래 코드는 버튼을 누를 때 마다 random한 값이 화면에 출력되는 기능을 구현한 코드입니다.

- 콘솔에 출력된 내용을 통해 `useEffect`를 사용해서 `componentDidMount`와 `componentDidUpdate`을 구현할 수 있는 것을 확인할 수 있습니다

```jsx
function App() {
  return (
    <div className="container">
      <h1>Hello World</h1>
      <FuncComp initNumber={2}></FuncComp>
    </div>
  );
}

let funcId = 0;

//function 방식은 자기자신이 render 함수
function FuncComp(props) {
  let numberState = useState(props.initNumber); // 전달하는 값이 state의 초기값이 된다. 배열을 리턴한다
  let setNumber = numberState[1]; // numberState의 두번째 함수가 상태를 바꿀 수 있는 함수
  let number = numberState[0];

  useEffect(function () {
    console.log(
      'func => useEffect (componentDidMount & componentDidUpdate)' + ++funcId
    );
  });
  console.log('func => render' + ++funcId);

  return (
    <div className="container">
      <h2>function style component</h2>
      <p>Number: {number}</p>

      <input
        type="button"
        value="random"
        onClick={() => {
          setNumber(Math.random());
        }}
      />
    </div>
  );
}

export default App;
```

- 처음 렌더링이 끝난 후 콘솔 창을 확인 하면 다음과 같이 `render, componentDidMount` 작업이 실행된 것을 알 수 있습니다.

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-27-react-hook-1.JPG?raw=true">

- 그리고 random 버튼을 눌러서 값을 업데이트 시킬 때 마다 콘솔 창에서 다음과 같은 결과가 출력되는 것을 통해서 `render, componentDidUpdate` 작업이 실행되는 것을 확인할 수 있습니다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-27-react-hook-2.JPG?raw=true">

- 이때 다음과 같인 `useEffect`의 두번째 인자로 비어 있는 값을 전달하면, `componentDidMount` 작업만 실행하도록 할 수 있습니다.

```jsx
useEffect(
  function () {
    console.log(
      'func => useEffect (componentDidMount & componentDidUpdate)' + ++funcId
    );
  },
  [] // 렌더링이 끝난 후 한번은 콘솔창에 출력되지만, 이후 값을 update해도 해당 부분 콘솔 창에 출력되지 않는다
);
```

- 그리고 `useEffect`의 두번째 인자로 값을 전달하면, 해당 값이 바뀐 경우에만 첫번째 인자로 전달된 콜백 함수(Callback Function)가 실행됩니다

```JSX
  useEffect(
    function () {
      console.log(
        'func => useEffect (componentDidMount & componentDidUpdate)' + ++funcId
      );
    },
    [number] // number 값이 바뀐 경우에만 콜백 함수 호출된다
  );
```

---

- `useEffect`에서는 함수를 반환할 수 있는데 이를 clean up 함수라고 합니다

- clean up 함수를 통해 컴포넌트가 업데이트 되기 전이나 Unmount 되기 전에 필요한 작업을 할 수 있습니다.

- 아래 코드에서 randome 버튼을 누르면 임의의 값이 화면에 출력되고 remove func 버튼을 통해 FuncComp 컴포넌트를 종료할 수 있습니다

```jsx
function App() {
  let [funcShow, setFuncShow] = useState(true);
  return (
    <div className="container">
      <h1>Hello World</h1>
      <input
        type="button"
        value="remove func"
        onClick={() => setFuncShow(false)}
      />
      {funcShow ? <FuncComp initNumber={2}></FuncComp> : null}
    </div>
  );
}
```

```jsx
const funcStyle = 'color:blue';
const funcStyle2 = 'color:red';
let funcId = 0;

//function 방식은 자기자신이 render 함수
function FuncComp(props) {
  let numberState = useState(props.initNumber);
  let setNumber = numberState[1];
  let number = numberState[0];

  //componentDidMount만 실행하고 싶다
  useEffect(
    function () {
      console.log(
        '%cfunc => useEffect (componentDidMount )' + ++funcId,
        funcStyle
      );

      // return 함수는 UnMount 될 때 호출 된다
      return function () {
        console.log(
          '%cfunc => useEffect return (componentWillUnMount)' + ++funcId,
          funcStyle2
        );
      };
    },
    [] //빈 배열을 전달하면 콜백함수 1회만 실행된다
  );

  useEffect(
    function () {
      console.log(
        '%cfunc => useEffect (componentDidMount & componentDidUpdate)' +
          ++funcId,
        funcStyle
      );

      return function () {
        console.log(
          '%cfunc => useEffect number return (clean up & componentWillUnmount)' +
            ++funcId,
          funcStyle2
        );
      };
    },
    [number]
    // 이 두번째 인자 값이 바뀔 때만 첫번째 콜백함수를 호출하도록 한다
  );

  console.log('%cfunc => render' + ++funcId, funcStyle);

  return (
    <div className="container">
      <h2>function style component</h2>
      <p>Number: {number}</p>
      <input
        type="button"
        value="random"
        onClick={() => {
          setNumber(Math.random());
        }}
      />
    </div>
  );
}

export default App;
```

- 처음 코드를 실행하면 콘솔창에서 아래처럼 렌더링이 끝난 후 `componentDidMount` 작업이 실행된 것을 확인할 수 있습니다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-27-react-hook-3.JPG?raw=true">

- 그리고 random 버튼을 눌러 값을 업데이트 하면 렌더링이 끝난 후 업데이트를 하기전 clean up 함수 부분이 실행되는 것을 확인할 수 있습니다.

* 이 때 비어 있는 값을 전달한 `useEffect`는 `componentDidMount` 작업만 수행하기 때문에 값이 업데이트 되더라도, clean up 함수 부분이 실행되지 않는 것을 확인할 수 있습니다.

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-27-react-hook-4.JPG?raw=true">

* 컴포넌트를 종료하기 위해 remove func 버튼을 누르면 컴포넌트가 종료되기 전에 `ComponentWillUnmount` 작업이 싫행되는 것을 확인할 수 있습니다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-27-react-hook-5.JPG?raw=true">

---

### useRef

- 리액트를 사용하면서 태그를 직접 처리해야할 경우 `useRef`를 사용해서 `document.getElementById()`를 사용한 것처럼 해당 태그를 직접 처리할 수 있습니다.

- 리액트에 있는 모든 컴포넌트는 reference element 를 가지고 있습니다

- 아래 코드처럼 `useRef` 호출하면 `ref`객체가 반환되는데 이 객체를 통해 input 태그에 접근할 수 있습니다

```jsx
import React, { Component, useState, useEffect, useRef } from 'react';

function App() {
  const tagSelector = useRef();
  setTimeout(() => console.log(potato.current), 5000);
  return (
    <div className="App">
      <div>H1</div>
      <input type="text" placeholder="la" ref={tagSelector} />
      //tagSelector에 의해 참조된다
    </div>
  );
}
export default App;
```

- 두번째 코드는 `Hi`라는 버튼을 누르면 `say hello`가 출력되는 코드입니다

- useEffect의 두번째 인자로 비어있는 값이 전달되었으로 `componentDidMount` 작업만 수행하게 됩니다.

- 렌더링을 과정에서 title은 `useRef` 통해 `ref` 객체를 반환받게 되므로 `<button>` 태그를 참조할 수 있게 됩니다.

- 렌더링이 끝난 후 `useEffect`가 `componentDidMount` 작업을 실행하므로 버튼에 이벤트를 등록하게 되고, 버튼을 클릭할 때 마다 say hello가 출력됩니다.

```jsx
import React, { Component, useState, useEffect, useRef } from 'react';

const useClick = (onClick) => {
  const element = useRef(); // 변경가능한 ref 객체를 반환한다
  useEffect(() => {
    console.log('element curent: ', element.current);
    //componentDidMount
    if (element.current) {
      element.current.addEventListener('click', onClick);
    }
    //componentWillUnmount 할 때 이벤트를 없애주어야 한다
    return () => {
      if (element.current) {
        element.current.removeEventListner('click', onClick);
      }
    };
  }, []);
  return element;
};

function App() {
  const sayHello = () => console.log('say hello');

  const title = useClick(sayHello);
  return (
    <div className="App">
      <button ref={title}>Hi</button>
    </div>
  );
}
export default App;
```

---

## Reference

- [리액트 공식 홈페이지 - Hooks](https://ko.reactjs.org/docs/hooks-intro.html)
- [생활코딩 - React class & functional style coding](https://opentutorials.org/module/4600)
- [노마드코더 - 실전형 리액트 Hooks 10개](https://nomadcoders.co/react-hooks-introduction/)
