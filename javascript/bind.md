# bind

- js에서 this는 호출하는 상황에 따라서 동적으로 결정됩니다.
- 그렇다면 함수를 호출하는 방법에 따라서 동적으로 결정되는 this의 값을 정적으로 고정시켜줄 수 있는 방법은 무엇이 있을까요 ?
- 여러 방법이 있겠지만 일단 첫번째 방법은 bind를 사용하는 것 입니다.

아래 예제를 확인해 봅시다.

```jsx
const personData = {
  name: "jangho",
  age: 12,
  mySelf: function () {
    console.log(this);
  },
};
personData.mySelf();
// 해당 this는 this를 부른 주체가 personData이기 때문에  personData가 출력된다.

let personMyself = personData.mySelf;
personMyself();
// 위에 personMyself는 전역에 속해있으니 this는 당연히 window가 됩니다.

let personMyself2 = personMyself.bind(personData);
personMyself2();
// 그렇다면 바로 위의 코드는 bind를 모를때엔 this의 값이 window라고 예상할 수 있는데요.
// 우리는 동적인 this를 위와 같은 선언으로 할당시켜줄 수 있습니다.
// personMyself2는 전역에 있는 변수임에도 불구하고 bind를 사용했기 때문에 고정된 this값이 할당된 것 입니다.
```
