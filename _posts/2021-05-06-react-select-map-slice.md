---
layout: post
title: 'React에서 select 태그의 option을 선택적으로 보여주는 방법'
subtitle: 'react map slice'
categories: frontend
tags: react
comments: true
---

- React에서 select 태그의 option을 선택적으로 보여주는 방법에 대해 정리한 내용입니다

---

- select 안의 option 태그를 반복적으로 보여주고 싶은 경우, `map` 함수와 함께 사용할 수 있다.

- 이를 코드로 나타내면 다음과 같다

```jsx
import React, { useState } from 'react'

const LoginPage = () => {

    const [period, setPeriod] = useState("1")

    const onPeriodChange = (e) => {
        const {value} = e.target
        setPeriod(value)

    }

    const periodOptions = [
        {value: "1", label:'01'},
        {value: "2", label:'02'},
        {value: "3", label:'03'},
    ]


    return (
        <div>
            <select 
                value={period}
                onChange={onPeriodChange}
            >
                {
                    periodOptions.map(period => (
                        <option value={period.value}>{period.label}</option>
                    ))
                }
            </select>

        </div>
    )
}

export default LoginPage



```

- 그리고 다른 컴포넌트에서 넘어온 props에 따라서, 선택적으로 option 태그를 보여주고 싶은 경우에는 `slice` 함수를 함께 사용한다.

- 예를들어, 1이 넘어온 경우에는. 1,2 값만 옵션으로 보여주고, 2가 넘어온 경우에는 1에 해당하는 값만 옵션으로 보여주고 싶은 경우 다음과 같이 작성할 수 있다


```jsx
// app.js

import LoginPage from './pages/LoginPage'

const App = () => {
  return (
    <LoginPage
      data={1}
    ></LoginPage>
    );
};

export default App;


```

```jsx

import React, { useState } from 'react'

const LoginPage = ({data}) => {

    const [period, setPeriod] = useState("1")

    const onPeriodChange = (e) => {
        const {value} = e.target
        setPeriod(value)

    }

    const periodOptions = [
        {value: "1", label:'01'},
        {value: "2", label:'02'},
        {value: "3", label:'03'},
    ]



    return (
        <div>
            <select 
                value={period}
                onChange={onPeriodChange}
            >
                {
                    periodOptions.slice(0, periodOptions.length - data).map(period => (
                        <option value={period.value}>{period.label}</option>
                    ))
                }
            </select>

        </div>
    )
}

export default LoginPage


```
---
