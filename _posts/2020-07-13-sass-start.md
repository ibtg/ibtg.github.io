---
layout: post
title: 'SASS 시작하기 - 기본 옵션, 변수, 선택자'
subtitle: 'sass start'
categories: development
tags: sass
comments: true
---

- SASS에 대해서 공부하고 정리한 글입니다.

---

## SASS (Syntatically Awesome StyleShets)

- SASS는 CSS Preprocessor (전처리기)

- CSS는 별도의 컴파일 과정없이 브라우저에서 바로 읽을 수 있는 언어이다.

* 그러다 보니 문법이 잘못되거나 오탈자가 생기면 브라우저가 알아서 처리 해주는데 CSS preprocessor 사용하면 일차적으로 문법적인 오류나 오탈자 있는 경우 컴파일 과정에서 에러 메세지를 보여주기 때문에 오류를 확인하기가 쉽다.

* 전처리기 작업을 한다음에 CSS로 넘어가는 일을 컴파일이라고 한다

- CSS를 작업하기 전에 계속 반복적으로 처리해야 하는 작업이나 CSS에서 처리할 수 없는 부분을 CSS제작 이전 단계 에서 미리 만들어 놓는다.

- 즉 SASS와 같은 전처리기에서 미리 작업을 해 놓고 CSS화 시킨다

* 장점

  - 공식홈페이지에서는 다음과 같은 것들을 장점으로 내세운다

  - CSS Compatible

    - CSS 호환성
    - 기준에 CSS 파일을 확장자만 SCSS로 고쳐도 정상적으로 컴파일 된다
    - 이러한 호환성 때문에 에 컴파일 과정이 필요한 것이외에 특별히 어려운 부분이 없다

  - Indstry approved

    - 많은 기업들이 SASS를 도입해서 대규모 프로젝트에 사용한 경우가 많다.

  - Featrue rich

    - 다른 CSS 확장 언어보다 많은 기능들은 제공한다.

  - Large community

    - Stackoverflow, CSS-tricks에 SASS 관련된 게시물들이 굉장히 많고 이슈 발생했을 때 검색으로만으로 해결 할 수 있는 경우가 많다

  - Mature

    - 2006년에 처음 등장한 이후 13년이 지났다

  - Frameworks
    - SASS를 개량한 Framework들이 있다
    - Gulp나 Grunt 또는 Webpack 과 같은 태스크러너에서 다른 플러그인들과 조합했을 때 하나의 플러그인으로써 SASS의 효율이나 활용성이 좋다

---

- 설치

```javascript

$ node install -g node-sass

```

---

### --watch

- watch 옵션을 쓰면 scss 확장자 파일에서 변경사항이 생길 때 마다 자동으로 반영해준다

```javascript
$ sass --watch "sass file":"css file"

// sass --watch ./sass-project/style.scss:./style.css
```

---

### --style [expanded, compressed]

- css 결과물의 포맷팅 스타일을 결정하는 옵션으로 기본값은 expaned이다

```css
/* css */

.section {
  background: white;

  .title {
    color: green;
  }
}
```

- expande 옵션은 풀어서 들여쓰기를 써서 작성한 형태이다

```css
/* scss expand */

.section {
  background: white;

  .title {
    color: green;
  }
}
```

- compressed 옵션은 한줄로 공백 없이 문자열을 최소화한 형태이다

```css
/* scss compressed */
.section {
  background: white;
}
.title {
  color: green;
}
```

---

### --source-map

- 기본 값은 source map 옵션을 사용하기 때문에 컴파일 시 map 파일 생성된다

- CSS와 달리 SASS에서는 소스파일들을 쪼개서 사용하는 경우가 많기 때문에 --source-map 옵션 사용안하면 요소 검사툴 (ex. 크롬 개발자 도구)에서 스타일의 위치를 추적하기가 어려운 점이 있다.

- 예를 들어, compress 옵션을 사용하는 같은 경우 행 번호가 없어서 더욱 힘들다.

- 배포할 때는 --source-map 옵션을 제거해준다.

---

### @import

- import는 CSS에도 있는 구문이다. 하지만 CSS에서 import를 사용할 경우에 추가 파일 리퀘스트가 발생하기도 하고 import 된 CSS 파일이 로딩되는 중간에 새로 렌더링 되는 이슈가 있어서 사용이 지양된다.

