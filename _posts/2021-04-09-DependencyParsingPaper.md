---
layout: post
title:  "[Survey/NLP/DP] A Survey of Unsupervised Dependency Parsing "
date:   2021-04-09
categories: PaperReview
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
- Dependency Model with Valence (DMV)
    - sentence와 parse tree를 동시에 생성함
    - dependency tree의 구조를 몰라도

    ![fig]([https://slidetodoc.com/presentation_image/20deaddd91e9bfe1f1880810a318c3c8/image-59.jpg](https://slidetodoc.com/presentation_image/20deaddd91e9bfe1f1880810a318c3c8/image-59.jpg))

3.1.2 Inference

- z∗ = arg max P(x,z;Θ)
    - z∈Z (x)

(13.1.3 Learning Objective

- log marginal likelihood
    - P(x;Θ) = ∑ P(x,z;Θ)

3.1.4 Learning Algorithm

- Expectation-Maximization (EM) algorithm

![fig]([https://i.stack.imgur.com/TVluI.png](https://i.stack.imgur.com/TVluI.png))

3.1.5 Pros and Cons

- EM 알고리즘의 활용 및 응용으로 쉽게 training 가능
- limited expressive power; because of the independence assumptions they make.

3.2 Discriminative Approaches

- 두가지 특징적인 Approaches; `Autoencoder-Based` ,  `Variational Autoencoder-Based`

3.2.1 Autoencoder-Based Approaches

![fig]([https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCfOU0%2FbtqzpGVI7EK%2FxsIldYG4293qA3R9kRE1xK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCfOU0%2FbtqzpGVI7EK%2FxsIldYG4293qA3R9kRE1xK%2Fimg.png))

- 출처; [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCfOU0%2FbtqzpGVI7EK%2FxsIldYG4293qA3R9kRE1xK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCfOU0%2FbtqzpGVI7EK%2FxsIldYG4293qA3R9kRE1xK%2Fimg.png)

![fig]([https://d3i71xaburhd42.cloudfront.net/a1733d564c3283199aa23cf68fdf9e944f0c5359/2-Figure1-1.png](https://d3i71xaburhd42.cloudfront.net/a1733d564c3283199aa23cf68fdf9e944f0c5359/2-Figure1-1.png))

- Autoencoder-based approaches aim to map a sentence to an intermediate representation (encoding) and then reconstruct the observed sentence from the intermediate representation (decoding).
- `CRFAE`(conditional random field autoencoder framework)
    - The encoder is a first-order graph-based discriminative dependency parser mapping an input sentence to the space of dependency tree
    - The decoder independently generates each token of the reconstructed sentence conditioned on the head of the token specified by the dependency tree.
    - encoding and decoding probabilities can be factorized by dependency arcs.
- `D-NDMV` Deterministic Variant
    - The encoder is an LSTM summarizing the sentence with a continuous vector s, while the decoder models the joint probability of the sentence and the dependency tree.
    - The decoder is a generative neural DMV that generates the sentence and its parse simulta- neously, and its parameters are computed based on the continuous vector s.
    - The reconstruction loss is optimized using the EM algorithm.

3.2.2 Variational Autoencoder-Based Approaches

Evidence Lower Bound (ELBO)를 maximize하도록 학습

- model (Li et al., 2019)
    - Recurrent Neural Network Grammars (RNNG)를 modify하여 variational autoencoder의 encoder와 decoder로 사용
    - RNNG가 overfitting에 위험(expressive)함을 보완하기 위해 posterior regularization을 사용
- `D-NDMV` Variational Variant
    -
- model (Corro and Titov, 2018)
    - Gumbel random perturbation를 사용함;
        - variational autoencoder가 gradient 계산에  Monte Carlo sampling을 해야하기 때문;
        - dependency dtree의 sampling 복잡도가 높기 때문

3.2.3 Other Discriminative Approaches

3.2.4 Pros and Cons

- discriminative model은 traditional 한 generative approach 보다 더 inference 계산이 복잡
- discriminative model은 traditional 한 generative approach 보다 더 feature 잘 얻음, expressie함

**4 Recent Trends**

**4.1 Combined Approaches**

- `generative model` 과 `discriminative model` 의 장점을 모두 살리고자 함
    - e.g. generative LC-DMV + discriminative Convex MST

4.2 Neural Parameterization

- generative model + Neural network (rule components의 vector representation을 input으로 함)
- '' + distriminative model (token과 rule 사이의 correlation 을 capture)

4.3 Lexicalization

- 처음엔 pos-tagging만 이용 → lexicalization 일부 + pos-tagging 일부 → 전체  lexicalization
    - full  lexicalization에는 data 양 등의 제약 존재
- lexicalized approach ; `word embedding` 가능함
    - overfitting의 위험이 있지만, invertible neural projection 으로 이를 피하려는 시도 존재

4.4 Big Data

- 데이터셋 확장해감 ; WSJ10 corpus → ... → 180k sentences

4.5 Unsupervised Multilingual Parsing

5. Benchmarking on the WSJ Corpus

- 대부분의 unsupervised D.P 에서는 Wall Street Journal (WSJ) corpus으로 성능을 평가
    - 아래와 같은 성능을 현재 보유하고 있음

![fig]([https://d3i71xaburhd42.cloudfront.net/8b79927546a450710e148c72f43eac18ff9910eb/7-Table1-1.png](https://d3i71xaburhd42.cloudfront.net/8b79927546a450710e148c72f43eac18ff9910eb/7-Table1-1.png))

**6 Future Directions**

**6.1 Utilization of Syntactic Information in Pretrained Language Modeling**

6.2 Inspiration for Other Tasks

6.3 Interpretability

7 Conclusion

<br>
