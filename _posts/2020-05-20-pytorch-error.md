---
layout: post
title:  "Google colab에서 luarocks 사용하기"
date:   2020-05-11
desc: "python pytorch"
keywords: "python, pytorch, DBMS"
categories: [Python]
tags: [Python, pytorch, CNN, charCNN]
icon: icon-html
---

<br>

이번 학기 `데이터 분석 캡스톤 디자인` 에서, 데이터 전처리를 끝내고

이후 모델 적용을 위해 [charCNN]("https://github.com/yoonkim/lstm-char-cnn") 을 이용하기로 했다.


이 모델은 pytorch 기반이고, 이후 nngraph와 luautf8의 load 가 필요했다.

현재 `Google colab notebook` 을 이용하고 있기에, pytorch는 내장되어 있었지만

```
$ luarocks install nngraph
$ luarocks install luautf8
```

의 과정이 필요했고, 이후 모델을 사용하기 위해서라도 `luarocks`라는 명령어가 필요한 상황.


```
    /bin/bash: luarocks: command not found
```


그러나 colab에는 luarocks가 없었고, 이 문제를 해결해보기로 했다.
