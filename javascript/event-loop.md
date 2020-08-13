# Event Loop

## What are you ?

싱글스레드 , 비동기적 언어

자바스크립트는 코드 실행, 이벤트 수집과 처리, 큐에 놓인 하위 작업들을 담당하는 이벤트 루프에 기반한 동시성(concurrency) 모델을 가지고 있습니다.

싱글스레드인 자바스크립트가 어떻게 비동기적인 코드와 이벤트를 처리하는지를 이해하기 위해서는 이벤트 루프에 대한 이해가 필요합니다.

![image](https://user-images.githubusercontent.com/49897409/90138994-b6dbd980-ddb2-11ea-8042-345945d4bf7e.png)

## Call Stack

함수 호출은 프레인들의 스택을 형성합니다.

```jsx
function multiply(a,b) {
erturn a * b ;
}

function square(n) {
return multiply(n , n) ;
}

function printSquare(n) {
let squared = square(n) ;
console.log(squared)
}

printSquare(4) ;
```

콜 스택은 **데이터 스트럭처**로 실행되는 순서를 기억하고 있으며 인자와 지역 변수를 포함하는 프레임이 생성됩니다.

1. 위의 예제의 경우에 printSquare() 함수가 실행되면서 첫번째 프레임이 생성됩니다.
2. printSquare 함수는 square 함수를 호출하고 있으므로 square 함수 즉 두번째 프레임이 생성됩니다.
3. square 함수는 multiply함수를 호출하고 있으며 multiply가 마지막 새번째 함수를 프레임으로 생성되며 스택의 구조에 쌓이게 됩니다.
4. 스택의 특성은 가장 마지막에 들어간 요소가 가장 먼저 실행된다는 특징이 있으며 위의 함수 실행의 순서는 multiply → square → printSquare 순서로 실행되며 마지막엔 스택이 비워집니다.

```jsx
function foo() {
  throw new Error("Oops!");
}
function bar() {
  foo();
}
function baz() {
  bar();
}
baz();
```

위 코드에서 Error를 보내는 함수는 foo 입니다.

이 코드를 실행하면 foo 함수에만 에러가 있다고 판단하지 않습니다.

callback의 특성으로 인해 브라우저는 baz가 호출한 bar 그리고 bar가 호출한 foo에 Error가 있다고 판단합니다.

## Heap

메모리 힙은 동적으로 만들어진 객체가 할당 됩니다.

## Web Apis

Web Apis는 기본적으로 비동기 처리 함수의 콜백 함수 비동기식 이벤트 핸들러 Timer 함수등을 관리합니다.

addEventListener로 특정 이벤트를 등록한다면 Web Apis에서 이를 관리하고 있다가 호출 시점에서 Callback Queue로 가게 되고 Call Stack이 비워지는 순간에 호출되게 됩니다.

## asynchronous callbacks

비동기 콜백

```jsx
console.log(`hi`);

setTimeout(function () {
  console.log(`there`);
}, 5000);

console.log(`hello world`);
```

위의 코드는 어떻게 실행될까요 ? 아마도 실행 순서는 hi → JSConfEU → there 이 될 것 입니다.

싱글스레드인 JS를 생각하면 분명히 there이 실행되기 전에는 console.log(`hello world`)는 실행이 불가능 합니다.

하지만 실제로 실행되는 코드는 hi → hello world → there 로 진행됩니다.

우리는 이런 현상을 이벤트 루프와 동시성으로 설명할 수 있습니다.

## Concurrency & the Event Loop

JS는 한번에 하나의 일만 할 수 있다는 말은 틀린말이 아닙니다.

하지만 브라우저가 Web API를 우리에게 제공하고 있죠.

Web API는 JS에서 호출할 수 있는 스레드를 효과적으로 지원하고 이로 인해 우리는 비동기 적인 결과를 얻을 수 있습니다 .

```jsx
console.log(`hi`);

setTimeout(function () {
  console.log(`there`);
}, 5000);

console.log(`hello world`);
```

위의 함수의 진행 방식은 아래와 같습니다.

1.일단 console.log(`Hi`)가 먼저 출력된다.

2.이후 setTimeout은 stack에 호출된 후 webapis에 영역으로 넘어간 후 지정한 5초를 카운트 한다.

3.console.log(`JSConfEU`)가 출력된다.

4.stack은 비워지고 5초의 카운트가 지난 setTimeout 은 task queue의 영역에 있다가 event loop를 통해서 stack에 아직 실행할 함수가 남아있음을 알려준다.

5.console.log(`there`)이 실행된다.

여기서 알아야할 부분은 stack은 완전한 JS의 영역이라는 것 입니다.

또한 이벤트 루프는 스택이 비워질때까지 기다린 후에 큐에 있는 콜백을 스택에 쌓게 됩니다.

이로 인하여 스택은 계속해서 실행될 수 있습니다.

→ **복잡한 예제 1**

```jsx
console.log("Started");

$.on("button", "click", function onClick() {
  console.log(`Clicked`);
});

setTimeout(function onTimeout() {
  console.log("Timeout Finished");
}, 5000);

console.log("Done");
```

위의 코드는 어떻게 실행될까요 ?

1.console.log('Started')가 실행됩니다.

2.이후 \$.on은 call stack에 호출된 후 web api의 영역으로 넘어가게 됩니다.

3.setTimeout은 web api의 영역으로 넘어가 5초를 count 하게 됩니다.

4.console.log('Done')이 call stack의 영역에서 호출되고 call stack은 비워집니다.

5.여기서 count가 끝난 setTimeout은 Queue에서 대기를 하다가 call stack이 비워지면 이벤트 루프를 통해서 call stack에 호출됩니다.

6.\$.on은 web api의 영역에서 대기하고 있습니다. 우리가 만약에 클릭 버튼으로 해당 함수를 실행한다면 web api→Queue로 전송 이벤트 루프를 통해서 call stack에 호출되게 되는 것 입니다.

**여기서 Queue의 특성 하나를 집고 넘어가자면 언제나 선입 선출을 기준으로 작동하는 것 입니다.**
