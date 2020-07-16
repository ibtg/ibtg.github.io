---
layout: post
title: 'SASS - 조건문, 반복문'
subtitle: 'sass if-for'
categories: development
tags: sass
comments: true
---

- SASS의 조건문과 반복문을 공부하고 정리한 글입니다

---

### 반복문

- SASS에서는 `@for`과 `@each`를 사용해서 반복적되는 부분에 스타일을 적용시킬 수 있다

```html
<ol class="list">
  <li class="ico1">text1</li>
  <li class="ico2">text2</li>
  <li class="ico3">text3</li>
  <li class="ico4">text4</li>
</ol>
```

```scss
// 내가 원하는 숫자보다 한칸 앞까지
@for $i from 1 to 5 {
  .list li.ico#{$i} {
    color: red;
  }
  .list li:nth-child(#{$i}):before {
    content: '제목 #{$i} ';
  }
}
```

---

```html
<body>
  <h1 class="ico_book">제목1</h1>
  <h2 class="ico_zoom">제목2</h2>
  <h3 class="ico_phone">제목3</h3>
</body>
```

```scss
//book, zoom, phone이라는 이미지를 순서대로 가져온다

@each $var in book, zoom, phone {
  .ico_#{$var} {
    background: url(images/#{$var}.gif) no-repeat;
  }
}

$heading: (
  h1: 30px,
  h2: 20px,
  h3: 15px,
);

//ele는 선택자, 변수명이 된다.
@each $ele, $fs in $heading {
  #{$ele} {
    font-size: $fs;
  }
}
```

---

### 조건문

- SASS에서는 `@if`를 사용해서 조건문을 구현할 수 있다

```html
<body>
  <a href="" class="btn">button</a>
</body>
```

```scss
@mixin btn_radius($h, $radius: true) {
  background: #000;
  color: #fff;
  padding: 0 20px;
  height: $h;
  line-height: $h;
  text-align: center;

  @if $radius {
    border-radius: $h/2;
  } @else {
    border: 1px solid red;
  }
}

//false이면 border-radius가 적용되지 않는다
//true,false 에 따라 다른 값이 나온다
.btn {
  @include btn_radius(30px, false);
}
```

```html
<body>
  <div></div>
</body>
```

```scss
@mixin position($x, $y, $z) {
  position: absolute;
  left: $x;
  top: $y;
  z-index: $z;

  @if $x == 50% and $y == 50% {
    transform: translate(-50%, -50%);
  } @else if $x == 50% {
    transform: translateX(-50%);
  } @else if $y == 50% {
    transform: translateY(-50%);
  }
}

div {
  width: 300px;
  height: 300px;
  background: #000;

  @include position(50%, 50%, 2);
}
```

---

## Reference

- [edwith - 부스트코스 웹 UI 개발](https://www.edwith.org/boostcourse-ui/joinLectures/20749)

- [리베하얀 SASS](https://www.youtube.com/watch?v=jdG5OFX7Aic&list=PL_6yF2upGJYtKji9Wqrb3NoaowD5yTdXg)

- [Sass-lang](https://sass-lang.com/)
