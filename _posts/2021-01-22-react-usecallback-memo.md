---
layout: post
title: 'React의 useCallback과 useMemo'
subtitle: 'react usecallback memo'
categories: frontend
tags: react
comments: true
---

- React의 useCallback과 useMemo에 대해서 정리한 글입니다.

---

### useCallback

- 다음과 작성된 코드를 보면 memo로 Navbar 컴포넌트를 감싸주었지만 `app.js` 의 button이 클릭 될 때 마다 `navbar.js`의 `"this is navbar"`와 `"navNum: "`도 출력되는 것을 확인할 수 있다

- 그 이유는 button을 클릭할 때 마다 app.js 컴포넌트가 다시 렌더링되면서 `handleNavbar`라는 함수가 다시 정의가 되기 때문이다

- 함수가 다시 정의되었기 때문에 Navbar 컴포넌트에 전달되는 props 값이 변경되고 Navbar 컴포넌트도 다시 렌더링 되므로 `"this is navbar"`와 `"navNum: "` 출력된다

```jsx
//app.js

import { useState } from 'react';
import Navbar from './components/navbar';

function App() {
  console.log('this is App');
  const [num, setNum] = useState(0);
  const [navNum, setNavNum] = useState(0);

  const handleClick = () => {
    setNum(num + 1);
  };

  const handleNavbar = () => {
    console.log('handle Navbar ');
  };

  return (
    <>
      <Navbar navNum={navNum} handleNavbar={handleNavbar}></Navbar>
      <div>
        {num}
        <button onClick={handleClick}>+</button>
      </div>
    </>
  );
}

export default App;
```

```jsx
import React, { memo } from 'react';

const Navbar = memo(({ navNum, handleNavbar }) => {
  console.log('this is havbar');
  console.log('navNum: ', navNum);

  return (
    <div>
      <button onClick={handleNavbar}>handleNav</button>
    </div>
  );
});

export default Navbar;
```

- 이러한 문제를 해결할 수 있는 방법이 바로 `useCallback`이다

- `useCallback`은 메모이징된 콜백을 반환한다 ( 공식 문서 - "useCallback returns a memoized callback")

- `useCallback`은 다음과 같이 선언해주는데 dependancy 배열이 수정되는 경우 즉, 아래 코드에서 a 또는 b 값이 바뀌는 경우에만 함수가 재정의된다

- 그리고 빈 배열인 경우에는 오직 처음 mount 될 때만 함수가 정의된다

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

- 따라서 아래 코드와 같이 `useCallback`으로 함수를 감싸주면 처음 app 컴포넌트가 mount 될 때만 함수가 정의되기 때문에 app 컴포넌트의 버튼을 클릭하더라도 `"this is app"`만 출력된다

```jsx
// app.js

import { useCallback, useState } from 'react';
import Navbar from './components/navbar';

function App() {
  console.log('this is App');
  const [num, setNum] = useState(0);
  const [navNum, setNavNum] = useState(0);

  const handleClick = () => {
    setNum(num + 1);
  };

  const handleNavbar = useCallback(() => {
    console.log('handle Navbar ');
  }, []);

  return (
    <>
      <Navbar navNum={navNum} handleNavbar={handleNavbar}></Navbar>
      <div>
        {num}
        <button onClick={handleClick}>+</button>
      </div>
    </>
  );
}

export default App;
```

- 공식문서를 확인해보면 `This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders (e.g. shouldComponentUpdate).` 와 같은 문장이 있다

- 즉 `useCallback`은 optimized childe component, 예를 들어 React.memo로 wrapping된 컴포넌트에 전달될 때 유용하다는 것이다

---

### useMemo

- 아래 코드의 app 컴포넌트에는 `+` 버튼을 누르면 addNum을 +1씩 증가시키고, `-` 버튼을 누르면 subNum을 -1씩 감소시키는 함수가 구현되어 있다

- 그리고 이렇게 변화되는 값을 Navbar 컴포넌트로 전달하면 Navbar 컴포넌트에서는 전달 받은 값의 10배를 출력해준다

