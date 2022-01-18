## [프로세스 관리 #1](https://core.ewha.ac.kr/publicview/C0101020140321144554159683?vmode=f)

### 프로세스 생성 (Process Creation)

- 부모 프로세스가 자식프로세스를 생성한다
- 프로세스의 트리 구조 형성
- 주소공간
  - 자식은 부모의 공간을 복사한다
  - 그 공간에 새로운 프로그램을 올린다

### 프로세스 종료 (Process Termination)

- 부모가 먼저 종료될 경우, 자식에게 할당한 task가 더이상 필요하지 않을경우, 자식이 할당 자원의 한계치를 넘어 설 경우에는 강제 종료(abort)

## [프로세스 관리 #2](https://core.ewha.ac.kr/publicview/C0101020140325134428879622?vmode=f)

### 프로세스와 관련한 시스템콜

- fork()
  - 부모를 그대로 복사
  - 주소 공간을 할당
- exec()
  - 새로운 프로그램을 메모리에 올림
- wait()
  - 자식이 부모에게 output data를 보냄
  - 프로세스의 각종 자원들이 운영체제에게 반납됨
- exit()
  - 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려줌

### 프로세스 간 협력 - with Kernel

- 독립적 프로세스: 프로세스는 각자의 주소 공간을 가지고 수행하되, 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함
- 협력 프로세스 :  하나의 프로세스는 다른 프로세스의 수행에 영향을 미칠 수 있음
- 협력 메커니즘(IPC)
- Thread : 하나의 프로세스이므로 프로세스간 협력으로 보기는 어려우나, 한 프로세스의 주소공간을 공유한다

### Pipe(익명)

- 부모 자식 process 간 통신에 이용된다
- Pipe를 이용해 단방향 통신 중 반 이중 통신을 이용하여 process를 연결하게 되고, 하나의 process는 읽기만, 하나의 process는 쓰기만 가능
- 양쪽의 Process가 통신을 해야할 상황이라면? 파이프를 두개를 만들어야 한다

### Name Pipe

- 연관이 없는 process간 통신에 사용된다
- 익명 Pipe 방식과의 차이라고 한다면, 다른 process의 pid를 알 경우에 사용이 가능한 기법

### Message Passing

- **커널**을 통해 메세지를 전달
  
  - direct communication - 통신할 프로세스의 이름을 명시적으로 표시해서 직접 전달 
  - indirect communication - mail box(port)를 통해서 간접적으로 메시지를 전달
  
  ![image-20220117203305001](C:\Users\gi718\AppData\Roaming\Typora\typora-user-images\image-20220117203305001.png)

### Shared Memory

- 일부 주소공간을 두 프로세스가 공유하게끔 하여 협력
- 모든 IPC 중에서 가장 빠르게 작동 할 수 있다는 장점이 있다

![image-20220117203324474](C:\Users\gi718\AppData\Roaming\Typora\typora-user-images\image-20220117203324474.png)

### Socket

- 네트워크 socket통신을 이용한 데이터 공유
- client - server 구조로 각각의 포트를 정하고 포트 간 데이터 통신을 진행한다
- 다른 시스템과의 통신이 가능하기때문에 양방향 통신이 가능

### RPC

- 분산형 네트워크 환경에서 주로 쓰이는 process communication 방법
- client - server 구조와 분산 컴퓨팅을 기준으로 하며, 자신의 주소공간 뿐 아니라 다른 주소공간에서 동작하는 프로세스의 함수를 실행할 수 있게 된다
- 즉, 플랫폼이나 구현 언어가 다르더라도 동일한 rpc 인터페이스를 구현하여 호출할 수 있다

![image-20220117211825643](C:\Users\gi718\AppData\Roaming\Typora\typora-user-images\image-20220117211825643.png)

- client/server : IDL로 작성이 되어있으며 사용자가 필요한 비즈니스 로직을 작성한다
- Stub : Stub Compiler에의해 IDL파일을 읽어 원하는 Language로 변환해준다
- RPC Protocol
  - static binding : 서버주소가 고정되어 있는 전달방식
  - dynamic binding : 주소변경에 유동적이나, 여분의 서버를 두어야 한다(load balancer, name server 등)

### CPU and I/O Bursts in Program Execution

- cpu만 사용하는 단계 cpu burst, i/o를 사용하는 단계 i/o burst
- 서로 번갈아가면서 진행됨

### CPU-burst Time의 분포

- cpu와 io장치 등을 이용하는 여러 종류의 process가 섞여 있기에 시스템 자원을 골고루 효율적으로 사용해야한다

### 프로세스의 특성 분류

- I/O - bound process : 사용자의 입출력위주
- CPU-bound process : 계산위주

### CPU Scheduler & Dispatcher

- CPU scheduler : ready 상태의 process중에서 cpu를 줄 프로세스를 고르는 코드
- dispatcher : cpu의 제어권을 cpu scheduler에 의해 선택된 프로세스에게 넘기는 코드

