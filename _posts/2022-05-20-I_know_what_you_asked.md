---
layout: post
title:  "[NLP] I Know What You Asked: Graph Path Learning using AMR for Commonsense Reasoning"
date:   2022-05-20
categories: PaperReview
---

- COLING 2020에 투고됨

- CommonsenseQA, Graph

- [paper link](https://aclanthology.org/2020.coling-main.222.pdf)

<br>

### Abstract

- CommonsenseQA 란, pre-defined knowledge를 사용하는 commonsense reasoning을 통해 올바른 정답을 predict하는 task
    - 대부분의 선행 연구는 question의 semantic representation을 고려하여 정답을 predict하지 않고, distributed representation으로만 성능을 향상하려고 함
- 본 논문에서는, question의 semantic interpretation을 고려하는 AMR-ConceptNet-Pruned (ACP) graph를 제안함
    - ACP graph는, input question으로부터 만들어진 Abstract Meaning Representation(AMR) 그래프와 external commonsense knowledge graph인 CN(ConceptNet)을 포함하는 전체 통합 그래프에서 prune됨 → 그런 다음 ACP 그래프를 활용하여 추론 경로를 해석하고 CommonsenseQA 작업에 대한 정답을 예측
    - 이 논문은 commonsense reasoning이 ACP 그래프가 제공하는 relations와 concept으로 interprete될 수 있는 방식을 제시
    - ACP 기반 모델은 baseline보다 outperform함을 보임

---

## 1. Introduction

- Commonsense reasoning == commonsense information을 사용하여 logical inference하는 process
    - Commonsense == knowledge shared by the majority of people in society and acquired naturally in everyday life.
    - chain을 가짐(e.g. “Blowfish is fish.” → “Fish lives in the water.” → “Water includes seas and rivers.” ⇒ “Blowfish lives in the sea.”)

        → 사람은 쉽게 추론 가능하지만, 모델은 그럴수 없음

    - 2가지 종류
        - using pre-trained language models with distributed representations, which exhibit high performances on most Natural Language Processing (NLP) tasks

            → these models must be trained with an excessive number of parameters and cannot explain the process of commonsense reasoning.

        - reasoning with a commonsense knowledge graph
            - e.g. ConceptNet

            → 기존의 reasoning with a commonsense knowledge graph은 질문의 interpretation이 충분하지 않음

- 본 논문에서는 AMR-ConceptNet-Pruned (ACP) graph를 제안함

    ConceptNet의 commonsense relation with pruning를 확장한 새로운 소형 AMR 그래프(즉 require-01 → purpose → live-01 → ARG0 → blowfish → AtLocation → sea)

    - LM이 AMR 그래프를 활용하여 문장의 논리적 구조를 이해할 수 있도록 함
        - Abstract Meaning Representation (AMR) illustrates “who is doing what to whom” that is implied in a sentence with a graph.
    - AMR 그래프와 ConceptNet 간의 dynamic interaction 이용하여 commonsense reasoning

## 2. Proposed Method

![fig](https://ai2-s2-public.s3.amazonaws.com/figures/2017-08-08/2c2d0edf65a26ccc6b32a8ed934074d40c9ed39e/3-Figure2-1.png)

- AMR logic structure 기반의 commonsense knowledge를 사용하는 commonsense reasoning framework를 제안
    1. generating the AMR graph from every question in the CommonsenseQA dataset
    2. integrating all the nodes of AMR with ConceptNet graphs.
- flow
    1. relation에 따라 AMR graph를 pruning함

        AMR-ConceptNet full graph는 적절하지 않은 relation을 포함하고 있기 때문에, interpreting questions이 잘못될 수 있기 때문

    2. graph path learning module이 pruned graph를 입력으로 받아, 전체 그래프 벡터를 생성하는 Graph Transformer를 사용하여 각 path의 Attention score를 계산
    3. 그래프 벡터는 최종적으로 Transformer에 들어감
        - AMR과 ConceptNet 그래프 간의 상호 작용을 모델링하고 최종 그래프 representation으로 변환하기 위해
    4. 데이터 세트의 질문과 후보 답변은 언어 모델 인코더를 통해 전달되어 언어 벡터를 생성
    5. 언어벡터(4)와 그래프 벡터(3)의 concat
    6. 정답을 예측하는 데 사용되는 final representation

### 2.1. Graph Integrating and Pruning

- Owing to these advantages of the graph structure and preserved semantic interpretation, we use the AMR graph for extracting commonsense knowledge graphs.
- commonsense reasoning을 위해 효과적으로 AMR을 expanding & pruning rules

    → 엄청난 수의 경로를 반복적으로 발견하는 것을 방지하기 위함

    - We expand the AMR graph on all nodes with the ConceptNet and prune the nodes that have edges known as ARG0 and ARG1 with ConceptNet.
    - Considering that ARG0 and ARG1 are the top two frequent relations among any other relations as shown in Table 1, we prune the full AMR-CN graph into a more compact graph that only contains ARG0 and ARG1 relations, which is called ACP graph.
    - we remove the ConceptNet relations and nodes connected to the frame node.
    - the frame node’s specific meaning additionally annotated by the number like “-01” at the end of the word is different from the meaning in ConceptNet’s node even though it has identical letters.
- graph 설명

    ![graph](https://github.com/midannii/midannii.github.io/blob/master/statics/assets/I_know_what_you_asked/graph.png){:height="300px" width="500px"}


### 2.2. Language Encoder and Graph Path Learning Module

- model receives two types of inputs (text and graph) and converts semantic representation to distributed representation

    → `[CLS]+Question+[SEP]+candidate answer` 로 만듦

    → graph integrating&pruning module의 ACP 그래프가 주어지면, Graph Path Learning Module은 GloVe를 사용한 concept embedding과 absolute position embedding의 합으로 concept node vector를 initialize

- ACP 그래프의 relation path에 대한 모델 reasoning를 만들기 위해 graph transformer를 수정
    - 모델이 explicit graph path를 인식할 수 있도록, 먼저 relation encoder를 사용하여 두 concept 간의 관계를 분산 표현으로 encoding
    - relation encoder는 두 concept간의 최단 경로를 identify하고, GRU(Gated Recurrent Unit)가 있는 RNN을 사용하여 sequence를 relation vector로 표현
- To inject this relation information into the concept representation, we follow the idea of relative position embedding (which introduces the attention score method based on both the concept representations and their relation representation)

→ concepts and their relations를 고려한 attention score를 계산

- ci, cj 는 다음과 같은 concept embedding임

    ![ci_cj](https://github.com/midannii/midannii.github.io/blob/master/statics/assets/I_know_what_you_asked/sij.png){:height="300px" width="500px"}

    AMR 그래프와 ConceptNet의 서로 다른 두 가지 개념 유형을 단일 그래프로 통합하기 때문에 모델은 해석 중에 질문과 관련성이 높은 경로를 전역적으로 인식
    언어 및 그래프 벡터를 얻은 후 모델은 두 벡터를 연결하고 이를 Softmax 계층에 공급하고 정답을 선택


→ attention score는 fully-connected communication을 유지하면서 concept embedding을 업데이트함

→  이를 통해 concept-relation interaction을 concept node vector에 inject가능

- 이러한 concept vector은 전체 그래프 벡터로 합산되고, AMR과 ConceptNet concept relation간의 interaction을 모델링하기 위해 Transformer Layer에 들어감
    - The major advantage of this relation-enhanced attention mechanism is that it provides a fully connected view of input graphs by making use of the relation multi-head attention mechanisms.
    - Since we integrate two different concept types from the AMR graph and ConceptNet into a single graph, the model globally recognizes which path has high relevance to the question during the interpretation.
- language, graph vector를 얻은 후,  모델은 두 벡터를 concat하고 이를 Softmax layer에 넣어 정답을 선택

## 3. Experiments

### 3.1. Data and Experimental Setup

- CommonsenseQA dataset
    - 12,102 (v1.11) natural language questions and each question has five candidate answers
        - training, development, and test sets included 8,500, 1,221, and 1,241 examples, respectively

### 3.2. Experimental Results

- Commonsense reasoning
- Comparison on different language models
- Extensions of BERT

    ![Table2](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS4783lkwsX6dI3HaeJY93oeLkmsbG6LXFotg&usqp=CAU){:height="300px" width="500px"}

- Comparison on different language models.

    ![table4](https://github.com/midannii/midannii.github.io/blob/master/statics/assets/I_know_what_you_asked/table4.png){:height="300px" width="500px"}

- Experiment on official test set

    ![table5](https://github.com/midannii/midannii.github.io/blob/master/statics/assets/I_know_what_you_asked/table5.png){:height="300px" width="500px"}


## 4. Discussion

### 4.1. Error Analysis

- Difficulty in discriminating hard distractor
    - CommonsenseQA의 candidate answers는 모두 question과 같은 relation을 가지고 있는 hard distractor임
    All candidate answers from the CommonsenseQA possess a hard distractor, which shares the same relation with the question. When the hard distractor exists in the ACP graph in the path learning module, it also uses the paths of the distractor, instead of the those of correct answer.

        → This may make the model confused as it considers the distractor as the correct answer.

- AMR graph generation error
    - Since the AMR graph is generated from the pre-trained model, our model is at risk of using an incorrect AMR graph. An incorrectly produced AMR graph may lead the model to incorrect interpretation and distortion in the wrong direction during the path reasoning process.

### 4.2. Case Study

ACP graph is capable of commonsense reasoning exploring the paths.
