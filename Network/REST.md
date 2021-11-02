## REST

### REST의 정의

- REpresentational State Transfer의 줄임말로, 2000년 로이 필딩이 발표한 박사학위 논문.
- Representational State
    - 자원(Resource)을 이름(Representation)으로 구분하여, 자원의 상태(정보)를
- Transfer
    - 주고 받는 것.
- REST란, 분산 하이퍼미디어 시스템을 위한 아키텍쳐 스타일.
    - 분산 하이퍼미디어 시스템이란 웹을 뜻한다.
    - 아키텍쳐 스타일이란, 제약조건들의 집합이다.
    - REST는 HTTP나 SOAP과 다르게 프로토콜이 아니다. 따라서 엄밀한 표준은 없고 로이 필딩이 발표한 논문에서 제안하는 내용이다. 그래서 개발자가 구현하기 나름이다.
- HTTP URI를 통해 자원(Resource)를 명시, HTTP Method를 통해 자원에 대한 상태 변경(CRUD)을 주고 받는다.
    - 결국 **REST는 자원 기반 구조(ROA, Resource-Oriented Architecture)**이고, HTTP 메소드를 통해 **자원**을 처리하도록 설계된 아키텍쳐 스타일

### REST라고 부르기 위한 제약조건

- REST는 HTTP를 더 발전시키기 위해 고안된 아키텍쳐 스타일이다. 따라서 제안된 아키텍쳐 스타일중 많은 부분이 HTTP를 사용했을 경우 만족한다.
1. client-server
    - 자원이 있는 쪽: 서버
    - 자원을 요청하는 쪽: 클라이언트
    - 이 구조가 갖는 장점은 서로간의 의존성이 줄어들기 때문에 독립적 진화가 가능하다.
2. stateless(상태 없음)
    - 모든 HTTP 요청이 독립적으로 발생. 즉, 앞 뒤에 어떤요청을 보내는지가 중요하지 않다.
    - stateless는 로드 밸런싱이 가능한 분산 서버 환경을 쉽게 제공한다. (ex: 인증&인가)
    - HTTP의 성질과도 동일하다.
3. cache
    - HTTP 프로토콜을 그대로 사용하기 때문에 가능. ex): HTTP의 ETag
        - ETag HTTP 응답 헤더는 특정 버전의 자원을 식별하는 식별자. 자원이 변경되면 ETag는 새로 생성됨.
4. layered system
    - 프록시, 로드밸런싱등 서버를 다중 계층으로 구성하여
5. code-on-demand(필수는 아님)
    - 서버로부터 스크립트를 받아 실행시킬 수 있어야 한다.
6. **uniform interface** - (이 조건을 만족 시키기 까다로움)
    - identification of resource: 리소스가 URI로 식별되면 된다.
    - manipulation of resources through representations: 리소스를 생성, 삭제, 수정 등을 할 때 메세지에 그 표현을 담아서 전송해야 한다. URI + Method
    - **self-descriptive messages:** 메세지는 스스로를 설명할 수 있어야 한다.
    - **hypermedia as the engine of the application state (HATEOAS):** 애플리케이션의 상태는 하이퍼링크를 이용해 전이가 되어야 한다. ex): HTML의 a, link태그

### 왜 Uniform interface가 필요하냐? - 독립적 진화를 위해

- 서버와 클라이언트가 각각 독립적으로 진화한다. 따라서 서버의 기능이 변경되어도 클라이언트를 업데이트 할 필요가 없다.
- REST를 만들계 된 계기는, 웹 생태계를 훼손하지 않으며 HTTP를 발전시키기 위함이었음. 목적에 맞게 현재까지 "웹" 에서는 REST가 잘 지켜짐. 그러나 API에서는...?
    - 웹 페이지를 변경했다고 웹 브라우저를 업데이트 할 필요가 없다.
    - HTTP 명세가 바껴도 웹은 잘 동작한다.
    - HTML 명세가 변경되어도 웹은 잘 동작한다.
