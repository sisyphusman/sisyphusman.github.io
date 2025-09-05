---
layout: post
title: PintOS 64bit - Thread
category: os
---

# Index
- [설명서](#설명서)
- [GDB 디버깅](#gdb-디버깅)
- [vscode에서 디버깅](#vscode에서-디버깅)
- [GDB 자주 쓰는 명령어 모음](#gdb-자주-쓰는-명령어-모음)
- [thread 테스트 스크립트 목록](#thread-테스트-스크립트-목록)

# 설명서
- https://casys-kaist.github.io/pintos-kaist/introduction/getting_started.html
- source ./activate 에서 source 는 쉘 내장 명령어로 주어진 스크립트 파일을 현재 쉘 환경에서 실행

&nbsp;

전체 테스트 = make check  
개별 테스트 = /threads/build에서 pintos -- -q run alarm-multiple

&nbsp;

도커 실행시 윈도우에서 CRLF 문제 해결법
- sudo apt-get update
- sudo apt-get install dos2unix
- sudo dos2unix 파일명

&nbsp;

# GDB 디버깅

1. sudo apt install gdb
2. pintos_22.04_lab_docker/pintos/threads/build 이동
3. pintos \-\-gdb \-\- \-q run alarm-multiple
4. 새로운 터미널 실행 후 pintos_22.04_lab_docker/pintos/threads/build로 이동
5. gdb kernel.o
6. (gdb 내부에서) target remote localhost:1234
7. break thread.c:109 (원하는 라인)
8. (gdb 내부에서 gdb 명령어 입력) c, n, s ...

&nbsp;

# vscode에서 디버깅

1. vscode 확장에서 Native Debug 설치 (GDB 설치 필요)
2. .vscode/launch.json 파일 생성
```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Pintos Debug (cppdbg)",
      "type": "cppdbg",
      "request": "launch",
      "program": "${workspaceFolder}/pintos/threads/build/kernel.o",   // 심볼
      "cwd": "${workspaceFolder}/pintos/threads",
      "MIMode": "gdb",
      "miDebuggerPath": "/usr/bin/gdb",
      "miDebuggerServerAddress": "localhost:1234",
      "externalConsole": false,
      "stopAtEntry": true,
      "setupCommands": [
        { "text": "-gdb-set disassembly-flavor intel" },
        { "text": "set pagination off" },
        { "text": "set breakpoint pending on" },
        // 소스 경로가 다를 수 있으면 아래 줄을 내 경로에 맞게 추가
        // { "text": "set substitute-path /컨테이너/빌드/경로 ${workspaceFolder}" }
      ]
    }
  ]
}
```
3. /build로 이동 후 pintos \-\-gdb \-\- \-q run alarm-multiple (db 스크립트 실행)
4. vscode 실행 후 thread.c 에서 브레이킹 포인터 생성, F5로 실행

&nbsp;

#### GDB 자주 쓰는 명령어 모음

- 기본

   | 명령어 | 설명 |
   |--------|------|
   | run (또는 r) | 프로그램 실행 시작 |
   | continue (또는 c) | 중단된 프로그램 계속 실행 |
   | start | 프로그램을 main 함수 첫 줄에서 멈춘 상태로 실행 |
   | quit (또는 q) | GDB 종료 |

- 브레이크포인트

   | 명령어 | 설명 |
   |--------|------|
   | break main (또는 b main) | main 함수에 브레이크포인트 설정 |
   | break init.c:42 | init.c의 42번째 줄에 브레이크포인트 설정 |
   | info breakpoints (또는 i b) | 현재 설정된 브레이크포인트 목록 보기 |
   | delete 1 | 브레이크포인트 번호 1 삭제 |
   | disable 1 / enable 1 | 브레이크포인트 일시 비활성화 / 다시 활성화 |

- 코드 실행 제어

   | 명령어 | 설명 |
   |--------|------|
   | step (또는 s) | 한 줄 실행 (함수 내부로 진입) |
   | next (또는 n) | 한 줄 실행 (함수 호출은 건너뜀) |
   | finish | 현재 함수가 끝날 때까지 실행 |
   | until | 현재 위치 이후 같은 함수의 다음 줄까지 실행 |

- 상태 확인

   | 명령어 | 설명 |
   |--------|------|
   | print var (또는 p var) | 변수 값 출력 |
   | display var | 변수 값을 실행할 때마다 자동 출력 |
   | info locals | 현재 함수의 지역 변수 값 출력 |
   | backtrace (또는 bt) | 함수 호출 스택 확인 |
   | frame 2 | 스택 프레임 번호 2로 이동 |
   | info registers | CPU 레지스터 상태 확인 |

- 메모리와 어셈블리 조사

   | 명령어 | 설명 |
   |--------|------|
   | x/10i $rip | 현재 RIP 근처 어셈블리 10줄 보기 |
   | x/16x addr | 주소 addr에서 16개의 메모리를 16진수로 보기 |
   | x/s addr | 주소 addr를 문자열로 출력 |
   | disassemble | 현재 함수의 어셈블리 코드 보기 |

- 기타 유용한 명령어

   | 명령어 | 설명 |
   |--------|------|
   | set disassembly-flavor intel | 어셈블리 출력 방식을 Intel 문법으로 변경 |
   | layout src | TUI 모드에서 소스 코드와 함께 실행 (터미널 UI) |
   | info threads | 스레드 목록 보기 |
   | thread 2 | 스레드 번호 2로 전환 |

# thread 테스트 스크립트 목록

alarm-single
alarm-multiple
alarm-simultaneous
alarm-priority
alarm-zero
alarm-negative
priority-change
priority-donate-one
priority-donate-multiple
priority-donate-multiple2
priority-donate-nest
priority-donate-sema
priority-donate-lower
priority-fifo
priority-preempt
priority-sema
priority-condvar
priority-donate-chain
mlfqs/mlfqs-load-1
mlfqs/mlfqs-load-60
mlfqs/mlfqs-load-avg
mlfqs/mlfqs-recent-1
mlfqs/mlfqs-fair-2
mlfqs/mlfqs-fair-20
mlfqs/mlfqs-nice-2
mlfqs/mlfqs-nice-10
mlfqs/mlfqs-block