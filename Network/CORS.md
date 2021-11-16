## CORS(Cross-Origin Resource Sharing)
- CORS는 교차 출처 리소스 공유의 줄임말로, **추가 HTTP 헤더를 사용**하여, 한 출처에서 실행 중인 웹 애플리케이션이 **다른 출처의 선택한 자원에 접근**할 수 있는 **권한을 부여**하도록 브라우저에 알려주는 **메커니즘**이다.
- 출처(Origin)
    - URL의 프로토콜, 호스트, 포트를 통한 정보

### CORS가 왜 등장한 개념인가?
- **SOP(Same-Origin Policy)**: SOP란 Same Origin Policy의 준말로, 다른 출처의 리소스를 사용하는 것에 대해 제한하는 방식. 즉, 같은 출처의 리소스만 사용하는 정책을 말한다.
- 보안상의 이슈로 다른 출처의 리소스 요청이 들어오면 개인정보 유출 등의 문제가 있어서 불법적인 요청으로 간주했다고 함.
- 즉, 잠재적으로 해로울 수 있는 다른 출처에서 가져온 리소스와 상호작용 하는 것을 제한하여 공격 경로를 줄여주는 것이다.
    - 공격이라 함은 CSRF, XSS등이 있다.
- 하지만 웹 사이트에서 할 수 있는 일이 많아지며, 보안 정책 때문에 불편한 일이 점점 늘어갔다.
    - 내가 제작하는 웹에서 지도 API를 사용하는 작업이 필요하다면? -> 다른 출처의 리소스가 필요한 상황이 되는 것이고 SOP를 위반하게 된다. 
    - 이와 같이 SOP를 통해 오히려 제한이 걸리는 상황이 많아졌고, 위와 같은 요구는 점차 늘어갔기 때문에 다른 출처(cross-origin)에 대한 리소스를 **보안상 안전하게** 가져올 방법이 필요하게 됨.
- CORS 이전에 JSONP라는 방식으로 우회해서 사용했다고 함
- JSONP대신, 공식적인 방법으로 다른 출처의 리소스를 사용할 수 있도록 하기 위해 CORS가 등장

## CORS 동작원리
1. 브라우저는 자신의 출처를 HTTP의 요청 헤더에 넣어서 보낸다. 
2. 서버는 응답을 할 때 HTTP `Access-Control-Allow-Origin` 헤더에 “접근이 허용된 출처” 라고 브라우저에게 알려준다. 
3. 브라우저는 Origin과 `Access-Control-Allow-Origin`의 값을 비교한다. 그리고 유효한지 검사한다. 

이는 가장 기본적인 흐름이고 구체적으로는 3가지의 방식으로 작동한다.

### Preflight Request
- 리소스 요청을 하기 전에 미리 HTTP **OPTIONS 메소드**를 통해 다른 출처가 접근해도 되는지(안전한지) 확인하는 방법
1. Preflight Request: OPTIONS와 클라이언트의 출처를 보냄
```
OPTIONS /api/~~
Origin: 클라이언트의 출처
```
2. 해당 출처에 대한 서버의 허가
```
200 Ok
Access-Control-Allow-Origin: 클라이언트의 출처
```
3. Main Request: 진짜 요청을 주고받음.

### Simple Request
- Preflight와 다르게 바로 본론부터, 즉 Main Request부터 보내보고, 서버가 검사하는 방식
1. Main Request: 진짜 요청을 보냄
```
GET /api/~~
```
2. 해당 출처에 대한 서버의 검증 및 허가. 만약 허가되지 않았다면, 즉 `Access-Control-Allow-Origin`가 반환되지 않았다면 거절된 것으로 간주
```
200 Ok
Access-Control-Allow-Origin: 클라이언트의 출처
```

- Simple Request는 사용에 조건이 있다.
    - 메소드가 GET, POST, HEAD인 경우에만 사용 가능. -> RESTful 하지 못함.
    - Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안된다.
    - 만약 Content-type을 사용하는 경우에는 application/x-www-form-urlencoded, multipart/form-data, text/plain 만 허용된다.
- 대부분의 API들은 JSON형태로 주고받기 때문에 3번째 조건을 만족하기 정말 어렵다. 그래서 Simple Request는 적용되기 힘들다.

### Credential Request
- 인증된 요청을 사용하는 방식. 보안을 조금 더 강화하는 방식으로 이해하면 된다.
- 다른 출처에 **인증 관련 정보를 담아서 요청해야 하는 경우**를 의미한다. 즉, 프론트엔드 javascript XMLHttpRequest나 fetch같은 API에서 제공
- 쿠키 정보나 인증과 관련된 헤더를 요청에 담을때 3가지 옵션이 있다.
    - same-origin: 같은 출처에 보낼 때만 담음
    - **include**: 모든 요청에 인증 정보를 담을 수 있음
    - omit: 모든 요청에 인증 정보를 담지 않는다
- include를 사용하는 경우 통신이 동작하기 위해서는 만족해야 하는 조건이 추가로 두 가지 존재한다.
    - 응답 헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야한다. 안그러면 브라우저가 거부함(에러 발생).
    - 서버측에서 Access-Control-Allow-Origin를 반환할때 와일드카드`*`를 사용하면 안되고 명시적인 출처를 반환해야함.


## CORS를 해결하는 방법
- 서버측에서 `Access-Control-Allow-Origin`에 올바른 리턴값 넣어주기.
- Credential로 요청이 들어올 수 있으니 와일드 카드 대신 출처 제대로 명시하기.
- 백엔드 프레임워크에선 CORS를 위한 모듈을 제공하니 적용하기 간편할 것. 다만 다양한 상황의 요청이 있을 수 있으니 숙지할 것.







