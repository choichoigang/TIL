## Promise

Javascript는 single-threaded의 언어로써 한번에 두가지 일을 할 수 없습니다.

또한 특정 코드의 실행을 완료까지 기다리지 않고 다음 코드를 먼저 수행하는 형태 역시 Javascript의 특징이라고 할 수 있습니다.

처음 Javascript를 사용할 때 “1번 Task의 처리를 기다리고 2번 Task를 처리해라”와 같은 일은 우리가 예상하지 않은 방식으로 처리하지 않을 수 있습니다.

이런 현상은 자바스크립트가 비동기 처리를 어떻게 처리하는지 ? 그리고 그 부분을 콜백 함수로 써 어떻게 해결할 수 있는지에 이해가 필요합니다.

콜백 함수로 코드를 동기적으로 작동시키는 방법은 흔히 콜백 지옥이라는 큰 오류가 있고 Promise는 이를 해결해 줄 수 있는 방법이라고 할 수 있습니다.

## States

Promise는 기본적으로 3가지의 상태를 가지며 이 3가지 상태는 Promise가 코드를 어떻게 처리하는지의 일련의 과정이라고 할 수 있습니다.

- Pending : Promise를 수행하기 전의 상태 입니다.
- Fulfilled : Promise가 성공적으로 실행된 상태라고 할 수 있습니다.
- Rejected : Promise가 어떤 이유로 실행되지 못했고 비동기 통신의 경우에 reject를 통해서 Error를 return 합니다.

## 작동 방식

Promise는 Callback 함수의 첫번째 인자 resolve , 두번쨰 인자 reject를 받습니다.

resolve는 정상적인 동작의 결과로써 실행된다고 생각하면 reject는 문제가 발생했을 때 발생하는 코드입니다.

### Resolve

정상적인 실행이 되는지 간단하게 확인해 볼 수 있는 방법은 reject( )를 호출하는 것 입니다.

인자로 전달할 값을 전달하면 then을 사용해서 첫번째 인자로 그 값을 받을 수 있습니다.

아래의 예제는 reject되는 상황이 없으니 Promise를 실행하고 정상적으로 then이 실행되고 그 결과로 console.log('Code Resolve');를 확인할 수 있을 것 입니다.

```jsx
//
const getData = () => {
  return new Promise((resolve, reject) => {
    resolve("Code Resolve");
    //reject("Error Fire !");
  });
};

getData()
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(error);
  });

// Code Resolve
```

### Reject

코드가 정상적으로 실행되지 않을 경우는 Reject( )의 실행으로 확인할 수 있습니다.

Promise는 코드가 정상적으로 실행되지 않았다면 catch로 그 오류 상황을 잡아낼 수 있습니다.

아래 코드에서는 의도적으로 reject("Error Fire !");를 전달했으며 then은 실행되지 않고 catch에서 해당 오류를 잡아낼 것 입니다.

```jsx
const getData = () => {
  return new Promise((resolve, reject) => {
    reject("Error Fire !");
    //resolve("Code Resolve");
  });
};

getData()
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(error);
  });

// Error Fire !
```

## Promise를 사용해서 동기적으로 작동하게 하는 법

Promise를 사용해서 then을 통해서 코드를 실행하고 난 후 then은 다시 Promise를 return 하며 원하는 값을 return하면 다음 then의 실행시에 [[PromiseValue]]의 값으로 들어가게 되고 then의 콜백 함수의 첫번째 인자로 받을 수 있습니다.

그렇게 return된 Promise를 사용해서 우리는 다음으로 실행할 동기적인 코드를 입력 가능합니다.

아래의 코드는 우리가 의도한 대로 Task가 1,2,3 정상적으로 실행되고 최종적으로 3이 더해진 결과를 보여줍니다.

```jsx
let count = 0;

new Promise((resolve, reject) => {
  resolve(count);
})
  .then((result) => {
    console.log("Task 1");
    result += 1;

    return result;
  })
  .then((result) => {
    console.log("Task 2");
    result += 1;

    return result;
  })
  .then((result) => {
    console.log("Task 3");
    result += 1;

    return result;
  })
  .then((result) => {
    console.log(result);

    return;
  });
```
