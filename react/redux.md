# Redux

---

Redux는 React 상태 관리에 있어서 유용한 라이브러리 입니다. 사실 Redux는 React 뿐만 아니라 Vanilla JS 에서도 사용하는 상태 관리 라이브러리 입니다. 현재 함수 컴포넌트의 hooks에서 지원하는 useContext와 useReducer를 사용해도 왠만한 크기의 상태 관리는 충분히 가능하지만 Redux 역시 아직까지 많이 사용되기 때문에 간단한 사용 방식을 알아보고자 합니다.

## Getting Started

해당 포스팅은 함수 컴포넌트에서 Redux를 사용하는 방법에 대해서 소개할 예정입니다. 일단 Redux를 사용하기 전에 알아야 할 3가지 개념이 있습니다. 바로 actionTypes, actions, reducer 이며 이 3가지 개념을 이해하고 간단히 Redux를 활용한 상태 관리에 대해서 알아보도록 하겠습니다.

```jsx
npm install redux
npm install react-redux
```

## ActionTypes

상태에 대한 변화를 우리는 ActionType으로 정의할 수 있습니다. 예를 들어서 숫자를 증가 , 감소 시키는 행동에 대해서는 아래와 같이 ActionTypes를 지정해 줄 수 있습니다.

```jsx
const INCREASE = "INCREASE";
const DECREASE = "DECREASE";
```

## Actions

그 다음으로 Action Object를 만들어 주는 액션 생성 함수를 만들어 줍니다. 상태의 변화가 발생할 때 마다 액션 객체를 만들어 줘야 하는데 매번 객체를 직접 작성하기 보다는 액션 객체를 생성하는 함수로 관리해 줍니다.

```jsx
const increase = () => ({
  type: INCREASE,
});

const decrease = () => ({
  type: DECREASE,
});
```

## Reducer

Reducer는 어떤 ActionType 인지에 따라서 실제로 값의 변화가 발생되는 함수 입니다.
초기에 존재하는 값을 변화 시키거나 payload를 받아서 그 값을 활용하여 업데이트 할 수도 있습니다.

```jsx
const initialState = {
  count: 0,
};

const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case INCREASE:
      return { count: state.count + 1 };
    case DECREASE:
      return { count: state.count - 1 };
  }
};
```

## Redux Ducks Pattern

