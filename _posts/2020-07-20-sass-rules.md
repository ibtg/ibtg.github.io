---
layout: post
title: 'SASS - @rules '
subtitle: 'sass rules'
categories: development
tags: sass
comments: true
---

- SASS의 @RULES 에 대해서 공부하고 정리한 글입니다

---

## @RULES

- @RULES이란 @로 시작하는 규칙들을 지칭한다

---

### @function

- `@function` 구문에는 반드시 `@return` 값이 필요하다
- `@function` 명은 `-`, `_`를 구분하지 않는다

```scss
@function grid-width($n) {
  @return $n;
}

#sidebar {
  width: grid-width(5);
}
```

---

### @at-root

- nesting 룰과 관련된 룰로써, 중첩된 요소에 `@at-root`를 추가하는 경우에는 이전까지의 중첩된 부모 셀렉터를 무시하고 `@root`에서부터 중첩이 계산된다.

- 아래와 같은 scss 파일은 아래처럼 `.parent` 부분을 무시한채 css 파일로 컴파일 된다

```scss
//scss

.parent {
  .child {
    color: red;
  }
  @at-root .child2 {
    color: blue;
  }
}
```

```css
/* css */

.parent .child {
  color: red;
}
.child {
  color: blue;
}
```

---

### @media

- CSS에서 `@media` 룰이 최상위 선언밖에 안되기 때문에 코드상 거리가 있어서 한눈에 보기 어려운 경우가 많은데 이렇게 셀렉터 안쪽에 `@media` 쿼리를 사용하게 되면 어떤 케이스에 어떤 속성이 변경됐는지 알아보기 쉬운 장점이 있다

```scss
@media screen {
  .americano {
    @media (orientation: portrait) {
      width: 300px;
    }
    @media (orientation: portrait) {
      width: 300px;
    }
  }
}
```

---

### @debug

- 자바스크립트의 경우 콘솔이나 alert으로 값을 확인하는 것과 마찬가지로 sass에서는 이 `@debug` 룰을 사용할 수 있다

```scss
@debug 10em + 12em;
```

---

### @warn

- 조건문 사용 시에 특정 문구를 출력해줄 수 있는 기능

- 경고 문구이기 때문에 컴파일을 중단시키지는 않고 그냥 메세지만 출력해준다

```scss
@mixin adjust-location($x, $y) {
  // 값에 단위가 있는지 확인
  @if unitless($x) {
    @warn "assuing #{$x} to be in pixels";
    $x: 1px * $x;
  }
  @if unitless($y) {
    @warn "assuing #{$y} to be in pixels";
    $y: 1px * $y;
  }
}
```

---

### @error

- `@warn`이랑 비슷한 기능을 하지만 문구도 출력시켜줄 뿐 아니라 컴파일 자체를 중단시킨다

- 따라서 어떤`@mixin`이나 `@function`에서 반드시 조건이 충족되어야 하는 경우를 대비해서 사용할 수 있다.

- try-catch 문은 지원되지 않는다

```scss
@mixin adjust-location($x, $y) {
  // 값에 단위가 있는지 확인
  @if unitless($x) {
    @error "assuing #{$x} to be in pixels";
    $x: 1px * $x;
  }
  @if unitless($y) {
    @error "assuing #{$y} to be in pixels";
    $y: 1px * $y;
  }
}
```

---

## Reference

- [edwith - 부스트코스 웹 UI 개발](https://www.edwith.org/boostcourse-ui/joinLectures/20749)

- [Sass-lang](https://sass-lang.com/)
