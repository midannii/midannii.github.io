---
layout: post
title:  "[paper] Deriving Machine Attention from Human Rationales "
date:   2022-03-20
categories: PaperReview
---
- 기존의 attention-based model은 많은 양의 data에서 학습할때 효과적임
- 본 논문에서는,  low-resource scenario에서도 효과적으로 attention-based model을 학습할 수 있음을 보임
    - R2A model 제안
        - discrete human-annotated rationales를 제안 & attention에 mapping함

        → 이러한 mapping이 domain에 general함 (즉 domain과 무관한 성능을 보임)하며, resource-rich로부터 low-resource로 transfer가능함을 보임

        - R2A는 domain-invariant representation을 joinltly learning하고, rationales와 attention사이의 mapping을 이끎
        - benchmark 실험 결과, 15% average error reduction

---

## 1. Introduction

- attention과 rationales는 서로 밀접하게 연관되어 있음

    둘다 final prediction에 대한 word importance를 강조함

    - rationale
        - importance 는 hard selection으로 표현됨
    - attention:
        - importance는 단어에 대한 soft distribution으로 표현됨
- low-resource dataset에서의 성능을 향상하기 위해서는  human rationale을  attention 생성을 위한 supervision으로 사용하는 것

    → 그러나 rationale자체로는 machine attention을 대체하기엔 부족함

    - human rationale은 relevance에 대한 binary indication만 제공함 (not soft distribution)
    - rationale은 주관적으로 정의되며, annotator에 따라 다름 & 주어진 model architecture에 대해 customize되지 않음
    - 이와 반대로 machine attention은 항상 specific model architecture로 파생됨

    - attention, rationale 모두 비슷한 방식으로 가중치를 부여함 : highlighting task-specific nouns and adjectives, while downplaying functional words.

→ 즉, attention은 resource-rich일때 성능이 좋음. low-resource일때는 attention을 사용하기가 좀 그래 (보장되어 있지 않은?)  human rationale이 attention이랑 관련되어 있기는 하지만, 그렇다도 그걸 attention 대신 사용하기엔 무리가 있음

- 본 논문에서는, 두가지 모델을 비교함
    - models informed by human rationales
    - models informed by high-quality attention
        - 다량의 annotator로부터 “oracle” attention을 얻음

            → 얻은 “oracle” attention은 training data의 일부에만 접근할 수 있는 model을 guide하는 데 사용됨

        - 이 모델은 oracle-free variant보다 성능이 뛰어나고,  human rationale로 학습된 모델보다 성능 좋음

    → 실제로는  “oracle” attention을 사용할 수 없으며, 이를 사용하려면  “oracle” attention의 대체를 찾아야 함

    → 따라서 본 논문에서는 이론적으로  “oracle” attention을 사용함 (?)


- 본 논문에서는 `R2A model` 을 제안함
    - R2A: Rationale to Attention
        - resource-rich로부터 low-resource 등 다양한 task로 generalizable할 수 있다(즉 tranfer될 수 있다)고 가정함
        - `R2A mapping`을 학습 및 적용하기 위해서는 source, target task 모두에서 rationale에 access해야 함
            - target taks에서는 rationale이 human에게서 왔다고 가정함
            - source taks에서, large scale의 rationale을 수집하는 것은 불가능하므로, machine-generated rationale을 proxy로 사용함
                - proxy란 ? client가 자신을 통해서 다른 network에 간접적으로 접속할 수 있게 해주는 시스템 (서버와 클라이언트 사이의 중계기)
                - rich → low로 transfer되는 거니까, source가 rich, target이 low가 되는 건가 ???
    - 3가지 comonent로 구성되어 있으며, jointly traing됨
        - source task(s)를 위한 attention-based model
        - transfer를 위해, domain-invariant representation을 학습
        - invariant representation & rationale을 결합 & attention을 얻음
    - 학습된 모델은 target task에 적용하여, fuman rationale로부터 attention을 생성해냄
        - 만들어진 attention은 target classifier의 training을 supervise하는데 사용됨
- 2가지 transfer setting에서 evaluate
    - transfer within single domain
    - domain transfer across multiple domains.

## 2. Related Work

- Attention-based models
    - attention 학습에 추가적인 supervision은 필요하지 않지만,추가적인 supervision이 존재하면 성능이 더 좋아짐
    - 본 논문에서는 connection between rationale and attention을 추가적인 supervision으로 사용
- Rationale-based models
    - low-resource senario에서 rationale을 사용하는 연구가 존재했음 but 충분히 많은 양의 데이터 필요했음
    - 본 논문에서는 human rationale을 직접적으로 사용하여, CNN classifier의 sentence-level attention을 guide했음
        - 기존 연구들과 달리, human rationale과 machine attention의 차이점을 분별했음
        - 본 논문에서는  low-resource model에게 richer supervision을 제공하기 위해, human rationale과 machine attention을 mapping하도록 모델을 학습시킴
