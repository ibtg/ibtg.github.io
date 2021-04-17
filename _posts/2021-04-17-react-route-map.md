---
layout: post
title: 'React에서 map 함수와 Route를 사용하는 방법'
subtitle: 'react route map'
categories: frontend
tags: react
comments: true
---

- React에서 map 함수와 Route를 사용하는 방법에 대해 정리한 글입니다.

---

- React에서는 다음과 같이 Route를 사용해서 여러 페이지로 이동을 할 수 있다

```jsx

// App.js

import { BrowserRouter, Route, Switch} from "react-router-dom";
import Page1 from './pages/Page1';
import Page2 from './pages/Page2';
import Page3 from './pages/Page3';



const App = () => {
  return (
    <BrowserRouter>
      <Switch>
        <Route exact path="/page1" component={Page1}></Route>
        <Route exact path="/page2" component={Page2}></Route>
        <Route exact path="/page3" component={Page3}></Route>
      </Switch>
    </BrowserRouter>
  );
};


```

- 하지만 이동해야할 페이지가 점점 더 많아지게 되면, 이동해야할 `Route`를 하나씩 작성하는 것 보다 관련있는 페이지들을 배열로 모은 다음 map 함수를 사용해서 코드를 작성하면 더 보기좋게 코드를 작성할 수 있다


```jsx

// route.js

export const routeList = [
  { label: "login", link: "/login", component: LoginPage },
  { label: "register", link: "/register", component: RegisterPage }
];

export const navList = [
  { label: "upload", link: "/upload", component: UploadPage },
  { label: "mypage", link: "/mypage", component: MyPage },
];


```

```jsx

// App.js

import { BrowserRouter, Route, Switch  } from "react-router-dom";
import { routeList, navList } from "./route";

const App = () => {
  return (
    <BrowserRouter>
      <Switch>
        {routeList.map(route => (
          <Route key={route.label} exact path={route.link} component={route.component} />
        ))}

        {navList.map(route => (
          <LoginRoute key={route.label} exact path={route.link} component={route.component} />
        ))}

      </Switch>
    </BrowserRouter>
  );
};


```

---

## Reference

- [Mapping Routes in React Router](https://www.digitalocean.com/community/tutorials/react-react-router-map-to-routes)