---
layout: post
title:  "Google colab에서 luarocks 사용하기"
date:   2020-05-20
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


<br>

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

<br>


원래 맥북에서 `luarocks`를 설치할 때에는



```
$ brew update
$ brew install luarocks

```

의 과정을 거치지만, colab에서는 brew도 없었다.




<br>



[구글링]("https://stackoverflow.com/questions/51030306/how-could-i-install-torch-on-google-colab-if-it-does-not-exist-the-file-bashr")의 방법을 시도해봤지만 큰 진전은 없었다.



<br>


혹시나 해서 아래와 같이 import 해봤는데 알고보니 되어서 약간 허무했다 ,,
앞으로는 미리미리 확인해보고 구글링하기로 'ㅅ'

+ 추가적으로 colab에서 지원하는 명령어? 가상환경?을 공부해서 아래에 정리해두었다.



<br>

<br>


#### 결론

- torch 필요한 모듈들이 입력되어 있었어서 => 앞으로 꼭 존재 여부 확인하고 삽질 시작하자
- colab에서 지원하는 명령어 종류가 궁금해졌다. 아마도 jupyter와 같겠지?
  - 이번 기회에 구글링해보고, 직접 update 해봤다.



가능 |`pip`,  `apt-get`
불가능 | `conda`, `brew`, `luarocks`
