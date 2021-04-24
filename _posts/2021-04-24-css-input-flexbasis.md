---
layout: post
title: 'flex-basis가 적용되지 않을 때 해결하는 방법'
subtitle: 'css input flexbasis'
categories: frontend
tags: css
comments: true
---

- flex-basis가 적용되지 않을 때 해결하는 방법에 대해 정리한 글입니다. 

---

- 아래는 `flex-basis` 속성을 사용해서 flex item의 크기를 조절하는 코드이다.

```html

<!-- index.html -->
    <div class="container">
        <span class="title">Title</span>
        <input class="input1" type="text" placeholder="input1"/>
        <input class="input1" type="text" placeholder="input2"/>
    </div>

```

```css

/* index.css */

.container{
    display: flex;
}

.input1{
    flex-basis: 10%; 
    /* 50%, 30%, 20% 로 크기를 조절 하면 줄어드는 것을 확인할 수 있지만
    어느 정도 값이 작아지면 더 이상 Input 크기가 줄어들지 않는다 */
}
```

- `flex-basis`로 값을 조정하다 보면, 어느 정도 값이 아래로 작아지면 input의 크기가 변하지 않는 것을 확인할 수 있다

- 그 이유는 flex item은 item의 크기에 따라 default 값으로 `min-width: auto`, `min-height: auto` 값을 가지기 때문이다 

- 즉, `flex-basis` 속성을 작게 하더라도 `min-width: auto`, `min-height: auto`로 설정된 flex item의 크기보다 작아질 수 없기 때문에 input 태그의 크기가 변하지 않는 것이다 

- 이를 해결하는 첫번째 방법으로는 `min-width`을 `0`으로 직접 설정하는 방식으로 이를 해결할 수 있다.

```css

.input1{
    flex-basis: 10%;
    min-width:0;
}

```

- 또 다른 방법으로는 기본적으로는 flex item은 `overflow:visible`이므로, `overflow:hidden` 속성을 사용할 수도 있다 

```css

.input1{
    flex-basis: 10%;
    overflow: hidden;
}

```


---

## Reference

- [Why don't flex items shrink past content size?](https://stackoverflow.com/questions/36247140/why-dont-flex-items-shrink-past-content-size)
- [HTML inputs ignore flex-basis CSS property](https://stackoverflow.com/questions/46684636/html-inputs-ignore-flex-basis-css-property)



