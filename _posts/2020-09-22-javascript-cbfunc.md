---
layout: post
title: '자바스크립트 클래스와 this 바인딩 '
subtitle: 'javascript this binding'
categories: frontend
tags: javascript
comments: true
---

- 자바스크립트 클래스의 this 바인딩 이슈에 정리한 글입니다

---

- 아래는 버튼에 이벤트 리스너를 등록하고 버튼을 클릭할 때 마다 `number` 클래스의 숫자를 출력하는 코드이다

```html
<body>
  <button class="button">button</button>
  <span class="number">0</span>
</body>
```

```javascript
class Field {
  constructor() {
    this.button = document.querySelector('.button');
    this.number = document.querySelector('.number');
    this.button.addEventListener('click', this.onClick);
  }

  onClick(event) {
    console.log(this.number.innerText);
  }
}
```

- 하지만 버튼을 클릭해도 이벤트가 작동하지 않는데 그 이유는 어떤 클래스 안에 있는 함수를 다른 콜백으로 인자로 전달할 때 `class` 정보는 함께 전달되지 않기 때문이다

- 따라서 `onClick` 함수 안의 `this`는 이 클래스 객체를 가르키지 않는다

- 자바스크립트에서 함수를 누군가에게 전달해 줄 때는 클래스 정보가 무시되기 때문에 함수를 클래스와 바인딩을 해주어야 한다

- 함수를 클래스와 바인딩하는 방법으로는 `bind` 함수를 사용하는 방법이 있다

- 다음과 같이 `bind` 함수를 사용해서 함수에 객체를 바인딩한 다음 버튼을 클릭하면 `number` 클래스 안의 숫자가 출력된다

```javascript
this.onClick = this.onClick.bind(this);
this.button.addEventListener('click', this.onClick);
```

- 또 다른 방법은 화살표 함수를 사용하는 것이다

- 아래처럼 화살표 함수 안에 실행하고자 하는 함수를 넣어 콜백 함수로 전달하면 `this`가 유지되기 때문에 버튼을 클릭 할 때 마다 결과값이 출력된다

```javascript
this.button.addEventListener('click', (event) => {
  this.onClick(event);
});
```

- 함수를 정의할 때 화살표 함수를 사용하는 방법도 있다
- 클래스 안에 있는 어떤 함수를 다른 콜백으로 전달할 때는 아래처럼 처음부터 화살표 함수로 전달할 함수를 정의하면 bind 함수나 콜백 함수안에서 화살표 함수를 사용하지 않아도 결과값이 출력된다

```javascript
class Field {
  constructor() {
    this.button = document.querySelector('.button');
    this.number = document.querySelector('.number');
    this.button.addEventListener('click', this.onClick);
  }

  onClick = (event) => {
    console.log(this.number.innerText);
  };
}
```
