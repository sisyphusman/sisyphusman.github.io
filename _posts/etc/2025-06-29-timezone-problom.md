---
layout: post
title: Github Pages에서 글이 안나오는 문제
category: etc
---

최근 빌드 시간(UTC): {{ 'now' | date: '%Y-%m-%d %H:%M:%S' }}

글의 날짜가 UTC보다 미래일 때 로컬에서는 잘 보이던 글이 github pages에서는 안 나온다

github action에서 빌드 로그를 보면 아래와 같다

``` Skipping: _posts/2025-06-29-add-verification.md has a future date ```

글의 시간이 미래인 경우 스킵이 된다

_config.yml에 아래와 같이 추가한다

``` timezone: Asia/Seoul ```

공백이 중요하므로 띄어쓰기를 주의한다