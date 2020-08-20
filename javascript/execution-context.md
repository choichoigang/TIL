# 실행 컨텍스트(Execution Context)

실행 컨텍스트는 쉽게 설명하자면 코드가 실행될 떄 문맥 정도로 해석할 수 있습니다.
컨텍스트는 크게 전역 컨텍스트와 함수 컨텍스트로 나눌 수 있으며 실행의 순서는 아래와 같습니다.

1. 브라우저가 JS 코드를 읽을 떄 모든것을 포함하는 전역의 컨텍스트가 생성됩니다.
2. 컨텍스트는 arguments, variable , scope chain, this를 생성하며 해당 컨텍스트의 문맥을 설명할 수 있으며 실행해 활용하는 재료들을 생성한다고 이해하시면 좋습니다.(전역은 arguments를 생성하지는 않습니다.)
3. 전역 컨텍스트가 생성된 후 함수가 실행될 떄 마다 함수 컨텍스트가 생성되며 함수 컨텍스트 역시 2번의 과정을 거칩니다.

아래의 예제에서 순서대로 컨텍스트와 함수가 어떻게 실행되는지 확인해 보도록 하겠습니다.

1. 브라우저가 스크립트를 해석하면서 전역 컨텍스트가 생성됩니다.
2. first_context 함수의 호출을 만나고 함수 컨텍스트를 생성한 후 실행합니다.
3. first_context는 secound_context를 생성하고 실행하며 secound_context의 컨텍스트가 생성되고 함수가 실행됩니다.
4. secound_context는 내부에서 a를 호출하고 있지만 자신의 컨텍스트에는 a의 값이 없으므로 scope chain을 통해 first_context로 가서 해당하는 a를 찾고 logging을 마칩니다.
5. secound_context는 thrid_context를 생성한 후 호출하면서 컨텍스트가 생성되고 함수가 실행됩니다.
6. thrid_context는 b를 호출하지만 역시 자신의 컨텍스트에는 a의 값이 존재하지 않으므로 scope chain을 통해 secound_context에 접근하고 logging을 마칩니다. 이때
   thrid_context는 secound_context , first_context 두개의 컨텍스트에 모두 접근이 가능합니다.

```jsx
const first_context = () => {
  const a = "a";
  const secound_context = () => {
    const b = "b";
    console.log(`secound_context ${a}`);
    const thrid_context = () => {
      console.log(`thrid_context ${b}`);
    };
    thrid_context();
  };
  secound_context();
};

first_context();
```

실행 컨텍스트는 위에서 말했듯이 코드가 실행할 때 사용하는 값등을 탐색하고 활용하기 위한 유기적인 관계를 위한 정보를 가지고 있다고 할 수 있습니다.
