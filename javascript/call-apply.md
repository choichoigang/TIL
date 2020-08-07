## call & apply

참고 : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

- 구문

```jsx
func.apply(thisArg, [argsArray]);
func.call(thisArg[, arg1[, arg2[, ...]]]);
```

첫번째 인자로 실행하는 함수의 this가 될 대상을 전달합니다.

apply : 두번째 인자로 배열을 전달하며 해당 배열의 각각 index의 Value는 함수의 인자로 들어가게 됩니다.

call : 첫번째 인자 이후로 받는 인자 각각은 함수의 인자로 들어가게 됩니다.

두 메서드의 큰 차이점은 첫번째 인자 이후에 받은 인자가 배열인지 인자의 목록인지가 가장 큰 차이점이라고 할 수 있습니다.

- 함수에 this와 인자 전달하기

```jsx
// 아래 user1과 user2는 값을 가지고 있는 객체이며 함수의 this로 전달시킬 객체입니다.
const user1 = {
  nickname: "world",
  level: 21,
  inventory: [],
};

const user2 = {
  nickname: "popo",
  level: 26,
  inventory: [],
};
// 해당 함수는 this를 전달받아 this의 값인 level을 증가시키고 Level Up log를 생성합니다.
const printLevelLog = function () {
  const beforeLevel = this.level;
  this.level++;
  console.log(
    `${this.nickname} has risen from ${beforeLevel} to ${this.level} levels`
  );
};

// 아래의 함수는 user1와 user2 객체가 this의 값으로 설정된 후 실행된 함수입니다.

printLevelLog.apply(user1);
printLevelLog.apply(user2);

printLevelLog.call(user1);
printLevelLog.call(user2);

// 아래 함수는 this를 전달하고 획득한 아이템을 인자로 받아 this의 inventory라는 Array에 추가하는 함수 입니다.

const printUserGetItemLog = function (item) {
  this.inventory.push(item);
  console.log(`${this.nickname} get ${item}`);
};

printUserGetItemLog.apply(user1, ["weed"]);
printUserGetItemLog.apply(user2, ["ax"]);

printUserGetItemLog.call(user1, "weed");
printUserGetItemLog.call(user2, "ax");
```
