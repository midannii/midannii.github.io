---
layout: post
title:  "[Issue]내 지킬 블로그 업로드가 안된다 왜지 ..? [2탄] "
date:   2020-10-29
desc: "Step by steps: issue and googling "
keywords: "jekyll, github, blog"
categories: [Research]
tags: [jekyll, github, blog]
icon: icon-html
---


아오! 아래와 같은 시도들을 했는데 여전히 안되고

결론은 gemfile에서 호환되는 버전과 jekyll에서 호환되는 버전이 달라요



1. bundle update

update는 완전 성공적으로 되었음




2.jekyll serve .  


```
Traceback

(생략)

`check_for_activated_spec!': You have already activated jekyll-sass-converter 2.1.0, but your Gemfile requires jekyll-sass-converter 1.5.2. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)


```

그래서 Gemfiled을 수정해주니, 계속 아래와 같이 수정할 사항이 생겼고


```

You have already activated jekyll-sass-converter 2.1.0, but your Gemfile requires jekyll-sass-converter 1.5.2.

You have already activated jekyll-watch 2.2.1, but your Gemfile requires jekyll-watch 1.5.1.


You have already activated kramdown 2.3.0, but your Gemfile requires kramdown 1.17.0.

You have already activated mercenary 0.4.0, but your Gemfile requires mercenary 0.3.6.


 You have already activated rouge 3.23.0, but your Gemfile requires rouge 2.2.1.


You have already activated jekyll 4.1.1, but your Gemfile requires jekyll 3.6.3.

```



이걸 다 고쳤더니 이제는

```

Could not find 'jekyll-sass-converter' (~> 2.0) - did find: [jekyll-sass-converter-1.5.2] (Gem::MissingSpecVersionError)
```

가 뜨더라,,,


결론은 gemfile에서 필요한 `jekyll-sass-converter`의 버전은 2.0 이상인데, 지킬에서 사용하는 버전은 1.5.2이였고


둘 중 하나라도 지우면 `Coud not file` 에러가,


지우지 않으면 `You have already activated jekyll-sass-converter 2.1.0, but your Gemfile requires jekyll-sass-converter 1.5.2. `에러가 뜸 (대환장)




3. bundle exec jekyll serve.



이렇게 하면, 내 midannii.github.io 페이지는 unable to connect 떠서

local에서만 만들어진다 엉엉


<br>
