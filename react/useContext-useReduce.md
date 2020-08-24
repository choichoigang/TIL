# useContext & useReduce

### useContext의 특징

- Props Drilling 패턴을 피할 수 있다.
- Depth가 깊은 Component 에서도 상위의 상태를 쉽게 이용할 수 있다. (위와 같은 말 ...)

### useContext 사용 방식

- useContext를 이용해서 children들에게 전해주는 props의 값을 useReducer의 state와 dispatch로 전달한다.

### 기본적인 사용법과 적용

아래의 로직과 같이 createContext로 Component를 생성한 후 Provider를 사용하면 children Component들은 모두 useContext로 전달 받은 value를 손쉽게 사용 가능하다.

정말 해당 Component만 사용하는 상태라면 useState를 사용하고 그 외에 여러곳에서 쓰일 여지가 있는 상태는 Context에 전달해서 관리해 보자 !

```jsx
import { cerateContext, useState } from "react";

const contextData = {
  state: { count: 0 },
  action: {
    setCount: () => {},
  },
};

// createContext에서 기본적으로 가지고 있을 상태의 형식을 정의한다.
// 기본적으로 값을 가져다 사용할 수 있는 state와 값을 변환할 수 있는 action을 분류한다.
// 이 부분은 useReducer를 이용하면서 state -> useReducer.state // action -> dispatch로 대체 된다.

const TestContext = cerateContext(contextData);

const testProvider = ({ children }) => {
  const [count, setCount] = useState(0);
  // 해당 값을 useState로 정의해 주고 하위의 Component들이 사용할 수 있도록 value로 전달해 준다.
  // 이때 state와 action으로 분류해서 사용한다.
  const value = {
    state: { count },
    action: { setCount },
  };

  return <TestContext.Provider value={value}>{children}</TestContext.Provider>;
};
```

### useReducer의 특징

- useReducer를 사용하면 상태 관리에 관한 로직이 Component에서 분리할 수 있는 장점이 있다.
- type이 무엇인지에 따라서 다양하게 값을 처리할 수 있다.
- State과 같이 원본의 불변을 유지해야 한다.

### useReducer 프로젝트 적용 방식

- useState를 이용하지 않고 useReducer를 이용하여 하나의 상태를 관리한다.
- Context.Provider의 value로서 useReducer의 state와 dispatch를 전달해서 사용한다.

### 기본적인 사용법과 프로젝트 적용사항

useReduer는 값을 더 많은 상황에서 다양하게 관리하는데 용의한 hook 입니다.

하지만 useReducer도 a에서 선언됐지만 e에서 값을 변환해야 하는 상황이 온다면 a→b→c→d→e 까지 전달해야만 사용할 수 있다는 단점이 있습니다.

이런 부분을 Context의 value로 전달함으로써 하위 Component들은 useContext로 간편하게 useReduer를 사용할 수 있습니다.

```jsx
import { cerateContext, useReducer } from "react";

const data = {
  count: 0,
};

const testReducer = (state, action) => {
  switch (action.type) {
    case "INCREASE":
      return { count: state.count + 1 };

    case "DECREASE":
      return { count: state.count - 1 };
  }
};

//action의 type에 따라서 state을 변화시킬 로직을 담는다.

const TestContext = cerateContext();

// Context는 기본값을 가지지 않는다.

const testProvider = ({ children }) => {
  const [state, dispatch] = useReducer(testReducer, data);

  // state는 위 예제의 useState의 state와 같으며 dispatch가 하는 역할은 useState에 setState와 같다고 할 수 있다.
  // 하지만 useReducer를 이용해서 더 많은 State와 setState를 한곳에서 쉽게 관리 가능하다.

  const value = {
    state: { state },
    action: { dispatch },
  };

  // value의 형식은 아까와 같지만 내부에 값은 다르다. state는 useReducer의 state , action은 dispatch로 Context의 value로 전달한다.
  // Context의 value로 useReducer의 값을 전달함으로써 children Component들이 useReducer의 기능을 모두 사용할 수 있도록 한다.

  return <TestContext.Provider value={value}>{children}</TestContext.Provider>;
};
```

### action 폴더의 분류 및 구성

- useReducer에 적용되는 switch에 action.type들을 String으로 관리하는 것이 아닌 변수에 담긴 String으로 관리한다.

```jsx
// 기존 useReducer 첫번째 인자에 들어가는 상태관리 함수의 사용법
const baseballReducer = (state, action) => {
  switch (action.type) {
    case "FETCH_TEAM_LIST":
      return { ...state, teamList: action.data.teamlist };
    case "SELECT_TEAM":
      return {
        ...state,
        selectedTeam: { name: action.name, image: action.image },
      };
    default:
      return state;
  }
};

// 각각의 action type들을 action 폴더에 변수로써 관리
const FETCH_TEAM_LIST = "FETCH_TEAM_LIST";
const SELECT_TEAM = "SELECT_TEAM";

const baseballReducer = (state, action) => {
  switch (action.type) {
    case FETCH_TEAM_LIST:
      return { ...state, teamList: action.data.teamlist };
    case SELECT_TEAM:
      return {
        ...state,
        selectedTeam: { name: action.name, image: action.image },
      };
    default:
      return state;
  }
};
```

- dispatch를 사용해서 값에 변화를 줄 때 기본적인 dispatch 함수에 첫번째 인자로 Object를 넣는것이 아닌 Object를 return하는 함수로 관리한다.

```jsx
//기본적인 사용법
dispatch({ type: "INCREASE", value: 5 });
//함수로 분류해서 사용
const increaseCount = (value) => {
  return { type: "INCREASE", value: value };
};
dispatch(increaseCount(e.target.value));
```
