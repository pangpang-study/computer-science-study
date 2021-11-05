###### 주 메모리의 관리

Logical vs Physical address

메인 메모리(쉽게말해 Ram)

logical - cpu가 사용하고 있는 메모리의 가상주소
physical - 물리적 메모리가 저장되어있는주소

소프트웨어가 경계를 정해줄 수 없다.

MMU(Memory Management Unit)  

logical address를 physical address로 변경해준다.
relocation address에 의해 physical address를 logical하게 변경해준다

mmu에있는 tlb(캐시)를 통해 빠르게 logical address를 이용해 physicall address 참조를 한다 없다? 페이지 테이블로 이동



메모리 관리 발전 과정

dynamic loading
메모리공간을 효율적으로 사용하기위해, 필요한 routine만 loading을 하자

static linking - loader가 바이너리 코드를 같이 묶어서 exe 파일 만듬
dynamic linking - linking을 실행시에 필요하면 link된 libaray를 call 해옴



단편화의 문제
외부 단편화 - contiguous memory allocation의 문제...
내부 단편화 - 페이징, 세그멘테이션에서의 문제..



contiguous memory allocation

 single section of memory를 가진다 
순서대로 프로세스를 넣기
앞에있는 프로세스가 먼저 종료되면 hole이 생김

dynamic memory allocation
first fit 들어갈수 있는 첫번째 공간에 넣기
best fit 들어갈수 있는 가장 작은 공간에 넣기
worst fit 들어갈수 있는 가장 큰 공간에 넣기



contiguous memory allocation -> segmentation -> paging

페이징
외부단편화를 피하기위한메모리 관리 기법
물리 메모리를 frame 단위로 나눔
논리 메모리를 같은 크기의 페이지로 나눔 -

페이지 테이블에선 page number, page offset으로 나뉨

논리 메모리 -> 페이지테이블 -> 물리 메모리

PTBR - 페이지테이블 어디를 참조할지 하는 레지스터(pcb에 존재함)

shared page
페이징을 통해 shared common code를 공유하면 너무 커진다 그러므로, 같은 물리 메모리를 참조하는애들이 가진 페이지를 모아놓은 곳

계층적 페이징 - 페이지테이블의 페이지테이블
hashed page table - 해시를 이용해서 페이지 테이블들을 관리
inverted page table - pid를 이용해서 페이지 테이블들을 관리

swapping
메인 메모리의 size보다 더 큰 프로그램을 실행해야할때 사용
swap in -> 디스크에서 메모리
swap out -> 메모리에서 디스크
