---
layout: post
title: 'SASS - @extend, %PLACEHOLDER '
subtitle: 'sass extend'
categories: frontend
tags: sass
comments: true
---

- SASS의 @extend, %PLACEHOLDER를 공부하고 정리한 글입니다

---

### @extend

- `@mixin`이 css 패턴이나 반복되는 부분을 미리 정의해두고 필요할 때 마다 사용하는 것이라면 `@extend`의 경우는 이미 정의된 부분을 받아온 뒤에 확장해서 사용한다.

```html
<body>
  <div class="a">
    <div class="b"></div>
  </div>
</body>
```

```scss
.a {
  font-size: 14px;
}

.b {
  @extend .a;
  background-color: 'blue';
}
```

---

### %PLACEHOLDER

- `%` 선택자를 사용하면 `상속(Extend)` 할 수는 있지만 해당 선택자는 컴파일 되지 않는다.

- `extend` 할 때 자식선택자나 인접선택자를 사용해서 `extend` 할 수 없고 동일한 미디어 블록 안에서의 `extend`는 유효하지만 아래처럼 미디어 구문 안에서 밖에 있는 셀렉터를 `extend` 할 수 없다는 단점이 있기 때문에 이런 경우에 `%PLACEHOLDER` 셀렉터를 활용한다

  ```scss
  // scss
  .espresso {
    background-color: #391919;
  }
  @media print {
    .latte {
      @extend .espresso;

      color: #887e61;
    }
  }
  ```

---

```html
<body>
  <div class="a">abc</div>
  <div class="b">bc</div>
  <div class="c">c</div>
</body>
```

```scss
%name {
  font-size: 14px;
  color: blue;
}
@mixin name($bold, $color) {
  font-weight: $bold;
  background: $color;
}

.fs20 {
  font-size: 20px;
}

.a {
  @extend %name;
  @include name(bold, gray);
}

//이렇게 클래스를 불러와서 쓸 수도 있다
.c {
  @extend .fs20;
}

.b {
  @extend %name;
}
```

---

```html
<body>
  <div class="visual">
    <div></div>
  </div>
  <div class="main">
    <div></div>
  </div>
  <div class="footer">
    <div></div>
  </div>
</body>
```

```scss
//화면에 꽉 차 있지만 가운데 컨텐츠가 보이도록 디자인 하는 경우

.visual {
  background: pink;
  height: 300px;
  div {
    @extend %m0auto;
    background: rgba(0, 0, 0, 0.5);
    height: 300px;
  }
}
.main {
  background: purple;
  height: 500px;
  div {
    @extend %m0auto;
    background: rgba(0, 0, 0, 0.5);
    height: 500px;
  }
}
.footer {
  background: violet;
  height: 150px;
  div {
    @extend %m0auto;
    background: rgba(0, 0, 0, 0.5);
    height: 150px;
  }
}

%m0auto {
  width: 1200px;
  margin: 0 auto;
}
```

---

## Reference

- [edwith - 부스트코스 웹 UI 개발](https://www.edwith.org/boostcourse-ui/joinLectures/20749)

- [리베하얀 SASS](https://www.youtube.com/watch?v=jdG5OFX7Aic&list=PL_6yF2upGJYtKji9Wqrb3NoaowD5yTdXg)

- [Sass-lang](https://sass-lang.com/)
