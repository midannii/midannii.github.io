---
layout: post
title:  "[Stanford/CS224n 심화] word2vec 구현하기 "
date:   2021-04-13
categories: NLP
---

<br>


앞서 배운 word2vec을 실제로 구현해봐야 내가 제대로 알 거 같다는 생각이 들었담,,


우선 영어 버전의 word2vec의 경우, `gensim`의 [models.word2vec](https://radimrehurek.com/gensim/models/word2vec.html)에 미리 학습된 word2vec model이 존재한다.

아래와 같이 로드해서 사용하면 된다.

<br>

```python
from gensim.test.utils import common_texts
from gensim.models import Word2Vec
model = Word2Vec(sentences=common_texts, vector_size=100, window=5, min_count=1, workers=4)

# model.save("word2vec.model")
# model = Word2Vec.load("word2vec.model")

model.train([["hello", "world"]], total_examples=1, epochs=1)
```

이를 바탕으로, 의미가 비슷한 단어를 찾아낼 수도 있다.


```python
vector = model.wv['computer']  # get numpy vector of a word
sims = model.wv.most_similar('computer', topn=10)  # get other similar words
```
<br>

물론 한국어의 경우에도, github에 다른 분들이 올려주신 pre-trained wore2vec model이 있다.

https://github.com/Kyubyong/wordvectors 에서 다운받고 설치하면 되지롱 ! [눌러서 다운로드](https://drive.google.com/file/d/0B0ZXk88koS2KbDhXdWg1Q2RydlU/view)

```python
ko_model = gensim.models.Word2Vec.load('./model/ko/ko.bin')
a = ko_model.wv.most_similar("강아지")
print(a) # 강아지와 유사도가 비슷한 단어 & 그 유사도 set
```

<br>

그런데 더 나아가서, 직접 원리를 눈으로 확인해보고 싶어졌다! [다른 분들의 블로그]https://sdc-james.gitbook.io/onebook/5./6.1./6.1.3.-word2vec)를 보고 공부해봤다.

사용된 데이터는 https://github.com/e9t/nsmc.git 에서 다운받을 수 있다.


`ratings.txt` = `ratings_train.txt` + `ratings_test.txt` 이다.


```python
# open file
import csv

f = open('nsmc/ratings_train.txt', 'r', encoding='utf-8')
rdr = csv.reader(f, delimiter='\t')
rdw = list(rdr)

f.close()
```


`ratings.txt`, `ratings_train.txt`, `ratings_test.txt`의 각 파일에 있는 Column 은 <영화 아이디, 영화 평, 영화 평점> 이고,

r을 출력해보면 아래와 같다.


```python
print(rdw[:5])

#[['id', 'document', 'label'],
# ['9976970', '아 더빙.. 진짜 짜증나네요 목소리', '0'],
# ['3819312', '흠...포스터보고 초딩영화줄....오버연기조차 가볍지 않구나', '1'],
# ['10265843', '너무재밓었다그래서보는것을추천한다', '0'],
# ['9045019', '교도소 이야기구먼 ..솔직히 재미는 없다..평점 조정', '0']]

```



<br>

우선 형태소 분석기를 통해 Josa, Eomi, Punctuation 는 제외한 단어들의 목록을 리스트에 저장한당.



```python
result = []
for line in rdw: # ['6684036', '나중이라도 꼭 한번 보고싶습니다!!', '1']

    #형태소 분석하기, 단어 기본형 사용
    malist = twitter.pos( line[1], norm=True, stem=True) # [('나중', 'Noun'), ('이라도', 'Josa'),('꼭', 'Noun'), ('한번', 'Noun'), ('보다', 'Verb'), ('!!', 'Punctuation')]
    r = []
    for word in malist:
        #Josa”, “Eomi”, “'Punctuation” 는 제외하고 처리
        if not word[1] in ["Josa","Eomi","Punctuation"]:
            r.append(word[0])
     #형태소 사이에 공백 " "  을 넣습니다. 그리고 양쪽 공백을 지웁니다.
    rl = (" ".join(r)).strip() # '나중 꼭 한번 보다'
    result.append(rl)
```

이후 아래와같이 word2vec을 학습 시켜 model을 만들어 사용한다.

```python
#형태소들을 별도의 파일로 저장 합니다.
 with open("NaverMovie.nlp",'w', encoding='utf-8') as fp:
     fp.write("\n".join(result))

#Word2Vec 모델 만들기
wData = word2vec.LineSentence("NaverMovie.nlp")
wModel =word2vec.Word2Vec(wData, size=200, window=10, hs=1, min_count=2, sg=1)
#wModel.save("NaverMovie.model")
#print("Word2Vec Modeling finished")

#wModel = word2vec.Word2Vec.load("NaverMovie.model")
print(model.most_similar(positive=["재미"]))
#[('ㅠㅡㅜ', 0.5964617729187012), ('없슴', 0.596453070640564), ('나용', 0.5938360095024109), ('푸시', 0.5832706689834595), ('사고사', 0.5669316053390503), ('이상하', 0.5638896822929382), ('섹씬', 0.5629161596298218), ('유익', 0.5620923042297363), ('재미없다', 0.5620167255401611), ('지렁이', 0.558078408241272)]

print(model.most_similar(positive=["최고"]))
#[('꼽을', 0.7225841283798218), ('꼽는', 0.6934826970100403), ('최고다', 0.6918784976005554), ('단연', 0.6749929785728455), ('ER', 0.6678911447525024), ('하이스쿨', 0.6403142809867859), ('손꼽다', 0.6399452686309814), ('으뜸', 0.6315333843231201), ('손에꼽는', 0.6309640407562256), ('투윅스', 0.6254955530166626)]
```


<br>

그런데 이것도 마찬가지로 구현된 word2vec을 사용한 거라,,,

진짜 pytorch로 word2vec 구현하신 분의 코드 http://cedartrees.co.kr/index.php/2020/12/30/word2vec-pytorch/ 도 공부했다.

-> [colab](https://colab.research.google.com/drive/1KJhtGhLJr2RZOvo8Qzr6PfGdCDKiqyu1)으로 정리해둠 !!



<br>



## Reference

- https://radimrehurek.com/gensim/models/word2vec.html

- https://omicro03.medium.com/%EC%9E%90%EC%97%B0%EC%96%B4%EC%B2%98%EB%A6%AC-nlp-14%EC%9D%BC%EC%B0%A8-word2vec-%EC%8B%A4%EC%8A%B52-8e518a358b6c


<br>
