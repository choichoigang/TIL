# This

- JS에서 함수의 this 키워드는 다른 언어와 조금 다르게 동작합니다.
- 대부분의 경우, this의 값은 **함수를 호출하는 방법에 의해 결정됩니다.**
- strict mode 와 non-strict mode. 사이에서도 일부 차이가 있습니다. js는 선언할 때 결정되는 것이 있고 호출할 때 결정되는 것이 있는데 , this는 호출할 때 결정되는 것 입니다.

---

---

```jsx
"use strict";
let someone = {
  name: "choijangho",
  whoAmI: function () {
    console.log(this);
  },
};

someone.whoAmI();
// someone을 통해서 whoAmI()에 직접 접근했다.
// 즉 이 값은 someone 객체 자체가 return 된다.

let myWhoAmI = someone.whoAmI;

myWhoAmI();
// 해당 변수에 담은 함수의 위치는 전역 즉 window
// 그러니 console.log에 찍히는 값 역시 window 객체를 바라보고 있다.

let btn = document.getElementById("btn");
btn.addEventListener("click", function () {
  console.log();
});

btn.addEventListener("click", function () {
  console.log(this);
});

btn.addEventListener("click", () => {
  console.log(this);
});

// html에 button이 있고 해당 DOM을 불러와서 이벤트를 발생시킬 때
// someone.whoAmI을 실행시킨다고 생각해 봅시다.
// 그렇다면 이번에 this는 누구를 바라보고 출력할까?
// 바로 button Element 자체를 this로 인식하고 출력하게 됩니다.
// 이번에 this를 호출한 주체는 바로 button이기 때문입니다.
// 메서드로 사용하는 함수와 일반 함수 선언식의 차이를 잘 알고 사용해야 this의 값이 어디로 bind되는지 알 수 있다
```

### 내부 함수에서의 this

```jsx
var numbers = {
  A: 5,
  B: 10,
  sum: function () {
    console.log(this === numbers); // true
    const plus = () => {
      console.log(this); // false
    };
    return plus();
  },
};
numbers.sum();
```

- 위와 같이 sum()을 호출할 경우에 numbers의 메서드 이므로 this값을 numbers로 이해할 수 있습니다.
- 즉 sum()을 실행 한다는 것은 numbers에 바인드된 메서드를 실행하는 것과 같습니다.
- 하지만 plus()같은 경우는 메서드가 아니라 함수입니다. 그리고 기본적인 함수 선언의 특징은 this값이 window로 할당 된다는 것입니다.
- 그러니 위에 예제 같은 경우에는 plus 함수 내부에서 numbers의 요소들을 사용할 수 없게 되는것입니다. window 전역에서 number 블록 단위로 접근에 불가능하기 때문이죠.
- 위와 같은 문제를 해결하기 위한 방법으로는 여러가지가 있지만 Arrow Function을 활용하는 방법이 있습니다.

```jsx
// arrow function

// Arrow function의 특징은 자신의 상위 스코프의 this값을 arrow funciton 내부의 this값으로 동일하게 가지게 된다는 특징이 있습니다.
// 함수가 선언되면 this가 window로 정해지는 표현식과는 다르죠 , 그로 인해 numbers를 this로 가지게 되면서 문제를 해결할 수 있습니다.

var numbers = {
  A: 5,
  B: 10,
  sum: function () {
    console.log(this === numbers); // true
    const plus = () => {
      console.log(this === numbers); // true
      return this.A + this.B;
    };
    return plus();
  },
};
numbers.sum();

// call

var numbers = {
  A: 5,
  B: 10,
  sum: function () {
    console.log(this === numbers); // true
    function plus() {
      console.log(this === numbers); // false
      return this.A + this.B;
    }
    return plus.call(this);
  },
};
numbers.sum();
```
