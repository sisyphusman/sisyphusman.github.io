---
layout: post
title: Git & GitHub CLI 완전 기초 입문 정리
category: etc
---

<!--문제 풀고 깃에 올림 리뷰어한테 pr 리퀘스트 -->

# Git 기본
- Git은 코드의 버전을 관리하는 도구 코드의 변화 이력을 추적하고, 협업 중 충돌을 최소화하며 효율적으로 작업할 수 있도록 도와줌 

- 버전 관리를 위해서 스테이징과 로컬 저장소를 나눔  
- 작업 디렉토리(Working Directory) -> 스테이징 영역(Staging Area) -> 로컬 저장소(Local Repository)  
    - 작업 디렉토리 (Working Directory): 실제 파일을 편집하고 수정하는 공간
    - 스테이징 영역 (Staging Area): 커밋할 파일을 임시로 담아두는 공간 (git add 명령어로 이동)
    - 로컬 저장소 (Local Repository): 커밋된 변경 이력이 저장되는 공간 (git commit 명령어로 이동)

- 원격 저장소(Remote Repository)와 로컬 저장소의 변경사항을 push/pull 명령어로 동기화

- 커밋 히스토리도 저장이 됨

&nbsp;

# Branch

- 독립적으로 기능을 개발하거나 수정을 할 수 있는 분기된 작업 공간

&nbsp;

# Pull Request(PR)

- 작업한 브랜치의 변경사항을 main 브랜치에 반영해달라고 요청하는 과정

&nbsp;

# Github

- 코드 저장소를 인터넷에 올려 협업하고 관리할 수 있는 플랫폼

1. Github의 저장소  
    Public: 누구나 접근 가능

    Private: 초대받은 사용자만 접근 가능 (2단계 인증 or Personal Access Token 필요)

2. [GitHub CLI 설치](https://cli.github.com/) (Github용 명령어 도구)
    - GitHub CLI를 통해 로그인할 때 Personal Access Token(PAT)을 사용하거나 웹 브라우저 인증 방식 중 선택할 수 있음

3. 터미널 창에 **gh auth login** 입력 후 (Repo 마스터의 가입 메일 승인 이후)  
    - Github.com 선택 -> Https -> Authenticate Git with your GitHub credentials? :   
    Yes -> Login with a web browser
    - 밑에 주소 뜨면 접속 후 터미널에 있는 코드를 복사 붙여넣기 한다

4. Private 저장소를 Git Clone을 할 수 있어짐 (예: git clone 리뷰용 private 저장소)

5. branch(Exercise/아이디 or ...)를 새로 만든 후 파일을 추가 및 변경함

6. git add . 해서 변경 파일을 스테이징 영역에 올림 -> git commit -m "메모할 내용"하여 커밋하여 로컬 저장소로 이동

7. git push -u origin 브랜치명

8. GitHub에서 'Create Pull Request' 버튼을 클릭한 후, 제목과 설명을 작성하고 리뷰어를 지정해 PR을 생성

9. PR을 승인하면 작성자 및 리뷰어가 main 브랜치로 병합(merge)

10. 자신의 브랜치를 삭제합니다

&nbsp;

# GitHub CLI(Command Line Interface)

GitHub CLI(gh)는 GitHub에서 제공하는 공식 명령줄 도구로, 웹 사이트에 접속하지 않고  
터미널에서 다양한 작업을 수행할 수 있게 해줌


- 이슈(issue) 관리

- 풀 리퀘스트(PR) 생성/확인

- 리포지토리 관리

- GitHub Actions 실행 확인

- 포크, 클론, 브랜치 작업 등

즉, **GitHub의 대부분의 기능을 명령어 한 줄로 조작할 수 있게 해주는 도구**