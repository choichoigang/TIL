# Functional Programming

- 프로그래밍 패러다임의 일종이다.
- 함수가 가장 중심이 되는 사고 방식으로 프로그래밍이 진행된다.

## Functional Programming 사고 방식

1. 부작용이 없는 순수 함수를 구현한다.
2. 순수 함수는 동일한 입력에 항상 같은 출력이 나와야 한다는 확신이 있어야 한다.
3. JS의 고차 함수 개념은 함수형 프로그래밍에서 중요하게 사용된다.
4. 일반적인 반복 보다는 고차 함수로 반복을 해결하는 것이 이상적이다. ( map , filter , reducer ... )
5. 상태를 immutable하게 관리한다.

## 순수 함수

순수 함수는 전역이나 그 어떤 상태에도 의존하지 않는다.
동일한 입력에는 동일한 출력이 나오는 함수를 만드는 것이 순수 함수를 만드는데 가장 중요한 점이다.
만약에 함수가 외부의 어떤 상태에 의존하게 된다면 그리고 그 의존하고 있는 상태가 변동될 여지가 있다고 한다면 그 함수는 동일한 입력에 항상 동일한 출력을 return 한다는 확신을 할 수 없는 함수라고 할 수 있으며 그런 함수는 순수한 상태를 가지는 함수라고 할 수 없다.

- 순수하지 않은 함수의 예시

```jsx
// 아래 printWelcomeMessage 함수는 전역 변수인 name에 의존하고 있습니다.
// 이런 형태의 함수는 추후에 전역의 상태에 변화가 생긴다면 우리가 의도하지 않은 결과가 나올 가능성이 있습니다.
// 이런 형태의 함수를 순수하지 않은 함수라고 말합니다.

const name = "jona";

const printWelcomeMessage = () => {
  return "Welcome " + name;
};

console.log(printWelcomeMessage());
```

- 순수 함수의 예시

```jsx
// 위와 같은 동작을 하지만 아래 함수는 더이상 전역의 name을 참조하지 않고 input으로 받은 값을 output에 활용합니다.

const printWelcomeMessage = (name) => {
  return "Welcome " + name;
};

console.log(printWelcomeMessage("jona"));
```

## 고차 함수(Higher Order Functions)

- 함수를 인자로 받거나 함수를 Return하는 함수를 만들 수 있다.
- 자바스크립트의 함수는 일급 객체이므로 인자로 전달도 가능하고 함수를 반환하는 일도 가능하다.

- 함수를 반환하는 함수

```jsx
const enterUser = () => {
  return () => {
    return `User has connected.`;
  };
};

enterUser()();
```

- 함수를 인자로 받아서 함수를 반환하는 함수

```jsx
const getAchievement = (func) => {
  return (achievement) => {
    return func(achievement);
  };
};

const printLog = (achievement) => {
  return console.log(`You have achieved the achievement. ${achievement}`);
};

const user = getAchievement(printLog);

user("Kill the boss");
user("Gather herbs");
```

## Array Method

배열의 다양한 메서드는 각 요소에 전달한 Callback 함수를 적용시켜서 다양하게 활용할 수 있는 여러 메서드를 제공합니다.
함수형 프로그래밍에서 일반적인 반복 보다는 고차 함수를 활용한 반복을 지양하며 Array Method를 활용해서 그 부분을 충족시킬 수 있습니다.

**ForEach**

가장 기본적인 반복을 피할 수 있는 방법은 for문 대신에 forEach를 사용하는 것 이며 아래와 같은 특징을 가집니다.

- 배열을 순회하며 배열의 각 요소를 callback의 첫번째 인자로 전달하여 콜백 함수를 실행합니다.
- 반환값은 undefined 입니다.
- 원본 배열을 변경하지 않지만 콜백 함수는 원본 배열을 변경할 수 있습니다.
- 조건에 따라서 반복을 멈출 수 없으며 배열의 마지막까지 순회합니다.
- 만약 조건에 따라서 반복을 멈추는 동작을 원한다면 every , some , find , findIndex 와 같은 다른 메서드를 활용할 수 있습니다.

```jsx
const arr = [1, 2, 3, 4, 5];
const squaredArr = [];

arr.forEach((el) => {
  squaredArr.push(el ** 2);
});

console.log(squaredArr);
```

원본 배열을 수정하는 법

```jsx
const arr = [1, 2, 3, 4, 5];

arr.forEach((_, idx) => {
  arr[idx] += 1;
});

console.log(arr);
```

**map**

- 배열의 각 요소를 순회하며 Callback의 결과로 새로운 배열을 생성해서 반환한다.
- 원본 배열은 변경되지 않는다.

```jsx
const arr = [1, 2, 3, 4, 5];

const squaredArr = arr.map((el) => el ** 2);

console.log(squaredArr);
```

**filter**

- 반복문 내부에 if 조건식을 사용할 경우에는 filter를 사용해서 대체가 가능하다.
- 배열의 요소를 순회하며 Callback 함수의 결과가 true인 경우만 return되어 새로운 배열을 생성한다.
- 원본 배열은 변경되지 않는다.

```jsx
const arr = [1, 2, 3, 4, 5];

const filterArr = arr.filter((el) => el > 1);

console.log(filterArr);
```

**reduce**

배열의 메서드중 가장 이해가 어려운 메서드가 아닐까 생각합니다.
배열의 각 요소에 대하여 이전 콜백의 결과를 전달하여 콜백 함수를 실행하고 그 결과를 반환하며 reduce는 함수형 프로그래밍의 pipe 기법에서도 사용되는 유용한 고차 함수 입니다.

callback의 최초 호출에서 첫번째 인수 accumulator의 값을 initialValue로 제공할 수 있으며 만약에 값을 제공하지 않았다면 배열의 0번째 값이 첫번째 인자로 들어가게 됩니다.

구문

```jsx
arr.reduce(callback[, initialValue])
```

아래의 실행 결과는 reduce의 두번째 인자 즉 initialValue를 제공하지 않았고 accumulator는 arr[0] = 0의 값으로 시작됩니다.
reduce의 callback에서 실행되는 순서를 확인해 보자면 아래와 같습니다.

1. 1 + 2 = 3
2. 3 + 3 = 6
3. 6 + 4 = 10
4. 10 + 5 = 15

```jsx
const arr = [1, 2, 3, 4, 5];

const reduceResult = arr.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
});

console.log(reduceResult);
// 15
```

이번에는 위와 같은 로직으로 initialValue를 제공해 보도록 하겠습니다.
이번에는 initialValue이 제공됐고 첫번째 callback 실행시에 accumulator는 10입니다.

1. 10 + 1 = 11
2. 11 + 2 = 13
3. 13 + 3 = 16
4. 16 + 4 = 20
5. 20 + 5 = 25

```jsx
const arr = [1, 2, 3, 4, 5];

const reduceResult = arr.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 10);

console.log(reduceResult);
// 25
```
