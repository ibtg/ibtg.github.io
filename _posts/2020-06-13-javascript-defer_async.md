---
layout: post
title: 'Javascript의 this'
subtitle: 'javascript this'
categories: development
tags: javascript
comments: true
---

- Javascript의 async와 defer에 대해서 정리한 글입니다.

---

- HTML에서는 script 태그를 사용해서 자바스크립트 파일을 포함할 수 있습니다. 이때 script 태그의 위치나 사용하는 속성에 따른 차이점이 있습니다.

### 1. head 태그 안에 script 태그를 사용해서 js 파일을 포함하는 경우

- 아래 코드는 main.js 파일을 포함하는 코드입니다

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="main.js"></script>
  </head>
  <body></body>
</html>
```

- script 태그가 head에 있는 경우 브라우저가 한줄한줄 html을 parsing 하다가 script 태그를 만나면 parsing을 멈추고 필요한 자바스크립트 파일을 다운 받고 실행한 다음에 계속해서 파싱을 진행합니다. 위 코드는 아래 그림과 같이 실행이 됩니다

- 이때 자바스크립트 파일의 크기가 엄청 크다면 사용자가 웹 사이트를 보는데 많은 시간이 소비될 수 있기 때문에 script를 head에 포함하는 것은 좋은 방법은 아닙니다.

![head](../assets/img/post_img/head.png?raw=true)

---

### 2. script 태그를 body 맨 아래에 두는 경우

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div></div>
    <script src="main.js"></script>
  </body>
</html>
```

- 위 방법의 경우 html parsing이 끝나고 페이지가 준비가 된 다음에 js파일을 받은 다음 실행합니다
- 사용자가 js 파일을 받아오기 전에 page에서 기본적인 HTML 볼 수 있다는 장점이 있지만 페이지가 js에 매우 의존적인 경우, 즉, 사용자가 의미있는 컨텐츠를 보기 위해서는 js 파일이 필요한 경우에는 시간이 오래걸린다는 단점이 있습니다

![body](../assets/img/post_img/body.png?raw=true)

---

### 3. head 태그 안에 script 사용하면서 async 속성을 이용하는 경우

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script async src="main.js"></script>
  </head>
  <body></body>
</html>
```

- async는 boolean type 속성으로 선언하는 것 만으로 사용할 수 있습니다.
- parsing을 하다가 async 만나면 멈추지 않고 병렬로 main.js 파일 받으면서 계속 parsing을 진행합니다. 그리고 js 파일 다운로드가 다 끝나면 parsing 멈추을 멈추고 js파일을 실행합니다.
- 이 방법의 장점으로는 body 끝에 script 태그를 둘 때보다는 병렬적으로 이루어지기 때문에 다운로드 받는 시간을 절약할 수 있습니다.
- 하지만, js파일이 HTML이 parsing 되기도 전에 실행되기 때문에 만약 js에서 query selector를 이용해서 DOM요소를 조작하거나, 조작하려는 시점에 html에 우리가 원하는 요소가 정의되어 있지 않을 수 있기 때문에 이러한 부분에서 문제가 발생할 수도 있습니다.
- 그리고 html을 parsing하는 동안 js를 실행하기 위해 parsing을 멈출 수 있기 때문에 사용자가 페이지를 보는데 여전히 시간이 조금 더 걸릴 수 있는 단점이 있습니다
- 또한 async가 어려개 인 경우, 순서에 상관 없이 먼저 다운로드 된 js 파일을 먼저 실행하기 때문에 js가 순서에 의존적인 것이라면, 예를 들어 b 스크립트를 실행하는데 있어 a 스크립트가 선행되어야 한다면, async 옵션을 사용하는 것이 문제가 될 수 있다

![async](../assets/img/post_img/async.png?raw=true)

---

### 4. head 태그 안에 script 사용하면서 defer 속성을 이용하는 경우

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script defer src="main.js"></script>
  </head>
  <body></body>
</html>
```

- defer 옵션을 쓰면, parsing 도중에 js 파일을 만나면 병렬적으로 js 파일을 다운받되, parsing이 끝난 다음에 js 파일을 실행합니다.
- 그리고 defer은 script 태그가 여러개 있어서 js 파일을 포함하더라도 다운로드 시작한 순서대로 parsing이 끝난 후 차례대로 시작합니다.
- 따라서 header defer 속성을 쓰는 방법이 위의 3가지 방법에 비해서 안전하고 더 효율적인 방법이라는 것을 알 수 있습니다

![defer](../assets/img/post_img/defer.png?raw=true)

---

## Reference

- [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/API/HTMLScriptElement)
- [https://www.youtube.com/watch?v=tJieVCgGzhs&t=82s](https://www.youtube.com/watch?v=tJieVCgGzhs&t=82s)
