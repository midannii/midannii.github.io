---
layout: post
title:  "[Issue]내 지킬 블로그 업로드가 안된다 왜지 ..? "
date:   2020-09-29
desc: "Step by steps: issue and googling "
keywords: "jekyll, github, blog"
categories: [Research]
tags: [jekyll, github, blog]
icon: icon-html
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

/lib/jekyll/utils.rb:141:in 'initialize': Too many levels of symbolic links @ rb_sysopen - /Users/midan/go/src/github.com/golangci/golangci-lint/test/testdata/symlink_loop/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg/pkg/subpkg (Errno::ELOOP)
```


이라는 에러가 떠버린것,, 근데 저 위에 로그도 엄청 많았다,,, 대환장
