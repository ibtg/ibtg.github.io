---
layout: post
title: 'React에서 props를 destructuring으로 전달받기'
subtitle: 'react props destructuring'
categories: frontend
tags: react
comments: true
---

- React에서 props를 destructuring으로 전달받는 방법에 대해 정리한 내용입니다

---

- 리액트는 다른 컴포넌트로 `props` 를 통해 데이터를 전달할 수 있다

---

```jsx

//App.jsx

import LoginPage from './pages/LoginPage'

const data = {
  date:"2021-04-25",
  weather:"sunny",
  location:{
    city:'seoul'
  }
}

const App = () => {
  return (
    <LoginPage data={data} ></LoginPage>
  );
};

export default App;

```

- `props`로 데이터를 전달받기 때문에 전달받는 데이터 앞에 `props`를 붙여준다

```jsx

// LoginPage.jsx

import React from 'react'

const LoginPage = (props) => {
    console.log("data: ", props.data) // App.jsx에서 전달한 data 객체를 확인할 수 있다
    return (
        <div>
            LoginPage
        </div>
    )
}

export default LoginPage


```

- 이렇게 데이터를 전달받을 때 destructuring을 통해 더 간단하게 전달받을 수 있는데, 아래처럼 전달받는 `{ }` 안에 전달받는 데이터의 이름을 입력해주면 `props` 없이 이름 만으로 데이터를 전달받을 수 있다


```jsx

import React from 'react'

const LoginPage = ({data}) => {
    console.log("data: ", data)// App.jsx에서 전달한 data 객체를 확인할 수 있다
    return (
        <div>
            LoginPage
        </div>
    )
}

export default LoginPage



```

- 그리고 지금 전달받는 `data` 객체 안에는 `location`이라는 객체가 있는데, 이처럼 객체 안에 객체가 있는 경우에는 다음과 같은 방법으로 `location`이라는 객체를 직접적으로 얻을 수 있다

```jsx

import React from 'react'

const LoginPage = ({data, data:{location}}) => {
    console.log("data: ", data)
    console.log("location: ", location)
    return (
        <div>
            LoginPage
        </div>
    )
}

export default LoginPage


```

---

## Reference

- [Destructuring Props in React](https://medium.com/@gazzaazhari/destructuring-props-in-react-d8f163d3ef84)
