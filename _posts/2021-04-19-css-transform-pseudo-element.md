---
layout: post
title: '가상 요소(pseudo element)에 transform 속성을 적용하는 방법'
subtitle: 'css pseudo element transform'
categories: frontend
tags: css
comments: true
---

- 가상요소에 transform 속성을 적용하는 방법에 대해서 정리한 글입니다.

---

- 아래 코드는 `button`태그에 가상요소 `::before`를 사용해서 클릭하면 해당 컨텐트가 사라지는 모양 `ㅡ` 모양의 아이콘을 만든 코드입니다. 

- 하지만 코드를 실행시켜보면, `ㅡ` 모양의 아이콘이 세로로 `|` 모양으로 되어있는 것을 확인할 수 있습니다. 

```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="./index.css">
</head>
<body>
    <div>
        <div class="container">
            <div class="content"></div>
            <button class="delete__button"></button>
        </div>
    </div>
</body>
</html>

```

```css
/* index.css */

.container{
    width: 200px;
    height: 100px;
    position: relative;
    padding: 10px;
}

.content{
    border: 1px solid #000;
    width: 100%;
    height: 100%;
}

.delete__button{
    width: 28px;
    height: 28px;
    border-radius: 50%;
    position: absolute;
    right: 0px;
    top: 0px;
    background-color: darkseagreen;
}

.delete__button::before{
    content: '';
    width: 12px;
    height: 0;
    border: 1px solid #fff;
}
```

- 이러한 문제를 해결하기 위해 아래에 `transform: rotate(90deg);` 속성을 추가해주어도 rotate가 일어나지 않습니다

```css
/* index.css */

.delete__button::before{
    content: '';
    width: 12px;
    height: 0;
    border: 1px solid #fff;
    transform: rotate(90deg);`
}

```

- 그 이유는 Inline 요소는 transform 속성을 적용할 수 없는데, 가상요소는 Inline 요소이기 때문입니다

- 따라서, `display:block` 또는 `display:inline-block` 으로 해당 요소를 block level로 바꾸어 줍니다 

```css
.delete__button::before{
    content: '';
    width: 12px;
    height: 0;
    border: 1px solid #fff;
    display: inline-block;
}
```
- block level로 바꾸어주면 `ㅡ` 모양으로 바뀌는 것을 확인할 수 있습니다.


- 그리고 해당 요소의 절반을 다시 위로 이동시켜 수직으로 가운데 올 수 있도록 `transform` 속성을 사용해서 정렬시켜줍니다 

```css
.delete__button::before{
    content: '';
    width: 12px;
    height: 0;
    border: 1px solid #fff;
    display: inline-block;
    transform: translateY(-50% );
}
```
---

## Reference

- [css rotate a pseudo :after or :before content:“”](https://stackoverflow.com/questions/9779919/css-rotate-a-pseudo-after-or-before-content)
- [Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)
