---
layout: post
title: 'React에서 Styled Components 사용하기'
subtitle: 'react styled components'
categories: frontend
tags: react
comments: true
---

- Styled Components에 대해서 정리한 글입니다.

---

### Styled Components

- `Styled Components`는 대표적인 CSS-in-JS 라이브러리입니다.

- CSS-in-JS란 별도의 CSS 파일에서 스타일 정의를 하는 것이 아니라 JavaScript 파일에서 스타일을 정의하고 이를 적용시키는 방법입니다.

- `Styled Components`를 라이브러리를 사용하면 스타일을 Javascript 파일에서 정의하고 컴포넌트에 바로 적용할 수 있습니다.

- 아래 코드는 기존에 많이 사용되는 css 파일에 스타일을 정의하고, import 해서 스타일을 적용시키는 코드입니다.

```css
/* App.css */
.button {
  border-radius: 50px;
  padding: 5px;
  min-width: 120px;
  color: white;
  font-weight: 600;
  -webkit-appearance: none;
  cursor: pointer;
}

.button:active,
.button:focus {
  outline: none;
}
.button--success {
  background-color: #2ecc71;
}

.button--danger {
  background-color: #e74c3c;
}
```

```jsx
//App.js

import React, { Fragment, Component } from 'react';
import './App.css';

class App extends React.Component {
  render() {
    return (
      <Fragment>
        <button className="button button--success">Hello</button>
        <button className="button button--danger">Hello</button>
      </Fragment>
    );
  }
}

export default App;
```

- 하지만 `Styled Components`를 사용하면 다음과 같이 코드를 작성할 수 있습니다.

- `styled`함수를 사용해서 스타일을 정의하면 해당 스타일이 적용된 컴포넌트를 반환합니다.

- 그리고 컴포넌트는 `props`를 전달 받을 수 있기 때문에 `props`의 조건에 따라 다른 스타일을 적용할 수 있습니다.

```jsx
import React, { Fragment, Component } from 'react';
import styled from 'styled-components';

class App extends React.Component {
  render() {
    return (
      <Container>
        <Button>Hello</Button>
        <Button danger>Hello</Button>
      </Container>
    );
  }
}

const Container = styled.div`
  height: 100vh;
  width: 100%;
  background-color: grey;
`;

const Button = styled.button`
  border-radius: 50px;
  padding: 5px;
  min-width: 120px;
  color: white;
  font-weight: 600;
  -webkit-appearance: none;
  cursor: pointer;
  &:active, // &는 this를 의미한다
  &:focus {
    outline: none;
  }
  background-color: ${(props) => (props.danger ? '#e74c3c' : '#2ecc71')};
  // 클래스명 없이 props에 따라 다른 색상이 적용된다
`;

export default App;
```

- 첫번째 코드와 비교해보면, 첫번째 코드에서는 두 개의 컴포넌트와 css파일 그리고 서로 다른 클래스명이 필요했지만 스타일 컴포넌트 덕분에 더 깔끔하게 코드를 작성할 수 있습니다.

---

### createGlobalStyle, styled()

- 전역에 글로벌 스타일을 적용시키고 싶은 경우 `createGloblStyle`을 사용해서 아래와 같이 코드를 작성해 줍니다

```jsx
// App.js

import React, { Fragment, Component } from 'react';
import styled, { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
body{
  padding: 0;
  margin: 0
}
`;

