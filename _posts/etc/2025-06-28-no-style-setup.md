---
layout: post
title: Jekyll과 No Style 테마 설정
category: etc
---

1. ruby 설치  
2. MSYS2 and MINGW development toolchain 옵션 선택  
3. gem install bundler  
4. gem install jekyll  
5. no style git을 zip으로 받은 후 원하는 위치 압축 풀기  
6. 해당 테마 폴더(Gemfile 위치)에서 bundle install
7. _config.yml에서 baseurl에 ""으로 설정  
8. Gemfile은 아래와 같이 설정  

    ```
    source "https://rubygems.org"
    # Hello! This is where you manage which Jekyll version is used to run.
    # When you want to use a different version, change it below, save the
    # file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
    #
    #     bundle exec jekyll serve
    #
    # This will help ensure the proper Jekyll version is running.
    # Happy Jekylling!
    #gem "jekyll", "~> 4.4.1"
    # This is the default theme for new Jekyll sites. You may change this to anything you like.
    #gem "minima", "~> 2.5"

    gem "no-style-please"

    # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
    # uncomment the line below. To upgrade, run `bundle update github-pages`.
    gem "github-pages", group: :jekyll_plugins

    # Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
    # and associated library.
    platforms :mingw, :x64_mingw, :mswin, :jruby do
    gem "tzinfo", ">= 1", "< 3"
    gem "tzinfo-data"
    end

    # Performance-booster for watching directories on Windows
    gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]

    # Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
    # do not have a Java counterpart.
    gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
    ``` 
&nbsp;

9. 터미널에서 bundle exec jekyll serve 입력 후 http://127.0.0.1:4000/ 에서 로컬 테스트  
10. 자신의 git repository에 push 후 이름을 닉네임.github.io로 설정  
11. 리포지토리 설정에서 Use your Github Pages website 체크  
12. 현재 no style 테마의 head.html 설정은 아래와 같다 (수학 수식 사용하려면 아래에 KaTeX 추가)

&nbsp;

```html

<head>
  <meta charset="utf-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="google-site-verification" content="Vph6V9TKkxC6bSim3cV6PTrIESWqZGR3rGAHZo3Tj5Q" />
  <meta name="naver-site-verification" content="37e703f14441d8deba6008e5635033e368c7074b" />
  
  <title>
  </title>

  <link rel="shortcut icon" type="image/x-icon" href="{{ site.favicon | relative_url }}" />
  <link rel="stylesheet" href="{{ "/assets/css/main.css" | relative_url }}" />
	
  <!-- 기본 KaTeX CSS -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.css" integrity="sha384-OH8qNTHoMMVNVcKdKewlipV4SErXqccxxlg6HC9Cwjr5oZu2AdBej1TndeCirael" crossorigin="anonymous">
  
  <!-- KaTeX JavaScript (렌더링용) -->
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" integrity="..." crossorigin="anonymous"></script>
  
  <!-- 자동 수식 감지 및 렌더링 -->
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" integrity="..." crossorigin="anonymous"></script>
  
  <!-- KaTeX 자동 렌더링 초기화 코드 -->
  <script>
    document.addEventListener("DOMContentLoaded", function () {
      renderMathInElement(document.body, {
        delimiters: [
          { left: "$$", right: "$$", display: true },
          { left: "\\[", right: "\\]", display: true },
          { left: "$", right: "$", display: false },
          { left: "\\(", right: "\\)", display: false }
        ]
      });
    });
  </script>
  
  <!-- 코드 블록 (예: ```python) 문법 하이라이팅 스타일을 적용 -->
  <link rel="stylesheet" href="{{ "/assets/css/syntax.css" | relative_url }}" />
  
  <!-- Google Fonts API 서버와 미리 연결하여 폰트 로딩 속도 향상 -->
  <link rel="preconnect" href="https://fonts.googleapis.com">

  <!-- 실제 폰트 파일(woff2 등)이 위치한 서버에 미리 연결, CORS 포함 -->
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap" rel="stylesheet">

</head>

```