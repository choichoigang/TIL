# Scope & Hoisting

## Scope

- 변수는 기본적으로 지역 범위와 전역 범위를 기준으로 나눠 집니다.
- 지역 범위와 전역 범위에 같은 이름의 변수가 있을시 지역 범위의 함수가 우선권을 가집니다.
- 지역 범위 역시 var 또는 let const에 따라서 블록 범위 인지 함수 범위 인지 나눠 집니다.
- var는 함수 범위 스코프를 가지며 let과 const는 블록 범위의 스코프를 가집니다.
- 하위 블록 범위는 상위에 변수들을 모두 접근이 가능하지만 상위 블록이 하위 블록의 변수에 접근은 불가능 합니다.

* 지역과 전역 범위에 같은 이름의 변수가 있을 시

```jsx
// a 변수는 전역 단위와 foo 함수의 지역 단위 모두에 존재 합니다.
// foo 함수는 a의 값을 logging하고 있고 이 때 지역 단위의 a가 우선권을 가지게 되며
// 결과는 33이 나오게 됩니다.
const a = 22;

const foo = () => {
  const a = 33;
  console.log(a);
};

foo();
// 33;
```

- 함수 단위와 블록 단위

```jsx
// 아래의 foo 함수 내부의 if 조건식의 블록 범위 내부에 var는 함수 범위의 스코프를 가지는 특성이 있습니다.
// 이에 조건문 블록 밖이라 해도 foo 함수 내부에서 어디든 사용이 가능합니다.

const foo = () => {
  if (true) {
    var a = 22;
  }
  console.log(a);
};

foo();
// 22

// 아래의 foo 함수는 위와 모든게 동일하지만 var 선언이 let으로 변경됐습니다.
// let은 블록 범위의 스코프를 가지며 if 조건식 내부에서만 사용이 가능하게 되었습니다.

const foo = () => {
  if (true) {
    let a = 22;
  }
  console.log(a);
};

foo();

// Error
```

## Hoisting

함수 선언과 변수 선언을 가능한 Scope 범위까지 끌어올려 유효한 범위의 상단에 선언하는 것을 말합니다.

- var 변수 선언과 함수 선언문만 호이스팅이 발생합니다.
- var 변수의 경우에는 위와 같이 함수 단위의 스코프를 가지기 때문에 함수 내부에서 어디든 접근이 가능합니다.
- let/const는 호이스팅이 발생하지 않습니다.

위의 특징을 통해서 예제를 작성해 보도록 하겠습니다.

- 함수 선언문

```jsx
foo();
// hello world !
// 아래의 식은 함수 선언문의 형식이며 호이스팅이 발생하고 정상적인 함수 작동이 발생한다.

function foo() {
  console.log("hello world !");
}
```

- 함수 표현식

```jsx
foo();
// error
// 아래의 함수는 함수 표현식이며 호이스팅이 발생하지 않아 foo 함수를 찾을 수 없게 되며
// error가 발생하게 된다.

var foo = function () {
  console.log("hello world !");
};
```

- var 변수

```jsx
console.log(a);
// undefined
// var 변수는 호이스트를 허용합니다. 즉 변수 선언 전에 코드에서 참조가 가능합니다.
var a = 2;
```

- 함수 선언문 내부의 함수 선언문

```jsx
// 함수 선언문은 호이스팅이 발생하며 그 내부에 함수를 아래와 같이 호출 가능하다.
function foo() {
  foo2();

  function foo2() {
    console.log("func foo2");
  }
}

foo();
```

- 함수 선언문 내부의 함수 표현식

```jsx
// 아래 예제는 함수 선언문은 호이스팅이 발생한다는 특징이 있어도 표현식은 그렇지 않기 때문에 에러가 발생한다.
function foo() {
  foo2();

  var foo2 = function () {
    console.log("func foo2");
  };
}

foo();

// 이와 같이 호출 순서를 지켜준다면 정상적으로 작동한다.

function foo() {
  var foo2 = function () {
    console.log("func foo2");
  };

  foo2();
}

foo();
```
