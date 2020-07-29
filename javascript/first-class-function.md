## 일급 함수

- 함수를 다른 변수와 동일하게 다루는 언어는 일급 함수를 가졌다고 말한다.
- 다른 객체 처럼 함수를 변수에 할당이 가능하고 함수에 인자로 함수를 전달하고 함수를 Return하는 함수를 만들수 도 있다.

## 변수에 함수를 할당

```jsx
const func = function () {
  console.log("hello world");
};
```

## 함수를 인자로 전달

```jsx
function Greetings(name) {
  console.log(`Welcome, ${name}`);
}

function main(func, name) {
  return func(name);
}

main(Greetings, "jinu");
```

## 함수를 반환하는 함수

```jsx
function func() {
  return function func2() {
    console.log("hello wolrd");
  };
}

func()();
```
