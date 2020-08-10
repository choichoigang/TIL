# Browser work

## How browser rendering works ?

![image](https://user-images.githubusercontent.com/49897409/89811698-fcfa2880-db79-11ea-80fb-04a4d0467cc9.png)

## 주요 렌더링 경로

- 주요 렌더링 경로 최적화는 현재 사용자 작업과 관련된 콘텐츠 표시의 우선순위를 지정하는 것을 말합니다.
- 주요 렌더링 경로를 최적화 하면 최초 페이지 렌더링에 걸리는 시간을 상당 단축시킬 수 있습니다.
- 연속 루프에서 실행되어 이상적인 속도는 초당 60프레임 입니다.

![image](https://user-images.githubusercontent.com/49897409/89811768-1a2ef700-db7a-11ea-872c-8585381a1ce2.png)

## 객체 모델 생성

- 브라우저가 페이지를 렌더링 하려면 먼저 DOM(Document Object Model)과 CSSOM(CSS Object Model)을 생성합니다.
- 브라우저에 전달 되서 객체 모델 생성 까지 과정은 바이트 → 문자 → 토큰 → 노드 → 객체 모델 순으로 진행 됩니다.
- DOM 및 CSSOM은 서로 독립적인 데이터 구조입니다.

**DOM**

![image](https://user-images.githubusercontent.com/49897409/89811859-3c287980-db7a-11ea-8df2-163c679482be.png)

1. Bytes → Characters : HTML의 원시 바이트를 디스크나 네트워크에서 읽어온 후 해당 파일에 대해서 지정된 인코딩 방식에 따라서 문자로 변환합니다.
2. Characters → Tokens : 변환된 문자열을 W3C HTML 표준에 지정된 고유 토큰으로 변환합니다.
3. Lexing(낱말 분석) : 변환된 토큰을 해당 속성 및 규칙을 정의하는 객체로 변환합니다.
4. DOM 생성 : HTML 마크업이 여러 태그 간의 관계를 정의하기 때문에 생성된 객체는 트리 데이터 구조로 형성됩니다.

**CSSOM**

CSSOM을 생성하는 과정은 DOM을 생성하는 과정과 마찬가지로 CSS 규칙을 브라우저가 이해할 수 있는 방식으로 변환되며 위의 HTML 프로세스의 과정을 반복하게 되며, CSSOM이 트리의 구조를 가지게 되는 이유는 하향식으로 규칙을 적용하며 계산된 스타일을 최종적으로 전달하는 일을 합니다.

아래의 그림에 경우에는 body의 하위에 속하는 span tag와 p tag 하위에 속하는 span tag는 같은 span이지만 다른 CSS속성을 가지게 되며 최종적으로 계산된 스타일을 적용하게 됩니다.

![image](https://user-images.githubusercontent.com/49897409/89811937-58c4b180-db7a-11ea-8fba-38497dfc0f5c.png)

## 렌더링 트리 생성, 레이아웃 및 페인트

- 생성된 DOM과 CSSOM은 Attachment 과정을 거쳐서 렌더링 트리를 생성합니다.
- 렌더링 트리는 표시되는 각 요소의 레이아웃을 계산하는데 사용되며 픽셀을 화면에 렌더링하는 페인트 프로세스의 입력으로 처리됩니다.

![image](https://user-images.githubusercontent.com/49897409/89812018-742fbc80-db7a-11ea-806e-303a80d6c6b4.png)

위의 과정과 같이 5단계를 다시 되짚어 본다면

1. HTML 마크업을 처리하고 DOM 트리를 빌드합니다.
2. CSS 마크업을 처리하고 CSSOM 트리를 빌드합니다.
3. DOM 및 CSSOM을 결합하여 렌더링 트리를 형성합니다.
4. 렌더링 트리에서 레이아웃을 실행하여 각 노드의 형태와 위치를 계산합니다.
5. 개별 노드를 화면에 페인트 합니다.

**주요 렌더링 경로를 최적화하는 작업은 위 단계를 수행할 때 걸린 총 시간을 최소화하는 프로세스 입니다.**