Link : [https://github.com/erikras/ducks-modular-redux](https://github.com/erikras/ducks-modular-redux)

actionTypes, actions, reducer이 3가지 요소를 관리하는데 일반적으로 사용되는 구조는 actions , constants , reducers 3개의 폴더를 구성하고 그 안에 추가되는 reducer의 단위별 , 기능별로 하나씩 추가되는 구조를 사용합니다. 하지만 이는 하나의 작업을 할 때 마다 매번 3가지 작업을 동시에 하는 번거로움이 있습니다. 이에 저희는 [Redux Ducks Pattern](https://github.com/erikras/ducks-modular-redux)을 활용해서 폴더를 구성해 보도록 하겠습니다. Redux Ducks Pattern은 actionTypes, actions, reducer가 한 폴더에 있는 구조로 액션이 추가되어도 한 파일 구조 내부에서 모든 작업을 진행할 수 있습니다.

modules 폴더를 생성한 후 하위에 counter.js라는 파일을 생성해 주도록 하겠습니다.

- counter.js

```jsx
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

export const increase = () => ({
  type: INCREASE,
});

export const decrease = () => ({
  type: DECREASE,
});

const initialState = {
  count: 0,
};

const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case INCREASE:
      return { count: state.count + 1 };
    case DECREASE:
      return { count: state.count - 1 };
    default:
      return state;
  }
};

export default countReducer;
```

일반적인 구조와 다른 부분은 actionTypes을 정의할 때 그 앞에 module의 이름을 같이 사용한다는 점 입니다. 이는 만약에 다른 모듈에서 같은 actionType이 있어도 module의 이름을 정의해 줌으로써 actionType의 중복을 막아줄 수 있다는 점에서 유용합니다.

## combineReducers

위와 같이 module 생성이 끝났다면 combineReducers를 생성해 주도록 합니다. 우리는 프로젝트의 규모에 따라서 여러개의 module이 있다는 점을 가정할 수 있습니다. 하지만 redux는 단일 스토어를 지양 하며 redux의 기능 중 combineReducers를 활용하여 여러개의 module을 하나도 합쳐줄 수 있습니다.

```jsx
import { combineReducers } from "redux";
import counter from "./counter";

const rootReducer = combineReducers({
  counter,
});

export default rootReducer;
```

## 스토어 생성하기

위에서 만들어준 rootReducer를 사용해서 index.js 파일에 store를 만들어 주도록 하겠습니다.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { createStore } from "redux";
import rootReducer from "./modules/index";

const store = createStore(rootReducer);

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

## Provider 사용하기

Redux의 Provider를 사용해서 하위 컴포넌트에 store를전달해 주도록 하겠습니다.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { createStore } from "redux";
import { Provider } from "react-redux";
import rootReducer from "./modules/index";

const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

## Component 만들기

components 라는 폴더를 만들고 하위에 counter.jsx 라는 파일을 만들어서 본격적으로 Redux를 사용해 보도록 하겠습니다 !
처음으로 react-redux의 useDispatch와 useSelector를 사용해서 Redux를 컴포넌트 내부에서 사용할 수 있는 환경을 만들어 주도록 하겠습니다. 이 두가지는 Redux에서 제공하는 일종의 hook의 개념이라고 이해할 수 있습니다.

## useDispatch & useSelector

**useDispatch** : 이 hook은 Redux store의 디스패치 함수에 대한 참조를 반환합니다. 우리가 만들어둔 액션 함수를 전달하면 그에 따라서 Redux에 값에 변화가 일어나게 됩니다.

**useSelector** : Redux에 있는 값들을 읽어올 수 있습니다. 우리 프로젝트의 경우에는 count의 값을 읽어올 것 입니다.

useDispatch는 우리가 만든 액션 함수와 함께 Redux의 값을 변화시킬 때 사용한다고 이해하면 useSelector는 값을 변화시키는 기능은 없지만 스토어의 값을 받아와서 화면을 구성할 때 또는 컴포넌트의 내부 로직에서 활용할 수 있습니다.

```jsx
import React from "react";
import { useSelector, useDispatch } from "react-redux";

const Counter = () => {
  const dispatch = useDispatch();
  const { count } = useSelector((states) => states.counter);

  return (
    <div>
      <div>{count}</div>
    </div>
  );
};

export default Counter;
```

## 함수 적용하기

이제 실제로 우리가 만든 환경이 잘 작동하는지 확인해 보도록 하겠습니다. 액션 함수들을 import하고 각각 onClick 함수들에 dispatch할 때 첫번째 인자로 액션 함수를 전달해 줍니다. 이 후 이벤트를 등록시켜 주면 count의 값이 잘 변화하는 것을 볼 수 있습니다.

```jsx
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { increase, decrease } from "../modules/counter";

const Counter = () => {
  const dispatch = useDispatch();
  const { count } = useSelector((states) => states.counter);

  const onIncrease = () => dispatch(increase());
  const onDecrease = () => dispatch(decrease());

  return (
    <div>
      <div>{count}</div>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

export default Counter;
```

## 후기

Redux는 상태 관리에 전부터 널리 사용되던 도구 입니다. 상태 관리 뿐만 아니라 redux-saga , thunk를 사용해서 비동기 통신에 대한 middleware 처리 역시 가능하죠 하지만 라이브러리가 무겁다는 이야기가 있습니다. 사실 react는 자체적으로 useContext와 useReducer를 통해서 Redux와 흡사한 상태 관리에 대한 방법을 제시하고 있습니다. 충분히 기능이 개발 되어서 두가지 모두 사용해 본 후 장단점을 느껴보는 것이 좋다고 생각합니다.
