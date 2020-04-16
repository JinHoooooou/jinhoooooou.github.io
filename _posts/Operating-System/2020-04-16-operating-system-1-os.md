---
title: "운영 체제(Operating System)"
excerpt: "운영체제의 정의와 역할에 대해 Araboza."
classes: wide
categories:
 - OS
tags:
 - Operating-System
last_modified_at: 2020-04-16
---



# 운영 체제



### 1. 운영 체제(Operating System) 정의

* 컴퓨터 시스템을 구성하는 <u>하드웨어</u>와 <u>사용자 또는 응용 프로그램</u>의 **중간**에 위치하여 사용자들이 보다 쉽고 간편하게 컴퓨터 시스템을 이용할 수 있도록 환경을 제공하는 시스템 소프트웨어
  * 👉 하드웨어와 사용자간의 interface
  * 🙋‍♂️ 그럼 여기서 드는 생각, 중간에서 **하드웨어**랑 **사용자** 입장에서 어떤 기능을 제공할까? 👉 운영체제의 역할이 뭐지?



### 2. 운영 체제의 역할

1. 사용자 입장 (User view)
   * User interface 제공 - CUI, GUI
   * Program execution - 프로그램을 메모리에 load 하고 실행, 종료 할 수 있도록 제공
   * I/O - 키보드,마우스 등 입력과 / 모니터 프린터 등 출력 제공
   * File system - 파일 읽기, 쓰기, 생성, 삭제 제공
   * Communication - 프로세스간 통신
   * Error detection - 하드웨어, 입출력장치, 프로그램 등 오류검출 및 복구
   * 👉 사용자가 편리하게 컴퓨터 시스템을 사용할 수 있도록 지원
2. 시스템 입장 (System view)
   * 자원 할당 및 관리 - CPU 사이클, 메모리, 저장장치, 입출력장치 등 하드웨어 자원들을 관리한다.

* 🙋‍♂️ 하는일이 많다. 사용자 입장에서는 하드웨어에 대해 잘 모르더라도 컴퓨터를 쉽게 사용하도록 지원해주고, 시스템 입장에서는 하드웨어 자원들을 관리한다. 👉 결국 사용자가 컴퓨터 시스템을 편리하게 사용할 수 있도록 지원하는게 운영체제의 역할이다.
* 🙋‍♂️ 그렇다면 여기서 또 드는 생각들, 1. 사용자가 컴퓨터를 **잘, 편리**하게 사용하게 해준다고 했는데 그 기준이 뭘까? 2. 어떻게 하드웨어 자원들을 관리할까?  👉 1. 운영체제의 목적, 성능 2. 운영체제의 자원관리



### 3. 운영체제의 목적, 성능

1. 처리능력(Throughput) 향상 - 일정 시간 내에 시스템이 처리하는 일의 양 👉 당연히 처리하는 양이 많을수록 좋다.

2. 반환 시간(Turn around time) 단축 - 처리를 요구한 시간부터 처리가 완료될 때까지 걸린시간 👉 당연히 시간이 짧을수록 성능이 좋다.

3. 사용가능도(Availability) 향상 - 원하는 시간 내에 시스템을 얼마나 빨리 사용할 수 있는가 

4. 신뢰도(Reliability) 향상 - 컴퓨터 시스템이 주어진 환경에서 원하는 기능을 얼마나 정확하게 수행하는가

   

### 4. 운영체제의 자원관리

1. 프로세스 관리(Process) - 프로세스 스케줄링 및 동기화, 프로세스 생성과 제거, 시작과 정지, 메시지 전달 등 관리 👉 Process, Thread, CPU scheduling, synchronizaion, deadlock
2. 기억장치 관리 - 메모리 할당 및 회수 관리 👉 Main memory, virtual memory
3. 주변장치 관리 - 입 출력 장치 관리 👉 Secondary storage, I/O system
4. 파일 관리 - 파일의 생성과 삭제, 변경 등 관리 👉 File system interface, implementation





🙋‍♂️ 운영체제의 정의를 알아봤는데, 사실 OS하면 Linux, Window, Mac 등을 바로 떠올린다. 그래서 뭔가 애매모호하게 느껴질 수 있는데, 이 OS들을 통해 우리가 파일 생성도하고, 프로세스들 사이 통신도 가능하고 등등 컴퓨터를 사용하는데 도움을 준다고 생각하면 점점 마인드맵을 그릴 수 있을것 같다.

🙋‍♂️ [OS면접 단골 질문들](https://github.com/JinHoooooou/Interview_Question_for_Beginner/tree/master/OS)

