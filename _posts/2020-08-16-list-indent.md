---
layout: post
title: 'li 태그 안에서 들여쓰기 하는 법'
subtitle: 'list inednt '
categories: frontend
tags: css
comments: true
---

- li 태그 안에서 들여쓰기하는 방법입니다

---

- css reset을 사용해서 margin이나 padding을 초기화 시키고 나면 그 이후에 각 태그가 생각대로 화면에 표시가 되지 않는 경우가 있습니다

- 아래는 `<ol>`태그와 `<li>` 태그에 `list-style-type` 속성 이외에 아무런 스타일을 적용하지 않은 상태입니다.

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-16-list-indent1.JPG?raw=true">

- 화면에서 확인할 수 있듯이 숫자 부분이 화면 끝에 붙어서 잘 보이지 않은 상태입니다

* `list-style-position` 값을 `inside` 바꾸게 되면 `<ol>` 태그의 parent tag의 padding은 적용이 되지만 아래 그림처럼 스크린의 크기를 줄이게 되면 두번째 줄 부터 들여쓰기가 적용되지 않는 것을 확인할 수 있습니다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-16-list-indent2.JPG?raw=true">

- 이를 해결하기 위한 방법으로 `text-indent`와 `padding-left`을 사용합니다

- `text-indent` 속성은 첫 줄의 들여쓰기를 위한 속성으로, 다음과 같이 속성을 지정하면, `padding-left` 때문에 모든 줄의 왼쪽에서 패딩을 가져야 하지만,`text-indent` 때문에 첫줄은 `-20px`만큼 padding을 가지게 됩니다

```css
li {
  list-style-position: inside;
  text-indent: -20px;
  padding-left: 20px;
}
```

- 따라서 아래처럼 두번째 줄 부터 들여쓰기가 되는 것을 확인할 수 있습니다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-16-list-indent3.JPG?raw=true">

---

## Reference

- [MDN web docs - list-style-position](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style-position)

- [MDN web docs - text-indent](https://developer.mozilla.org/en-US/docs/Web/CSS/text-indent)
