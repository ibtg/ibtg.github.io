---
layout: post
title: '브라우저 런타임 환경의 이해(4)- Call Stack, Task Queue, Microtask Queue에서의 Event Loop'
subtitle: 'javascript queue'
categories: frontend
tags: javascript
comments: true
---

- Javascript Runtime Environtment의 Event Loop가 동작하는 방법에 대해서 공부하고 정리한 내용입니다.

---

- 자바스크립트 런타임 환경에서는 태스크 큐와 마이크로 태스크 큐를 이용해서 비동기 처리를 한다

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-09-05-jsruntime-microq.png?raw=true" width="600">

### Call Stack의 Loop

- 아래 코드는 콜 스택에서 작업이 수행되고 있는 동안은 절대로 렌더링이 발생하지 않는 것을 보여주는 예시이다.

- 아래처럼 작성된 코드를 실행하게 되면, 버튼을 클릭한 다음에 다시 버튼의 색깔이 돌아오지 않고 브라우저가 멈추게 된다

- 버튼을 클릭하면 web API에 의해 등록된 콜백 함수가 태스크 큐로 이동하게 되고, 이벤트 루프에 의해 콜 스택으로 콜백 함수가 이동되어 작업을 수행하게 된다.

- 하지만 `while(true)` 부분 때문에 계속해서 작업이 콜 스택에서 수행되기 때문에 이벤트 루프가 계속해서 머무르게 되고, 다시 렌더링을 하지 못하기 때문에 브라우저는 클릭을 해도 반응이 없고 버튼의 색깔도 다시 돌아오지 않는 것이다

- 그리고 시간이 지나면 `Page Unresponsive`라는 페이지가 반응을 하지 않는다는 에러 메시지가 발생한다.

- 이러한 예시를 통해서 알 수 있는 것은 콜 스택에 등록하는 함수를 작성할 때 너무 오랫동안 작업을 수행해야 하는 함수를 작성하는 것은 좋지 않다는 것이다

- 왜냐하면 이벤트 루프가 오랫동안 머물러 있는 만큼 브라우저에는 업에이트가 되지 않기 때문에 사용자의 클릭처리를 비롯한 이벤트들이 발생하지 않게 되기 때문이다

- 따라서 이러한 콜백 함수에는 간단한 작업을 수행하는 내용을 작성하는 것이 좋고 반복문이나 재귀 함수 같은 것들은 조심해서 작성하는 것이 좋다

```html
<style>
  button:hover {
    background-color: bisque;
  }
</style>

<body>
  <button>Click</button>
  <script>
    const button = document.querySelector('button');
    button.addEventListener('click', () => {
      while (true) {
        //repeat
      }
    });
  </script>
</body>
```

- 따라서 콜 스택에 등록하는 함수를 작성할 때는 너무 오랫동안 작업을 하는 함수를 작성하는 것은 좋지 않다

- 이벤트 루프가 콜 스택에 오랫동안 머무를 수록 브라우저에 업데이트가 되지 않고, 사용자의 클릭 처리 같은 이벤트도 발생하지 않기 때문에 콜백은 간단하게 작성해준다

- `while`이나 `for` 같은 `loop`나 `재귀 함수`를 작성할 때는 조심해서 사용하는 것이 좋다

---

### Call Stack의 setTimout

- 또 다른 예제는 재귀적으로 함수가 호출되는 경우이다.

- 버튼을 클릭하게 되면 콜백 함수가 실행되고 이 콜백 함수에 있는 `handleClick` 함수가 실행된다

- `handleClick` 함수가 실행되면 `handleClick` 이라는 문자열을 출력되고, Web API에 의해 `0 밀리 세컨드` 후에 등록된 콜백 함수가 태스크 큐에 이동하게 된다

- 태스크 큐로 이동한 콜백 함수는 다시 콜 스택으로 이동해서 수행되는데, 이 때 또 다시 `handleClick` 함수를 실행하게 된다.

- `handleClick` 함수를 호출하게 되는 과정이 계속해서 반복되기 때문에 태스크 큐에 콜백함수가 계속해서 들어오게 된다

```html
<style>
  button:hover {
    background-color: bisque;
  }
</style>

<body>
  <button>Click</button>
  <script>
    const button = document.querySelector('button');
    function handleClick() {
      console.log('handleClick');
      setTimeout(() => {
        console.log('setTimeout');
        handleClick();
      }, 0);
    }

    button.addEventListener('click', () => {
      handleClick();
    });
  </script>
</body>
```

- 앞의 예제인 `while 문`을 쓸 때와 달리 button의 색깔이 바뀌거나, button 을 클릭할 수가 있는데 그 이유는 콜 스택에 비면 이벤트 루프가 또 다시 순회를 하기 때문이다