- Transfer learning
    - 기존 연구에서는 target task에서의 labeled data를 사용할 수 있으면, source task에서 학습된 encoder를 fine-tuning하거나, shared encoder를 사용하여 모든 taks에 대해 multi-taks learning을 통해 knowledge를 tranfer함
        - knowledge transfer란?
            - large corpus에서 pre-trainig 후 학습된 모델을 기반으로 최종 출력층을 바꿔 학습함

                즉, 학습된 모델의 최종 출력층을 보유 중인 데이터에 대응하는 출력층으로 바꾸고,교체한 출력층의 결합 파라미터(그릭고 앞 층의 결합 파라미터)를 소량의 데이터로 다시 학습함! 이때 입력층에 가까운 부분의 결합 파라미터를 변화시키지 않음!



    - 본 논문에서는 human rationale을 통해 task-specific attention의 transferability를 탐색
        - unsupervised domain adaptation과 비슷함; R2A model의 training에서는 target annotation이 없기 때문
        - 본 논문에서는, 기존과 다르게 rationale to attention mapping을 사용함 → 학습 끝나면, different target task에서 적용가능
            - 기존 연구에서는 source & target domain사이의 representaiton을 align함으로서 classifier를 적용했음

## 3. Method

- problem formulation
    - for source task S,
        - 충분한 양의 labled example이 있다고 가정함
        - 각 source example에 대해, 자동으로 rationale을 만들어낼 수 있음
    - for target task T,
        - 한정적인 양의 labeled example과 대량의 unlabeled data가 존재함
        - labeled example에서, human annotated rationale에 접근한다고 가정
- overview
    - Our goal is to improve classification performance on the target task by learning a mapping from human rationales to high-quality machine attention (R2A)

        1) high-quality attention을 쉽게 얻을 수 있는 resource-rich task로부터 mapping을 학습함

        2) source task에서 파생된 rationale과 attention간의 mapping을 target task로 export

        - 이를 위해, model은 adversarial objective를 통해 구현하는 invariant representation에 대해 operate해야 함
    - mapping이 파생되면, source language의 human rationale을 target language의 high-quality attention으로 translate가능
        - 이렇게 만들어진 attention은 target task의 attention-based classifier에서 추가적인 training signal로 사용됨
        - R2A mapping == 여러 task에 걸쳐,attention distribution보다 우선적으로 생성하는 meta model → 뭔소리야

![png](https://d3i71xaburhd42.cloudfront.net/8b37185ad6e25c8e1a6ec622a73d3281e80f4378/3-Figure2-1.png)

- model architecture

    ![Untitled](https://d3i71xaburhd42.cloudfront.net/8b37185ad6e25c8e1a6ec622a73d3281e80f4378/4-Figure3-1.png)

    - multi-task learning:
        - R2A mapping을 학습시키기 위해, attention에 대한 annotation이 필요함 ! multi-task learning module로 high-qualtiy attention을 생성해냄
        - 생성된 attention은 source task에서의 prediction error를 최소화하기 위한 intermediate result로 사용됨
    - domain-invariant encoder
        - 첫번째 module로부터 얻은 contextualized representation을 domain-vairant version으로 transform함
            - source data, unlabeled target data에 대한 domain adversarial training을 진행
    - attention generation
        - domain-invariant representation & rationale을 기반으로, 첫번째 모듈에서 얻은 intermediate attention을 predict하는 방법을 학습

### 3.1 Multi-task learning

- goal: to learn good attention for each source task
    - 학습된 attention은 이후 attention generation module에서의 supervision으로 사용됨
    - source task의 input text로부터 label을 예측

        → 모든 labeled source data로부터 prediction error를 minimize해야함 !

- source task $t∈${$S_1, ..., S_N$}과 training instance $(x^t, y^t)$에 대해,

    1) input sequence $x^t$를 hidden state $h^t = enc(x^t)$로 encoding

    - 이때, enc는 bi-directional LSTM
    - enc는 모든 source task에서 share됨
    - 각 position i에 대해, dense vector $h^t_i$는 word $x^t_i$의 context information&content를 encoding함

    2) sequence $h^t$를 task-specific attention module에 전달하여, attention생성

    3) contextualized representation의 weighted sum을 사용하여 $x^t$의 label을 예측함

    - pred^t : task-specific multi-layer perceptron

    4) loss $L_(lbl)$d을 minimize하기 위해, 이 module을 학습함

    $L_(lbl)$: 모든 source task에 대해, annotated label과 prediction의 차이

    - classification에는 cross-entropy loss를, regression에서는 MSE를 사용

### 3.2. Domain-invariant encoder

- goal

    1) learning a general encoder for both source and target corpora

    → language modeling objective를 optimize

    2) learning domain-invariant representation

    → source&target distribution 사이의 Wasserstein distance를 최소화하면서 optimize

- effective transfer를 가능케함 - 특히 source&target domain사이의 significant variance가 있을 때


### 3.3 Attention generation

- goal: to generate high-quality attention for each task
- input: domain-invariant representation과 task-specific rationales

    → 이를 바탕으로 task-specific attention score를 계산

    - predicted attention과, multi-task learning module에서 얻어진 intermediate attention의 거리를 최소화하고자 함
