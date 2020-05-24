---
layout: post
title:  "[Issue]Script Shell file with colab: th command not found"
date:   2020-05-21
desc: "Step by steps: issue and googling "
keywords: "colab, sh ,scriptshell, terminal"
categories: [research]
tags: [colab, sh, scriptshell]
icon: icon-html
---

<br>

`데이터분석캡스톤디자인` 에서, 모델 학습을 위해 `colab` 에서 `CharCNN`을 실행하던 도중 아래와 같은 오류를 발견하였따.



<br>

```
!sh run_models.sh
```



```
run_models.sh: 9: run_models.sh: th: not found
run_models.sh: 10: run_models.sh: -use_words: not found
run_models.sh: 12: run_models.sh: th: not found
run_models.sh: 13: run_models.sh: -use_words: not found
run_models.sh: 15: run_models.sh: th: not found
run_models.sh: 16: run_models.sh: -use_words: not found
run_models.sh: 18: run_models.sh: th: not found
run_models.sh: 19: run_models.sh: -char_vec_size: not found
run_models.sh: 24: run_models.sh: th: not found
run_models.sh: 25: run_models.sh: -use_words: not found
run_models.sh: 27: run_models.sh: th: not found
run_models.sh: 28: run_models.sh: -use_words: not found
run_models.sh: 30: run_models.sh: th: not found
run_models.sh: 31: run_models.sh: -use_words: not found
run_models.sh: 33: run_models.sh: th: not found
run_models.sh: 34: run_models.sh: -char_vec_size: not found
```



<br>

빠른 진행을 위해 구글링하여 이 issue를 해결해보도록 하쟈,,,



<br>

2020.05.24
----------


구글링 결과, sh는 torch 실행을 위한 명령어였다!

그래서 torch를 깐다고 깔았는데, 알고보니까 python에서의 pytorch가 아니라

lua에서의 torch 더라고 ㅎㅎ,,, 구글링을 더 해보고 설치도 해보고 뭐 이것저것 삽질해보니

결국은 colab 에서는 lua도 torch도 지원되지 않는 것 같다.

<br>


일단 나는 저 model 말고 다른 model을 사용하기로 했지만,

혹시 나와 같은 에러를 마주한 분들이라면 [torch를 pytorch로 변환하는 코드](https://github.com/clcarwin/convert_torch_to_pytorch)를 이용해도 좋을 거 같다!
