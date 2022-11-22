---
title:  "Computer Architecture"
excerpt: "Operating System Summaty"

categories:
  - Operating System

tags:
  - [Computer Science, Operating System]

toc: true
toc_sticky: true
layout: post
use_math: true
 
date: 2021-09-06
last_modified_at: 2022-11-08
---

# 컴퓨터구조

# 컴퓨터 계층구조

- <span style="color:skyblue">계층적 구조</span>를 통해 <span style="color:orange">간접적으로 자원을 제공</span>
  - 사용자는 응용프로그램을 통해 컴퓨터 활용
  - 응용 소프트웨어는 운영체제를 통해 자원<sup>컴퓨터 하드웨어</sup>활용
- 운영체제가 <span style="color:red">모든 자원에 대한 배타적 독점적 지배</span>를 통해 <span style="color:gold">하드웨어 자원에 대한 추상화</span> 제공
    > ![컴퓨터 계층 구조](/assets/img/layered%20architecture.png)

# ***폰 노이만/하버드 구조***

### ***폰 노이만 구조***

- 프로그램 명령어와 데이터가함께 메모리에 있어 CPU를 통해 연산하는 구조
  > ![폰 노이만 구조](/assets/img/%ED%8F%B0%EB%85%B8%EC%9D%B4%EB%A7%8C%20%EA%B5%AC%EC%A1%B0.png)
- 데이터 메모리와 프로그램 메모리가 구분되어 있지 않고 하나의 버스를 가지고 있는 구조 때문에 CPU가 명령어와 데이터에 동시 접근할 수 없어 속도가 떨어진다.

### ***하버드 구조***

- 명령어와 데이터가 서로 다른 메모리와 버스를 갖고 연산하는 구조
  > ![하버드 구조](/assets/img/%ED%95%98%EB%B2%84%EB%93%9C%EA%B5%AC%EC%A1%B0.png)
- 명령어와 데이터를 동시에 CPU에 적재할수있어 폰 노이만 구조보다 속도가 빠르지만 구현하기 위해 회로설계가 복잡

### ***CPU***

- 명령어를 해석하여 실행하는 장치
- **Register**
  - CPU내에 데이터를 임시로 보관
  - 실제 CPU에서 모든 연산은 레지스터가 저장한 값을 가지고 수행
    - 즉 ALU, Control Unit은 Register값 밖에 모름
  > ![하버드 구조](/assets/img/register.png)

- **ALU<sup></sup>**
  - 데이터의 덧셈, 뺄셈, 곱셈, 나눗셈 같은 산술연산과 AND, OR 같은 논리연산을 수행

- **Control Unit**
  -<span style="color:blue">명령어</span>를 해석하고 해석 결과에 따라 작업을 지시 

### ***Memory<sup>주로 RAM</sup>***

- 명령어와 필요한 데이터들을 저장
- ***메모리 계층구조***
  - 메모리와 CPU의 속도차이를 완화하기 위해 메모리를 계층적으로 배치한 구조
    > ![메모리 계층구조](/assets/img/%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B3%84%EC%B8%B5%EA%B5%AC%EC%A1%B0.png)
  - 메모리와 CPU사이에 빠른 속도의 캐시메모리를 두어 앞으로 사용할 것 같은 데이터 데이터의 시간 지역성과 공간 지역성을 통해 미리 저장
    - 시간 지역성
      - 이전에 사용했던 데이터를 이후에 다시 사용할 가능성이 높음
    - 공간 지역성
      - 근처 데이터를 사용하면 이후에 근처 데이터도 사용할 가능성이 높음
  - CPU에서 메모리에 접근할 때 캐시 메모리를 먼저 확인하고 있으면 그 값을 사용<sup>cache hit</sup>하고, 없을때만 메모리에 직접접근<sup>cache miss</sup>
    > ![캐시](/assets/img/%EC%BA%90%EC%8B%9C.png)

### ***BUS***
- 메모리에서 연산에 필요한 데이터와 명령어를 CPU로 옮기고 그 연산결과를 다시 다른 메모리에 저장할 수 있는 일종의 데이터 통신 링크

# 컴퓨터 시스템/프로그램의 작동원리

### CPU의 명령어 처리 과정

- Fetch<sup>가져와서</sup>! → Decode<sup>해석하고</sup>! → Execute<sup>실행한다</sup>!
- Control Unit을 통해 메모리에서 PC<sup>Program Counter</sup>위치의 명령어<sup>기계어 코드</sup>를 읽고 연산할 값을 레지스터에 적재한 후 해석하고 ALU를 통해 명령어를 수행.
>![cpu 구동 얘시](/assets/img/cpu%EA%B5%AC%EB%8F%99%20%EC%96%98%EC%8B%9C.png)
- Fetch이후 PC가 1 증가하여 다음 명령어를 가르킨다.

### Polling과 Interrupt

- CPU가 일을 하는 중에 외부로 들어온 작업요청을 처리하는 방법
- 키보드 조작, 네트워크 패킷수신, USB연결 확인 요청, 에러 발생 이벤트 등

- **Polling**
  - CPU가 직접 외부 상태를 주기적으로 감시하다가 요청이 발생하면 처리하는 방식
  - 검사하는 주기에 따라 메세지 응답속도가 증가하지만 CPU가 처리해야하는 추가적인 태스크가 생기기 때문에 성능문제 발생
- **Interrupt**
  - 외부 장치가 CPU에 직접 요청을 하면 CPU가 처리하는 방식 
  - 인터럽트가 발생 시 PC가 변경되어 ISR<sup>Interrupt Service Routine</sup>을 실행
  - ISR이 끝나면 이전에 처리하던 명령어 주소로 PC가 복구된다

  - **Interrupt 순서**
    - I/O 장치 가 CPU에서 서비스를 원할 때 인터럽트 요청로 벡터 번호를 CPU에 전송
      - vector : 각 인터럽트 서비스 루틴의 시작 주소 테이블
    - CPU는 I/O 장치의 요청을 처리할 준비가 되었을 때 인터럽트 승인 신호를 나타낸다
      > ![인터럽트](/assets/img/interrupt.png)
  
  - **Interrupt 종류**
    - 하드웨어 인터럽트
    - 소프트웨어 인터럽트

# 병렬처리
- 동시에 여러개의 명령어을 처리하여 작업의 능률을 올리는 방식