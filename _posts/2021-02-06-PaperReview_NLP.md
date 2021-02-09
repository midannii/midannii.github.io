---
layout: post
title:  "[NLP paper] Text Classification Algorithms: A Survey + NLP 용어 정리 "
date:   2021-02-06
desc: " "
keywords: "DL, ML"
categories: [Deeplearning]
tags: [DL, ML, pytorch]
icon: icon-html
---


두번째로 정리해볼 [논문](https://arxiv.org/abs/1904.08067)은 2019년 상반기에 나온

# Text Classification Algorithms: A Survey


이다. 다른 NLP(text feature extractions, dimensionality reduction method, ...)와는 다른 text classification의 특징 등을 담은 `review paper` 이다.


따로 [github 링크](https://github.com/kk7nc/Text_Classification)도 달아두셔서 공부하기도 세상 편했던 !!!


<br>


## Index


1. Introduction

2. Text preprocessing

    1. Text cleaning & pre-processing

    2. syntactic word representation

    3. weighted words

    4. word embedding

3. dimensionality reduction

    1. Component Analysis

    2. Linear Discriminant Analysis (LDA)

    3. Non-negative matrix factorization (NMF)

    4. Random projection

    5. Autoencoder

    6. t-SNE

4. Existing Classification Techniques

    1. Rocchio Classification

    2. Boosting & Bagging

    3. Logistic Regression

    4. Naiive Bayes Classifier

    5. K-Nearest Neighborhood

    6. Support Vector Machine (SVM)

    7. Decision Tree

    8. Random Forest

    9. Conditional Random Field (CRF)

    10. Deep learning

    11. Semi-Supervised Learning for Text Classification


5. Evaluation

    1. Macro-Averaging and Micro-Averaging

    2. Fβ score

    3. Matthews Correlation Coefficient (MCC)

    4. Receiver Operating Characteristics (ROC)

    5. Area Under ROC Curve (AUC)


6. Discussion

    1. Text and Document Feature Extraction (about section 2)

    2. Dimentionality reduction (about section 3)

    3. Existing classification Techniques (about section 4)

    4. Evaluation (about section 5)

7. Text classification Usage

    1. text classification applications

    2. text classification support


8. Conclusion



index만 보더라도, 전반적인 `text classification`을 잘 설명해줄 수 있을 것 같아 골랐다

이미 아는 내용도 많긴 하지만, 쭉 보면서 정리하고 싶은 애들은 아래에 추가적으로 적어나가기로 했다!

그러다가 의문이 생기면 citated paper 도 차근차근 따라가는거지 뭐!



<br>

<br>

<br>

## [2] Text preprocessing

모든 데이터 사이언스, 특히 NLP 에서 제일 중요한 `preprocessing`!

<br>

1. Text cleaning & pre-processing

  1.  tokenization진행

    - e.g. After sleeping for four hours, he decided to sleep for another four.

    - > `'After', 'sleeping', 'for', 'four', 'hours', 'he', 'decided', 'to', 'sleep' ,'for', 'another' ,'four'`


  2. stop words, noise 제거

  3. capitalization

  4. slang & abbreviation(약어) 제거

  5. spelling correction

    - `context-sensitive spelling correction` , `Trie and Damerau-Levenshtein distance bigram`

  6. Stemming

    - e.g. studying -> study

    - 사전에 없는 어간을 추출하기도 함

  7. Lemmatization (표제어 추출)

    - 사전에 있는 어간만 추출함

    - [stemming과 Lemmatization의 차이](https://m.blog.naver.com/PostView.nhn?blogId=vangarang&logNo=220963244354&proxyReferer=https:%2F%2Fwww.google.com%2F)


2. syntactic word representation

  1. N-gram

    - 단어들의 순서에 따라 n개의 단어 묶음으로 자름

    - e.g. After sleeping for four hours, he decided to sleep for another four.

      - 2-grams

        - “After sleeping”, “sleeping for”, “for four”, “four hours”, “four he” “he decided”, “decided to”,
        “to sleep”, “sleep for”, “for another”, “another four”

      - 3-grams

        - “After sleeping for”, “sleeping for four”, “four hours he”, “ hours he decided”, “he decided to”,
        “to sleep for”, “sleep for another”, “for another four”

3. weighted words

  1. Bag Of Words (`BoW`)

    - 단어들의 순서는 전혀 고려하지 않고, 단어들의 frequency에만 집중한다.

    -  각 단어에 고유한 정수 인덱스를 부여한 후, 각 인덱스의 위치에 단어 토큰의 등장 횟수를 기록한 벡터를 만든다.


    - e.g. “As the home to UVA’s recognized undergraduate and graduate degree programs in systems engineering. In the UVA Department of Systems and Information Engineering, our students are exposed to a wide range of range

      - Bag-of-Words (BoW)

        - {“As”, “the”, “home”, “to”, “UVA’s”, “recognized”, “undergraduate”, “and”, “graduate”, “degree”, “program”, “in”, “systems”, “engineering”, “in”, “Department”, “Information”,“students”, “ ”,“are”, “exposed”, “wide”, “range” }

      - Bag-of-Feature (BoF)

        - Feature = {1,1,1,3,2,1,2,1,2,3,1,1,1,2,1,1,1,1,1,1}


    - limitations

      - 단어의 양이 많아지면 연산이 무작정 늘어남 (확장성 문제)

      - 문맥적 의미 고려 x (is this an apple과 this is an apple이 동일한 의미 갖게 됨 )


  2. Term Frequency - Inverse Document Frequency (`TF-IDF`)

    -  단어의 빈도(term frequency)와 역 문서 빈도(inverse document frequency)를 사용하여, DTM 내의 각 단어들마다 중요한 정도를 가중치로 줌

    -  DTM을 만든 후, TF-IDF 가중치를 부여함


      ![fig](https://blog.kakaocdn.net/dn/b73SmS/btqBtNyPGpT/G6RJlEJpc96OEMz18UnFr1/img.jpg)

    - BoW의 한계는 극복했지만, 문맥적으로 유사한 의미를 갖는 단어는 감지하지 못함

    

4. word embedding

  1. Word2Vec


  2. Global Vectors for Word Representation (`GloVe`)


  3. FastText


  4. Contextualized Word Embedding


  - 같은 단어라도 문맥에 따라 다른 vector를 만들어낸다. 순방향 모델과 역방향 모델에서의 값을 합쳐 사용


      - 순방향 모델

![fig](https://miro.medium.com/max/475/1*cWenLMAfpVSrgeuuLyJ3PA.png)

      - 역방향 모델

![fig2](https://miro.medium.com/max/496/1*XaWi2NSF6GLNgWVygZSxPw.png)

  -  이건 처음 접해보는 내용이라 [이 논문](https://arxiv.org/abs/1802.05365)을 따로 읽어야 겠다

<br>
