---
layout: post
title:  "[NLP] Blenderbot 2.0 ; Beyond Goldfish Memory: Long-Term Open-Domain Conversation "
date:   2021-09-08
desc: " "
keywords: "DL, ML"
categories: [Deeplearning]
tags: [DL, ML]
icon: icon-html
---


## Abstract 

- we collect and release a human-human dataset consisting of multiple chat sessions

- We show how existing models trained on existing datasets perform poorly in this long-term conversation setting in both automatic and human evaluations, 

- we study long-context models that can perform much better.

- we find retrieval-augmented methods and methods with an ability to summarize and recall previous conversations outperform the standard encoder-decoder architectures currently considered state of the art.

<br>


## 1. Introduction 

we study methods for long-term open-domain conversation. As to the best of our knowledge no public domain task exists to study such methods

we collect and release a new English dataset, entitled Multi-Session Chat (MSC). The dataset consists of human-human crowdworker chats over 5 sessions

- 각 session은 최대 14개의 utterance로 이루어져 있음 

- 이전 session에는 이후의 대화에 유용할 수 있는 personal point의 summarization이 annotate됨 

→ 이를 fine-tuning에 적용함 


We study the performance of two long-context conversational architectures on this task: 

(i) retrieval-augmented generative models

(ii) a proposed read-write memory-based model that summarizes and stores conversation on the fly.


<br>


## 3. Multi-Session Chat 


우리가 사용한 dataset은 MSC(multi-session chat) 

- " individual chat session is long"  == 중간에 몇시간 ~ 며칠 paused 됨 

      - 우리는 이러한 multi-session long conversation setup을 고려함 


- Session 1에서는 기존 PERSONACHAT인 짧은 대화 내용을 바탕으로 처음 만난 두 사람의 간략한 정보만 서로 나누는 대화 태스크로 진행이 됩니다.
- Session 2~4까지는 정해진 시간의 term(1~7시간 단위 또는1~7일 단위)을 두고, 기존 persona를 유지하고 이전에 했던 대화를 반영한 대화를 나누게 합니다. 마치 이전에 대화하다가 시간이 흘러 다시 대화를 나누는 두 사람처럼 주제가 확대될 때도 이전에 나눈 대화와 일관성이 있게 대화를 진행하게 합니다.
- Conversation summaries (확장된 persona) 워커간에 이전 대화를 볼 수도 있고, 이전 히스토리의 파악이 가능한 환경을 주더라도 사람은 제한된 시간내에 그 정보를 읽고 활용하기는 한계가 있기 마련입니다. 따라서 각 session에서 중요 포인트를 기록하고 요약하여 대화의 참고 자료로 활용하게 합니다.
- Dataset statics를 보면 이전에 구축한 짧은 2.6 ~ 14.77의 짧은 턴에 비해 MSC는 53 ~ 66턴 등의 긴 발화수를 가집니다.

몇시간 ~ 며칠 pause된 후 재개할때, 화자는 (i) 이전 주제에 대해 계속 이야기하거나 (ii) 과거 공유 기록에서 다른 주제를 꺼내거나 (iii) 새로운 대화를 다시 시작함 

- CrowdWorkers + 1155명의 Personas 사용; 첫번째 대화 이후 시간이 흐른 뒤 대화 하도록 .. or 그런 척 

      - 4000 episodes with 3 sessions, and 1001 episodes with 4 sessions.



<br>
