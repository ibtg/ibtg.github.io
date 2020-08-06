---
layout: post
title: 'Javascript의 redux'
subtitle: 'javascript redux'
categories: development
tags: javascript
comments: true
---

- 생활코딩 redux 강의를 듣고 정리한 글입니다.

---

### Redux

- 리덕스 공식 홈페이지에서는 Redux를 a predictable state container for javascript apps 라고 소개하고 있다.

- 코드의 복잡성을 낮추어야 예측 불가능한 애플리케이션의 복잡성을 낮추어주는 역할을 하는 도구가 리덕스이다.

- Single Sourec of Truth, 하나의 상태를 갖는다는 것이 리덕스의 특징이다.

- 상태는 객체로써 하나의 객체 안에 어플리케이션에서 필요한 모든 데이터를 관리함으로써 복잡성을 낮추어 준다.

- 한 곳에 중앙집중적으로 데이터를 관리하면 여러곳에 나누어 관리하는 것 보다 관리하기 더 편하다.

- 그리고 이때 데이터를 아무나 읽고 쓰지 못하고 인가된 함수를 통해서만 가능하다.

- 각각의 스테이트를 만들 때 철저하게 통제하고 데이터를 만들 때 데이터 원본 바꾸는 것이 아니라 원본 복제하고 복제한 데이터를 새로운 원본으로 바꾸는 방법을 채택하고 있기 때문에 각각의 상태의 변화가 서로에게 전혀 영향을 주지 않는다.

- 따라서 undo와 redo 같이 어플리케이션의 상태를 바꾸는 것이 굉장히 쉽다.

- 리덕스를 이용하면 현재 상태 뿐 아니라 이전의 상태 까지도 꼼꼼하게 레코딩하는 걸 통해서 과거의 어느 시점으로 돌아가 그 어플리케이션의 상태가 무엇인지를 찾아낼 수 있다.

- 이러한 리덕스의 특징은 어플리케이션의 상태를 훨씬 더 예측가능하게 한다

---

- 다음은 리덕스의 각 요소간의 관계를 나타낸 그림이다

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-07-08-javascript-redux1.png?raw=true">

---

### store

- store은 정보가 저장되는 공간으로 모든 정보가 store에 저장된다 (Ex. 현재 선택한 글에 대한 정보 등)

- store 안에는 state라는 실제정보가 저장되어있다.

- 절대로 state에 직접 접속하는 것이 금지되어있다 누군가를 통해서만 가능하다.

- store 만들면서 reducer도 만들어주어야 한다

---

### render

- store 밖에 있다.

- 리덕스랑은 상관 없이 우리가 작성한 코드로 ui를 만들어주는 역할을 한다.

- render는 언제나 state 값을 참조해서 ui를 만든다.

- state에 직접 접속할 수 없으므로 앞단에 dispatch, subscribe, getState라는 함수가 있다.

- render가 getstate에게 정보를 요청하면, getState가 state로 가서 정보 받아오고 다시 render에게 전달해준다.

---

### subscribe

- subscribe에 render 함수 등록하면, state 바뀔 때 마다 render 함수가 호출 되면서 ui가 갱신된다

---

### getState

- store에 잇는 state에 직접 접근하는 것이 금지이므로 getState틀 통해서 state를 가져온다

---

### dispatch & reducer

- submit나 onclick 같은 action이 일어나면 dispatch에게 전달된다.

- 그러면 dispatch가 reducer를 호출하게 되고 state 값과 action을 통해 전달되는 데이터 두개의 값을 인자로 전달한다.

- reducer 는 state와 action으로 전달된 데이터를 참조해서 새로운 state 값을 만들어서 반환한다.

- state가 새로운 값으로 변경되면 dispatch가 subscribe에 등록되어 있는 모든 render 를 다시 호출한다

* render가 호출되면서 다시 getState 호출하고 getState는 state를 가져와서 다시 render 에 전달해준다.

- 그리고 render는 변경된 state를 참조해서 다시 화면을 갱신한다

* 그러면 새로운 state에 맞게 ui가 바뀌게 된다

* 이것이 리덕스가 동작하는 전체적인 흐름이다

---

### Redux의 장점

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-07-08-javascript-redux2.png?raw=true">

- 위 그림에서 하나의 블록 마다 클릭하면 모든 색깔 바꾸는 기능을 추가한다고 하자.

