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
6. _config.yml에서 baseurl에 ""으로 설정  
7. Gemfile은 아래와 같이 설정  

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

8. 터미널에서 bundle exec jekyll serve 입력 후 http://127.0.0.1:4000/ 에서 로컬 테스트  
9. 자신의 git repository에 push 후 이름을 닉네임.github.io로 설정  
10. 리포지토리 설정에서 Use your Github Pages website 체크  