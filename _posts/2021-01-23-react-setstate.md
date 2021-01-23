---
layout: post
title: 'React의 setState에서 callback 함수 사용하기'
subtitle: 'react usecallback memo'
categories: frontend
tags: react
comments: true
---

- React의 setState에서 callback 함수를 사용하는 방법에 대해 정리한 글입니다.

---

- 리액트의 `setState`는 비동기적으로 작동한다

- 따라서 아래와 같은 코드에서 버튼을 클릭하면 3씩 증가하는 것이 아니라 1씩 증가한다

```jsx
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  };

  return (
    <div>
      <span>{`count is ${count}`}</span>
      <button onClick={handleIncrement}>+</button>
    </div>
  );
}

export default App;
```

- 이러한 경우 이전 `state`를 사용해서 새로운 `state`를 계산하는 함수를 `setState`에 전달함으로써 문제를 해결할 수 있다

- 아래 코드처럼 `setCount`에 `count`라는 인자를 전달받는 콜백함수를 전달하게 되면, 함수는 `count`의 이전 `state`를 값으로 받기 받는다.

- 따라서 각 함수가 이전 `state` 값에서 1 만큼 증가하기 때문에 버튼을 누르면 3씩 증가한다.

```jsx
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount((count) => count + 1);
    setCount((count) => count + 1);
    setCount((count) => count + 1);
  };

  return (
    <div>
      <span>{`count is ${count}`}</span>
      <button onClick={handleIncrement}>+</button>
    </div>
  );
}

export default App;
```

- 또 다른 상황으로는 아래처럼 버튼을 누르면 1씩 증가한 값이 화면에 출력되는 코드가 있다

- handleIncrement 함수를 보면 map 함수를 사용해서 하나씩 card가 선택한 card와 id와 같은지 비교한 다음에 같은 card에 대해 count를 1만큼 증가시킨다

- 하지만 이렇게 반복적으로 map 또는 loop를 사용하는 것은 좋지 않은데 반복적으로 비교해야하는 요소가 많을수록 성능이 좋지 않기 때문이다

```jsx
//App.js

import { useState } from 'react';
import Card from './components/card';

function App() {
  const [cards, setCards] = useState({
    1: { id: 1, name: 'TypeScript', count: 0 },
    2: { id: 2, name: 'JavaScript', count: 0 },
    3: { id: 3, name: 'Python', count: 0 },
  });

  const handleIncrement = (seletecCard) => {
    const newCards = Object.keys(cards).map((key) => {
      if (cards[key].id === seletecCard.id) {
        return { ...seletecCard, count: seletecCard.count + 1 };
      }
      return cards[key];
    });
    setCards(newCards);
  };

  return Object.keys(cards).map((key) => (
    <Card key={key} card={cards[key]} handleIncrement={handleIncrement}></Card>
  ));
}

export default App;
```

```jsx
//Card.js

import React from 'react';

const Card = ({ card, handleIncrement }) => {
  const onClick = () => {
    handleIncrement(card);
  };

  return (
    <div>
      <div>{`name is ${card.name}`}</div>
      <span>{`count is ${card.count}`}</span>
      <button onClick={onClick}>+</button>
    </div>
  );
};

export default Card;
```

- 이러한 상황을 `setState`에 콜백함수를 전달하는 방법으로 해결할 수 있다

- 아래처럼 prevCards라는 이전 state을 받아서 updateCards라는 새로운 객체를 만들고

- 이 객체에 클릭되는 항목의 count를 1만큼 증가시켜서 새로 객체에 할당하는 방식으로 문제를 해결할 수 있다

```jsx
//App.js
import { useState } from 'react';
import Card from './components/card';

function App() {
  const [cards, setCards] = useState({
    1: { id: 1, name: 'TypeScript', count: 0 },
    2: { id: 2, name: 'JavaScript', count: 0 },
    3: { id: 3, name: 'Python', count: 0 },
  });

  const handleIncrement = (seletecCard) => {
    setCards((prevCards) => {
      const updatedCards = { ...prevCards };
      updatedCards[seletecCard.id] = {
        ...seletecCard,
        count: seletecCard.count + 1,
      };

      return updatedCards;
    });
  };

  return Object.keys(cards).map((key) => (
    <Card key={key} card={cards[key]} handleIncrement={handleIncrement}></Card>
  ));
}

export default App;
```

- `useCallback`을 사용할 때 `setState`에 콜백함수를 전달함으로써 해결할 수 있는 상황도 있다

- 아래 코드는 위의 `setState`에 콜백함수를 사용하기 전 map 함수를 사용한 코드에서 dependency가 빈 배열인 `useCallback`을 사용한 경우이다

- 만약 아래처럼 코드를 작성 한후 `+`버튼을 눌러 특정 항목의 count를 증가 시킨 후 다음 항목의 `+`버튼을 누르면 이전에 count가 증가했던 항목의 count는 다시 0이 된다

- 그 이유는 `useCallback` 은 메모이징된 콜백 함수를 반환하는데, 아래 코드에서 콜백 함수로 전달된 함수에서의 cards는 처음 할당된 cards 객체를 값으로 가지기 때문이다

- dependency가 빈 배열이기 때문에 함수는 처음 한번만 정의되고, 새로운 항목의 + 버튼을 클릭하더라도 다시 최초의 cards에 대해 계산을 하기 때문에 다른 항목은 다시 count 값이 0이 된다

```jsx
//App.js

import { useState, useCallback } from 'react';
import Card from './components/card';

function App() {
  const [cards, setCards] = useState({
    1: { id: 1, name: 'TypeScript', count: 0 },
    2: { id: 2, name: 'JavaScript', count: 0 },
    3: { id: 3, name: 'Python', count: 0 },
  });

  const handleIncrement = useCallback((seletecCard) => {
    const newCards = Object.keys(cards).map((key) => {
      if (cards[key].id === seletecCard.id) {
        return { ...seletecCard, count: seletecCard.count + 1 };
      }
      return cards[key];
    });
    setCards(newCards);
  }, []);

  return Object.keys(cards).map((key) => (
    <Card key={key} card={cards[key]} handleIncrement={handleIncrement}></Card>
  ));
}

export default App;
```

- 이러한 경우도 아래처럼 `setState`에 콜백 함수를 전달함으로써 해결 할 수 있다

- `useCallback`을 사용할때 `setState`에 이전 state를 전달하는 콜백함수를 사용하면 함수가 한번만 정의되어 있더라도 최신의 state를 참조할 수 있기 때문이다

```jsx
//App.js

import { useState, useCallback } from 'react';
import Card from './components/card';

function App() {
  const [cards, setCards] = useState({
    1: { id: 1, name: 'TypeScript', count: 0 },
    2: { id: 2, name: 'JavaScript', count: 0 },
    3: { id: 3, name: 'Python', count: 0 },
  });

  const handleIncrement = useCallback((seletecCard) => {
    setCards((prevCards) => {
      const updatedCards = { ...prevCards };
      updatedCards[seletecCard.id] = {
        ...seletecCard,
        count: seletecCard.count + 1,
      };

      return updatedCards;
    });
  }, []);

  return Object.keys(cards).map((key) => (
    <Card key={key} card={cards[key]} handleIncrement={handleIncrement}></Card>
  ));
}

export default App;
```

---

## Reference

- [React 공식 문서](https://ko.reactjs.org/docs/hooks-reference.html#usestate)
- [setState를 바르게 사용하는 방법](https://blog.grotesq.com/post/728)
