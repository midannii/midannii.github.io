---
layout: post
title:  "[NLP paper] A Survey of Unsupervised Dependency Parsing "
date:   2021-04-09
desc: " "
keywords: "DL, ML"
categories: [Deeplearning]
tags: [DL, ML, pytorch]
icon: icon-html
---

<br>

이번에 리뷰할 [논문](https://arxiv.org/abs/2010.01535)은

# A Survey of Unsupervised Dependency Parsing

이다. dependency parsing 중에서 Unsupervised의 연구 flow를 정리한 논문이라, 공부할 겸 읽어보았당 ~!



<br>

## abstract

unsupervised d.p는 correct parse tree에 대한 annotation이 없는 dependency parser를 학습하는 것으로,

어렵지만, 대부분의 텍스트 데이터에서 무궁하게 사용할 수 있기에 다양한 시도들이 존재해왔다.

1. introduction

- `supervised DP`의 경우, 새로운 언어나 새로운 domain에 대해 매번 새로운 tree를 만들어야 하는데, 이 과정이 expensie & time-consuming...
- annotation 적은 D.P 중에서 unsupervised DP가 제일 어렵지만, 제일 활용도가 높고, semi-supervised나 transfer에서의 basis로 사용됨

```bash
In this paper,
we conduct a survey of unsupervised dependency parsing research.

1. We introduce the definition and evaluation metrics of unsupervised dependency parsing, and discuss research areas related to it.
2. we present in detail two major classes of approaches to unsupervised dependency parsing: generative approaches and discriminative approaches.
3. we discuss important new techniques and setups of unsupervised dependency parsing that appear in recent years.
```

2. Background

2.1 Problem Definition

![fig]([https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQNAmYvYfyyXxgW-6jKpSNLRowjO1g8Ad38sw&usqp=CAU](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQNAmYvYfyyXxgW-6jKpSNLRowjO1g8Ad38sw&usqp=CAU))

위와 같은 그림에서, n(여기서는 10)개의 단어로 이루어진 `input sentenc x`  와 `dependency tree z`  에 대해,

각 단어 아래의 숫자를 바탕으로, 각 문장의 단어는 x1, ..., xn으로 표시되며, x0은 root word이다.

- unsupervised Dependency Parsing의 observation은, annotated sentence를 사용하지 않고 Dependency Parser를 만드는 것
- 2 evaluation metrics
    - directed dependency accuracy (DDA); 맞게 예측한 edge의 개수
    - undirected dependency accuracy (UDA); 맞게 예측한 edge의 개수(방향은 고려 x)

2.2. Related Areas

- Supervised Dependency Parsing
- Cross-Domain and Cross-Lingual Parsing
- Unsupervised Constituency Parsing
- Latent Tree Models with Downstream Tasks

**3. General Approaches**

**3.1 Generative Approaches**

**3.1.1 Models**

- traditional generative approach = grammer의 확률성(문장의 확률적인 구조?)에 기반 + context free assumption
    - context free assumption; the generation of a token is only dependent on its head token and is independent of anything else
    - but, 각 token을 만들 때 context , generation history 등에서는 이용불가능했음,,,
