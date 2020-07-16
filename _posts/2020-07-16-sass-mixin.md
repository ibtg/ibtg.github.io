---
layout: post
title: 'SASS - mixin, include, interpolation'
subtitle: 'sass mixin'
categories: development
tags: sass
comments: true
---

- SASS의 mixin, include, interpolation을 공부하고 정리한 글입니다

---

### @mixin & @include

- 자주 사용하는 css 패턴이나 반복되는 부분을 `@mixin`으로 작성한 다음, 필요할 때 마다 `@include`해서 사용한다

```html
<body>
  <div>test</div>
</body>
```

```scss
@mixin test {
  width: 100px;
  height: 100px;
  border: 1px solid red;
}

// div에 test가 바로 적용된다
// elements style 보면 확인할 수 있다
div {
  @include test();
}
```

---

```html
<body>
  <div>
    <p>1</p>
    <p>2</p>
  </div>
</body>
```

- 아래와 같이 SASS의 코드를 작성하면 `p`는 `float: left`이지만 `div`는 `float`의 부모이기 때문에 높이가 사라지게 된다

- 따라서 이러한 문제 해결하기 위해서 `clear`를 써야 한다

```scss
@mixin test {
  width: 100px;
  height: 100px;
  border: 1px solid red;
}

  border: 3px solid blue;
}
p {
  float: left;
  @include test();
}
```

- 우리가 앞서 배운 대로라면 다음과 같이 처리할 수 있지만 `clear` 는 매우 자주 나오기 때문에 `mixin`으로 만들어 준다

```scss
div {
  border: 3px solid blue;
  &:after {
    content: '';
    display: block;
    clear: both;
  }
}

p {
  float: left;
  @include test();
}
```

- `clear`를 `mixin`으로 만든 결과

```scss
@mixin test {
  width: 100px;
  height: 100px;
  border: 1px solid red;
}

@mixin clear {
  &:after {
    content: '';
    display: block;
    clear: both;
  }
}
div {
  border: 3px solid blue;
  @include clear();
}
p {
  float: left;
  @include test();
}
```

---

- `mixin`과 매개변수를 함께 사용할 수 있다 .

```html
<body>
  <div>test</div>
  <p>test2</p>
  <span>test3</span>
</body>
```

```scss
@mixin border($color) {
  border: 1px solid $color;
}

div {
  @include border(red);
}

p {
  @include border(blue);
}

span {
  @include border(pink);
}
```

```html
<body>
  <div>test</div>
</body>
```

```scss
@mixin box($w, $h) {
  width: $w;
  height: $h;
}

div {
  @include box(50px, 100px);
  border: 1px solid blue;
}
```

---

### interpolation

- SASS에서 변수를 사용할 때는 `#{}`안에 변수명을 넣어서 변수를 처리해준다

```html
<body>
  <div class="box">test</div>
</body>
```

```scss
$bx: box;
div.#{$bx} {
  border: 1px solid red;
}
```

```html
<body>
  <div class="box">
    <p>test</p>
  </div>
</body>
```

```scss
@mixin bx($position, $wid, $color) {
  border-#{$position}: $wid solid $color;
}

div {
  @include bx(right, 2px, red);
}

div {
  @include bx(bottom, 5px, gray);
}
```

---

## Reference

- [edwith - 부스트코스 웹 UI 개발](https://www.edwith.org/boostcourse-ui/joinLectures/20749)

- [리베하얀 SASS](https://www.youtube.com/watch?v=jdG5OFX7Aic&list=PL_6yF2upGJYtKji9Wqrb3NoaowD5yTdXg)

- [Sass-lang](https://sass-lang.com/)
