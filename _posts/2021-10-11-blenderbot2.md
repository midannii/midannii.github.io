---
layout: post
title:  "[NLP] Blenderbot 2.0 ; Internet-Augmented Dialogue Generation "
date:   2021-10-11
categories: NLP Papers
---

## Abstract

- 본 연구에서는,
    - conversational agent에게, 방대한 information의 접근 권한 주기
    - context-base의  internet search query를 ****generate하도록 학습하는 approach 제안
    - search result에 대한 조건을 지정하여 최종적인 response를 generate하도록 제안

## 1. Introduction

- we study generative models that are instead capable of accessing the vast knowledge of the internet dynamically in order to inform their responses. Utilizing encoder-decoder architectures, we consider models that, given a dialogue context, first generate a search query.

    → 만들어진 query는 Fusion-in-Decoder를 사용하여, Encoding된 대화 기록 앞에 추가된 관련 지식을 검색하는 데 사용됨

    → 이렇게 Encoding된 knowledge를 고려하여 최종적으로 decoder를 사용하여 응답이 생성됨

- collect a new crowdsourced English dataset involving human-human conversations, where one of the workers plays the role of a “wizard” who conducts internet searches in order to inform their responses during knowledge-grounded conversations.
    - 모든걸 다 아는 wizard와, 질문을 던지는 견습생
- We show that internet-augmented models trained to replace the human wizard outperform conventional non-augmented generation models on this task as measured by automatic metrics as well as human evaluations.

    ![스크린샷 2021-08-15 오전 12.30.35.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24d91ed8-d113-4ee2-9ea1-121f38e5f94c/스크린샷_2021-08-15_오전_12.30.35.png)


## 3. Internet - Augmented Generation

- internet의 webpage에 access하는 2가지 방법

    (i) using a cached set of pages that are stored in a distributed approximate nearest- neighbor database, FAISS (Johnson et al., 2019) (a.k.a `FAISS-based methods`)

    - RAG (Retrieval Augmented Generation)
        - which consists of two components which are trained end-to-end:

            (i) the neural-in-the-loop retrieval system

            (ii) an encoder-decoder for generating final responses given the results of the retrieval.

    - FiD (Fusion in Decoder)
        - pre-train retriever is used
        - each of the top N documents returned is prepended to the context and encoded separately by the encoder, and finally all the results are concatenated.
    - FiD-RAG
        - FiD approach
        - no end-to-end training
        - First the retriever is trained in a RAG setup, and then FiD is used with that retriever. This was shown to give superior results to both RAG and FiD on dialogue tasks.
    - FAISS + Search Query-based Retrieval
        - an encoder-decoder is employed to generate a search query given the context. The search query is input into a DPR model to produce a dense vector, and is matched to documents in the FAISS index. Returned documents can then be used in the final response generation encoder- decoder as before.

    이러한 방법은 several disadvantage 존재

    1. they may be difficult to update to real-time web documents;
    2. there may be a limit to the number of documents storable in local FAISS deployments;
    3. such methods will not take advantage of the high quality ranking that has been finely tuned in Internet Search engines over decades of use.

    (ii) using an Internet Search Engine directly to retrieve pages (a.k.a `SEA` )

    - 아래와 같은 2가지 components로 구성되어 있음
        - A search query generator
            - `encoder-decoder Transformer`; dialogue context(input) → search query를 생성
            - This is given to the black-box search engine API, and N documents are returned.
            - 필요한 것 ; `(context, search query)` pair
        - A FiD-style encoder-decoder model
            - 각각의 document를 encode하고, dialogue context encoding으로 concatenate하고, next response를 생성
            - Fid 사용함
                - 기존 Fid; training search query에 대한 관련 document context를 구축하기 위해, trained search query generator를 사용하여 반환된 검색 결과를 사용
                - Fid-Gold; training set에 대해 사람이 작성한 search query와, their corresponding search result를 둘다 사용
            - 필요한 것 ; `(context, response)` pair
    - Search Engine
        - 본 연구에서는 Bling Search API 사용 (다른 모델로 대체 가능)
            - 각 query 에 대한 url list 생성 후, 해당 query에 대한 page sets를 채우기 위해 생성된 url을 key로 사용하여 page content를 찾음 (from lookup table we built for our Common Crawl snapshot)

                → This makes our comparison more direct with our FAISS-based methods. + wikipedia url인지 확인 가능

    - Knowledge Response Regularization
        - retrieval을 통해 augment하는 large LM 에서 발생하는 문제점 ;
            - LM의 weight 내에서 기억된 지식을 copy하는 것과, retrieved document에서 제공되는 지식 중 하나를 선택할때 trouble 발생

                → general regularization을 통해 이를 보완함

                - we propose a general regularization method to more finely control this mechanism:

                    when training, we multi-task between the (i) original response generation task and (ii) a new task which consists of generating the selected knowledge from retrieved documents indicated by human annotators.

                    - (ii) 는 retrieved document를 encourage하는 regularizer임