- REST 가 웹의 독립적 진화에 어떻게 도움을 주었나?
    - HTTP에 지속적인 영향
        - Host 헤더 추가
        - URI 길이 제한 → 414 Requested-URI Too Long
    - URI 에서 리소스의 정의가 추상적으로 변함 → 식별하고자 하는 무언가

### Self-descriptive 와 HATEOAS가 독립적 진화에 어떻게 도움을 주는가?

- Self-descriptive
    - 서버나 클라이언트가 변경되더라도 오고가는 메세지는 self-descriptive 하므로 언제나 해석이 가능하다.
    - 서버나, 클라이언트가 변해도 "메세지"만 가지고 모든게 해석 가능하기 때문에 커뮤니케이션이 **확장가능**하다.
- HATEOAS
    - 애플리케이션 상태 전이의 late binding이 가능해진다. (런타임 바인딩)
    - 어디서 어디로 전이가 가능한지 미리 결정되지 않는다. 어떤 상태로의 전이가 완료되고 나서야 그 다음 전이될 수 있는 상태가 결정된다. 즉, **링크는 동적으로 변경**될 수 있다. 서버가 링크를 바꿔도, 클라이언트는 바뀐 링크대로 따라가면된다.
    - 의존성이 없다 = 독립적 진화가 가능하다.

### REST 구성요소

- 자원(Resource): URI
    - 모든 자원에 고유한 ID가 존재하고 이 자원은 서버에 존재 (서버 내의 디렉토리라고 봐도 무방)
    - 자원을 구별하는 ID는 HTTP URI.
- 행위(Verb): HTTP 메소드
    - GET: 자원 읽기를 요청
    - POST: **자원 추가**를 요청
    - PUT: 자원 수정을 요청
    - PATCH: 자원 일부 수정을 요청, Update와 가장 어울리는 메소드
    - DELETE: 자원 삭제를 요청
    - HEAD: 표현의 메타데이터만 조회
    - OPTIONS: 어떤 HTTP 메소드를 지원하는지 검사. ex): CORS Preflight에서 사용
- 표현(Representation of Resource)
    - 클라이언트가 자원의 상태에 대한 조작(CRUD)을 요청하면 서버는 이에 적절한 응답(Representation)을 보낸다.
    - Representation은 JSON, XML등 여러 형태로 나타낼 수 있다. 주로 JSON을 사용한다.

### 안정성(Safety) & 멱등성(Idempotent)

- 안정성
    - GET, HEAD와 같은 요청은 데이터를 읽기만 하고 서버의 상태를 바꾸지 않는다.
    - 따라서 어떤 URI가 와도 서버는 안전하다고 느낀다. 개인정보의 유출 이런 관점이 아니라, 서버 내부 데이터에 영향을 주지 않을 것이라는 관점.
- 멱등성
    - 멱등성이란 N회 수행해도 서버 자원의 상태가 동일한 것을 의미한다.
    - PUT, DELETE도 멱등이다. 왜냐하면, PUT의 엄밀한 정의는 서버에 해당 데이터가 특정 상태로 존재함을 보장해라. 라는 뜻이다. 따라서 100번 PUT을 보내도 첫 1회에 수정된 이후로는 절대로 바뀔 일이 없다. DELETE도 마찬가지다. 1회 삭제하면 100회 삭제해도 더이상 삭제할 것이 없어 서버입장에선 똑같다. (100x1x1x1x...x1) , (100x0x0x0x0x0...)
    - POST는 멱등성이 성립하지 않는다. POST는 서버의 데이터를 수정한다. 특정 locate(URI)에 새로운 데이터를 집어넣는다. 100번 수행하면 100개의 같은 데이터가 존재하게 될 것이다. PUT과 POST의 가장 큰 차이점이다.
- 왜 안전성과 멱등이 중요한가?
    - 안정성과 멱등은 신뢰할 수 없는 네트워크 상의 HTTP를 나름 신뢰 할 수 있게 만든다.
    - GET 요청이 실패해도 또 보내도 문제가 없음을 신뢰할 하게 만드는 것이다. 반면 POST는..?
