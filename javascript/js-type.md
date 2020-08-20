## Primitive && Reference type

Js의 타입은 크게 기본형(Primitive Type)과 참조형(Reference Type)으로 나눠지며, 세부적으로는 아래와 같은 Type을 가진다.

- 기본형(Primitive Type)

1. Number
2. String
3. Boolean
4. null
5. undefiend
6. Symbol

- 참조형(Reference Type)

1. Object (Array , Function , RegExp )

- 기본형의 경우에는 값을 그대로 할당되며 참조형은 값이 저장된 주소들을 할당한다.
- JS에서 Array , Function , RegExp는 Object에 포함되며 각각 배열 , 함수 그 자체가 아니라 함수 객체 배열 객체로 표현되며 typeof를 사용해서 확인해 본다면 결과는 object라는 결과가 나온다.
- 참조형을 다른 데이터에 할당할 때는 복사가 아닌 원본 데이터를 참조한 상태가 할당되며 이런 특징을 유의하면서 사용해야 한다.

```jsx
// 아래의 코드를 실행해 본다면 a의 값을 변경했지만 a를 할당받은 b의 값 역시 변경된걸 볼 수 있다.
const a = [1, 2, 3, 4];

const b = a;

a[0] = 44;

console.log(b);
// [44,2,3,4];
```
