---
layout: post
title:  "[Issue]Script Shell file with colab: th command not found"
date:   2020-05-21
desc: "Step by steps: issue and googling "
keywords: "colab, sh ,scriptshell, terminal"
categories: [Research]
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