- 그러나 SASS에서는 import를 컴파일 과정에서 불러오기 때문에 여러 개의 파일이 아니라 하나의 파일로 합쳐서 CSS 파일을 생성해준다

```css
/* _espresso.scss , 언더 바 있는 것은 임포트 전용으로 쓰겠다는 의미 */

@import 'espresso';
/* 파일명 첫번째 언더바(_) 생략 가능 */

@import 'espresso.scss';
/* 확장자는 생략 가능 */

@import 'scss/espresso';
/* 하위 폴더 지정 */

@import 'espresso', 'reset', 'common';
/* 여러개 파일 import */
```

---

### & selecter

- 자바스크립트의 this 와 유사한 개념

- 블록 안에 &를 넣게 되면 그 &는 바로 차상위의 셀렉터를 가리킨다.

- 따라서 부모 참조 셀렉터라고 부른다

```css
.button {
  display: block;
  width: 200px;
  height: 50px;
  &:hover {
    background: $gray;
    color: $white;
  }
}
```

---

### SASS 변수

- 변수 선언은 `$`로 시작한다
- 최 상단에서 선언하면 어디서든 쓸 수 있는 전역 변수가 된다.
- 셀렉터 블록 안에서 쓰면 블록안에서만 유효한 스코프를 가지는 지역 변수가 된다

```html
<body>
  <a href="#a" class="btn1">button1</a>
  <a href="#a" class="btn2">button2</a>
</body>
```

```css
$color: yellow;
/* 전역변수, */

/* 변수 선언하면 계속해서 변수로 선언한 것들 쓸 수 있다 */
$gray: #c9c9c9;
$white: #fff;
$blue: blue;
$red: red;
$fs30: 30px;

.btn1 {
  display: block;
  width: 200px;
  height: 50px;
  line-height: 50px;
  font-size: $fs30;
  color: $gray;
  background: $white;
  border: 1px solid $gray;
  text-align: center;
  &:hover {
    background: $gray;
    color: $white;
  }
}

.btn2 {
  display: block;
  /* 100%되어서 꽉찬 형태로 나온다 */
  padding: 10px;
  font-size: $fs30;
  color: $gray;
  background: $blue;
  border: 1px solid $red;
  text-align: center;
  &:hover {
    background: $gray;
    color: $white;
  }
}

body {
  background: $color;
}
```

---

### 주석

- SASS의 주석처리 방법으로는 두가지가 있다

- `//` 한 줄 주석처리를 한 부분은 SASS에는 있지만 CSS로 컴파일 되면 주석 처리한 부분이 없어진다.

- 따라서 SASS 를 위한 주석처리는 한 줄 주석처리로 하고 css 파일에서도 보여져야할 부분은 `/**/` 여러 줄 주석처리로 한다

---

### SASS 선택자

- Nesting

- 부모 선택자 블록 안에 중첩해서 사용할 수 있다.부모 선택자를 반복해서 쓰지 않아도 되기 때문에 가독성이 더 뛰어나고 구조화된 코드를 작성할 수 있다.

- 하지만 너무 중첩을 남발하면 오히려 가독성이 떨어질 수 있다

```html
<body>
  <div class="a">
    <ul>
      <li><a href="#a">테스트1</a></li>
      <li><a href="#a" class="bbb">테스트2</a></li>
      <li><a href="#a">테스트3</a></li>
    </ul>
  </div>
</body>
```

```css
.a {
  color: red;
  ul {
    border: 1px solid red;
    > li {
      background-color: pink;
      a {
        text-decoration: none;
        &:hover {
          color: red;
        }
        &.bbb {
          font-size: 11px;
        }
      }
      .bbb {
        text-indent: 100px;
      }
      &:last-child {
        border: 1px dotted black;
      }
    }
  }
}

/* css로 변한된 것과 비교해 보면 중복된 코드를 작성하지 않아도 된다는 것을 확인할 수 있다 */
```

---

## Reference

- [edwith - 부스트코스 웹 UI 개발](https://www.edwith.org/boostcourse-ui/joinLectures/20749)

- [리베하얀 SASS](https://www.youtube.com/watch?v=jdG5OFX7Aic&list=PL_6yF2upGJYtKji9Wqrb3NoaowD5yTdXg)

- [Sass-lang](https://sass-lang.com/)