## 4. Wizard of the Internet Task

    - search engine in-the-loop 을 사용할 수 있는 generative model을 학습하고 평가하기 위해, 데이터셋 설계 &수집
        - One plays the role of the `wizard`, who has access to a search engine during conversation, the other, `the apprentice`, does not.
            - `apprentice` 는  assigned persona 존재 ; 관심사 존재
                - 관심사는 본인이 선택함 e.g. I love tennis, Rafael Nadal is my favorite player (기존 데이터셋에서 관심사 목록 가져옴)
                - 관심사에 대한 충분한 지식 가지므로, 이에 대한 합리적 대화 가능
            - **The purpose of the exchange is to have an “in-depth conversation about [those] assigned interests”.**
        - `wizard` 와 `apprentice` 중 누가 먼저 말할지는 랜덤
        - `wizard` 는 매 차례마다 자유 text 검색 가능 & 다른 검색어 자유롭게 시도 가능
            - 검색결과를 반영한 대답을 할수도 있고, 무시한 대답을 할수도 있음
        - 대화는 봇보다 인간의 interest을 중심으로 할 가능성이 더 높으며, 봇은 search engine을 사용한 지식을 기반으로 합니다. 따라서 이 작업에 대해 교육하거나 평가할 때 지정된 모델이 `wizard`의 역할을 대체함.

### 4.1. Overall Dataset

    - 9633 dialogues in total, with 82952 utterances in the training set, and validation and test sets of 5781 and 4932 utterances




## 5. Experiments

### 5.2. Result

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7126dd81-6e09-4cca-85c1-ab2e4a10f59b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7126dd81-6e09-4cca-85c1-ab2e4a10f59b/Untitled.png)

    - KF1; gold의 체크와 실제 모델의 체크가 얼마나 일치하는지 -
        - 체크란 ?
    - pre-train에는 Blenderbot 2.7B(no knowledge)와 BART-Large(gold knowledge)사용
        - No Knowledge vs gold knowledge baselines
            - `No Knowledge`; non-retrieval augmented model
            - `gold knowledge`; human annotators (wizards) labeled as being used to craft responses.
            - We train models on the `Wizard of Wikipedia (WoW)` dataset as baselines, to compare the difference between coverage of the WoW task and our new WizInt task, in both the no knowledge and gold knowledge settings.
                - wow와 wixint 의 multi-tasking 가능

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2c02daff-3b54-400a-a92d-b8421527ddb8/Untitled.png)

### 5.3. Human Evaluation

    We compared the `WizInt BART-Large Transformer (no-knowledge) model`, which is a standard Transformer with no retrieval augmentation, to the `WizInternet Search engine FiD model, with live Bing search` (without using a CC subset). The results are given in Table 6. For each model, around 750 responses were annotated over nearly 100 model conversations. The search engine-based method outperformed the no-knowledge baseline across the board. Not only was the search engine-based model judged to be knowledgeable more often (46.5% vs. 38.6% of the time) and factually incorrect less often (5.3% vs. 7.1%), but it also was measured to be more consistent (76.1% vs. 66.5%) and more engaging (81.4% vs. 69.9% on an ut- terance level, and 3.73 vs. 3.64 on a conversation level).

## 6. Conclusions

    This work has studied the problem of siloed knowledge in large language models, whereby they cannot access the knowledge of the world other than through their fixed training set.
    Developing methods that instead can access the internet as an augmentation to the generation process, we have showed such models can display more knowledge and generate less factually incorrect information during dialogue with humans.
