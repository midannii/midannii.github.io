---
layout: post
title:  "[NLP paper] Natural Language Processing Reveals Vulnerable Mental Health Support Groups and Heightened Health Anxiety on Reddit During COVID-19: Observational Study "
date:   2021-01-20
desc: " "
keywords: "DL, ML"
categories: [Deeplearning]
tags: [DL, ML, pytorch]
icon: icon-html
---



처음으로 정리해볼 [논문](https://www.jmir.org/2020/10/e22635?utm_source=ground.news&utm_medium=referral)은,

### Natural Language Processing Reveals Vulnerable Mental Health Support Groups and Heightened Health Anxiety on Reddit During COVID-19: Observational Study


이다. LIWC, LDA, TF-IDF, sentimental analysis 등 다양한 언어학적 특성을 도출하여 covid19가 사회적으로 미친 영향을 심리적 관점에서 바라보았다.

`reddit`이라는 Social media를 이용한 것도 흥미로웠으며, 방법론도 간단명료한 느낌이었고, 이후 다양한 분야에서 benchmark 할 수 있을 것 같다.


<br>


![fig](static/assets/img/blog/papers/NLP1.png)


<br>

간단하게 요약하면, 이 논문에서는 코로나 전과 후의 감정상태를 reddit을 통해 분석하였다.

이 링크에 코드와 데이터를 공개해서 쉽게 따라갈 수 있게 되어있다. -> [😇😇](https://osf.io/7peyq/)


<br>

## Workflow

1. reddit_data_extraction.ipynb


우선 크게 4가지 종류의 데이터를 수집한다.

```
- post: mid-pandemic (2020.01.01 ~ 04.20)
- pre: pre-pandemic (2018.12 ~ 2019.12)
- 2019: post data의 control (2019.01.01 ~ 04.20)
- 2018: post data의 control (2028.01.01 ~ 04.20)

```


수집한 subreddit 종류는 아래와 같다.

```
- 15 specific mental health communities (r/EDAnonymous, r/addiction, r/alcoholism, r/adhd, r/anxiety, r/autism, r/BipolarReddit, r/bpd, r/depression, r/healthanxiety, r/lonely, r/ptsd, r/schizophrenia, r/socialanxiety, and r/SuicideWatch),

- 2 broad mental health subreddits (r/mentalhealth and r/COVID19_support),

- 11 nonmental health subreddits (r/conspiracy, r/divorce, r/fitness, r/guns, r/jokes, r/legaladvice, r/meditation, r/parenting, r/personalfinance, r/relationships, and r/teaching).
```


총 82.6만명의 user의 글을 수집하였고, null인 글이나 중복되는 author의 글은 삭제하였다

(특정 author의 게시글만 많으면 그 author의 특징을 전체의 특징으로 오해할 가능성 때문)


2. reddit_feature_extraction.ipynb


아래와 같은 feature를 추출하였다.

```
LIWC (n=62);
sentiment analysis (n=4);
basic word and syllable counts (n=8);
punctuation (n=1);
readability metrics (n=9);
term frequency–inverse document frequency (TF-IDF) ngrams (256-1024) to capture words and phrases that characterize specific posts; manually built lexicons about suicidality (n=1), economic stress (n=1), isolation (n=1), substance use (n=1), domestic stress (n=1), guns (n=1).
```




3. Analysis - classification_results.py, reddit_descriptive.ipynb, Unsupervised_Clustering_Pipeline.ipynb, reddit_lda_pipeline.ipynb, reddit_cluster.ipynb, reddit_cluster.py


2에서 수집한  feature 를 바탕으로

`regression` - trend를 분석하거나 (TF-IDF를 제외한 feature 사용),

`supervised ML` - support group으로 분리하고 주요 feature를 해석함으로서 어떻게 언어에서 다른 문제가 manifest되는지 분석하거나 차원축소를 진행하고,

clustering 과 같은 `unsupervised ML` 을 진행하였다.


이 논문에서 특이한 점은 분석 결과를 figure 1~ 6으로 나타냈는데,

각 figure를 그릴 수 있는 코드를 ipynb file이랑 매치했다.



![fig](static/assets/img/blog/papers/fig1.jpeg)


우선 fig 1은 covid case와 reddit post의 관계를 timeline landmark한 그림이다. `reddit_descriptive.ipynb`에 코드가 있다.

![fig](static/assets/img/blog/papers/fig2.jpeg)

fig 2는 post-pandemic 전과 후의 linguistic feature 변화를 나타낸 그림이다. 부정적인 변화가 큰 subreddit도 B에 따로 정리해 두었다. `reddit_descriptive.ipynb`에 코드가 있다.

![fig](static/assets/img/blog/papers/fig3.jpeg)

fig3은 pre-pendemic과 post-pendemic의 unsupervised clustering을 비교하여 UMAP으로 나타낸 것이다. `Unsupervised_Clustering_Pipeline.ipynb`에 코드가 있다.

![fig](static/assets/img/blog/papers/fig4.jpeg)

fig4는 LDA를 이용하여 topic distribution의 shift 변화를 나타낸 것이다. `reddit_lda_pipeline.ipynb`에 코드가 있다.

![fig](static/assets/img/blog/papers/fig5.jpeg)

fig5는 `reddit_lda_pipeline.ipynb`, `Unsupervised_Clustering_Pipeline.ipynb`에 코드가 있다.

![fig](static/assets/img/blog/papers/fig6.jpeg)

fig6은 차원축소를 통해 similarity를 관찰했다. healthanxiety가 다른 정신질환 subreddit과 비슷해졌음을 알 수 있다. `reddit_cluster.ipynb`에 코드가 있다.


4. train & test model

mental health subreddit인지 control group인지를 구분하는 binary classification model을 학습하였다.

이때 사용한 모델은 `SGD with L1 penalty`, `SGD with net penalty`, `linear SVM`이다.


<br>

## Reference


- https://www.jmir.org/2020/10/e22635?utm_source=ground.news&utm_medium=referral

- https://osf.io/7peyq/

- https://github.com/danielmlow/reddit

<br>