- 리덕스가 없는 경우, 각각 블럭이 나의 블록 색깔을 바꾸는 코드, 나 빼고 나머지 두 블록의 색깔을 바꾸는 코드가 각각 필요하므로 각 블록당 3개의 코드가 필요하고 총 9개의 코드가 필요하다

- 블록이 4개면 16개, 100개면 10000개와 같이 블록이 늘어날수록 필요한 필요한 코드가 급격하게 늘어난다.

* 이러한 문제를 리덕스를 통해서 개선할 수 있다

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-07-08-javascript-redux3.png?raw=true">

- 위 그림이 리덕스를 구현한 것이고 가운데 버튼이 store라고 하자.

- 앞서 설명한 리덕스의 작동 방식에 따르면 한 블록의 state가 바뀌는 경우, store를 subscribe 하고 있는 다른 블록들에게 통보한다.

- 각각의 블록에 store의 상태에 따라 어떻게 달라져야하는지 각각의 코드를 구현해 놓는다면 그 코드에 따라서 state가 변경될 때 마다 동작한다

- 이 때 각각의 블록에서 필요한 코드는 다른 블록의 state가 변경되었을 때 통보받을 코드와 자신의 state가 변경되었을 때 store에게 알려줄 코드 이렇게 총 두개가 필요하다

- 따라서 3개의 블록이 있다면 총 6개, 5개의 블록이 있다면 10개, 100개의 블록이 있으면 총 200개의 코드가 필요하다.

- 이렇게 리덕스를 통해서 구현하면 훨씬 더 효율적으로 상태를 관리할 수 있다

---

- 아래 코드는 리덕스 없이 구현한 코드로, 각각의 red의 버튼을 누르면 모든 블록이 red로, green 버튼을 누르면 모든 블록이 green , blue 버튼은 모든 블록이 blue 색깔로 변하는 코드이다.

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .container {
        border: 5px solid black;
        padding: 10px;
      }
    </style>
  </head>
  <body>
    <div id="red"></div>
    <div id="green"></div>
    <div id="blue"></div>
    <script>
      function red() {
        document.querySelector('#red').innerHTML = `
        <div class="container" id="component_red">
          <h1>red</h1>
          <input type="button" value = "fire" onclick="
          document.querySelector('#component_red').style.backgroundColor='red';
          document.querySelector('#component_green').style.backgroundColor='red';
          document.querySelector('#component_blue').style.backgroundColor='red';
          "/>
        </div>`;
      }
      red();

      function green() {
        document.querySelector('#green').innerHTML = `
        <div class="container" id="component_green">
          <h1>green</h1>
          <input type="button" value = "fire" onclick="
          document.querySelector('#component_green').style.backgroundColor='green'
          document.querySelector('#component_red').style.backgroundColor='green'
          document.querySelector('#component_blue').style.backgroundColor='green';"/>
        </div>`;
      }
      green();

      function blue() {
        document.querySelector('#blue').innerHTML = `
        <div class="container" id="component_blue">
          <h1>blue</h1>
          <input type="button" value = "fire" onclick="
          document.querySelector('#component_green').style.backgroundColor='blue'
          document.querySelector('#component_red').style.backgroundColor='blue'
          document.querySelector('#component_blue').style.backgroundColor='blue';"/>
        </div>`;
      }
      blue();
    </script>
  </body>
</html>
```

- 이렇게 코드를 작성하게 되면 다음과 같은 문제점이 생긴다

- 각각의 블록이 서로 연관되어 있기 때문에 블록의 수가 늘어날 수록 추가해야할 코드수도 엄청나게 증가하게 된다.

- blue 함수를 보면 red와 gren에 대한 정보도 있다. 모든 함수가 서로 의존하고 있기 때문에 red나 green 부분을 지워버리면 blue 함수는 에러를 발생하게 된다

- 그리고 새로운 함수 추가하면 기존의 모든 함수에 새로운 함수의 정보를 추가해야 하기 때문에 새로운 함수의 수가 늘어날 수록 추가해야할 코드수도 엄청나게 증가하게 된다.

---

- 이러한 문제를 리덕스를 사용해서 새결할 수 있는데 다음과 같이 코드를 구현할 수 있다

- 우선 리덕스를 사용할 수 있게 해주는 script를 추가한다

```html
   <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
    <!-- 리덕스를 사용할 수 있게 해주는 script 추가 -->
  </head>
