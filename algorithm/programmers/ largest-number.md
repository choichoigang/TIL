# 가장 큰 수 (Programmers)

- 인자로 받은 Array를 map method를 사용해서 String으로 변환한다.
- sort method을 사용해서 b + a / a + b 를 뺀 값중에 우선 순위를 정한다.
- join method를 사용해서 결과를 하나의 String으로 변환한다.
- 만약에 Array의 모든 값이 0일 경우에 "00000" 값으로 return 될 수 있으므로 answer의 첫번째 값을 비교 후 삼항연산자를 사용해서 return 한다.

```jsx
function solution(numbers) {
  const answer = numbers
    .map((el) => el + "")
    .sort((a, b) => {
      console.log(`b + a = ${b + a}`);
      console.log(`a + b = ${a + b}`);

      return b + a - (a + b);
    })
    .join("");

  return answer[0] === "0" ? "0" : answer;
}

solution([6, 10, 2]);
```
