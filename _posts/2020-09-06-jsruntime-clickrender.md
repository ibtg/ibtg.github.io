---
layout: post
title: '브라우저 런타임 환경의 이해(3) - Web API의 콜백 함수의 동작 순서'
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

- 또 다른 예시로 다음과 같은 코드가 있다

```html
<body>
  <button class="btn">Click</button>
  <div
    class="box"
    style="width:100px;height:100px;background-color: bisque;"
  ></div>
  <script>
    const btn = document.querySelector('.btn');
    const box = document.querySelector('.box');
    btn.addEventListener('click', () => {
      box.style.transition = 'transform 1s ease-in';
      box.style.transform = 'translateX(800px)';
      box.style.transform = 'translateX(500px)';
    });
  </script>
</body>
```

- 해당 코드를 작성한 의도가 버튼을 누르면 800px만큼 오른쪽으로 이동하고, 또 다시 누르면 이동한 위치에서 왼쪽으로 돌아와 처음 위치에서 500px만큼 곳에 박스가 있도록 하기 위한 것이라면 실제로는 의도한 대로 동작하지 않는다

- 어떤 식으로 translate를 얼마나 많이 추가했는지 상관 없이 Rendering이 발생할 때는 최종적으로 transform에 등록한 것이 적용된다

- 그 이유는 앞서 설명한 것과 같이 콜 스택의 콜백 함수의 코드가 모두 끝날 때 까지 이벤트 루프는 콜 스택에서 기다리기 때문이다

- 즉, 콜백 함수가 수행되는 동안 이벤트 루프가 기다리기 때문에 브라우저에는 변경사항이 보여지지 않게 되고 콜백 함수가 종료된 다음에 이벤트 루프가 최종적으로 적용된 사항을 Rendering 해서 브라우저에 표시가 되는 것이다

---

## Reference

- [이벤트 루프와 매크로·마이크로태스크](https://ko.javascript.info/event-loop)

- [JS 태스크 큐와 이벤트 루프](https://kjwsx23.tistory.com/311)
