---
title:  "What is Operating System"
excerpt: "Operating System Summaty"

categories:
  - Operating System

tags:
  - [Computer Science, Operating System]

toc: true
toc_sticky: true
layout: post
use_math: true
 
date: 2021-09-02
last_modified_at: 2022-09-02
---

# 운영체제란?

- <span style="color:gold">컴퓨터라는 하드웨어를 잘쓰게 할수 있도록 하는 소프트웨어</span>
- 전원을 켜면 가장 먼저 실행되는 소프트웨어(BIOS에서 해당 OS가 있는 위치를 실행)

### 운영체제의 핵심 기능
- 프로세스 관리
- 메모리 관리
- 파일 시스템 관리
- 입출력 관리
- 프로세스간 통신 관리

### 역할과 목표
- 역할
  - 컴퓨터의 <span style="color:orange">자원을 독점적으로 관리</span>하는 특별한 소프트웨어
  - 컴퓨터의 <span style="color:orange">자원을 추상화</span> 하고 사용 <span style="color:orange">인터페이스를 제공</span>한다.
- 목표
  - <span style="color:skyblue">효율적</span>이고(Eifficiency), <span style="color:skyblue">안정적</span>이면서(Robustness, Security), <span style="color:skyblue">확장</span>도 잘되고(Scalability, Extensibility, Portability), <span style="color:skyblue">편리하게</span>(Usability, Interactivity), <span style="color:gold">자원을 관리하고 제공</span>

# 운영체제의 구조

### 커널 (좁은의미의 운영체제)
- 프로세스 관리, 메모리 관리, 저장장치 관리등의 운영체제의 핵심적인 기능을 모아둔것
- 응용 소프트웨어는 커널에 요청을 하여 하드웨어 자원을 이용하게 됨
- 보안을 위해 user mode, kernel모드를 분리하여 커널을 사용할때만 잠깐 사용
- system call을 호출하면 커널이 입력값, 권한 검증을 하여 커널을 보호함

### 인터페이스
- 커널에 (응용 소프트웨어의 요쳥)명령을 전달하고 실행결과 반환
- API(Application Programming Interface)로 인터페이스를 호출하면 인터페이스가 System call로 커널에 연결하게 된다.

### 디바이스 드라이버
- 커널과 하드웨어 사이의 인터페이스
- 하드웨어 입출력신호에 대한 약속을 정의

# 가상머신
- 운영체제와 응용프로그램 사이에서 작동
- 운영체제간 이식성을 높여줌
- API 호환 레이어라고 볼 수 있음