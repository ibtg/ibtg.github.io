---
layout: post
title: '리액트 "handler Error: Maximum update depth exceeded." 에러를 해결하는 방법'
subtitle: 'react handler Error'
categories: development
tags: react
comments: true
---

- 리엑트 "handler Error: Maximum update depth exceeded." 에러를 해결하는 방법에 대해서 정리한 글입니다.

---

- 보통 자바스크립트에서는 함수를 호출할 때 소괄호`()`와 함께 함수를 호출합니다.

- 하지만 동일한 방법으로 소괄호`()`를 사용해서 리액트에서 이벤트 핸들러(Event Handler)를 호출하게 되면 에러가 발생하게 됩니다

- 아래 예제는 숫자가 1씩 증가하는 함수를 `<button>` element에서 onClick의 이벤트 헨들러로 호출하는 코드입니다.

```jsx
import React from 'react';

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  add = () => {
    console.log('add');
    this.setState((current) => ({ count: current.count + 1 }));
  };

  render() {
    return (
      <div>
        <h1>Number {this.state.count}</h1>
        <button onClick={this.add()}>Add</button>
      </div>
    );
  }
}

export default App;
```

- 위 코드를 실행하게 되면 - `Error: Maximum update depth exceeded. This can happen when a component repeatedly calls setState inside componentWillUpdate or componentDidUpdate. React limits the number of nested updates to prevent infinite loops.` 라는 에러가 발생하게 됩니다.

- 이러한 에러가 발생하는 이유는 소괄호`()`와 함께 함수를 호출하면 `render`함수가 호출 될 때 마다 해당 함수가 호출되기 때문입니다.

- click 이벤트가 일어나면 `this.add()` 함수가 호출 되고, 함수가 호출되었기 때문에 또 다시 `render`함수가 호출 됩니다
- 그리고 `render`함수가 호출 되었기 때문에 또 다시 `this.add()`가 호출되고 계속해서 이렇게 함수 호출이 반복되기 때문에 에러가 발생하게 됩니다

---

- 에러를 해결하는 첫번째 방법은 함수의 소괄호`()` 없이 함수를 호출하는 것입니다.
- 아래와 같이 코드를 작성하게 되면 이벤트 핸들러는 해당 버튼이 클릭이 된 경우에만 호출됩니다.

```jsx
<div>
  <h1>Number {this.state.count}</h1>
  <button onClick={this.add}>Add</button>
</div>
```

- 또 다른 해결 방법으로는 인라인 함수(inline function)를 사용할 수 있습니다.
- 이 경우에는 소괄호`()`와 함께 함수를 호출합니다

```jsx
<div>
  <h1>Number {this.state.count}</h1>
  <button onClick={(e) => this.add()}>Add</button>
</div>
```

---

## Reference

- [https://upmostly.com/tutorials/react-onclick-event-handling-with-examples
  ](https://upmostly.com/tutorials/react-onclick-event-handling-with-examples)
- [https://stackoverflow.com/questions/48497358/reactjs-maximum-update-depth-exceeded-error](https://stackoverflow.com/questions/48497358/reactjs-maximum-update-depth-exceeded-error)