```

- 그리고 store를 만들고 reducer 를 추가한다

```javascript
function reducer(state, action) {
  //dispatch에 의해서 액션이 들어오면 액션 값과 기존의 스테이트
  //값과 비교해서 새로운 스테이트 값을 만들어준다
  if (state === undefined) {
    //state 값이 setting되지 않은 경우= > 최초의 초기화 단계
    //초기화 될 때는 state 값이 없으므로
    return { color: 'red' };
  }
}

let store = Redux.createStore(reducer);
// 리덕스를 만들고 인자로 reducer 를 전달해준다

console.log(store.getState());
//getState를 사용해서 state를 가져온다
```

- reducer와 store 부분이 완성된 코드는 다음과 같다

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .container {
        border: 5px solid black;
        padding: 10px;
      }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
    <!-- 리덕스를 사용할 수 있게 해주는 script 추가 -->
  </head>
  <body>
    <div id="red"></div>

    <script>
      function reducer(state, action) {
        //dispatch에 의해서 액션이 들어오면 액션 값과
        //기존의 스테이트 값과 비교해서 새로운 스테이트 값을 만들어준다
        if (state === undefined) {
          //state 값이 setting되지 않은 경우= > 최초의 초기화 단계
          //초기화 될 때는 state 값이 없으므로
          return { color: 'red' };
        }
      }
      let store = Redux.createStore(reducer);
      // 리덕스를 만들고 인자로 reducer 를 전달해준다
      console.log(store.getState());

      function red() {
        let state = store.getState(); // state 값을 가져온다
        document.querySelector('#red').innerHTML = `
        <div class="container" id="component_red" style="background-color:${state.color}">
          <h1>red</h1>
          <input type="button" value = "fire" onclick="
          document.querySelector('#component_red').style.backgroundColor='red';
          document.querySelector('#component_green').style.backgroundColor='red';
          document.querySelector('#component_blue').style.backgroundColor='red';
          "/>
        </div>`;
      }
      red();
    </script>
  </body>
</html>
```

- 그리고 reducer와 action을 이용해서 새로운 state를 만드는 기능을 추가해 준다

- 이 때 reducer에서 원본을 변경하는 것이 아니라 원본을 복사하고 복사한 값을 변경해서 반환해주어야 한다

- action에 의해서 state가 바뀔 때 마다 바뀌는 각각의 데이터들은 완전히 서로 독립되 데이터들이어야 하기 때문이다

- 변경된 코드는 다음과 같다

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .container {
        border: 5px solid black;
        padding: 10px;
      }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
  </head>
  <body>
    <div id="red"></div>

    <script>
      //reducer는 actio과 이전의 state의 값을 비교해서 새로운 state 값을 리턴해 준다

      // 그리고 리턴한 값ㅇ른 원본을 바꾼 것이 아니라
      // 원본을 복사한 값을 변경시킨 새로운 state이다
      function reducer(state, action) {
        //fire 버튼 누르면 확인 가능 하다
        console.log(state, action);
        if (state === undefined) {
          return { color: 'yellow' };
        }
        //state를 직접 바꾸지 말고 복사본을 만들고
        // 변경해야지 undo redo 같은 기능을 사용가능하다
        var newState;
        if (action.type === 'CHANGE_COLOR') {
          newState = Object.assign({}, state, { color: 'red' });
          console.log(newState);
        }
        return newState;
      }
      let store = Redux.createStore(reducer);
      // 인자로 reducer 를 전달해준다
      console.log(store.getState()); // 변경된 state 확인가능
      function red() {
        let state = store.getState();
        // state 값을 가져온다
        // 버튼에서 onclick 이벤트가 발생하면
        //dispatch의 인자값이 reducer의 action의 인자 값으로 전달된다

        document.querySelector('#red').innerHTML = `
        <div class="container" id="component_red" style="background-color:${state.color}">
          <h1>red</h1>
          <input type="button" value = "fire" onclick="store.dispatch({type:'CHANGE_COLOR', color:'red'})"/>
        </div>`;
      }
      red();
    </script>
  </body>
