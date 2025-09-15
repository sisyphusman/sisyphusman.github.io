---
layout: post
title: "Pintos 64bit - User Programs"
category: os
---

Pintos 과제 2: User Program의 목표는 커널 위에서 사용자 프로그램을 실행할 수 있는 환경을 만드는 것

핵심 목표
- 프로세스 생성 및 관리
- ELF 실행 파일을 메모리에 로드하고, 사용자 스택을 준비한 뒤 실행합니다.
- 부모 프로세스가 자식 프로세스의 종료를 기다릴 수 있도록 process_wait() 구현이 필요합니다.
- fork, exec, exit, wait 같은 시스템 콜을 지원합니다.

시스템 콜 구현
- 사용자 프로그램이 printf, read, write 같은 함수 호출 시, 실제로는 시스템 콜 번호를 통해 커널의 syscall_handler로 진입합니다.
- 예: write(fd, buffer, size) → syscall(SYS_WRITE, fd, buffer, size)

주요 시스템 콜:
- halt : OS 종료
- exit : 프로세스 종료
- fork : 부모 프로세스를 복제
- exec : 새 프로그램 실행
- wait : 자식 프로세스 종료 대기
- read, write : 입출력
- open, close, filesize, seek, tell, create, remove 등 파일 관련

메모리와 스택 관리
- process_exec()에서 ELF 로더(load_segment)를 통해 코드/데이터를 페이지 단위로 로드
- 스택에는 argc, argv[], return address 등을 올려 C 프로그램이 정상 시작되도록 구성합니다

예외 처리 및 보안
- 사용자 영역의 잘못된 메모리 접근을 Page Fault (#PF) 핸들러에서 감지합니다
- get_user(), put_user() 같은 함수로 사용자 주소를 검증 후 접근합니다

라이브러리 지원
- entry.c : 사용자 프로그램의 시작점 _start → main(argc, argv) 호출
- console.c : printf, putchar 등을 시스템 콜 기반으로 구현
- stdlib.c, string.c : 사용자 라이브러리 (atoi, qsort, bsearch, strtok_r 등)

구조 개요
- kernel (threads, userprog, devices 등)
- process.c : 프로세스 생성, ELF 로딩, 스택 준비
- syscall.c : 시스템 콜 핸들러 (커널 진입점)
- exception.c : 잘못된 메모리 접근 처리
- userprog (라이브러리)
- syscall.c : 사용자 측 시스템 콜 wrapper
- entry.c : 프로그램 시작점 _start
- console.c, string.c, stdlib.c 등 표준 라이브러리

예시 실행 흐름
- init.c에서 커널이 부팅되고, run 'echo x' 같은 명령 실행
- process_create_initd("echo x") 호출 → 자식 프로세스 생성
- ELF 실행 파일(echo) 로드 → 사용자 스택 구성 (argv = {"echo","x"})
- _start() → main(argc, argv) 실행
- printf("x\n") → write() 시스템 콜 → 커널 → 콘솔 출력
- 프로그램 종료 시 exit(status) → 부모 process_wait()이 상태 회수


#### 테스트 스크립트 목록

```
tests/userprog/args-none
tests/userprog/args-single
tests/userprog/args-multiple
tests/userprog/args-many
tests/userprog/args-dbl-space
tests/userprog/halt
tests/userprog/exit
tests/userprog/create-normal
tests/userprog/create-empty
tests/userprog/create-null
tests/userprog/create-bad-ptr
tests/userprog/create-long
tests/userprog/create-exists
tests/userprog/create-bound
tests/userprog/open-normal
tests/userprog/open-missing
tests/userprog/open-boundary
tests/userprog/open-empty
tests/userprog/open-null
tests/userprog/open-bad-ptr
tests/userprog/open-twice
tests/userprog/close-normal
tests/userprog/close-twice
tests/userprog/close-bad-fd
tests/userprog/read-normal
tests/userprog/read-bad-ptr
tests/userprog/read-boundary
tests/userprog/read-zero
tests/userprog/read-stdout
tests/userprog/read-bad-fd
tests/userprog/write-normal
tests/userprog/write-bad-ptr
tests/userprog/write-boundary
tests/userprog/write-zero
tests/userprog/write-stdin
tests/userprog/write-bad-fd
tests/userprog/fork-once
tests/userprog/fork-multiple
tests/userprog/fork-recursive
tests/userprog/fork-read
tests/userprog/fork-close
tests/userprog/fork-boundary
tests/userprog/exec-once
tests/userprog/exec-arg
tests/userprog/exec-boundary
tests/userprog/exec-missing
tests/userprog/exec-bad-ptr
tests/userprog/exec-read
tests/userprog/wait-simple
tests/userprog/wait-twice
tests/userprog/wait-killed
tests/userprog/wait-bad-pid
tests/userprog/multi-recurse
tests/userprog/multi-child-fd
tests/userprog/rox-simple
tests/userprog/rox-child
tests/userprog/rox-multichild
tests/userprog/bad-read
tests/userprog/bad-write
tests/userprog/bad-read2
tests/userprog/bad-write2
tests/userprog/bad-jump
tests/userprog/bad-jump2
tests/filesys/base/lg-create
tests/filesys/base/lg-full
tests/filesys/base/lg-random
tests/filesys/base/lg-seq-block
tests/filesys/base/lg-seq-random
tests/filesys/base/sm-create
tests/filesys/base/sm-full
tests/filesys/base/sm-random
tests/filesys/base/sm-seq-block
tests/filesys/base/sm-seq-random
tests/filesys/base/syn-read
tests/filesys/base/syn-remove
tests/filesys/base/syn-write
tests/userprog/no-vm/multi-oom
tests/threads/alarm-single
tests/threads/alarm-multiple
tests/threads/alarm-simultaneous
tests/threads/alarm-priority
tests/threads/alarm-zero
tests/threads/alarm-negative
tests/threads/priority-change
tests/threads/priority-donate-one
tests/threads/priority-donate-multiple
tests/threads/priority-donate-multiple2
tests/threads/priority-donate-nest
tests/threads/priority-donate-sema
tests/threads/priority-donate-lower
tests/threads/priority-fifo
tests/threads/priority-preempt
tests/threads/priority-sema
tests/threads/priority-condvar
tests/threads/priority-donate-chain
```