- 계산하는 것들
    - $u^t$
    - $u_i^t$~
    - $a_i^t$^
    - $L_(att)$: $a_i^t$^와 $a_i^t$ 사이의 distance
    - soft-margin cosine distance:


### 3.4 Pipeline

- Training R2A
    - 앞서 언급한 3가지 module을, source data와 unlabeled target data를 사용하여 jointly하게 학습함

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d311e2e8-d3a7-4531-a121-8b3e62206681/Untitled.png)

    - 이때 λ는 각 training objective의 importance를 control하는 hyperparameter
    - L은 loss function을 나타냄
- R2A inference
    - R2A model이 학습되면, human-annotated rationales를 기반으로 각 labeled target example에 대한 attention을 만들어낼 수 있음
- Training target classifier
    - target task에서 testing할 때, rationale이나 label은 주어지지 않음
    - target task에서의 prediction을 위해, label과 R2A-generated attention을 바탕으로 새로운 classifier를 training시킴
        - target classifier의 joint objective는 아래 두개를 더함

            - $L^T_(lbl)$: labeled target data
            - $L^T_(att)$: R2A-generatd attention과 target classifier-generated attention의 prediction loss
        - multi-task learning module과 동일한 architecture를 공유함



## 4. Experimental Setup

two transfer settings:
(1) transfer among aspects within the same domain
(2) transfer among different domains

### 4.1 Datasets

- aspect transfer

    We first consider the transfer problem between multiple aspects of one domain: beer review

    - BeerAdvocate review dataset 사용

- domain transfer

    Our second experiment focuses on domain transfer from beer reviews to different aspects of hotel reviews.

    - TripAdvisor hotel review dataset 사용
        - 3가지 측면을 tranfer target으로 (location, cleanliness, service) - 각 측면에서 3 이상은 긍정적, 이하는 부정적으로 표시

### 4.2 Baselines

- basic classifier
    - linear SVM using bag-of-ngrams representation
        - uni-, bi-, tri-gram을 combine하여 feature로 사용 & tf-idf weighting을 사용
- rationale augmented classifier
    - rationale augmented SVM (RA-SVM)
        - classifer의 decision boundary를 regularize하기 위해 human rationale을 사용하는, SVM-based model
    - rationale augmented CNN (RA-CNN)

        (1) 먼저 CNN-based sentence classifier를 훈련하여, 주어진 문장의 rationale word가 포함될 확률을 추정함

        (2) RA-CNN은 이러한 추정치에 비례하도록 전체 representation에 대한 각 문장의 contribution을 조정함

        → 이 전체 representation에서 final predict

- transfer method :
    - target classifier의 joint objective는 아래와 같음

        $L = L^T_{lbl} + λ^T_{att}L^T_{att}$

    - TRANS: an attentionbased classifier that does not use human rationales from the target task
        - objective Eq2에서의 cross-entropy loss L_lbl만 optimize함
    - RA-TRANS, an attention-based classifier that directly uses human rationales to supervise the attention
        - objective Eq2에서의 L_att가 attention&rationale사이의 consine distance로 대체됨

    → 두 모델의 encoder는 source data와 unlabeled target data로부터 학습된 enc로 initialize됨

- oracle :
    - R2A model과 같은 구조이지만, oracle attention에 의해 supervise됨
        - oracle attention: target task에 대한 대규모 annotation이 있는 held-out dataset으로부터 파생됨
    - target task의 고유한 limitation을 분리함으로서, R2A의 contribution을 분석하는데 도움이 됨

### 4.3 Implementation details

- fastText embeddings
- language encoder: 200-dimension bidirectional LSTM
- R2A encoder: 50-dimension bi-directional LSTM
- word embedding & classifier의 last hidden layer에서 0.1 Dropout
- adam optimizer
- 기타 hyperparameters,,,,
- L_lm을 학습시키면 시간,메모리적으로 비효율적 - 복잡도가 voacb size에 비례하기 때문

    → 이를 막기 위해, 전체 vocab을 random하게 split & 정확한 token 예측대신 bin prediction의 loss를 minimize하도록 학습


## 5. Results

- Aspect transfer

    ![Untitled](https://d3i71xaburhd42.cloudfront.net/8b37185ad6e25c8e1a6ec622a73d3281e80f4378/7-Table3-1.png)

- Domain Transfer

    ![Untitled](https://d3i71xaburhd42.cloudfront.net/8b37185ad6e25c8e1a6ec622a73d3281e80f4378/7-Table4-1.png)

- Ablation Study

    ![Untitled](https://d3i71xaburhd42.cloudfront.net/8b37185ad6e25c8e1a6ec622a73d3281e80f4378/7-Table5-1.png)

- visualization of representation

    ![Untitled](https://d3i71xaburhd42.cloudfront.net/8b37185ad6e25c8e1a6ec622a73d3281e80f4378/8-Figure5-1.png)
