# Prototype

참고 : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)

- 상속 관점에서 JS의 유일한 생성자는 객체 뿐이다.
- 각각 객체는 [[Prototype]]이라는 Private 속성을 가지며 자신의 프로토타입이 되는 다른 객체를 가리킨다.

## Property(속성 접근자)

속성 접근자는 점 또는 괄호 표기법으로 객체의 속성에 접근할 수 있도록 해줍니다.

## Prototype

## 프로토타입 체인 그리고 상속

### Prototype Chain

```jsx
const obj = {
  a: 1,
  b: 2,
};

// Object.create()를 사용해서 새로운 객체를 생성할 수 있으며 새로 생성된 객체의 Prototype은 첫번째 인자로 지정됩니다.
// obj2는 obj1를 인자로 받아서 생성된 객체이며 obj의 프로퍼티에 접근할 수 있습니다.
const obj2 = Object.create(obj);

// obj2의 속성으로 c를 할당해 줬습니다.
obj2.c = 3;

// obj3는 obj를 상속받은 obj2를 인자로 받으며 obj와 obj2의 값 모두에 접근이 가능합니다.
const obj3 = Object.create(obj2);

console.log(obj3.c); // 3
console.log(obj3.a); // 1

console.log(obj.c); // undefined
// obj는 obj2의 prototype이긴 하지만 obj2를 prototype으로써 사용하고 있지는 않으므로 obj2.c에 할당된 c의 값에 접근할 수 없습니다.
```

### Method 상속

- JS에서 메소드 라고 따로 지칭하는 개념은 없다.
- 하지만 JS는 객체의 속성으로 함수를 지정할 수 있고 속성의 값을 사용 가능하다.
- 상속된 함수가 실행될 때 this는 상속된 객체를 바라본다.
- 이때 객체의 속성으로 함수를 지정할 때 Arrow Function은 지양한다.
  (Arrow Function은 항상 상위 스코프의 this를 가리키는 특징이 있으며 일반적인 함수 표현식이 동적으로 결정되는 것과 다르게 Arrow Function은 정적으로 결정되며 Method로 사용할 경우 상위 스코프인 window 객체를 바라보게 된다.)

```jsx
const obj = {
  a: 2,
  b: 3,
  sum: function () {
    return this.a + this.b;
  },
};

console.log(obj.sum()); // 5

const obj2 = Object.create(obj);

obj2.a = 33;

console.log(obj2.sum()); // 36
```

### Class를 사용한 상속

- ES6에서 생긴 Class를 통해서 속성을 할당하고 메서드를 생성할 수 있으며 class 선언으로 사용이 가능합니다.
- 함수식은 호이스팅이 가능하지만 Class는 그렇지 않으므로 호출의 순서에 신경을 쓰셔야 합니다.
-

```jsx
class Calculate {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }

  plus() {
    return this.a + this.b;
  }

  subtract() {
    return this.a - this.b;
  }

  multiply() {
    return this.a * this.b;
  }
}

class Calculate2 extends Calculate {
  constructor(a, b) {
    super(a, b);
  }

  dividing() {
    return this.a / this.b;
  }
}

const calc = new Calculate2(3, 5);
```
