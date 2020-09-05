---
layout: post
title: '브라우저 런타임 환경의 이해 - Microtask Queue, Render'
subtitle: 'javascript MicroQ rnder'
categories: development
tags: javascript
comments: true
---

- Javascript Runtime Environtment의 Microtask Queue와 Render를 공부하고 정리한 내용입니다.

---

- 자바스크립트 엔진의 콜 스택에 수행중인 작업은 끝날 때 까지 보장이 된다

- 즉, 중간에 다른 태스크나 다른 일들을 할 수 없고, 지금 수행중인 코드 블럭이 끝날 때 까지 이벤트 루프가 기다렸다가 콜 스택에서 해당 작업 아래에 있는 블럭을 수행하거나 태스큐에 있는 작업이 실행된다

- 자바스크립트 런타임 환경에서 태스크 큐 이외에도 다양한 것들이 있지만, 대표적으로 Render와 Microtask Queue가 있다

---

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-09-05-jsruntime-microq.jpg?raw=true">

### Render

- 브라우저에서 요소를 움직이거나 애니메이션이 일어날 때 주기적으로 브라우저 화면을 업데이트 해주는 역할을 한다

- 브라우저에서 DOM 요소를 변형한 것이 브라우저에 표시되기 위해서는 Render Tree가 만들어지고 Layout - Paint - Composition 과정을 통해서 브라우저에 표시가 된다

- 그 전에 Request Animation Frame 이라는 API가 있는데 이 API를 통해서 콜백을 등록해 놓으면, 다음에 브라우저가 업데이트가 되기 전에 등록된 콜백 함수들을 실행하게 된다

- Even Loop는 위 그림과 같이 Call Stack과 Render, Microtask Queue, Task Queue 사이를 계속해서 돌고 있다.

- 그리고 만약 콜 스택에서 수행 중인 함수가 있다면, 해당 함수의 작업이 끝날 때 까지 콜 스택에 머무르게 된다. 그렇기 때문에 콜 스택의 함수에서 시간이 오래 걸리는 작업을 한다면 사용자에게 화면이 업데이트 되어 보여지지 않고 다른 이벤트, 클릭과 같은 것들이 발생해도 등록된 콜백함수가 실행되지 않는다

- 콜 스택에 등록된 함수의 작업이 다 끝나고 나서 이벤트 루프는 다시 움직이게 되는데 이 때, Render 쪽으로는 갈 수도 있고 가지 않을 수도 있다

- 브라우저에서는 우리가 업데이트하는 내용을 사용자에게 60 fps (frame per seconds), 1초 동안 60개의 프레임을 보여주기 위해서 노력하는데, 그렇게 하기 위해서는 16.7ms 동안 업데이트가 일어나야 한다.

- 이벤트 루프는 아주 빠른 속도로 돌고 있기 때문에 한 바퀴를 도는데 1ms도 걸리지 않는다. 그래서 1ms 마다 Render를 업데이트 할 필요가 없기 때문에, 어느 정도 시간이 있다가 16.7ms 안에서 Render를 업데이트 한번 해주고 다른 작업을 하다가 다시 어느정도 시간이 지나면 Render를 업데이트 하는 방식으로 움직인다

---

### Microtask Queue

- 태스크 큐에는 우리가 등록한 이벤트가 발생했을 때, WEB APIs에 의해 콜백함수가 태스크 큐에 들어가는 것이라면, 마이크로 태스크 큐에는 Mutation observer의 콜백함수와 와 프로미스에 등록한 콜백 함수가 프로미스를 다 수행하고 나면 들어가게 된다

- 즉, 백엔드에서 데이터를 받아오는 fetch를 이용해서 프로미스를 만들고 then에 콜백 함수를 등록해 놓았다면, 프로미스의 수행이 다 끝나고 resolve가 되면 그 때 등록된 콜백이 마이크로 태스크 큐에 들어가게 된다.

- 이벤트 루프가 마이크로 태스크 큐에 콜백 함수들이 있는 것을 확인하게 되면, 콜 스택으로 가져가게 된다. 그리고 이 때 콜 스택으로 가져간 콜백 함수가 작업이 다 끝나고난 이후에도 마이크로 태스크 큐에 또 다른 콜백 함수가 있다면 이 콜백 함수를 다시 또 콜 스택으로 가져가게 된다.

- 예를 들어, 마이크로 태스크 큐의 콜백 함수를 콜 스택에서 수행하는 도중에 또 다른 콜백 함수가 마이크로 태스크 큐에 들어오게 되면, 콜 스택의 함수가 끝나는 순간 마이크로 태스크 큐에서 새로 들어온 콜백 함수를 콜 스택으로 가져가게 된다

- 즉, 태스크 큐에서는 한번에 콜백 함수 하나만 콜 스택으로 가져오는 것과 달리, 마이크로 태스크 큐가 모두 비어있을 때 까지 이벤트 루프는 마이크로 태스크 큐에 머무르게 된다

---

## Reference

- [JavaScript Visualized: Promises & Async/Await](https://dev.to/lydiahallie/javascript-visualized-promises-async-await-5gke?fbclid=IwAR3cfIk3iVpt1EoFOflRVs4VFe6GC2m2nbkP99bWgSduAkxVCIFSXVgKYzE)

- [이벤트 루프와 매크로·마이크로태스크](https://ko.javascript.info/event-loop)

- [JS] 태스크 큐와 이벤트 루프](https://kjwsx23.tistory.com/311)
