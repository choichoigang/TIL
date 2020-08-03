# Bubbling & Capturing

![image](https://user-images.githubusercontent.com/49897409/89212062-ca8e7f80-d5fd-11ea-9d65-a344a0161315.png)

위 사진은 W3S에서 표준 DOM 이벤트로 정의한 3가지 흐름입니다.

캡처링 단계 – 이벤트가 하위 요소로 전파되는 단계
타깃 단계 – 이벤트가 실제 타깃 요소에 전달되는 단계
버블링 단계 – 이벤트가 상위 요소로 전파되는 단계

## Bubbling

HTML DOM의 Event 발생 방법의 기본은 Bubbling 입니다.

HTML의 요소에서 이벤트가 발생하면 해당 요소에 대한 이벤트가 가장 우선적으로 발생하고 이 후 가장 최상단의 요소에 접근할 때 까지 부모 요소의 이벤트가 발생합니다.

- 예제 동작 방식

아래의 예제는 기본적인 이벤트의 동작 방식이 Bubbling인 것을 가정하면 Button에 대한 Event가 우선적으로 호출됩니다.

이 후 부모 요소인 wrapper의 이벤트가 최종적으로 호출됩니다.

```jsx
<body>
    <div class="wrapper">
      <button class="item_1">1</button>
    </div>
  </body>
  <script>
    const wrapper = document.querySelector(".wrapper");
    const button_1 = document.querySelector(".item_1");

    wrapper.addEventListener("click", (e) => {
      alert("Click Wrapper");
    });

    button_1.addEventListener("click", (e) => {
      alert(e.target.className);
    });
  </script>
```

아래 코드와 같이 e.stopPropagation()를 사용해서 Bubbling을 막을 수는 있지만 대부분의 경우에는 Bubbling에 대한 동작을 막는 행위를 지양합니다.

```jsx
button_1.addEventListener("click", (e) => {
  e.stopPropagation();
  alert(e.target.className);
});
```

위와 같은 방법으로 Bubbling을 막기 보다는 아래와 같이 부모 요소에서 이벤트의 지점을 전달 받아서 문제를 해결하는 편이 더 좋습니다.

```jsx
<body>
    <div class="wrapper">
      <button class="item_1">1</button>
    </div>
  </body>
  <script>
    const wrapper = document.querySelector(".wrapper");

    wrapper.addEventListener("click", (e) => {
      alert(e.target.className);
    });
  </script>
```

만약에 e.stopPropagation()를 사용하는 경우에 문제가 생길 수 있는 경우에 대해서 생각해 보도록 하겠습니다.

위 코드에서 사용자가 선택한 버튼에 대한 기록을 Caching하는 이벤트를 부모 요소에 구현해 놨다고 생각해 본다면 e.stopPropagation()는 해당 이벤트 이외에 부모 요소의 이벤트를 실행하지 않으니 우리가 원하는 정상적인 작동이 발생하지 않을 수 있습니다.

## Capturing

Capturing의 작동 방식은 간단히 말해서 Bubbling의 반대라고 생각하면 됩니다.

아래 예제는 Bubbling의 관점에서는 button의 이벤트가 발생한 후 그 부모 요소인 wrapper의 이벤트가 발생했지만 Capturing은 wrapper의 이벤트가 우선적으로 발생한 후 button의 이벤트가 발생합니다.

```jsx
<body>
    <div class="wrapper">
      <button class="item_1">1</button>
    </div>
  </body>
  <script>
    const wrapper = document.querySelector(".wrapper");
    const button_1 = document.querySelector(".item_1");

    wrapper.addEventListener("click", (e) => {
      alert("Click Wrapper");
    });

    button_1.addEventListener("click", (e) => {
      alert(e.target.className);
    });
  </script>
```

Capturing의 작동 방식을 사용하기 위해서는 addEventListener의 3번째 인자에 Boolean값으로 true로 설정해야 합니다.

```jsx
wrapper.addEventListener(
  "click",
  (e) => {
    alert("Click Wrapper");
  },
  true
);
```
