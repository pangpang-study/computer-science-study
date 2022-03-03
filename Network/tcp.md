# 트랜스 포트 계층
- 수많은 데이터가 오고가는 와중에 혼선이 생기지 않는 이유 
- 컴퓨터 안까지 들어온 데이터를 프로그램까지 전달하는 것이 트랜스 포트 계층
 
## TCP
- TCP/IP라고 용어가 대표적으로 사용될 만큼 TCP는 많이 활용된다. 
- 데이터가 정확하게 전달되어야 하는 통신에 사용된다 
- 근데 제대로 표시 되지 않는 경우도 있지 않나? --> 다른 문제상황이 원인이라고 봐야한다. 


## 트랜스 포트 계층
- 컴퓨터가 받은 데이터를 애플리케이션 계층까지 전달하는 계층이다.


### 트랜스 포트 계층의 TCP 프로토콜은 수신지에 데이터가 정확하게 전달되도록 전송속도를 조절하고 도달하지 않은 데이터를 재전송 한다.

## 포트번호 
- 트랜스 포트 계층에는 인터넷 계층에서 전달된 다양한 패킷이 들어온다.
- 그럼 누구한테 전달되어야 하는지 어떻게 알아? --> **포트번호**
- 0 ~ 1012 => 웰 노운 포트  , 1024 ~ 49151 => 레지스터드 포트 , 49152~65535 => 다이나믹 포트
- 다이나믹 포트 : 클라이언트가 받을 포트를 다이나믹 포트중에 비어있는 번호로 할당된다. 

## 클라이언트와 서버의 접속과정 
- 클라이언트와 서버가 통신하려고 함
- 클라이언트가 사용한 포트를 다이나믹 포트중에 비어있는 번호로 할당
- 서버의 포트에 접속 요청(HTTP인 경우 서버 측에서 수신대기하는 포트 번호 80)
- 웹서버가 접속을 허용하면 통신 가능한 상태가된다. 

    ## 서버 포트는 고정인데 그럼 클라이언트는 어떻게 식별할까?
    - IP어드레스 와 포트번호의 조합(추후에 추가적인 설명)
    
## TCP가 정확하게 데이터를 전달하는 방법
- TCP : 트랜스포트 계층의 프로토콜의 하나로 웹이나 이메일 , FTP같이 정확한 데이터 전달이 필요한 통신에 사용
- 어떻게 신뢰성을 더하지?? -> 세그먼트라는 단위로 분할 , 전송속도를 조정, 데이터가 제대로 전달이 안되면 재전송 
    - 세그먼트 : TCP에서 분할된 데이터 (TCP에서의 패킷)
    - 송신 TCP는 HEADER를 붙혀 세그먼트를 만든 후 IP계층으로 전달
    - 수신쪽을 HEADER를 떼고 APPLICATION으로 전달
    ### TCP 헤더의 구조 
    - TCP 세그먼트 : 데이터 본체 + TCP 헤더
    - 수신한 바이트 수 , 송신한 바이트 수 , TCP 헤더의 길이, 한번의 수신할 수 있는 데이터 크기, 데이터 훼손 여부 
- 컨트롤 비트 : 현재 통신상태를 표현하는 플래그 (9BIT로 표현)
    - CWR : 통신경로 복잡 , 전송량 줄여야 함
    - ECE : 통신 경로 혼잡, 수신이 불가함
    - ACK : 이전동작을 확인했다, 확인 응답번호와 조합해서 사용
    - SYN : 접속을 시작할 때 ON으로 설정한다. 
    - FIN : 데이터 송신이 완료되어 통신을 종료하고 싶다고 알림


## 통신 개시부터 통신 종료까지의 흐름
- 커넥션 연결에서 시작
- 커넥션을 맺는 과정은 3단계로 진행 --> **3-WAY HandShake**
    - 통신을 시작하고 싶을 때 SYN을 on으로 설정한다. 
    - SYN에 대한 응답 확인을 ACK신호와 함께 보냄
    - 응답 확인에 대한 ACK를 보냄
- 일련번호와 채되 세그먼트 크기를 사전에 조율 
    - 커넥션 시 일련번호와 최대 세그먼트 크기(mss)를 서로 합의하고 조율함
    - 만약 저서로 조율하려는 크기가 다르다면 작은값을 따라간다. 
    