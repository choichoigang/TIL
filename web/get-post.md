# GET & POST

참고 링크 : [https://www.w3schools.com/tags/ref_httpmethods.asp](https://www.w3schools.com/tags/ref_httpmethods.asp)

HTTP의 여러 Method들 중 가장 자주 사용되는 두가지는 아마 GET과 POST 일 것 입니다.
이 두가지는 어떤 차이점이 있고 어떻게 활용되는지 확인해 보도록 하겠습니다.

## GET

GET은 데이터를 요청하는 가장 기본적인 메서드 입니다. 해당 메서드는 데이터를 받아오는 역할만을 수행합니다.
QueryString을 활용해서 데이터를 요청하는 특징이 있으며 이런 특징은 보안에 아주 취약합니다. 그러므로 GET 요청을 할 때 QueryString에 민감하거나 노출이 되면 안되는 값을 노출시키는 행동을 조심해야 합니다.

```jsx
// 유저 로그인에 대한 중요한 정보를 QueryString에 활용하는 건 좋지 못한 사례 입니다.

/main?id=aws&pw=zzz1234

// 아래와 같이 민감하지 않은 정보를 활용하는 건 이상적인 사례라고 할 수 있습니다.

/category?categoryId=3&page=3
```

또한 GET은 아래와 같은 특징을 가집니다.

1. GET 요청은 캐싱이 가능하다.
2. GET 요청은 브라우저의 기록에 남아 있습니다.
3. GET 요청을 북마크 할 수 있습니다.
4. 중요한 데이터 요청에 대해서는 GET 요청의 사용을 지양합니다.
5. QueryString으로 정보를 기재하며 길이의 제한이 있습니다.
6. GET 요청은 데이터를 요청하는데만 사용이 가능합니다.

## POST

POST는 생성 / 업데이트하기 위해 서버로 데이터를 보내는 데 사용됩니다. 말그대로 서버에 내가 요청한 데이터를 추가하는 작업을 원할 때 사용하는 메서드 입니다. 서버로 보내는 데이터는 GET 방식과는 다르게 body에 담아서 보내서 보안적인 측면에서는 GET보다 높다고 할 수 있습니다.

일반적으로 자바스크립트에서 post를 요청할 때 사용하는 코드 입니다. header에 해당 데이터의 타입을 명시해 줘야 합니다. 또한 우리가 보낼 데이터를 body에 포함시킬 수 있습니다.

```jsx
const postData = (url, data) => {
  return fetch(url, {
    method: "POST",
    mode: "cors",
    cache: "no-cache",
    credentials: "same-origin",
    headers: {
      "Content-Type": "application/json",
    },
    redirect: "follow",
    referrer: "no-referrer",
    body: JSON.stringify(data),
  }).then((data) => data.json());
};
```

또한 POST는 아래와 같은 특징을 가집니다.

1. POST 요청은 캐싱이 되지 않습니다.
2. POST 요청은 브라우저에 기록이 남지 않습니다.
3. POST 요청은 북마크를 할 수 없습니다.
4. POST 요청은 데이터 길이에 제한이 없습니다.

## 차이점은 ?

일단 가장 기본적으로 두가지 메서드는 서로 쓰임이 다릅니다. GET 요청의 경우에는 사용자에게 필요한 화면에 대한 데이터를 받아오는 역할이라고 한다면 POST는 웹상에서 사용자의 어떤 행동을 Server에 반영하는 일을 한다고 보면 좋을거 같습니다. GET은 QueryString을 활용하는 만큼 사용자가 즐겨찾기를 할 수도 있고 기록을 캐싱할 수도 있는 반면 POST는 전달하는 data가 노출되지 않는 특징이 있습니다. 그래서 캐싱 또는 즐겨찾기 역시 불가하죠 각 메서드는 가장 많이 사용되는 두가지인 만큼 이해가 필요하다고 생각합니다.
