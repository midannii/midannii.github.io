---
layout: post
title:  "[NLP] Dependency Parser "
date:   2021-03-23
desc: " "
keywords: "DL, ML"
categories: [Python]
tags: [DL, ML, pytorch]
icon: icon-html
---


`Dependency parsing` 이란, 단어들의 관계를 통해 의미 구조를 파악하는 것이다.

![fig](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F99DD094F5CCF4AC41482F0)

언어에 상관없이 {subject, predicate, object}를 추출 문장의 의미 구조를 파악할 수 있다.



<br>


한국어의 경우, `Sejong Treebank`를 바탕으로 하는 선행 연구가 있었다.


![fig](https://image.slidesharecdn.com/spmrl11-slide-111006081330-phpapp02/95/statistical-dependency-parsing-in-korean-from-corpus-generation-to-automatic-parsing-5-728.jpg?cb=1317889111)


이처럼 형태소 tagging을 바탕으로 진행하며, `koalanlp`에서 간단한 예시를 테스트해볼 수 있다.


```python
from koalanlp.Util import initialize, finalize
from koalanlp import API
from koalanlp.proc import Parser

initialize(hnn='LATEST')

parser = Parser(API.HNN) # 또는 API.KKMA, API.ETRI
# ETRI 분석기의 경우 API 키를 필수적으로 전달해야 합니다. 예: Parser(API.ETRI, etri_key=API_KEY)
parsed = parser("이 문단을 분석합니다. 문단 구분은 자동으로 합니다.")
# 또는 parser.analyze(...), parser.invoke(...)
# 첫번째 문장의 의존구조를 출력합니다.
for dep in parsed[0].getDependencies():
    print(dep)
# 첫번째 문장의 구문구조 트리를 출력합니다.
print(parsed[0].getSyntaxTree().getTreeString())  
```



<br>


# Reference

- https://gnoej671.tistory.com/5

- https://nlp100.github.io/en/ch05.html

- https://www.slideshare.net/jchoi7s/statistical-dependency-parsing-in-korean-from-corpus-generation-to-automatic-parsing


<br>
