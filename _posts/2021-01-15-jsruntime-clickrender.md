---
layout: post
title: '브라우저 런타임 환경의 이해 - Web API의 콜백 함수의 동작 순서'
subtitle: 'javascript runtime callback'
categories: frontend
tags: javascript
comments: true
---

- Javascript Runtime Environtment에서 Web API의 콜백 함수의 동작 순서 정리한 내용입니다.

---

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-09-05-jsruntime-microq.png?raw=true" width="600">

- 아래 코드는 element를 생성하고나서 body에 등록한 다음 style과 innerText를 추가한 코드이다.

```html
<body>
  <button class="btn">Click</button>
  <script>
    const btn = document.querySelector('.btn');
    btn.addEventListener('click', () => {
      const element = document.createElement('h1');
      document.body.appendChild(element);
      element.style.color = 'red';
      element.innerText = 'hello';
    });
  </script>
</body>
```

- element를 생성한 다음 아무것도 하지 않고 body에 그대로 등록하면 비어져 있는 element가 body에 등록이 되기 때문에 body에 등록하기 전에 style과 innerText를 지정해야지 정상적으로 작동할 것 같지만 body 등록한 이후에 style과 innerText를 추가해도 정상적으로 동작한다

- 코드가 정상적으로 동작하는 이유를 이해하기 위해서는 우선 브라우저 런타임 환경에 대해 이해해야 한다.

- 코드를 보면 버튼에 웹 API 중 하나인 addEventListenr라는 함수를 이용해서 콜백 함수를 등록해 놓았다

- 따라서 콜백 함수의 코드 블럭은 클릭이 되었을 때 웹 API에 의해 태스크 큐로 이동하게 된다

- 태스크 큐에 등록된 아이템은 이벤트 루프에 의해 콜 스택으로 이동하게 되고, 콜 스택에서 해당 함수의 모든 작업이 다 끝날 때 까지 이벤트 루프는 기다리게 된다

- 즉, 콜백 함수 안에 있는 부분, element를 만들고 body에 등록하고 style과 innerText를 추가하는 모든 부분이 다 끝난 다음 콜 스택이 비어지게 된다

- 그리고 나서 이벤트 루프는 모든 수정 사항들이 다 적용된 상태에서 Render 시퀀스로 가게 되는 것이다

- 다시 말해, Render Tree를 만들 때는 콜백 함수 내의 모든 것들이 적용된 상태이다.

- 그러므로, 콜백 안에서 작성된 코드는 어떤 순서로 작성되던지 상관이 없는데 왜냐하면 콜백 함수가 콜 스택에 들어가는 순간 이벤트 루프는 함수가 다 실행될 때 까지 기다렸다가 나중에 Rendering 될 때 전체적으로 수정된 사항들이 Layout - Paint에 걸쳐 브라우저에 표시되기 때문이다

---

## Reference

- [이벤트 루프와 매크로·마이크로태스크](https://ko.javascript.info/event-loop)

- [JS 태스크 큐와 이벤트 루프](https://kjwsx23.tistory.com/311)