- `handleClick` 함수가 실행되면 `setTimeout`에 등록한 콜백 함수는 Web API에 의해 `0 밀리세컨드`가 지나면 태스크 큐에 들어오게 되는데 이 때, 콜 스택이 비게되면 이벤트 루프가 다시 순회를 하게 된다

- 이벤트 루프는 태스크 큐에서 한번에 하나씩 콜 스택으로 가져오고 콜 스택의 모든 작업이 끝나면 다시 순회를 하게 되는데 이렇게 이벤트 루프가 순회하는 도중에 가끔씩은 `Rendering`를 해주기 때문에 버튼 하이라이트도 바꾸고 다양한 이벤트를 동시에 처리할 수 있게 된다

- 이렇게 `setTimeout`을 `0 밀리세컨드` 후에 실행하는 코드를 작성하는 테크닉도 많이 사용하는데, 이는 콜 스택 안에서 해당 콜백 함수를 실행하는 것이 아니라 콜 스택에서 작업이 끝나고 이벤트 루프가 다시 한 바퀴 돈 다음에 콜백 함수를 실행하고 싶을 때 사용한다

---

### Microtask Queue의 Promise

- 이벤트 루프는 큐 태스크에 있는 것들을 콜 스택으로 하나씩 가져오지만, 마이크로 태스크에서는 큐 안에 있는 것들이 모두 없어질 때 까지 계속해서 콜 스택으로 가져오기 때문에 이벤트 루프가 머무르는 것 처럼 작동한다.

- 이벤트 루프가 `마이크로 태스크 큐`에 머무는 것 처럼 작동하는 이유는 `마이크로 태스크 큐`가 일반 `태스크 큐` 보다 더 높은 우선순위를 갖기 때문에 이벤트 루프가 `마이크로 태스크 큐`에 있는 것들을 먼저 콜 스택으로 가져오기 때문이다

- 콜 스택에서 작업이 끝나면 이벤트 루프는 `마이크로 태스크 큐`를 먼저 확인하고, 만약 콜백 함수가 있다면 해당함수를 콜 스택에서 실행한다

- 아래 예시는 마이크로 태스크 큐에 등록된 콜백 함수가 동작하는 것을 보여주는 예시이다

```html
<body>
  <button>Click</button>
  <script>
    const button = document.querySelector('button');
    function handleClick() {
      console.log('handleClick');
      //
      Promise.resolve(0) //
        .then(() => {
          console.log('then');
          handleClick();
        });
    }

    button.addEventListener('click', () => {
      handleClick();
    });
  </script>
</body>
```

- 위 코드에서 Web APIs에 의해 클릭 이벤트가 발생하면 `handleClick` 함수가 `태스크 큐`에 들어간 다음 이벤트 루프에 의해 콜 스택으로 이동해서 실행된다.

- `handleClick` 이라는 문자열을 출력되고 프로미스가 만들어지는데 이 프로미스는 만들어지면서 resolve라는 API를 호출하기 때문에 0 이라는 값을 리턴하는 프로미스가 된다.

- 프로미스가 정상적으로 0을 만들게 되면 then에 등록된 콜백 함수가 `마이크로 태스크 큐`로 이동하게 되는데, 콜 스택에서 모든 작업을 마친 이벤트 루프가 순회를 하다가 `마이크로 태스크 큐`에 등록된 콜백 함수를 콜 스택으로 가져와 다시 실행하게 된다.

- 하지만 이 때 콜백 함수가 수행이 되면 then을 출력한 다음 `handleClick`을 다시 호출하게 된다

- `handleClick`에서는 또 다시 프로미스를 만들고 then에 등록된 함수는 `마이크로 태스크 큐`로 이동하게 된다.

- `마이크로 태스크 큐`에 등록된 콜백 함수에서 `handleClick`을 또 다시 호출하기 때문에 `마이크로 태스크 큐`에 계속해서 콜백 함수가 들어오게 된다.

- 앞의 예제인 `setTimeout`과 유사한데, 이번 예제는 프로미스를 사용하기 때문에 `마이크로 태스크 큐`를 이용하게 되고, 따라서 버튼을 클릭하면 브라우저가 멈추게 되면서 다시 버튼 색깔이 바뀌지 않고 무한정 로그가 출력된다

- 이처럼 `마이크로 태스크 큐`에 아이템이 있는 경우에는 계속해서 콜 스택으로 가져와서 실행하게 되고 `마이크로 태스크 큐`가 모두 비어있게 되면 다시 이벤트 루프는 순회를 하게 된다

---

## Reference

- [https://meetup.toast.com/posts/89](https://meetup.toast.com/posts/89)
- [The event loop by Philip Roberts at JSConf](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
