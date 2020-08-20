# Closure

클로저는 함수 안에 선언된 내부 함수가 외부 함수의 환경을 기억하고 접근할 수 있는 것을 말합니다.

이러한 특성으로 인해서 전역의 공간에 변수를 사용해서 전역 공간을 오염시키고 정보가 유출되는 문제점을 막을 수 있으며 데이터를 기억하고 기억한 데이터를 활용할 수 있습니다.

최초 선언된 함수의 Scope와 정보를 기억할 수 있는 특징이 있습니다.

- 지역 변수를 접근할 수 없게 만드는 방법

---

```jsx
// js는 함수 내부에 함수를 선언할수도 있고 함수를 return 하는 함수를 만들 수 있습니다.
// 아래의 예제는 함수를 return하는 함수이며 return 되는 innerFunc는 outterFunc의 환경에 접근이 가능합니다.
// 이런 방식으로 클로저의 특징을 이용한다면 해당 함수에 접근해서 num 변수의 값을 변경할 수 있는 방법이 없습니다.

const outterFunc = () => {
  let num = 500;
  return (innerFunc = () => {
    console.log(num);
  });
};

// outterFunc에 있는 num 변수에 접근이 가능합니다.

const store = outterFunc();
store();

// 만약에 기억하고 있는 데이터를 변경하고 싶다면 아래와 같이 return하는 값으로 권한을 줄 수도 있습니다.

const wallet = () => {
  let money = 3000;

  return {
    get getWallet() {
      return money;
    },
    set setWallet(update) {
      money = update;
    },
  };
};

const myWallet = wallet();

console.log(myWallet.getWallet); // 3000
myWallet.setWallet = 5000;
console.log(myWallet.getWallet); // 5000
```

```jsx
// 함수 안에 함수 즉 클로저는 외부의 상태를 기억하기 때문에 인자로 각각 다른 값을 전달하고 필요에 의해서 사용 역시 가능합니다.
// 이미 외부 함수의 실행은 끝났지만 외부 함수의 지역 변수등은 여전히 참조가 가능합니다.
// 이런식으로 클로저를 사용한 지역 변수의 활용은 외부에서 접근이 불가능 하다는 점에서 유용합니다 .

const getUserName = (name) => {
  const userName = name;

  return (printUser = () => {
    console.log(`hello ${userName}`);
  });
};

const user_1 = getUserName("min");
const user_2 = getUserName("hoi");
const user_3 = getUserName("jun");

user_1();
user_2();
user_3();
```

클로저는 위와 같이 변수들을 Private하게 만들수 있으며 자신이 원하는 값만 return하여 사용자에게 정보를 주거나 특정 부분에만 권한을 줘서 값을 바꿀 수 있는 기능을 제공할 수 있습니다.

## 주의점

내부 함수가 외부 함수의 지역 변수를 기억하고 접근할 수 있다는 말은 곧 그만큼의 메모리가 할당되어 있다고 볼 수 있습니다.

그러므로 불필요한 상황에서 클로저를 사용해서 데이터를 기억하는 일은 성능에 대한 저하를 야기할 수 있습니다.