class App extends React.Component {
  render() {
    return (
      <Container>
        <GlobalStyle></GlobalStyle>
        <Button>Hello</Button>
        <Button danger>Hello</Button>
        <Anchor href="http://google.com" target="_blank">
          go to google
        </Anchor>
      </Container>
    );
  }
}
```

- 그리고 다음과 같이 코드를 작성하면 `<Button>` 컴포넌트의 스타일이 적용된 새로운 컴포넌트를 만들 수 있습니다

```jsx
const Anchor = styled(Button.withComponent('a'))`
  text-decoration: none;
`;
```

---

### keyframes, css

- `styled Components`에서도 `keyframe`을 사용해서 애니메이션 효과를 사용할 수 있습니다

- 아래 코드와 같이 `keyframes`를 사용해서 미리 스타일을 정의하고 `` css `{사용할 스타일}` `` 의 괄호에 적용할 스타일을 추가하면 `props`에 따라 `keyframes` 으로 정의한 스타일이 적용되는 것을 확인할 수 있습니다

```jsx
class App extends React.Component {
  render() {
    return (
      <Container>
        <GlobalStyle></GlobalStyle>
        <Button>Hello</Button>
        <Button danger rotationTime={1}>
          Hello
        </Button>
        <Anchor href="http://google.com" target="_blank">
          go to google
        </Anchor>
      </Container>
    );
  }
}

const Button = styled.button`
  border-radius: 50px;
  padding: 5px;
  min-width: 120px;
  color: white;
  font-weight: 600;
  -webkit-appearance: none;
  cursor: pointer;
  &:active,
  &:focus {
    outline: none;
  }

  background-color: ${(props) => (props.danger ? '#e74c3c' : '#2ecc71')};
  // 클래스명 없이 props로 스타일 변경이 가능하다
  animation: ${(props) =>
    props.danger
      ? css`
          // 다른 정의되어있는 css를 가져온다
          ${rotation} ${props.rotationTime}s linear infinite
        `
      : ''};
`;

const rotation = keyframes`
from{
  transform: rotate(0deg);
}
to{
  transform: rotate(360deg);
}`;
```

---

### input.attrs

- 다음과 같이 코드를 작성하면 태그의 속성을 지정할 수 있습니다.

```jsx
const Input = styled.input.attrs({
  required: true,
})`
  border: none;
`;
```

- 그리고 이전에 만든 컴포넌트를 다른 컴포넌트에서도 사용해서 스타일을 적용시킬 수 있습니다

```jsx
const awesomeCard = css`
  box-shadow: 0 4px 6px rgba(50, 50, 93, 0.11), 00 1px 3px rgba(0, 0, 0, 0.88);
  background-color: white;
  border-radius: 10px;
  padding: 20px;
`;

const Input = styled.input.attrs({
  required: true,
})`
  border: none;
  ${awesomeCard}
`;
```

---

### ThemeProvider

- 여러 컴포넌트에 공통적으로 정의되어 있는 스타일을 바꾸어야 하는 경우, 공통된 스타일이 각각의 컴포넌트에 따로 정의되어 있다면 해당하는 컴포넌트의 모든 스타일을 일일이 수정해야하는 번거로움이 생깁니다

- 이 때 공통된 스타일이 정의된 파일을 만들고 `ThemeProvider` 컴포넌트로 전달하면, `ThemeProvider` 컴포넌트에 nesting 되어 있는 모든 컴포넌트에 스타일을 적용할 수 있습니다.

```jsx
//theme.js

// 공통으로 적용할 스타일  정의
const theme = {
    mainColor: 'white';
    dangerColor: 'red';
    successColor: 'gree';
}

export default theme
```

```jsx
//App.js

import React, { Fragment, Component } from 'react';
import styled, { createGlobalStyle, ThemeProvider } from 'styled-components';
import theme from './theme';

const GlobalStyle = createGlobalStyle`
body{
  padding: 10;
  margin: 10;
}
`;

const Card = styled.div`
  background-color: red;
`;

const Container = styled.div`
  height: 100vh;
  width: 100%;
  background-color: grey;
  //컴포넌트를 reference할 수 있다
  ${Card} {
    background-color: blue;
  }
`;

class App extends React.Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        {/* ThemeProvider에 전달해준다 */}
        <Container>
          <Form></Form>
        </Container>
      </ThemeProvider>
    );
  }
}
```

---

## Reference

- [https://styled-components.com/](https://styled-components.com/)
- [노마드코더 - Styled Components](https://www.youtube.com/watch?v=MqGxMOhPqeI&list=PL7jH19IHhOLNUIOJcGj6egl-dNB-QXjEm&index=1)