</html>
```

- 위 코드처럼 작성하면 fire 버튼 클릭하면 reducer가 호출 되어서 state 값이 변하게 되고 콘솔창에는 color : "red"가 출력되지만 실제로 red 함수가 호출 되지 않는다.

- 따라서 다음과 같이 subscrbie 에 render 함수 red를 등록해 놓아야지만 state가 바뀔 때 마다 red 함수가 호출되어서 값이 변경되는 것을 확인할 수 있다.

```javascript
// subscriber에 render를 등록해 놓으면
// 디스패치가 스테이트 값을 바꾸고 난 다음에 red 함수를 호출하게 된다
// 즉, state 값이 바뀔 때 마다 호출 된다
store.subscribe(red);
```

- blue와 green을 추가한 최종 완성된 코드는 다음과 같다

- 아래와 같이 코드 작성하면 red 함수의 fire 버튼을 눌러서 state가 변경되더라도 subscribe 에 등록된 blue 함수도 호출 되기 때문에 동시에 배경색이 변경된다

- 즉, action이 발생해서 dispatch가 호출되고, dispatch가 reducer를 호출해서 reducer가 기존의 state와 변경된 state를 비교한 후에 state가 변경되었다면, 변경된 새로운 state를 반환하게되고 이 때 state가 변경되면 subscribe에 등록된 모든 함수가 다시 호출된다

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .container {
        border: 5px solid black;
        padding: 10px;
      }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.js"></script>
  </head>
  <body>
    <div id="red"></div>
    <div id="blue"></div>
    <div id="green"></div>

    <script>
      //reducer는 actio과 이전의 state의 값을 비교해서 새로운 state 값을 리턴해 준다
      // 그리고 리턴한 값ㅇ른 원본을 바꾼 것이 아니라
      // 원본을 복사한 값을 변경시킨 새로운 state이다
      function reducer(state, action) {
        //fire 버튼 누르면 확인 가능 하다
        console.log(state, action);
        if (state === undefined) {
          return { color: 'yellow' };
        }
        //state를 직접 바꾸지 말고 복사본을 만들고
        // 변경해야지 undo redo 같은 기능을 사용가능하다
        var newState;

        if (action.type === 'CHANGE_COLOR') {
          newState = Object.assign({}, state, { color: action.color });
          console.log(newState);
        }
        return newState;
      }

      let store = Redux.createStore(reducer); // 인자로 reducer 를 전달해준다
      console.log(store.getState());

      function red() {
        let state = store.getState(); // state 값을 가져온다
        document.querySelector('#red').innerHTML = `
        <div class="container" id="component_red" style="background-color:${state.color}">
          <h1>red</h1>
          <input type="button" value = "fire" onclick="store.dispatch({type:'CHANGE_COLOR', color:'red'})"/>
        </div>`;
      }
      // subscriber에 render를 등록해 놓으면
      // 디스패치가 스테이트 값을 바꾸고 난 다음에 레드함수를 호출하게 된다
      // 즉, state 값이 바뀔 ㄸ ㅐ마다 호출 된다
      store.subscribe(red);
      red();

      function blue() {
        let state = store.getState();
        document.querySelector('#blue').innerHTML = `
        <div class="container" id="component_blue" style="background-color:${state.color}">
          <h1>blue</h1>
          <input type="button" value = "fire" onclick="store.dispatch({type:'CHANGE_COLOR', color:'blue'})"/>
        </div>`;
      }
      store.subscribe(blue);
      blue();

      function green() {
        let state = store.getState();
        document.querySelector('#green').innerHTML = `
        <div class="container" id="component_green" style="background-color:${state.color}">
          <h1>green</h1>
          <input type="button" value = "fire" onclick="store.dispatch({type:'CHANGE_COLOR', color:'green'})"/>
        </div>`;
      }
      store.subscribe(green);
      green();
    </script>
  </body>
</html>
```

- 리덕스를 통해서 모든 상태를 중앙 집중적 으로 관리해주면 되면 지금 작성하는 부분만 집중하면 된다는 장점이 있다

- 그 부분에서 어떤 action이 일어났을 때 store에게 action을 전달하는 코드와 state가 바뀌었을 때 바뀐 state를 전달 받는 부분만 고려해서 코드를 작성하면 된다

- a라는 부품을 만들때는 a라는 부품에만 집중하면 되고 다른 부품에는 신경 쓰지 않아도 된다는 장점이 있기 때문에 덜 복잡한 코드로 복잡한 코드에 도전할 수 있게 된다

* 따라서 리덕스를 통해 구현한 마지막 코드를 보면 blue 함수에 red나 green 의 정보가 필요가 없다

---

## Reference

- [생활코딩 - Redux](https://opentutorials.org/module/4078)
- [코드](https://github.com/ibtg/redux-opentutorials)
