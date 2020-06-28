---
layout: post
title: 'React의 React Router DOM '
subtitle: 'react react router dom'
categories: development
tags: react
comments: true
---

- React의 React Router DOM에 대해서 정리한 글입니다.

---

### SPA

- SPA는 SInge Page Application의 약자로 말 그대로 단일 페이지로 이루어져 있는 어플리케이션입니다.

- 전통적인 웹 어플리케이션은 여러 페이지로 이루어져있고, 클라이언트가 새로운 페이지를 요청할 때 마다 전체 페이지를 다운로드 하고 렌더링 하는 방식이었습니다.

- 하지만 SPA 방식은 전체 페이지를 다시 렌더링 하는 것이 아니라 변경 되는 부분만을 업데이트 하기 때문에 더 효율적이라는 장점이 있습니다

- 리액트를 사용해서 SPA 방식을 구현할 때 사용하는 라이브러리가 React Route DOM 입니다.

- 라우터(Router)란 사용자가 어떤 주소로 들어왔을 때, 그 주소에 해당되는 적당한 페이지를 사용자에게 보내주는 것을 의미합니다

---

### BrowserRouter

- 우선 리액트 라우터 돔을 적용하기 위해서는 우선 최상위 컴포넌트를 다음과 같이 `<BrowserRouter>`로 감싸줍니다

```jsx
ReactDOM.render(
  <BrowserRouter>
    <App></App>
  </BrowserRouter>,
  document.getElementById('root')
);
```

---

### Router

- 클라이언트가 요청하는 주소에 따라 다른 컴포넌트를 보여주기 위해서는 `<Route>` 컴포넌트를 사용해서 아래와 같이 코드를 작성합니다

```jsx
<Route path="주소" component={컴포넌트}></Route>
```

- 하지만 아래처럼 코드를 작성하고나면 `Topic`과 `Contact` 태그를 클릭하더라도 아래화면 처럼 컴포넌트가 중복되어 화면에 나타납니다

```jsx
//index.js

import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter, Route } from 'react-router-dom';

function Home() {
  return (
    <div>
      <h2>Home</h2>
      Home...
    </div>
  );
}

function Topics() {
  return (
    <div>
      <h2>Topics</h2>
      Topics...
    </div>
  );
}

function Contact() {
  return (
    <div>
      <h2> Contact </h2>
      Contact ...
    </div>
  );
}

function App() {
  return (
    <div>
      <h1>React Router DOM example</h1>
      <ul>
        <li>
          <a href="/">Home</a>
        </li>
        <li>
          <a href="/topics">Topic</a>
        </li>
        <li>
          <a href="/contact">Contact</a>
        </li>
      </ul>

      <Route path="/" component={Home}></Route>
      <Route path="/topics" component={Topics}></Route>
      <Route path="/contact" component={Contact}></Route>
    </div>
  );
}

ReactDOM.render(
  <BrowserRouter>
    <App></App>
  </BrowserRouter>,
  document.getElementById('root')
);
```

  <img src ="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-28-react-reactrouterdom1.jpg?raw=true">

- 그 이유는 `"/"` 때문입니다.

- `http://localhost:3000/` 주소로 이동하면 `path="/"` 때문에 `Home` 컴포넌트를 화면에 출력합니다

- 그리고 `http://localhost:3000/topics`이동하더라도 `path="/"`와 `path="/topics"`에 공통된 부분이 있기 때문에 두 화면이 출력되는 것입니다

- 이러한 문제를 `exact` 속성을 사용해서 해결할 수 있습니다

- 다음과 같이 `Route` 컴포넌트에 `exact` 속성을 추가하면 정확하게 주소가 일치하는 경우에만 해당하는 주소로 이동할 수 있게 됩니다

```jsx

<Route exact path="/" component={Home}></Route>
<Route path="/topics" component={Topics}></Route>
<Route path="/contact" component={Contact}></Route>
//topic 클릭하면 토픽의 주소와 exact path = "/" 부분이 정확히 일치하지 않기 대문에 라우팅되지 않는다

```

---

### Switch

- `exact`를 쓰지 않고도 `<Switch>` 컴포넌트를 사용해서 문제를 해결할 수 있습니다
- 우선 `<Switch>` 컴포넌트로`<Route>` 컴포넌트 부분을 감싸줍니다
- `<Switch>` 컴포넌트로 감싼 부분은 `path`가 일치하는 컴포넌트를 발견하면 해당 컴포넌트를 실행하고 나머지 컴포넌트를 실행하지 않습니다
- 그래서 다음과 같이 코드를 작성하면 `Topics`와 `Contact` 컴포넌트로 이동하려고 해도 `<Home>` 컴포넌트만 실행됩니다
- 그 이유는 `path ="/"`가 일치하는 `<Home>` 컴포넌트가 실행되면 아래 두 `<Topics>`와 `<Contact>` 컴포넌트는 실행되지 않기 때문입니다

