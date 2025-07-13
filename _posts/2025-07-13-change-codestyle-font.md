---
layout: post
title: jekyll에서 코드 색 변경과 폰트 변경
category: etc
---

#### Code Color  

1. https://spsarolkar.github.io/rouge-theme-preview/ 에서 원하는 스타일 이름을 가져온 후 아래 base16.monokai.dark 란을 바꿔준다  
    
    그리고 Gemfile이 있는 디렉토리에 쉘에 아래와 같이 적는다  
    ``rougify style base16.monokai.dark > assets/css/syntax.css``  

2. _includes/head.html에서 아래 소스를 추가한다  
``  <link rel="stylesheet" href="{{ "/assets/css/syntax.css" | relative_url }}" />``  

3. .gitignore에 *.css 등 무시된 파일이 있는지 확인한다


#### Body Font  

1. ![구글_폰트_사이트](/assets/images/google-font-site.jpg)  

2. https://fonts.google.com/selection/embed 사이트에서 원하는 폰트의 embed 소스를 가져온다  

3. _includes/head.html에서 아래 코드를 추가한다  

    ```html
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap" rel="stylesheet">
    ```

4. _sass/no-style-please.scss 에서 body의 font-family: 에서 폰트를 원하는 폰트로 바꾼다  
  ```html
  body {
    color: black;
    font-family: Noto Sans KR; 
  ```
