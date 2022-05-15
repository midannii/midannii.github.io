---
layout: post
title:  "[Issue]내 지킬 블로그 업로드가 안된다 왜지 ..? "
date:   2020-09-29
categories: research
---

`project.yml`, `skills.yml`, `careers.yml` 을 수정해도 반영이 안되더니

이제는 글도 업로드 안되는 대환장 상황 나타남,,, 아이고,,, 진짜 왜지 뭐지 왜그러지


<br>


일단 문제점,,,

```
$ bundle update                                                           
Could not locate Gemfile

```

아니 왜 갑자기 gemfile 없는거야..?

bundle 부터 차근차근 깔아주고

조금 더 뒤적거리다가 이걸 해봤는데



```
$ jekyll build --verbose --trace
.
.
(생략)
.
.
.
~~~~/lib/jekyll/utils.rb:141:in 'initialize': Too many levels of symbolic links @ rb_sysopen - ~~/github.com/golangci/golangci-lint/test/testdata/symlink_loop/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg (Errno::ELOOP)
```


이라는 에러가 떠버린것,, 근데 저 위에 로그도 엄청 많았다,,, 대환장

로그는 이렇게 네개가 반복적으로, 총 315개 있었음 아오


```
Traceback (most recent call last):
.
.
.
4: from ~~~/lib/ruby/gems/2.6.0/gems/jekyll-4.1.1/lib/jekyll/reader.rb:48:in `each'
3: from ~~/.rbenv/versions/2.6.4/lib/ruby/gems/2.6.0/gems/jekyll-4.1.1/lib/jekyll/reader.rb:52:in `block in read_directories'
2: from ~~/.rbenv/versions/2.6.4/lib/ruby/gems/2.6.0/gems/jekyll-4.1.1/lib/jekyll/utils.rb:141:in `has_yaml_header?'
1: from ~~/.rbenv/versions/2.6.4/lib/ruby/gems/2.6.0/gems/jekyll-4.1.1/lib/jekyll/utils.rb:141:in `open'
```


아 참고로 ~로 처리한 건 내 Path라서 가려버린 것


아니 근데 golanci/golanci-lint가 왜 내 깃허브 하위 디렉토리에 있는거야..?
짜증나서 golanci 폴더를 지워버림



이후 다시 실행했더니,


```
$ jekyll build --verbose --trace

Incremental build: disabled. Enable with --incremental
     Generating...
                   done in 4.462 seconds.
Auto-regeneration: enabled for '/Users/midan/go/src/github.com'
   Server address: http://127.0.0.1:4000
 Server running... press ctrl-c to stop.
       ** ERROR: directory is already being watched! **

       Directory: /Users/midan/go/src/github.com/alecthomas/gometalinter/_linters/src

       is already being watched through: /Users/midan/go/src/github.com/alecthomas/gometalinter/_linters/src

       MORE INFO: https://github.com/guard/listen/wiki/Duplicate-directory-errors


```

요런 에러가 엄청 많이 뜨는 것이다,,



<br>

일단은 어떤 에러가 나타나는지 기다리기로 하고,

`jekyll.rb`, `utils.rb` 파일을 열어봤당

사실 루비 파일이라서 자세히 해석은 못해봄,,,






<br>


돌고 돌아 결국 [이런 글](https://github.com/jekyll/jekyll/issues/8416)도 남겼고,,,

그 결과 [지킬 커뮤니티](https://talk.jekyllrb.com/) 회원가입까지 완료,,



<br>

그리고! [해당 카테고리에 bundler를 제대로 설치](https://gist.github.com/MichaelCurrin/5c8c45a86bcf53d7b49a7763c02943b1)하라는 글을 찾아 드뎌 성공,,,! (감격)



```
$ bundle config set --local path vendor/bundle              
$ bundle install    
```


감동 이제 드뎌 멀쩡한 블로그 획득 ~!
