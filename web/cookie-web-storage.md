# Cookie & Web Storage APIs

HTTP 프로토콜은 Connectionless Stateless 라는 특징을 가지고 있다. Response가 끝나면 연결을 끊고 서버와의 통신이 끝나면 상태 정보에 대해서는 유지하지 않는 특징인데 이런 특징을 Session과 Cookie가 보완해 준다.

## Cookie

Link : [https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)

HTTP 쿠키는 서버가 사용자의 웹 브라우저에 전송하는 데이터 입니다. 크롬 등등 개발자 도구를 열어 Application을 확인해 본다면 key와 value의 형태로 값이 저장되어 있는 것을 볼 수 있습니다. 또한 만료 날짜 , 시간 , 경로 정보 역시 포함되어 있죠. 쿠키는 요청이 동일한 브라우저에서 왔는지를 판단할 수 있습니다. 서버에 요청을 보낼 때 쿠키에 있는 값들 역시 서버에 전송되기 때문이죠. 이러한 방법을 사용하면 로그인을 유지하는 것 역시 가능합니다. 상태를 유지하지 않는 Stateless의 특징을 보안하기 위해서 Cookie를 활용해서 요청에 대한 판단을 하는 것 입니다. 쿠기는 사용자가 브라우저를 종료하면 사라지는 Session Cookie 웹 브라우저를 꺼도 사라지지 않는 Permanent Cookie가 있습니다.

MDN에 정리된 글로는 쿠키는 주로 3가지 목적을 위해서 사용된다고 말하고 있습니다.

1. **세션 관리 (Session management)**
   서버에 저장이 필요한 로그인 , 장바구니 , 찜목록 , 게임 스코어 등의 정보 관리
2. **개인화(Personalization)**
   사용자 선호 , 테마 등의 세팅
3. **트래킹(Tracking)**
   사용자의 행동을 기록하고 분석하는 용도

Cookie를 사용한 저장 방식은 과거에 주로 사용되던 방식입니다. 또다른 대안인 modern storage APIs가 나온 이후에는 이를 사용해서 저장하는 걸 권장합니다. 쿠키는 매번 요청할 때 마다 Cookie의 정보가 서버에 같이 전송된다는 단점이 있었습니다. 네트워크 통신이 상당히 느린 동작인 것을 감안하면 이런 특징은 성능이 떨어지는 원인이 될 수 있다고 할 수 있습니다.

## modern storage APIs

새롭게 권고하는 사항으로 modern storage APIs는 클라이언트에 데이터를 저장할 수 있도록 지원하는 기술입니다. Cookie와 비교할 때 기능은 크게 차이가 없지만 쿠키의 저장 공간이 4KB 라는 걸 감안하면 웹 스토리지는 5MB까지 저장 공간을 제공 합니다. Cookie와 다르게 매번 정보를 서버로 전송하지 않습니다.

## LocalStorage

LocalStorage는 저장하는데 만료 시점이 없습니다. 사용자가 브라우저를 종료해도 값들을 유지합니다. 예들 들어서 우리가 검색 엔진을 이용할 때 우리의 최근 검색한 내역을 5개에서 10개정도 보여주는 기능을 하는 걸 볼 수 있을 것 입니다. 이런 검색 기록은 보통 LocalStorage에 값들을 저장하고 사용하곤 합니다.

1. 데이터가 사용자의 로컬 디스크에 저장된다.
2. 저장된 정보는 유효기간 없이 유지된다.

## SessionStorage

SessionStorage의 특징은 페이지 세션이 끝날 때 제거된다는 특징이 있습니다.

1. LocalStorage와 다르게 세션이 종료되는 시점에서 제거된다.

## 언제 사용할까 ?

일단 이 둘은 지금도 활발하게 사용 되는 기술입니다. Web Storage APIs가 Cookie의 문제점을 보안하기 위해서 나왔다고 하지만 Cookie가 가진 특징을 모두 가지고 있는 것은 아닙니다. 이 둘을 선택할 때는 클라이언트가 요청을 보낼 때 항상 필요한 데이터인가 ? 아니라면 클라이언트 쪽에서 크게 활용되는 요소이고 서버는 굳이 정보를 알 필요가 없는가 ? 라는 기준점을 가지고 사용할 수 있겠습니다.