```jsx
import { BrowserRouter, Route, Switch } from 'react-router-dom';

<Switch>
  <Route path="/" component={Home}></Route>
  <Route path="/topics" component={Topics}></Route>
  <Route path="/contact" component={Contact}></Route>
</Switch>;
```

- 하지만 `path="/"`을 맨 끝에 두게 되면 각각 다른 컴포넌트가 화면에 출력됩니다.

```jsx
<Switch>
  <Route path="/" component={Home}></Route>
  <Route path="/topics" component={Topics}></Route>
  <Route path="/contact" component={Contact}></Route>
</Switch>
```

- 또한 `<Switch>` 컴포넌트를 사용하면 사용자가 정해진 주소가 아니라 다른 주소로 들어올 때, 화면에 에러 메세지를 출력할 수 있습니다

- 다음과 같이 코드를 작성하면, 맨 첫줄의 `<Rotue>` 컴포넌트에서는 `exact` 때문에 `path="/"`인 경우에만 `Home`컴포넌트가 화면에 출력됩니다

- 따라서 맨 아래 줄에 `exact`가 없는 `path="/"`인 `<Route>`를 추가하면 정해지지 않은 다른 주소로 접속할 경우 가장 아래의 `<Route>` 컴포넌트의 주소의`path="/"`로 인해 error 메세지를 화면에 출력할 수 있습니다

```jsx
<Switch>
  <Route exact path="/" component={Home}></Route>
  <Route path="/topics" component={Topics}></Route>
  <Route path="/contact" component={Contact}></Route>
  <Route path="/">{'error'}</Route>
</Switch>
```

---

### Link

- `<Link>` 컴포넌트는 클릭했을 때 다른 주소로 이동시키는 컴포넌트로써 `<a>` 태그와 유사한 역할을 합니다.

- 개발자 도구의 Network 탭에서 비교해 보면 `<Link>` 컴포넌트와 `<a>` 태그의 차이를 알 수 있습니다.

- `<a>`태그를 사용하면, 태그를 눌러 새로운 화면을 불러 올 때 마다 새로고침되어 모든 파일을 다시 불러오지만 `<Link>` 컴포넌트를 사용하면 파일을 새로 불러오지 않는 것을 확인할 수 있습니다

```jsx
<ul>
  <li>
    <Link exact to="/">
      Home
    </Link>
  </li>
  <li>
    <Link to="/topics">Topic</Link>
  </li>
  <li>
    <Link to="/contact">Contact</Link>
  </li>
</ul>
```

---

### Navigation

- `<NavLink>`컴포넌트도 다른 주소로 이동시키는 컴포넌트입니다.

```jsx
<ul>
  <li>
    <NavLink exact to="/">
      Home
    </NavLink>
  </li>
  <li>
    <NavLink to="/topics">Topic</NavLink>
  </li>
  <li>
    <NavLink to="/contact">Contact</NavLink>
  </li>
</ul>
```

- `<NavLink>` 컴포넌트를 사용하면 `<Link>` 컴포넌트를 사용했을 때와 화면에 출력되는 내용은 똑같지만 개발자 도구의 Element 탭을 보면 아래와 같이 `class="active"` 부분이 생긴 것을 확인할 수 있습니다.

  <img src ="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-06-28-react-reactrouterdom2.jpg?raw=true">

* 컴포넌트를 클릭할 때 마다 새로운 클래스가 생기므로, css을 파일을 만들고 속성을 정의해 주면 컴포넌트를 클릭 할 때 마다 효과를 줄 수 있습니다.

* 아래처럼 css 파일을 만들고 import 하면 `<NavLink>`를 클릭해서 `active` 클래스가 생길 때 마다 css 파일에서 정의한 효과가 나타납니다

```css
// index.css

.active {
  background-color: tomato;
  text-decoration: none;
}
```

---

### useParams

- 우리가 갖고 있는 페이지가 3개가 아니라 수 백개 그 이상이 된다면, 수동으로 모든 페이지를 만들 수 없습니다.

- 따라서 모든 컴포넌트를 정의하는 것 대신 배열을 만들고 해당 배열과 일치하는 인덱스 정보를 화면에 보여주는 방식을 이용할 수 있는데 이 때 사용하는 것이 `useParmas`입니다

- `useParams`를 통해 파라미터 값을 전달받을 수 있습니다.

- `<Route>` 컴포넌트의 path 에 `<Route path="/:id">`와 같이 파라미터를 전달하면 `<Route>` 컴포넌트로 감싼 컴포넌트에 정의된 `useParams`로 전달된 값을 받을 수 있습니다.

