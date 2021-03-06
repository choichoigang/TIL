# SSR(Server Side Rendering & Client Side Rendering)

전통적인 SSR(Server Side Rendering) 그리고 모바일의 생태계가 커지면서 등장한 CSR(Client Side Rendering)의 방식을 사용하는 SPA(Single Page Application) 이 둘의 장점과 단점 그리고 무엇이 다른지 알아보도록 하겠습니다.

### SSR

기존의 SSR 방식은 초기 로딩 그리고 사용자가 페이지를 이동할 때 마다 새로운 html을 받아야 하며 페이지를 로딩할 때 마다 서버에서 리소스를 받아서 해석하고 화면에 보여주는 작업을 반복했습니다. 이런 방식의 문제점은 아래와 같은 단점을 가질 수 있습니다.

- 화면을 제공함에 있어서 Sever에서 매번 html을 준비하고 받아야 한다면 Server에 부담이 갑니다.
- 화면을 이동할 때 매번 서버에 리소스를 요청하고 새로 고침이 발생하기 때문에 전 화면의 상태를 유지하기 힘듭니다.
- 화면 이동시 변경되지 않아도 되는 부분까지 다시 렌더링이 발생하기 때문에 이런 부분은 비효율적인 동작입니다.

하지만 SSR을 사용하는 이유와 장점 역시 존재하며 그 이유는 아래와 같습니다.

- 렌더링할 준비가 된 HTML을 받기 때문에 사용자에게 더 빠르게 화면을 제공할 수 있습니다.
- 일관된 SEO(Search Engine Optimization)성능을 가집니다.

이런 장단점을 가지는 SSR은 사실 요즘같이 웹에서 사용자 인터랙션이 많은 상황에는 그렇게 이상적이지 않은 방법일 수 있습니다.

### CSR

CSR은 뷰와 관련된 렌더링은 사용자의 브라우저가 담당하게 하며 사용자의 인터랙션이 발생할 때 마다 자바스크립트를 사용해서 업데이트하고 뷰를 다시 그려줍니다. 즉 SSR의 단점을 보완하는 동작을 한다고 이해할 수 있습니다. 하지만 CSR의 동작 방식은 아래와 같은 문제를 가질 수 있습니다.

- 자바스크립트를 통해서 뷰를 업데이트 하기 때문에 페이지의 규모가 커질수록 자바스크립트 파일의 규모 역시 커집니다.
- 자바스크립트를 실행해서 뷰를 그리기 떄문에 SSR보다 사용자가 페이지를 접하는 시간이 느립니다.
- 자바스크립트를 통해서 화면을 그리고 정보를 보여주기 때문에 자바스크립트를 실행하지 않는 크롤러는 페이지의 정보를 정확하게 수집할 수 없습니다. 이는 일관된 SEO(Search Engine Optimization)성능을 가질 수 없다는 의미와 같습니다.

CSR의 장점은 아래와 같습니다.

- 초기 렌더링 이후에는 서버에서 필요한 데이터만 받아서 화면을 구성하기 때문에 사용자에게 더 빠른 인터랙션을 제공합니다.
- 화면 이동에 있어서 변경 이전의 상태를 관리하기 용이합니다.
