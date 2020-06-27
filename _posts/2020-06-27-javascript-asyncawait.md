---
layout: post
title: 'Javascript의 async & await '
subtitle: 'javascript async wait'
categories: development
tags: javascript
comments: true
---

- Javascript의 async, wait에 대해서 정리한 글입니다

---

### async & await

- `async & await`는 프로미스에 좀 더 간편한 api를 제공하는 역할을 하는 syntatic sugar입니다.

- `async & await`를 사용하면 프로미스를 좀 더 간결하고 동기적으로 실행되는 것 처럼 코드를 작성할 수 있습니다.

- 프로미스 계속해서 체이닝 하면서 코드를 작성하면 코드가 난잡해 질 수 있습니다
- 이 때 `async, await`을 사용하면 동기식으로 코드가 순서대로 실행되는 것처럼 코드를 작성할 수 있습니다.

---

### async

- 프로미스를 사용하지 않고 코드를 더 간편하게 사용할 수 있는 방법은 함수 앞에 `async`라는 키워드를 붙여주는 것입니다.

- `async` 를 함수 앞에 붙여주면 프로미스를 쓰지 않아도 자동적으로 함수안에 있는 코드 블록이 프로미스로 변환이 됩니다.

- 아래 코드를 실행하고 콘솔 창을 확인해 보면 프로미스가 리턴 된 것을 확인할 수 있습니다

```javascript
async function fetchUser() {
  return 'user!';
}

const user = fetchUser();
user.then(console.log);
console.log(user);
```

---

### await

- `await` 키워드는 프로미스를 기다리기 위해서 사용됩니다.
- 아래 코드처럼 `await`를 사용해서 firstFunc 함수가 1초 후, SecondFunc 함수를 2초후에 호출할 수 있습니다

```javascript
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function firstFunc() {
  await delay(1000);
  // 1초가 지나면 resolve 호출하는 함수
  // await 키워드 때문에 1초가 끝날 때 까지 기다려야 한다
  return 'first';
  // delay 함수가 성공적으로 수행되고 resolve를 실행하게 되면, firstFunc 함수에서도 first를 인자로 하는 resolve를 실행한다
}

async function secondFunc() {
  await delay(2000);
  return 'second';
}
```

- 만약 secondFunc 함수를 프로미스를 사용해서 구현한다면 다음과 같이 작성할 수 있습니다

```javascript
function secondFunc() {
  return delay(2000).then(() => {
    'second';
  });
}
```

- 동일 하게 프로미스로 구현한 코드와 `async & await`을 사용한 코드를 비교해 보면 프로미스를 체이닝 하는 것 보다 더 `async & await`을 사용한 코드가 더 직관적으로 이해할 수 있다는 것을 알 수 있습니다.

- 아래 코드는 위에서 작성한 firstFunc와 secondFunc 함수를 프로미스를 중첩해서 작성한 코드입니다

- 아래 코드는 `first to second` 까지만 출력 되지만 그 이상의 숫자를 출력하기 위해서 프로미스를 너무 많이 중첩적으로 사용하면 콜백 지옥을 작성할 때 처럼 코드의 가독성이 떨어진다는 단점이 있습니다

```javascript
function print() {
  return firstFunc().then((first) => {
    return secondFunc().then((second) => `${first} to ${second}`);
  });
}

print().then(console.log);
```

- 하지만 `async & await`를 사용하면 다음과 같이 `first to second` 이상의 숫더 간결하게 코드를 작성할 수 있습니다.

```javascript
async function print() {
  const first = await firstFunc();
  const second = await secondFunc();
  const third = await thirdFunc(); // third 리턴하는 함수
  const fourth = await fourthFunc(); // fourth 리턴하는 함수
  return `${first} + ${second} + ${third} + ${fourth}`;
}
print().then(console.log);
```

- 또한 error가 발생하는 경우에도 `try, catch`를 사용해서 해결할 수 있습니다

```javascript
async function secondFunc() {
  await delay(2000);
  throw 'error';
  return 'second';
}

async function secondFunc() {
  await delay(2000);
  return 'second';
}

async function print() {
  try {
    const first = await firstFunc();
    const second = await secondFunc();
  } catch {
    return 'error';
  }
}

print().then(console.log);
```

---

- 아래 코드는 위에서 작성한 first + second를 출력하는 코드입니다.

- 이 코드에서는 firstFunc 함수 1초, secondFunc를 2초를 각각 기다려야 합니다

- 하지만 두 함수는 서로 연관되어 있지 않기 때문에 서로 기다릴 필요가 없습니다

```javascript
async function print() {
  const first = await firstFunc();
  const second = await secondFunc();
  return `${first} + ${second}`;
}
print().then(console.log);
```

- 따라서 두 함수가 각각 병렬적으로 실행되도록 하기 위해서 아래 코드처럼 각각의 프로미스를 만들어 줍니다

- 프로미스는 만들어지는 순간 바로 실행되기 때문에 코드를 다음과 같이 수정하면 두 함수가 각각 병럴적으로 실행됩니다

```javascript
async function print() {
  // 프로미스를 만들자 마자 코드 블록 안의 함수가 실행된다, 병렬적으로 두 함수가 실행된다
  const firstPromise = firstFunc();
  const secondPromise = secondFunc();
  // await 키워드를 통해 두 함수의 실행이 끝날 때 까지 기다린다
  const first = await firstPromise;
  const second = await secondPromise;

  return `${first} + ${second}`;
}
```

---

- 이렇게 병렬적으로 프로미스를 실행하는 경우 프로미스에서 제공하는 api를 사용해서 더 간결한 코드로 나타낼 수 있습니다.

- `Promise.all`메서드에 프로미스를 담은 배열을 전달하게 되면, 배열에 있는 모든 프로미스들이 완료된 후 새로운 프로미스가 실행되는데, 배열에 있는 각각의 프로미스의 결과값들이 새로운 프로미스의 결과값이 됩니다.

```javascript
function print() {
  //firstFunc, secondFunc 두 함수에서 반환하는 first, second를 then의 인자로 전달한다
  return Promise.all([firstFunc(), secondFunc()]).then((number) =>
    number.join(' + ')
  );
}

print().then(console.log);
```

- 병렬적으로 프로미스들이 실행될 때 가장 먼저 완료되는 프로미스를 사용하기 위해서는 `Promise.race`를 사용합니다.

- 아래 코드를 실행하면 완료까지 firstFunc 함수는 1초, secondFunc는 2초가 걸리기 때문에 firstFunc 함수의 결과값인 `first`가 출력됩니다

```javascript
function print() {
  return Promise.race([firstFunc(), secondFunc()]);
}

print().then(console.log);
```

---

## Reference

- [MDN web docs - async](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN web docs - await](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await)
- [MDN web docs - Promise.all](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
- [https://www.youtube.com/watch?v=aoQSOZfz3vQ](https://www.youtube.com/watch?v=aoQSOZfz3vQ)