- 아래 코드는 `<Topics>` 컴포넌트를 눌렀을 때 또 다른 컴포넌트를 만들고 컴포넌트에 해당하는 값을 화면에 출력하는 코드입니다.

- 이전 코드에서 `<Topics>` 컴포넌트를 수정하고, `<Topics>` 컴포넌트를 누르면 출력되는 `<Topic>`컴포넌트와 `<Topics>` 컴포넌트를 눌렀을 때 출력되는 `contents` 배열을 추가하였습니다.

```jsx
// index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import {
  BrowserRouter,
  Route,
  Switch,
  NavLink,
  useParams, // usePrams import
} from 'react-router-dom';

function Home() {
  return (
    <div>
      <h2>Home</h2>
      Home...
    </div>
  );
}

// Topic 컴포넌트에서 출력을 위한 배열추가
let contents = [
  { id: 1, title: 'HTML', description: 'HTML is ... ' },
  { id: 2, title: 'JS', description: 'JS is ... ' },
  { id: 3, title: 'React', description: 'React is ... ' },
];
```

- `<Topic>` 컴포넌트는 `useParams`를 통해 전달된 파라미터를 받고 해당 `id` 값과 일치하는 `content`를 화면에 출력합니다.

- 예를 들어 `<Topic>` 컴포넌트를 통해 출력된 React 라는이름을 가진 `<NavLink>` 컴포넌트를 클릭하면 `topic_id`로 `3`이 전달됩니다.

- 그리고 아래 `useParams`를 통해 값을 전달받고 반복문을 통해 `selected_topic` 값을 정한 다음 해당하는 값을 출력하게 됩니다

```jsx
function Topic() {
  let params = useParams(); // 컴포넌트로 전달된 파라미터를 받는다

  //topic_id로 전달 받은 값들 확인가능
  console.log('params: ', params, 'params.topic_id', params.topic_id);

  const topic_id = params.topic_id;
  let selected_topic = {
    title: 'Sorry',
    description: 'Not Found',
  };

  //파라미터로 전달받은 id와 일치하는 배열 값 찾는다
  for (let i = 0; i < contents.length; i++) {
    if (contents[i].id === Number(topic_id)) {
      selected_topic = contents[i];
      break;
    }
  }

  //파라미터로 전달받은 id와 일치하는 배열에 해당하는 title, description 출력
  return (
    <div>
      <h3>{selected_topic.title}</h3>
      {selected_topic.description}
    </div>
  );
}
```

- `<Topics>` 컴포넌트에서는 위 `<Topic>` 컴포넌트에 파라미터를 전달해줍니다

- `<Route path="/:topic_id">`를 통해 `id` 값을 파라미터로 전달합니다

```jsx
function Topics() {
  //화면에 출력하기 위한 배열
  let list = contents.map((content) => (
    <li key={content.id}>
      <NavLink to={'/topics/' + content.id}>{content.title}</NavLink>
    </li>
  ));

  return (
    <div>
      <h2>Topics</h2>
      Topics...
      {/* list의 내용이 출력된다 */}
      <ul>{list}</ul>
      {/* list에 해당하는 Route를 만둘어주었다 */}
      <Route path="/topics/:topic_id">
        <Topic></Topic>
      </Route>
    </div>
  );
}
```

- 아래는 나머지 전체 코드입니다.

```jsx
function Contact() {
  return (
    <div>
      <h2> Contact </h2>
      Contact ...{' '}
    </div>
  );
}

function App() {
  return (
    <div>
      <h1>React Router DOM example</h1>
      <ul>
        <li>
          <NavLink exact to="/">
            Home
          </NavLink>
        </li>
        <li>
          <NavLink to="/topics">Topic</NavLink>
        </li>
        <li>
          <NavLink to="/contact">Contact</NavLink>
        </li>
      </ul>
      {/* url에 따라 달라져야 하는 컴포넌트를 Route로 감싸준다  */}

      <Switch>
        <Route exact path="/">
          <Home></Home>
        </Route>
        <Route path="/topics">
          <Topics></Topics>
        </Route>
        <Route path="/contact">
          <Contact></Contact>
        </Route>
        <Route path="/">Not found</Route>
      </Switch>
    </div>
  );
}
ReactDOM.render(
  <BrowserRouter>
    <App></App>
  </BrowserRouter>,
  document.getElementById('root')
);
```

---

## Refernce

- [React Router DOM Guide](https://reacttraining.com/react-router/web/guides/quick-start)

- [생활코딩 - React Router DOM](https://www.youtube.com/watch?v=WLdbsl9UwDc)
