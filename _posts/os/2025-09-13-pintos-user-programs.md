---
layout: post
title: "Pintos 64bit - User Programs"
category: os
---

Pintos 과제 2: User Program의 목표는 커널 위에서 사용자 프로그램을 실행할 수 있는 환경을 만드는 것

&nbsp;

명령어
- pintos --fs-disk=10 -p tests/userprog/args-single:args-single -- -q -f run 'args-single onearg'
- pintos --gdb --fs-disk=10 -p tests/userprog/args-single:args-single -- -q -f run 'args-single onearg'

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