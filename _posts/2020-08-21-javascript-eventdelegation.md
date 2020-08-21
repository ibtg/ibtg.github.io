---
layout: post
title: 'Event Capturing & Bubbling 그리고 Event deleation'
subtitle: 'javascript event delegation'
categories: development
tags: javascript
comments: true
---

- Event Capturing & Bubbling , Event delegation에 대해서 공부하고 정리한 글입니다

---

### Event Capturing & Bubbling

- 아래 코드를 실행시키면 가장 안쪽에 있는 button을 클릭하여도 outer와 middle 부분이 출력되는 것을 확인할 수 있다

```html
<!-- css -->
<style>
  .outer {
    width: 300px;
    height: 300px;
    background-color: burlywood;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .middle {
    width: 200px;
    height: 200px;
    background-color: yellowgreen;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
```

```html
<!-- body, script -->
<body>
  <div class="outer">
    <div class="middle">
      <button>Click</button>
    </div>
  </div>
  <script>
    const outer = document.querySelector('.outer');
    const middle = document.querySelector('.middle');
    const button = document.querySelector('button');

    outer.addEventListener('click', () => {
      console.log(`outer: ${event.currentTarget},${event.target}`);
    });

    middle.addEventListener('click', () => {
      console.log(`middle: ${event.currentTarget},${event.target}`);
    });

    button.addEventListener('click', () => {
      console.log(`button1: ${event.currentTarget},${event.target}`);
    });

    button.addEventListener('click', () => {
      console.log(`button2: ${event.currentTarget},${event.target}`);
    });
  </script>
</body>
```

- click me 버튼을 눌렀을 때 middle outer 호출되는 이유는 이벤트 캡처링과 버블링 때문이다

- click me 버튼을 클릭했을 자식 요소에서 발생한 이벤트가 부모 요소 부터 자식 요소까지 도달하는 것을 캡처링이라고 한다

- 그리고 캡처링이 끝나고 나면 자식 요소부터 시작해서 발생한 이벤트가 부모요소까지 도달하는 것을 버블링이라고 한다

- 이러한 이유로 부모 요소는 자식 요소에서 발생하는 모든 이벤트를 전달받을 수 있다

- 대부분은 캡처링이 단계에서 무엇인가를 처리하는 것은 흔하지 않고 버블링 단계에서 이벤트를 처리 한다

- target은 이벤트가 발생한 요소이고, currentTarget은 이벤트 핸들러를 등록한 요소를 지칭한다

- target과 currentTarget이 다르면 이 요소에서 이벤트가 일어나지 않은 것을 알 수 있다

- 해당 요소에서 발생한 이벤트를 상위 요소로 전달하지 않기 위해서는 `evet.stopPropagation()` 를 추가하면 된다

```html
<script>
  button.addEventListener('click', () => {
    console.log(`button1: ${event.currentTarget},${event.target}`);
    event.stopPropagation();
  });
</script>
```

- 그리고 한 요소에 두개의 이벤트 핸들러를 등록했을 때 다른 이벤트 리스너도 방지하고 싶다면 아래처럼 `event.stopImmediatePropagation()`을 추가한다

```html
<script>
  button.addEventListener('click', () => {
    console.log(`button1: ${event.currentTarget},${event.target}`);
    event.stopImmediatePropagation();
  });
</script>
```

- 하지만 이 두개는 가능한 사용하지 않는 것이 좋은데, 다른 이벤트가 해당 요소의 이벤트가
  일어났을 때 관련되어있는 일을 처리해야할 수도 있기 때문이다

- 따라서 내가 관심이 있을 때만 처리를 하도록 currentTarget과 target에 따른 조건을 설정해준다

- 아래처럼, outer와 middle의 코드를 수정하면, outer와 middle에 대한 부분이 출력되지 않는 것을 확인할 수 있다

```html
<script>
  outer.addEventListener('click', () => {
    if (event.target !== event.currentTarget) {
      return;
    }
    console.log(`outer: ${event.currentTarget},${event.target}`);
  });

  middle.addEventListener('click', () => {
    if (event.target !== event.currentTarget) {
      return;
    }
    console.log(`middle: ${event.currentTarget},${event.target}`);
  });
</script>
```

### Event delegation

- 아래 코드는 li 태그를 누르면 배경 색깔이 바뀌는 코드이다

```html
<!-- css -->
<style>
  .selected {
    background-color: turquoise;
  }
</style>
```

```html
<!-- body script -->

<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
    <li>9</li>
    <li>10</li>
  </ul>

  <script>
    const lis = document.querySelectorAll('li');
    lis.forEach((li) => {
      li.addEventListener('click', () => {
        li.classList.add('selected');
      });
    });
  </script>
</body>
```

- 하지만 이렇게 모든 요소를 찾아서 일일이 이벤트를 등록하는 것은 좋은 코드가 아니다

- 자식 요소에서 발생한 이벤트를 부모 요소에서 전달받을 수 있기 때문에 다음과 같이 li 태그의 상위 요소인 ul 태그에 이벤트 리스너를 등록해주면 된다

```html
<script>
  const ul = document.querySelector('ul');
  ul.addEventListener('click', (event) => {
    if (event.target.tagName === 'LI') {
      event.target.classList.add('selected');
    }
  });
</script>
```

- 부모 안의 자식들에게 공통적으로 무엇인가 처리해야 할 때 일일이 이벤트 리스너를 자식 노드에 추가하는 것 보다 부모에 등록하는 것이 더 좋다
