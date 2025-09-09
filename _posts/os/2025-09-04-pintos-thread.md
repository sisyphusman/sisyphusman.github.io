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
- [PintOS 빌드](#pintos-빌드)

&nbsp;

# 설명서
- https://pkuflyingpig.gitbook.io/pintos
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

&nbsp;

#### PintOS 빌드 
 
   - 전체 테스트    
      pintos/threads/ make check  
   
   - 부분 테스트   
      /threads/build/에서 pintos -- -q run alarm-multiple  
      -> 에러 여부만 알 수 있음  
      pintos/threads/build$ make tests/threads/priority-sema.result  
      -> 이렇게 하면 부분 테스트로 pass/fail까지 알수 있음  

&nbsp;

#### thread 테스트 스크립트 목록

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

&nbsp;

#### main.c

1. bss_init()
  - 커널의 BSS 섹션(초기값 0인 전역/정적 변수 영역)을 0으로 클리어합니다
2. argv = read_command_line()
  - 부트로더가 넘긴 커맨드라인 문자열을 읽어 공백 단위로 토큰화합니다
3. argv = parse_options(argv)
  - 위에서 분해한 토큰들을 파싱해 전역 옵션 플래그를 설정합니다
4. thread_init()
  - 현재 실행 중인 컨텍스트를 “커널 스레드”로 구성하고, 스케줄러의 준비 리스트(ready list), thread 구조체 풀, TCB 초기화, 우선순위/타이머 틱 계수 등 스레드 서브시스템의 기초 자료구조를 만듭니다
5. console_init()
  - 콘솔(스크린/시리얼) 출력 경합을 막기 위한 콘솔 락을 설정하고 I/O 경로를 준비합니다
6. mem_end = palloc_init()
  - 페이지 할당자 초기화. 물리 메모리를 페이지 단위의 프리 비트맵/프리 리스트로 구성해 커널이 “물리 페이지”를 얻고 반납할 수 있게 합니다
7. malloc_init()
  - 커널 힙(가변 크기) 할당기 초기화. 내부적으로 페이지 할당자 위에 kmalloc 스타일의 소규모 동적 메모리를 제공합니다
8. paging_init(mem_end)  
  - 페이징/가상메모리의 커널 부분을 설정합니다. 커널 주소 공간을 페이지 테이블/페이지 맵 레벨로 구성하고, 커널 코드/데이터/장치 메모리를 매핑하며, CR3 로드 및 페이징 활성화(아키 의존)까지 마무리합니다. 위에서 받은 mem_end를 이용해 올바른 상한선으로 매핑·가드 영역을 잡습니다
(USERPROG)
9. intr_init()
  - IDT 구축 및 공통 트랩/예외 핸들러 등록. 마스크/플래그 초기화로 커널이 외부 인터럽트를 받을 준비를 합니다(아직 전역 인터럽트 enable 전).
10. timer_init()
  - PIT/APIC 타이머를 설정해 주기적 타이머 인터럽트를 발생시킵니다. 스케줄링 타임 슬라이스와 ticks 카운트의 기반이 됩니다
11. kbd_init()
  - 키보드 컨트롤러 초기화 및 IRQ 라우팅 설정
12. input_init()  
  - 키보드 등 입력 장치에서 올라오는 스캔코드를 고수준 입력 큐로 전달하는 공용 계층 초기화. 키보드 외 마우스/시리얼 입력을 하나의 링버퍼로 합치는 경우도 있습니다.
(USERPROG)
13. thread_start()
  - 스케줄러를 실제로 시작합니다 타이머 인터럽트를 허용(전역 인터럽트 enable)하고, idle 스레드와 초기 커널 스레드를 준비 리스트에 넣은 뒤 컨텍스트 스위칭을 시작합니다
14. serial_init_queue()
  - 시리얼 포트의 송수신 큐(링 버퍼) 초기화. 콘솔과 별개로 UART 기반 I/O를 버퍼링해 끊김 없는 입출력을 제공
15. timer_calibrate()  
  - 바쁜 대기 루프에 사용할 “루프/틱” 보정값을 측정합니다 실제 하드웨어 타이머 주기와 비교해 busy_wait() 계열 함수의 정밀도를 맞춥니다
(FILESYS)  
(VM)
16. run_actions(argv)
  - 커맨드라인으로 지정한 액션을 실행, 실제 과제 채점/테스트는 대부분 여기서 트리거
17. power_off()
  - 가상 머신 전원을 끕니다
18. thread_exit()
  - 마지막으로 현재 커널 스레드를 종료합니다

&nbsp;

#### thread.c

1. thread_init() — 스레드 서브시스템(ready 리스트, 초기 스레드 등) 준비
2. intr_init(), timer_init(), kbd_init(), input_init() … 하드웨어/인터럽트 관련 초기화
3. thread_start() — 스케줄러 시작 + 내부에서 intr_enable()로 인터럽트 활성화
4. 이후 테스트/액션에서 thread_create() 등으로 새 스레드들을 만들고 실행

&nbsp;

#### timer.c

1. 하드웨어 타이머 초기화 - timer_init()
2. 전역 틱 카운터 관리 - 부팅 이후로 몇 틱이 지났는지를 기록하는 전역 변수 ticks
3. 스레드 깨어나기 관리
4. 보조 기능 제공

&nbsp;

# 과제 1. Alarm Clock

   - timer_sleep()에 정의된 것을  다시 구현합니다 devices/timer.c   
   - 작동하는 구현이 제공되지만, "바쁜 대기" 상태입니다. 즉, 현재 시간을 확인하고 충분한 시간이 지날 때까지 호출을 반복합니다 .thread_yield()   
   - 바쁜 대기를 피하기 위해 다시 구현하세요  
   
   &nbsp;

   - include/threads/thread.h
     - 자신이 깨어나야 할 tick을 저장하는 wakeup_tick 변수 추가
     - void thread_sleep (int64_t ticks); 선언부 추가
     - void thread_awake (int64_t now_tick); 선언부 추가
   - threads/thread.c
     - thread_init() -> list_init (&sleep_list); (sleep_list 초기화)  

     ```c

      // 앞이 뒤보다 작으면 true
      bool thread_wakeup_cmp(const struct list_elem *a, const struct list_elem *b, void *aux UNUSED)
      {
        const struct thread *x = list_entry(a, struct thread, elem);
        const struct thread *y = list_entry(b, struct thread, elem);
        
        return x->wakeup_tick < y->wakeup_tick;

      }
      
      // wakeup_tick 기록 + sleep_list 삽입 + block 처리
      void thread_sleep (int64_t wakeup_tick)
      {
      	struct thread* cur = thread_current();		    							// 현재 스레드를 가져온다
      	enum intr_level old_level = intr_disable();									// 인터럽트 비활성화
      	cur->wakeup_tick = wakeup_tick;												      // 계산된 틱을 저장한다
      
      	ASSERT(!intr_context());                                    // 인터럽트 핸들러 안에서는 sleep 불가
      
      	list_insert_ordered(&sleep_list, &(cur->elem), thread_wakeup_cmp, NULL);	// sleep 리스트에 비교 함수에 따라 새로운 원소를 넣는다
      
      	thread_block();											                        // 현재 스레드를 BLOCKED 상태로 전환
      
      	intr_set_level(old_level);													        // 인터럽트 활성화
      }
            
      void thread_awake (int64_t now_tick)
      {
        enum intr_level old = intr_disable();                                                 // 인터럽트 비활성화 (리스트 조작 보호)
                          
        while (!list_empty(&sleep_list))                                                      // sleep_list는 wakeup_tick 오름차순으로 정렬되어 있음
        {
          struct thread *t = list_entry(list_front(&sleep_list), struct thread, elem);        // sleep_list 맨 앞 원소 확인

          if (t->wakeup_tick <= now_tick)
          {
            list_pop_front(&sleep_list);                                                      // 깨울 시간이 되었으면 리스트에서 제거 후 READY 상태로
            thread_unblock(t);													                                      // READY로
          }
          else
          {
            break;																                                            // 맨 앞 스레드조차 아직 깰 시간이 안 되었으면 그 뒤의 스레드들도 전부 아직임 → 루프 종료
          }
        }

        intr_set_level(old);                                                                  // 인터럽트 활성화
      }
      ```

  &nbsp;

   - devices/timer.c  

        ```c
       /* 대략 ticks 틱 동안 실행을 중단한다. */
        void
        timer_sleep (int64_t ticks) {
          if (ticks < 0)
          {
            return;
          }

          int64_t start = timer_ticks ();	/* 현재 시간(틱 수)을 저장 */

          ASSERT (intr_get_level () == INTR_ON);	/* 인터럽트가 켜져 있는지 확인 */

          // while (timer_elapsed (start) < ticks)
          // thread_yield ();	/* 원하는 시간이 지날 때까지 CPU를 양보 */
          
          int64_t wake = start + ticks;						            // 언제 깨워야 하는지 계산
          thread_sleep(wake);									                // wake_tick 값을 기록
        }

      /* 타이머 인터럽트 핸들러 */
      static void
      timer_interrupt (struct intr_frame *args UNUSED) {
        ticks++;
        thread_tick ();
        thread_awake(ticks);                                    // 매번 체크함
      }
      ```

# 과제 2. 