- `+` 버튼을 클릭한 경우 addNum의 값이 변하지만 subNum의 값은 변하지 않고, `-` 버튼을 클릭하는 경우에는 subNum의 값이 변하지만 addNum의 값은 변하지 않는다

- 하지만 `+` 버튼 또는 `-` 버튼을 클릭한 경우 모두 `this is App` , `multiplyAddNum`, `multiplySubNum` 이 출력되는데 이를 통해서 Navbar 컴포넌트에 prop으로 전달되는 값이 변하지 않더라도 두 함수가 새로 호출되는 것을 알 수 있다

```jsx
// App.js

import { useState } from 'react';
import './App.css';
import Navbar from './components/navbar';

function App() {
  console.log('this is App');
  const [addNum, setAddNum] = useState(0);
  const [subNum, setSubNum] = useState(0);

  const handleAdd = () => {
    setAddNum(addNum + 1);
  };

  const handleSubstract = () => {
    setSubNum(subNum - 1);
  };

  return (
    <>
      <Navbar addNum={addNum} subNum={subNum}></Navbar>
      <div>
        {`addNum is ${addNum}, subNum is ${subNum}`}
        <button onClick={handleAdd}>+</button>
        <button onClick={handleSubstract}>-</button>
      </div>
    </>
  );
}

export default App;
```

```jsx
//navbar.js

import React, { memo, useMemo } from 'react';

const Navbar = memo(({ addNum, subNum }) => {
  const multiplyAddNum = (num) => {
    console.log('multiplyAddNum');
    return num * 10;
  };

  const multiplySubNum = (num) => {
    console.log('multiplySubNum');
    return num * 10;
  };

  const addMultiply = multiplyAddNum(addNum);
  const subMultiply = multiplySubNum(subNum);

  return (
    <div>
      {`addMultiply Num is ${addMultiply}, subMultiply Num is ${subMultiply}`}
      <button>handleNav</button>
    </div>
  );
});

export default Navbar;
```

- addNum의 값이 변할 때는 subNum의 값이 변하지 않기 때문에 다시 subMultiply 함수를 호출할 필요가 없다

- 이처럼 다시 함수를 호출하지 않고 이전에 계산된 값을 다시 사용하고 싶을 때 `useMemo` 를 사용한다

- `useMemo`는 메모이징 된 값을 반환한다(공식문서 - "Returns a memoized value.")

- 즉, `useMemo`는 depedency 배열의 값 중 하나가 변경된 경우에만 메모이징 된 값을 다시 계산한다

- 아래 코드처럼 `useMemo`로 함수를 감싸주고 dependency를 정해주면, dependency가 바뀌는 경우에만 함수를 호출해서 새롭게 값을 계산한다

- `useCallback`이 특정 함수를 새로 만들지 않고 재 사용할 때 사용한다면 `useMemo`는 특정 결과 값을 재사용할 때 사용한다

```jsx
// navbar.js

import React, { memo, useMemo } from 'react';

const Navbar = memo(({ addNum, subNum }) => {
  const multiplyAddNum = (num) => {
    console.log('multiplyAddNum');
    return num * 10;
  };

  const multiplySubNum = (num) => {
    console.log('multiplySubNum');
    return num * 10;
  };

  const addMultiply = useMemo(() => multiplyAddNum(addNum), [addNum]);
  const subMultiply = useMemo(() => multiplySubNum(subNum), [subNum]);

  return (
    <div>
      {`addMultiply Num is ${addMultiply}, subMultiply Num is ${subMultiply}`}
      <button>handleNav</button>
    </div>
  );
});

export default Navbar;
```

---

## Reference

- [React 공식 문서](https://reactjs.org/docs/react-api.html#reactmemo)
- [이제는 사용해보자 useMemo & useCallback](https://leehwarang.github.io/2020/05/02/useMemo&useCallback.html)
- [벨로퍼트와 함께하는 모던 리액트](https://react.vlpt.us/basic/18-useCallback.html)
