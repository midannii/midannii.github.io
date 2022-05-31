---
layout: post
title:  "[NLP/RE] Matching the Blanks: Distributional Similarity for Relation Learning "
date:   2021-03-19
categories: PaperReview
---

<br>

이번에 리뷰할 [논문](https://arxiv.org/abs/1906.03158)은,


# Matching the Blanks: Distributional Similarity for Relation Learning

이다.


<br>

# 1. Introduction

RE에서의 대표적인 3가지

1. supervised or distantly supervised RE
    - 제한된 schema에서 text로부터 relation을 mapping하는 것을 학습
2. openIE
3. universal schema
    - 다양한 text와 schematic relation을 포용

          → arbitrary textual input & arbitrary entity pair로 확장된 joint representation을 생성

    - but, text에 align되는 large knowledge graph에 의존함

이 논문에서는, text로부터 바로 RE를 학습할 수 있는 방법을 제시

#Transformer Neural Network architecture

#knowledge graph나 human annotator없이 RE할 수 있음

#이전의 supervised RE보다 성능 좋음

![fig1](https://miro.medium.com/max/1732/1*jHlbU40LukJrHhhGIpuKxg.png)

(1) training dataset

- relation statement ; 2개의 marked entity를 포함하고 있는 text의 block

    → entity를 [BLANK]로 처리한 relation statement로 training data 만듦

(2) training

- Our training procedure takes in pairs of blank-containing relation statements, and has an objective that encourages relation representations to be similar if they range over the same pairs of entities.

(3) after training

- 학습된 RE를 `FewRel task(specific relation)` 에 이용함
    - 새로운 domain에서의 relation에 적용해보며 평가함
    - outperfom함 !
- maching blank를 학습시키고 labeled data로 tuning해도 outperform함 !

# 2. Overview

**Task Definition**

- Let x = [x0 . . . xn] be a sequence of tokens, where x0 = [CLS] and xn = [SEP] are special start and end markers
- Let s1 = (i, j) and s2 = (k, l) be pairs of integers such that 0 < i < j − 1, j < k, k ≤ l − 1, and l ≤ n

- relation statement is a triple r = (x, s1, s2)
    - s1,s2 는 x의 entity mention을 결정
        - sequence [xi . . . xj−1] , sequence [xk . . . xl−1]는 entity를 mention

- 목표: 아래 함수(고정된 길이의 벡터 hr에 relation statement를 mapping해줌)를 학습

    ![fig](static/assets/img/blog/papers/MTB_1.png)

    ![fig](static/assets/img/blog/papers/MTB_2.png)

    - 엔티티(s1, s2) 의 relation(x)

**Contributions**

1. Transformer sequence model 위에 build된 fθ(relation encoder)에 대해 다양한 architecture 연구 (in 3.1)

    → 이후, suprevised training으로 RE benchmark에 적용하여 architecture들을 평가

2.  fθ가, entity linked text의 form으로 distant supervision으로부터 학습될 수 있음을 보임(in 4)

# 3. Architectures for Relation Learning

text로부터 바로 relation representation을 만드는 모델을 개발하는 것이 첫 목표

**3.1 Relation Classification and Extraction Tasks**

- supervised RE benchmark에서, 다양한 representation방법을 평가함

    → RE 종류 2가지

    - `fully supervised relation extraction`
        - goal: 주어진 `r(relation statement)` 로부터 `t(relation type)` 를 예측
        - SemEval 2010 Task 8, KBP-37, TACRED 로 평가
    - `few-shot relation matching`
        - goal: `rq(query relation statement)` 에 대한 `tq`  를 예측 (tq∈{1, ..., K})
            - we are given K sets of N labeled relation statements Sk = {(r0, t0) . . . (rN , tN )} where ti ∈ {1 . . . K } is the corresponding relation type.
        - FewRel로 평가

**3.2 Relation Representations from Deep Transformers**

- `BERT-Large model` 이용
- 2가지 modeling question
    - how do we represent entities of interest in the input to BERT
    - how do we extract a fixed length representation of a relation from BERT’s output

![fig](https://i.imgur.com/IzqJyKq.png)

- BERT encoder로 focus span(s1,s2)의 정보를 가져오는 option (3가지)
    - standard input (reference)
        - entity span(s1, s2)의 어떤 명백한 identification(식별)에도 access 할 수 없는 BERT model
        - x가 3개 이상의 entity mention을 가지고 있으면, 어떤 2개의 entity에 집중해야 하는지를 알 수 없음

            → 즉, 명확한 entity를 identification 하지 못함

    - `positional embedding`
        - 2가지의 segmentation embedding
            - span s1의 모든 toekn을 추가함
            - span s2의 모든 token을 추가함

        → RE에 positional embedding을 추가한 것과 비슷

    - `entity marker tokens`
        - relation statement에서, x에서의각 entity mention의 앞과 끝을 구분하기 위해 4가지 word pieces 를 사용
            - [E1start], [E1end], [E2start], [E2end]
                - x = [x0 ... `[E1start]`xi ... xj-1 `[E1end]` .... `[E2start]` xk .... xl-1 `[E2end]` ... xn] 로 두고, 이를 x대신 BERT에 token sequence로 입력함
        - insert token을 다루기 위해, `entity index` 인 `s1 = (i+1, j+1)` 과 `s2 = (k+3, l+3)`을 수정

**3.3 Fixed length relation representation**



###내 궁금증; 왜 fixed length ?? 어떤 length를 fix하는지 ??  

- BERT encoder로부터 `hr(fixed length relation representation)` 을 추출하기 위한 3가지 method

    → transformer network의 마지막 hidden layer를 추출하는 데 의존함

    - we define as H = [h0, ...hn] for n = |x| (or |x ̃| if entity marker tokens are used)
    - `[CLS] token`
        - `[CLS] token` 에 corresponding하는 BERT의 output state가 length sentence representation을 fix하는데 쓰임

            → 이 논문에서,`[CLS] output` (h0)을을 first relation representation으로 적용할 것임

    - `entity mention pooling`
        - hr = < `he1` | `he2` >
            - <a|b>는 a와 b의 concatenation
            - 2개의 entity mention
                - `he1` = MAXPOOL([hi...hj−1])
                - `he2` = MAXPOOL([hk...hl−1])

                → `he1`, `he2` 를 얻기 위해, 각 entity mention의 word piece에 corresponding하는 final hidden layer를 Max-pooling해서 `hr` 을 얻음

    - `entity start state`
        - entity marker ([E1start], .., [E2end]) 를 사용한 후, relation representation은 `rh = <hj | hj+2>`

모든 task에서, `ENTITY MARKERS input representation`과  `ENTITY START output representation` 이 제일 성능이 좋음

![fig](https://bpben.github.io/assets/paper/matchingblanks_1.png)

- model의 input & output architecture를 define하기 위해, 학습에 쓰이는 training loss를 fix 해야함
- 모든 model에서, Transformer network에서의 output representation은,

    linear activation를 포함하거나, representation에 대해 layer normalization을 수행하는

    FC layer로 들어감

    → Post Transfomer layer의 choice을 hyperparameter로 취급하고, 각 task에 대해 가장 성능이 좋은 layer type을 사용한다.

**#supervised task**

- we introduce a new classification layer W ∈ RKxH where H is the size of the relation representation and K is the number of relation types.
- The classification loss is the standard cross entropy of the softmax of hrWT withrespecttothetruerelationtype.

**#few-shot task**

- we use the dot product between relation representation of the query statement and each of the candidate statements as a similarity score.
- we also apply a cross entropy loss of the softmax of similarity scores with respect to the true class.

# 4. Learning by Matching the Blanks

predefined ontology나 relation-labeled training data 없이 fθ를 학습하는 방법을 소개할 예정

- relation statement를 `r` , `r'`  라고 정의
    - inner product인 `fθ(r)^T fθ(r')`
        - `r` , `r'` 가 semantically similar relation일수록 높음 & semantically different relation일수록 낮음
- 학습할 때, relation label을 사용하지 않고, entity linked text에서 fθ를 학습
    - 중복이 많은 web text에서, s1과 s1'이 동일한 entity를 refer하고,  s2와 s2'이 동일한 entity를 refer할 때,

        →  **r = (x, s1, s2)가 r′ = (x′, s′1, s′2)와 동일한 semantic relation을 encode할 가능성이 높음을 이용함**

**4.1 Learning Setup**

- binary classifier

    ![binaryClassifier](static/assets/img/blog/papers/MTB_3.png)

    - E ; predefined set of entities
    - D = [(r0,e01,e02)...(rN,eN1 ,eN2 )] be a corpus of relation statements that have been labeled with two entities ei1 ∈ E and ei2 ∈ E
    - Each item in D is created by pairing the relation statement ri with the two en- tities ei1 and ei2 corresponding to the spans si1 and si2
    - fθ(relation statement encoder) ; 2개의 relation statement가 같은 relation을 encode하고 있는지를 결정하는데 사용

- loss

    ![loss](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTcN2Z9ou9wNUakeAjhRbz1QAM6ht5-8hIQFA&usqp=CAU)

    - δe,e′ is the Kronecker delta that takes the value 1 iff e = e′, and 0 otherwise.
    - 이 loss는, entity linking system에 의해 minized될 수 있다.

**4.2 Introducing Blanks**

- modified corpus D ̃:

    ![D](static/assets/img/blog/papers/MTB_D.png)

    - 단순히 linking system만 재학습하는 것을 피하기 위해 도입함
    - 이 D에는, 한두개의 entity가 [BLANK] symbol로 대체된 ri = (x ̃i, si1, si2)가 있다.
    - 특히,x ̃에는 확률 α로 s1,s2에 의해 정의된 span을 포함하며, span은 [BLANK] symbol로 대체되어있다.

→ We hypothesize that training on D ̃ will result in a fθ that encodes the semantic relation between the two possibly elided entity spans !!

**4.3 Matching the Blanks (MTB) Training**

BERT의 training setup과 비슷하게 구성함

- 2개의 loss가 동시에 사용됨 ; `masked language model loss` & `the matching the blanks loss`

- training corpus 생성

    → English Wikipedia에서 text추출 →  `off-the-shelf entity linking system` 으로 text span을 id와 함께 표시 → relation statement 추출

    - 이때, bias를 방지하기 위해 entity를 포함하는 relation statement를 random sampling & 개수 제한

    - (D ̃)를 줄이기 위해,  `noise-contrasitive estimation`  을 사용함

        → 같은 entity를 가지고 있는 모든 relation statement의 positive pair를 고려하여, 같은 entity쌍을 가지지 않는 모든 relation statement를 합함

        - 그 대신, 전체 relation statement에서 random sampling한 relation statement이나 하나의 entity만 공통으로 가지는 **relation statement를**   negative sampling함
        - `hard negative` set를 포함함
            - We include the second set ‘hard’ negatives to account for the fact that most randomly sampled relation statement pairs are very unlikely to be even remotely topically related, and we would like to ensure that the training procedure sees pairs of relation statements that refer to similar, but different, relations.

        → α = 0.7으로 entity를 [BLANK] symbol로 대체

# 6. Conclusion & Future Work

우리꺼 성능 좋다!!!! 🥳🥳🥳🥳

<br>
