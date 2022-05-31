---
layout: post
title:  "[NLP/Attention/PLM] Attention is not not Explanation "
date:   2022-04-20
categories: PaperReview
---

참고 : [https://velog.io/@tobigs_xai/5주차-Attention-is-not-explanation](https://velog.io/@tobigs_xai/5%EC%A3%BC%EC%B0%A8-Attention-is-not-explanation)

[https://hoonst.github.io/2021/08/26/Attention-is-not-not-explanation/](https://hoonst.github.io/2021/08/26/Attention-is-not-not-explanation/)

[http://dsba.korea.ac.kr/seminar/?pageid=2&mod=document&uid=1831](http://dsba.korea.ac.kr/seminar/?pageid=2&mod=document&uid=1831)

기존 “attention is not Explanation”에서는, attention은 성능 향상에 도움은 되지만 explanation으로는 부족함을 다음과 같은 가정을 실험으로 검증

- Attention weight는 feature importance를 측정하는 다양한 방법들과 상관관계가 있다.

    → Attention과 Feature importance 방법 간의 상관관계 실험을 통해, Attention weights는 contextualized embedding이 있는 모델에서 Feature importance와 상관관계가 낮음을 확인

- Alternative(adversarial) attention weight는 예측 결과의 변화를 야기할 것이다.

    → 두가지 실험으로 검증

    - 원래의 attention weight를 단순히 섞어 재배치해도 성능 변화 적음
    - 원래의 distribution과 가장 다르지만 같은 결과를 도출하는 adversarial distribution을 직접 생성해도 성능변화 적음
- 그러나 limitation 많았음 ,,,

---

## 1. Introduction

- NLP에서 중요한 역할을 수행하는 attention + RNN model
    - attention의 intermediate representation이 model prediction에 대한 reasoning을 설명하고, 이를 통해 attention의 intermediate representation이 model의 decision making process에 대한 통찰력에 도달하는지 여부에 대한 관심이 증가
- <Attention is not Explanation>에서는, explainable attention distribution은 feature-importance measuer와 일치해야한다는 전제에 기초하여, attention score을 model의 explanation 점수로 사용하면 안된다고 함
    - 그러나 이 논문의 단점을 발견 →
        - Attention is not Explanation 하다고 했지만, explanation의 정의에 따라 다름
        - Attention is not Explanation함을 테스트하려면, model의 element가 되는 모든 요소를 고려해야 함

        → attention이 explanation으로 사용될 수 있는지를 판단하기 위한 4가지 test 방법을 제안

        - simple uniform-weight baseline (3.2)
        - a variance calibration based on multiple random seeds (3.3)
        - a diagnostic framework using frozen weights from pre-trained model (3.4)
        - end-to=end adversarial attention training protocol (4)
- Figure 1은 네가지 실험에 대한 전체적 flow

    ![스크린샷 2022-03-19 오후 5.38.16.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/2-Figure1-1.png)


- Figure 2의 예시 - binary classification에서의 adversarial distrubution이 그렇게까지 극단적이지는 않음

![스크린샷 2022-03-19 오후 5.38.39.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/3-Table1-1.png)

## 2. Attention might be Explanation

→ 본 논문의 chap 2에서는, 원래 모델에서 얻은 result와 유사한 result를 생성하는 alternative attention distribution이 존재하면, 원래 모델의 attention score를 사용하여 model prediction을 충실하게 설명할 수 없다고 주장

- 다른 interpretability measure와 attention score의 correlation에서,
    - 기존 논문은 well-grounded feature importance metrics(gradient-based & leave-one-out)와 attention이 강한 correlation이 없음을 밝힘

        →


### 2.1 Main Claim

→ 2.1에서는, 기존 논문의 저자가 질문을 던지고 english dataset에 대한 model decision을 explaning할 때, 실험 설계에 사용된 몇가지 주요 가정이setup에서 많은 양의 freedom을 남기며, 궁극적으로 attention distribution의 유용성을 측정하기 위한 방법을 남기지 않음(?)을 밝힘

- Attention Distribution is not a Primitive
    - modeling관점에서 model에서 얻은 attention score를 분리하면, model 자체가 저하됨
- Existence does not Entail Exclusivity.

## 3. Examining Attention Distributions


### 3.1 Experimental Setup

- we focus on experimenting with the binary classification subset of their tasks, and on models with an LSTM architecture
- datasets : 기존논문과 동일

- single-layer bidirectional LSTM + tanh activation
    - 예전 논문의 LSTM setting과 동일하게
        - 추가적인 attention layer & softmax prediction을 뒤에 붙임
        - hyperparameter 사용
        - evaluation metrics 사용
            - F1 scores on the positive class
            - Total Variation Distance(TVD), Jensen-Shannon Divergence(JSD) 사용

        → 예전 논문의 실험 결과 재현했음 → 이를 baseline으로 사용함


Diabetes, Anemia, IMDB, SST, AgNews, 20News (ADR tweets, groupnews 제외)

### 3.2 Uniform as the Adversary (첫번째 실험)

→3.2에서는 이 질문에 대해 model-driven approach를 적용함 - attention score가 uniform distribution일 때의 baseline을 사용하여 attention model의 contribution을 테스트함 → 그 결과 일부 data의 경우 frozen attention distribution이 학습된 attention weight만큼 성능을 발휘함 & randomly or adversarially-perturbed distributions이 attention≠explanation에 대한 evidence가 아님

![스크린샷 2022-03-19 오후 5.41.44.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/4-Table2-1.png)


### 3.3 Variance within a Model (두번째 실험)

→ 3.3에서는 random seed로 multiple-training sequence를 초기화하여, attention-produced weight의 expected variance를 조사 → trained model에서 expect할 수 있는 variance의 양을 더 잘 quantify하도록  (이러한 stochastic variance를 기존 모델과 비교함으로서, adversarial result를 더 잘 해석하도록 함)

![스크린샷 2022-03-19 오후 5.39.18.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/5-Figure2-1.png)

### 3.4 Diagnosing Attention Distributions by Guiding Simpler Models (세번째 실험)

→ 3.4에서는 non-contextual MLP archtecture에서 frozen weight으로 사용하여 attention distribution의 usefulness를 테스트하는 disgnostic tool제안 → LSTM-trained weight의 좋은 성능을 통해, trained attention score의 coherence를 보임 (attention component가 실제로 instance에서 token에 대한 meaningfule model-agnostic interpretation을 제공함을 알수있음)

![스크린샷 2022-03-19 오후 5.39.39.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/6-Figure3-1.png)

## 4. Training an Adversary (네번째 실험)

→ 4에서는 model-consistent training protocol을 소개함 + previous approach 수정 → (We train a model using a modified loss function which takes into account the distance from an ordinarily-trained base model’s attention scores in order to learn parameters for adversarial attention distributions) 주어진 data및 mode에 대해 alternative explanation을 구성할 수 있는지 여부를 검증

we introduce a model-consistent training protocol for finding adversarial attention weights, correcting some flaws we found in the previous approach.

- model
- prediction performance

    ![스크린샷 2022-03-19 오후 5.42.40.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/7-Table4-1.png)

- adversarial weights as guides

    ![스크린샷 2022-03-19 오후 5.42.10.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/6-Table3-1.png)

- TVD/JSD tradeoff

    ![스크린샷 2022-03-19 오후 5.40.02.png](https://d3i71xaburhd42.cloudfront.net/ce177672b00ddf46e4906157a7e997ca9338b8b9/7-Figure4-1.png)

- concrete example

## 5. Defining Explanation

→ 5에서는 interpretability와 explainability의 정의에 대한 theoretical discussion 진행

- explainable AI는 다음과 같은 개념을 포함
    - transperency
    - explainability
    - interpretability
-

Attention mechanisms do provide a look into the inner workings of a model, as they produce an easily-understandable weighting of hidden states.

## 6. Attention is All you need it to be

- attention이 explanation인지아닌지는 explanation의 definition에 따라 다름
- 본 연구를 통해 이전 연구가 무효가 되지는 않겠지만, 일부 classification task에서 LSTM에 대한 adversarial distribution을 찾을 수 있음을 확인하였음

    → 이를 통해 input과 output사이의 model이 설정한 연결에 대한 해석을 위해 attention


- future work에서는 다양한 task, 다른 modeling을 통해 실험 및 검증할 것을 제안
