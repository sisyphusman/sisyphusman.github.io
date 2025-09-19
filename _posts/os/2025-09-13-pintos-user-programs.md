---
layout: post
title: "Pintos 64bit - User Programs"
category: os
---

Pintos 과제 2: User Program의 목표는 커널 위에서 사용자 프로그램을 실행할 수 있는 환경을 만드는 것

&nbsp;

명령어
```
pintos --fs-disk=10 -p tests/userprog/args-single:args-single -- -q -f run 'args-single onearg'

pintos --gdb --fs-disk=10 -p tests/userprog/args-single:args-single -- -q -f run 'args-single onearg'
```  

  - os.dsk cannot be temporal 에러 -> make를 안해서 dsk 파일이 없을때 생기는 오류  

&nbsp;

명령어 설명  

```
pintos --gdb --fs-disk=10 -p tests/userprog/create-exists:create-exists -- -q -f run create-exists
```
  - pintos --gdb : Pintos는 QEMU를 실행시키고 그 위에서 OS를 구동합니다. QEMU를 GDB stub 모드로 실행
  - --fs-disk=10 : 파일 시스템 디스크의 크기를 10MB로 설정
  - -p SRC:DEST 형식 : 
     - SRC = 호스트(리눅스) 쪽 경로: tests/userprog/create-exists
     - DEST = Pintos 가상 파일시스템 안의 파일 이름: create-exists
  - -q -f run create-exists
     - -q → 테스트 끝나면 Pintos 자동 종료
     - -f → 실행 시작 시 파일시스템 포맷
     - run create-exists
        - run = Pintos 커널이 지원하는 액션
        - create-exists = 앞서 -p로 올린 실행 파일을 가상 파일시스템에서 실행

&nbsp;

핵심 목표
- 프로세스 생성 및 관리
- ELF 실행 파일을 메모리에 로드하고, 사용자 스택을 준비한 뒤 실행합니다.
- 부모 프로세스가 자식 프로세스의 종료를 기다릴 수 있도록 process_wait() 구현이 필요합니다.
- fork, exec, exit, wait 같은 시스템 콜을 지원합니다.

&nbsp;

구현

1. file_name 문자열을 토큰으로 나누어 첫 번째 토큰만 filesys_open() 에 넘기도록 수정
  - 밖에서 파싱하고, load() 에는 실행파일 이름만 넘기는 게 정석