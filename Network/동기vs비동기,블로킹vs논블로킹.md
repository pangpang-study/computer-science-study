
## I/O

- input/output. 데이터를 주고 받는 행위
- 파일, 소켓, 디스크 등 여러 입출력이 있음.
- I/O 처리는 커널 레벨에서 권한을 갖고 처리됨. 따라서 유저 레벨에서는 커널의 결과 반환을 기다려야 함.
- 여기서 어떻게 처리하고 기다릴지에 따라 동기, 비동기, 블로킹, 논블로킹이 결정된다.

## 블로킹 vs 논블로킹

블로킹 vs 논블로킹의 구분은 **결과값(완료)을 언제 알려주는가**이다.

**블로킹(Blocking I/O)**

- 블로킹은, I/O 작업을 호출하고 작업이 끝날 때까지 기다림. 작업이 끝나면 결과 완료 통지를 "즉시" 받는다.
- 블로킹이 순차 처리를 보장하는 것은 아니다. 중간에 다른 작업이 인터럽트 등을 이용해 끼어들 수 있다는 뜻. 이론상으로는 그렇지만 대부분 동기 + 블로킹을 조합해서 사용하기 때문에 자주 헷갈리는 내용이다.

**논블로킹(Non-blocking I/O)**

- 논블로킹은, I/O 작업을 호출하고 작업의 완료 여부와 상관없이 호출한 함수에서는 return을 받고, 결과 완료에 대한 통지는 나중에 받는다.

## 동기 vs 비동기

동기 vs 비동기의 구분은 **작업의 시작과 끝의 순서**다.

**동기(Synchronous)**

- 동기는 작업의 순서가 보장되는 것을 말한다. A, B, C 순서대로 요청이 들어왔다면 결과도 순서대로 도출되는 것을 말한다.
- 직렬과 유사한 느낌.

**비동기(Asynchronous)**

- 비동기는 작업의 순서가 보장되지 않는 것을 말한다. 들어온 요청에 따라 수행되는 것이 아니다.
- 병렬과 유사한 느낌.

## 4가지 조합

동기-블로킹 I/O

- `read`, `write` system call
- 블로킹 덕분에 I/O발생시 block이 된다. 즉 CPU 점유를 할 수 없다. 커널이 I/O에 대한 결과를 줄 때까지 block 상태.

**동기-논블로킹**

- `read`, `write` + `O_NONBLOCK`
    - `O_NONBLOCK`이 붙게 되면, 데이터가 없으면 그냥 바로 -1을 return 한다.
- 논블로킹이라 I/O 발생해도 block되지 않는다. 그러나 동기라서 커널이 I/O 처리를 다 했는지 Polling한다.
- 컨텍스트 스위칭 비용이 늘기 때문에 오히려 성능상 이점은 없다.

**비동기-블로킹**

- `select` , `poll` system call이 대표적
    - 여러 파일(file descriptor, 소켓도 포함)을 감시하며 변화가 있을 경우 이를 감시해서 읽는다.
    - 작업의 순서가 딱히 없고, 파일을 감시하고 기다리는 시간(timeout 시간)동안 블록이 된다.
- 유저 레벨(Application)에서는 비동기적으로 처리하고 싶어함. 순서보장이 필요없음. 그러나 커널이 `select`와 같은 시스템콜에 의해 block을 걸기 때문에 다른 작업을 할 수가 없는 상태.

비동기-논블로킹

- 유저레벨과 커널레벨 모두 비동기, 논블로킹으로, 작업의 완료에 딱히 관심이 없고 순서보장도 필요없음. 그래서 그동안 유저레벨 프로세스는 다른 일을 처리할 수 있음.
