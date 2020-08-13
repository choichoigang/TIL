# typeof

typeof는 피연산자의 자료형을 문자열로 반환합니다.

## 반환 가능한 값

typeof로 반환이 가능한 값들은 아래와 같습니다.

- Undefined -> 'undefined'
- Null -> 'object'
- Boolean -> 'boolean'
- Number -> 'number'
- BigInt -> 'bigint'
- String -> 'string'
- Symbol -> 'symbol'
- Function -> 'function'
- Object -> 'object'

## 단점

위와 같이 여러 자료형을 표현할 수 있는 장점이 있지만 typeof에는 단점도 역시 존재합니다.
이유는 바로 정확한 자료형을 구별하지 못 할 떄가 있다는 점에서 그렇습니다.

```jsx
// 아래의 경우에 우리는 Array라고 예상할 수 있지만 결과는 object 입니다.
// js에서 배열은 배열 자체가 아닌 배열 객체이며 점 표기법으로 여러 프로토타입을 사용할 수 있기 때문입니다.

console.log(typeof []);
// 'object'

// js에서는 전역 변수에 속하는 NaN라는 변수가 있습니다.
// 이는 Not a Number라는 숫자가 아님을 의미하지만 typeof 에서는 'number' 타입으로 판별합니다.

console.log(typeof NaN);
// 'number'

// 또한 null type 역시 object로 표현하며 이는 타입을 체크하는 사용자의 입장에서 혼란을 줄 수 있습니다.

console.log(typeof null);
```

## 해결 방법

위와 같이 typeof는 큰 단위에서의 타입에 대한 궁금증은 해결해 줄 지 몰라도 객체의 타입까지는 불가능 합니다.
아래와 같은 방법은 이를 해결하는데 도움을 주는 방법 입니다.

toString.call() 사용하기

```jsx
// 해당 메서드를 사용하면 객체의 타입까지 구체적으로 표현이 가능합니다.

toString.call([]);
// "[object Array]"

toString.call(null);
// "[object Null]"
```
