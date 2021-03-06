## 페이지 교체 정책
> 메모리, 캐시 성능 높이기 위해 어떤 페이지를 방출할까?
- 페이지 교체 정책(Page Replacement Policy)이란, 메모리 가상화 방법중 페이징(Paging)에서 페이지를 어떻게 교체할 것인가에 대한 방법론이다
- 사실 캐시나 TLB에서 Miss Rate을 줄이기 위한 Replacement Policy들과 동일하다. 이하 포괄적인 Swapping Policy로 퉁쳐서 설명
- 교체 방법에서 가장 중요한 목표는 성능이다
    - Swap수를 줄인다 -> **Disk I/O Access 빈도를 낮춘다**.

### 최적의 방법 (Optimal)
- Belady가 만든 방법, 가장 Miss(Page Fault)가 적은 방법
- 가장 먼 미래에 쓰일 Page를 메모리에서 교체(추방)시키는 것
- 즉, 미래를 알아야함 -> 불가능
- 다만 다른 방법들이 얼마나 Optimal에 근접한지 성능 평가의 기준으로 쓰인다

### FIFO
- 먼저 들어온 페이지를 먼저 교체(추방)한다
- 장점
    - 정말 간단하게 구현이 가능하다(프로그래밍적으로)
- 단점
    - Locality를 고려하지 않은 방법으로, 워크로드에 따라 정말 높은 Miss Rate을 가질 수 있다
    - Belady's Anomaly: 분명 Queue의 사이즈가 커지는데 Miss Rate이 오히려 커지는 현상

### Random
- 무작위하게 추방
- 장점
    - 운이 좋으면 optimal
- 단점
    - 운이 나쁘면...

### LRU(Least Recently Used)
- 과거의 기록을 참고하여 가장 사용(참조)한지 오래된 페이지를 추방
- LRU는 locality에 의거하여 최근에 사용했다면 또 사용할 확률이 높을 것이라고 판단하기 때문
    - for, while과 같은 반복문
- 장점
    - 워크로드의 형태에 따라 다르겠지만 80-20 워크로드에서 가장 Optimal에 유사한 성능을 내는 것으로 알려져 있다
    - 80-20 워크로드: 80%의 참조가 20%의 페이지로 이루어진다. 나머지 20%의 참조가 80%의 페이지로 이루어진다. 즉, 자주 쓰는것만 쓴다
- 단점
    - 실제로 구현하기 어렵다.
    - 모든 워크로드를 커버하는 방법은 아니다. 루프문에서 캐시 사이즈가 너무 작아지면 Miss Rate이 오히려 100%가 될 수 있다.


**추가 논제: LRU를 어떻게 구현할 것인가?**
- 참조 링크: [Geeksforgeeks - LRU 구현 방법](https://www.geeksforgeeks.org/lru-cache-implementation/)
- Double Linked List + Hash: Hash는 Linked List의 노드 위치를 bucket에 담아서 탐색 시간을 O(1)로 만들어 주는 역할
  - Hit: O(1) (Hash로 탐색: O(1), LL에서 특정 값을 삭제하는 시간: O(1), 다시 LL꼬리에 추가하는 시간 O(1))
  - Miss: O(1) (Hash로 탐색: O(1), LL에서 pop: O(1), Hash에서 remove: O(1))
- 지적 받습니다.


### Clock - 유사 LRU
- LRU 구현이 힘들기 때문에 나온 꼼수
- Use Bit을 이용해서 반복하며 counting 하는 방법.
    - 모든 값을 시계마냥 돌면서 매칭되지 않았다면 use bit의 값을 1 감소
    - 만약 use bit의 값이 0이 되면 해당 데이터 방출.
    - 만약 매칭이 되었다면 해당 bit을 다시 n(상수값)으로 설정
- 장점
    - LRU 보다 구현하기 쉬움
- 단점
    - LRU보다 성능이 구림

## 페이지 선택 정책
> 성능을 위해 페이지를 1개씩 다루지 말자
- Miss Rate을 줄이기 위해서는 성능을 높일 방법들이 추가로 있음. 즉, 무엇을 방출할 지 말고 무엇을 더 효율적으로 가져올지에 대한 내용

### Prefetching
- OS가 다음에 어떤 페이지가 참조될지 locality에 의거해서 예측한다.
    - Page 1을 가져올 때 옆에 있는 Page2, 3을 동시에 가져온다.
    - ex): 배열, 반복문 등 주변 페이지를 활용할 확률이 높을 때

### Clustering/Grouping
- 디스크에 write을 해야 할 경우 여러 페이지를 모아서 한 번에 하자. I/O Access를 줄이자
