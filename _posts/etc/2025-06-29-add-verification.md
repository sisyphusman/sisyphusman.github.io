---
layout: post
title: 구글과 네이버에 검색 등록하기
category: etc
---

1. robots.txt를 아래와 같이 추가한 뒤 리포지토리에 올린다
    ```
    User-agent: *
    Allow: /

    Sitemap: https://닉네임.github.io/sitemap.xml
    ```

2. 구글은 URL 접두어로 사이트 주소를 추가한다
3. 구글과 네이버가 사이트 소유권을 확인하기 위한 메타 주소를 _includes/head.html에 복붙한다
4. 소유권이 확인되었으면 _config.yml에 아래 플러그인을 추가한다

    ```
    plugins:
    ...
    - jekyll-sitemap
    - jekyll-seo-tag
    ...
    ```
5. 구글과 네이버에 sitemap.xml의 URL을 제출한다