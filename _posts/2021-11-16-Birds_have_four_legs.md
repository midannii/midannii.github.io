> 요약
>

- PTML의 neural knowledge base는 numerical commonsense knowledge에 해당하지 않음  (e.g., a bird usually has two legs)
- 본 논문에서, PTLM이 numerical commonsense knowledge를 유도할 수 있는지를 연구함
    - 이를 연구하기위해, 새로운 데이터셋 제안 : **`NUMERSENSE`**
        - containing 13.6k masked-word-prediction probes (10.5k for fine-tuning and 3.1k for testing).
- 분석 결과,

    (1) BERT 및 RoBERTa는 fine-tuning 전에 diagnostic dataset(즉, `NUMERSENCE`)가 제대로 수행되지 않음

    (2) fine-tuning with distant supervision는 약간의 성능 향상 가져옴

    (3) 여전히 human performance보다 부족함


---

## 1. Introduction

- PTML은 commonsense knowledge에 대해서는 잘 text-generate하지만, reasoning-based masked-word-prediction task에서는 numerical commonsense knowledge를 잘 활용하지 못함

    → 따라서, 본 논문에서는 "whether PTLMs capture numerical commonsense knowledge" 에 대해 연구할 것

    - numerical commonsense knowledge란, 두개의 entity간의 numeric relation을 제공하는 commonsense knowledge임
    - e.g. a bird usually [MASK] legs에서, numerical word만을 고려하여, [MASK]에 오는 정답인 'two'를 예측할 수 있는지 ??
- `NUMERSENSE` dataset 제안
    - 8가지 다른 category의 question을 포함하고 있음

    → 본 연구에서, BERT가 처음엔 [MASK]를 "four"로 예측하다가, 간단한 단어("round")를 추가해주니까 "two"라고 옳게 예측함

- 2가지 방법으로 PTLM을 evaluate

    (1) a zero-shot setting; meaning no probes from our dataset were used to fine-tune the models before evaluation;

    (2) a distant supervision setting; where models were fine-tuned on examples from related commonsense reasoning datasets before being evaluated on ours.

    → 두번쨰 방법이 성능 향상에 도움을 주긴 하지만, 여전히 PTLM은 human보다 떨어짐


## 2. The NumerSense Probing Task

- We introduce our numerical commonsense reasoning probing task, as well as the creation process of the namesake dataset, `NUMERSENSE`

### 2.1. Task Formulation

- masking된 위치에 들어갈 적당한 단어를 PTLM이 채움 (PTLM이 후보 단어들을 추리고 softmax score로 순위 매김)
    - 만약 number word(one, two,..)와 같은 numerical commonsense knoweldge가 제일 높은 순위이고 그 대답이 정답이라면, PTLM은 numerical commonsense를 잘 가지고 있는 것

### 2.2. Probing Data Collection

- probing 구축 과정
    1. 기구축된 `OMCS(Open Mind Common Sense)` 데이터에서 최고 12개의 number word를 포함하고 있는 sentence를 추출
    2. noisy data를 수동으로 정제함
        - noisy data; 1) incorrect, 2)containing typos, 3) having no numerical commonsense logic

    → 1131개의 cleaned statements for probing 얻음

    1. model의 robustness를 연구하기 위해, adversarial example을 2에 추가함
        - 각 probe에서, numerical reasoning에 포함되어있는 noun의 앞에 adjective를 추가함
        - 이러한 adjective는 querying relevant triple에서 추출함 (e.g. <wheel, HasProperty, round>)

            → 이러한 adjectiv는 ConceptNet과 같은 commonsense knoweldge graph와 human에 의해 자연스럽다고 판단됨


    →  `NUMERSENSE` 와 같은 diagnostic dataset의 3145개 test probing 구축함

- `NUMERSENSE` 데이터 구축
    - *numerical commonsense reasoning probing task*
    - 8 type; object, biology, geometry, unit, math, physis, geography, misc

    [NumerSense: Probing Numerical Commonsense Knowledge of Pre-trained Language Models](https://inklab.usc.edu/NumerSense/)


### 2.3. Supervised for Fine-tuning PTLMs

fine-tuning으로 성능을 향상시키기 위해, generic commonsense statement인 GenericKB corpus의 문장을 추가로 수집함

1.  GenericKB corpus문장 중에서 MSCOCO, VATEX와 같은 caption corpus에서 자주 나오는 noun을 갖는 문장들을 선택
2. 이후 관심있는 number word가 하나 이상 포함된 문장을 선택
3. human annotator 검증 과정

→ 10,492 문장

### 2.4. Statistics of NUMERSENSE

- train, test set의 숫자 분포가 조금 다르지만, 크게 보면 2,3,4가 제일 많음

## 3. Empirical Analysis

### 3.1. Experiment Set-up

- zero-shot inference
    - pre-train masked-word-prediction head로 BERT, RoBERTa 사용
- additional supervision via fine-tuning
    - 2.3에서 수집한 additional supervision dataset를 사용
    - 각 문장의 number word를 masking

→ 이후 masked sentences에 대해 fine-tuning

### 3.2. Evaluation Metric and Human Bound

- masked-word-prediction head가 vocab에 대한 확률 분포 생성

    → `NUMERSENSE` 가 , 모든 number word의 순위를 매기기 위해 이 확률 분포 사용

- hit @ 1/2/3 evaluation 사용; top k개의 number word중, correct number의 %
- human performance evaluation; open-book, closed-book

### 3.3. Experiment results

- model size 클수록 성능 높
- BERT보다 RoBERTa가 성능 높(더 큰 corpus에서 training되고, pre-training에서 MLM에 더 초점을 두기 때문)
- 그럼에도, 여전히 human의 절반 수준임
- 모든 모델은 adversarial set에서 성능 하락함 ; 앞으로 PTLM은 structured inductive biases을 더 고려해야할 것임

## 4. Case Studies

- object bias
    - RoBERTa의 pre-train strategy가 bias를 줄여줌
- attention distribution
    - root word "has"가 처음 몇 layer에서 attention높고, number word인 "two"는 마지막 layer에서 attention이 높았음

    → 즉, sub/obj와 number word의 relation을 BERT, RoBERTa에서 많이 잃어버림


## 5. Open-Domain ‘How-Many’ Questions

- `NUMERSENSE` 로 common sense의 "how many" question을 targeting할 수 있음

## 6. Related Work

- Probing Tasks for PTLMs
- Probing Commonsense Knowledge
- Numerical Commonsense Knowledge
- Encoding Numerics for Computation

## 7. Conclusion
