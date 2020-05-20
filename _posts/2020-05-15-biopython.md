---
layout: post
title:  "How to use BioPython: Alignment with muscle"
date:   2020-05-15
desc: "using biopython"
keywords: "database, DB, DBMS"
categories: [Python]
tags: [python, biology, BioPython, alignment, muscle]
icon: icon-html
---


`biopython` 의 많은 기능들 중에서, muscle을 이용하여 FASTA format의 protein sequence를 alignment 하자.

 본 포스팅에서는 HLA 의 [fasta file](https://github.com/ANHIG/IMGTHLA)을 이용하였다.


<br>
## step 1) download files


```
$ git clone https://github.com/ANHIG/IMGTHLA.git
```

으로 repository 내의 전체 파일을 다운받는다.

이 파일의 내부 정보는 [README.md](https://github.com/ANHIG/IMGTHLA)에 담겨있는데, 일부를 가져오면


```python
FASTA folder
All files in this folder are provided in the FASTA sequence format. Please note the FASTA format contains no alignment information.

Files designated “X_prot.fasta”, where X is a locus or gene, contain protein sequences. Please note that alleles that contain non-coding variations may be identical at the protein level.

Files designated “X_nuc.fasta”, where X is a locus or gene, contain the nucleotide coding sequences (CDS). Please note that alleles that contain non-coding variations may be identical at the CDS level.

Files designated “X_gen.fasta”, where X is a locus or gene, contain genomic DNA sequences. Please note for alleles that do not possess genomic sequences, there will be no entry in the file.
```

와 같다. 이 중에서 `X_prot.fasta`의 format을 갖는 protein 파일들만 이용하도록 하자.

본 포스팅에서는 HLA-A의 fasta data인 **A_prot.fasta** 에 대해 설명한다.



<br>

## step 2) Biopython library load & data load

```python
import Bio

import os
os.chdir('path/IMGTHLA/fasta')

```

```python
# for HLA-A
from Bio import SeqIO, Seq
input_file = 'A_prot.fasta'
records = SeqIO.parse(input_file, 'fasta')
records = list(records) # make a copy, otherwise our generator is exhausted after calculating maxlen
maxlen = max(len(record.seq) for record in records)

# pad sequences so that they all have the same length
for record in records:
    if len(record.seq) != maxlen:
        sequence = str(record.seq).ljust(maxlen, '.')
        record.seq = Seq.Seq(sequence)
assert all(len(record.seq) == maxlen for record in records)
```


<br>

## step 3) make dataframe with fasta information


우선 description을 모아 저장해두자.

```python
des = []
for record in records:
    des.append(record.description)
```

각각의 description은 `HLA:HLA00001 A*01:01:01:01 365 bp` 꼴로 이루어져 있으며,

`A*01:01:01:01`는 각각 gene이름(A), separator( * )이고,

`01:01:01:01`은 앞에서부터 allele group, specific HLA protein, synonymous DNA substitution, non-coding region과의 차이를 ,

`365bp`는 길이를 의미한다.


여기에서 우리는 01:01:01:01을 따서 full_id로, 01:01을 id로 저장하고

중복되는 id들을 제거하기 위해서 맨 처음으로 나오는 full_id를 채택한다.


```python
ids = []
full_ids = []
delete = []
name = []
for i in range(len(records)):
    des = records[i].description[13:] # A*01:01:01:01 365 bp
    if des[0] == 'A':
        full_ids.append(des[2:des.find('bp')-5])
        name.append(records[i].name)
        if des.count(':')<2:
            ids.append(des[2:des.find('bp')-4])
        else:
            ids.append(des[2:des.find(':', des.find(':')+1)])
```


또한 dataframe으로 만들었으며, 아래와 같다.

```python
df = pd.DataFrame(data = {'ids': ids, 'full_id': full_ids, 'name':name})
```

![df](/static/assets/img/blog/python/df.png){: width="400" height="300"}



이제 중복을 제거하여 dataframe으로 저장하자.


```python
uniques = list(df['ids'].unique())

overlap = [] # alleles of overlapped
not_overlap = [] # alleles of not overlapped
for uniq in uniques:
    if sum(df.ids == uniq) != 1:
        overlap.append(uniq)
    else:
        not_overlap.append(uniq)

only = [] # delete overlapped alleles
for ovl in overlap:
    ovls = []
    for i in range(len(df)):
        if df.ids[i] == ovl:
            ovls.append(i)
        elif df.ids[i] in not_overlap:
            only.append(i)
    only.append(ovls[0])
only = sorted(list(set(only)))

```


```python
# dataframe으로 저장
data = np.asarray(df)[only]

hla_a = []
a_name = []
for i in range(len(data)):
    hla_a.append('HLA-A-' + data[i][0][0:2] + data[i][0][3:5])
    a_name.append(data[i][2])
```


![df](/static/assets/img/blog/python/df2.png){: width="400" height="300"}


<br>

## step 4) fasta file download


```python
from Bio import Align
from Bio.Align import Applications
from Bio.Align.Applications import MuscleCommandline
from Bio import SeqIO
records = (r for r in SeqIO.parse('A_prot.fasta', 'fasta') if len(r)<900)
```

이때, `len(r)<900` 이라는 조건은 필수는 아니지만, 비정상적으로 길이가 긴 내용을 거르기 위해서 추가한 코드이다.



```python
from io import StringIO
handle = StringIO()
SeqIO.write(records, handle, 'fasta')
data = handle.getvalue()
```

string을 handle로 이용하여, 파일 속 정보를 불러온다.

handle을 이용함으로서 크기가 큰 파일을 용이하게 다룰 수 있다.


<br>


이후 로드한 파일에 대해 3과 비슷한 원리로 **name, id, full_id, sequence가 담겨있는 dataframe** 을 만든다.

이때 각 line의 구분은 따로 없으며, 내용 상의 구분은 **>** 으로 이루어져 있으니 아래와 같이 코딩한다.


```python
lines = []
ind1 = 0
ind2 = 0
while ind2 != -1:
  ind2 = data.find('>', ind1+1)
  lines.append(data[ind1:ind2])

```

```python
name = []
ids = []
full_id = []
seq = []
for line in lines:
  a = line.find('*')
  name.append(line[1:a-2]) # HLA:HLA00001
  b = line.find('bp')
  full_id.append(line[a+1:b-5]) #01:01:01:01
  ids.append(line[a+1:b-9]) #01:01
  seq.append(line[b+2:])

df = pd.DataFrame(data = {'name': name, 'id': ids, 'full id': full_id, 'sequences': seq})
```



<br>




## step 5) alignment with muscle


```python
muscle_exe = '/home/midan/_midanniiii/muscle3.8.31_i86linux64'
muscle_cline = MuscleCommandline(muscle_exe)
stdout, stderr = muscle_cline(stdin=data)
```


![muscle](/static/assets/img/blog/python/muscle.png){: width="400" height="300"}



```python
empty = []
new = []
for i in range(len(stdout)):
  if stdout[i] == '>':
    new.append(empty)
    empty = []
  else:
    empty.append(stdout[i])

align = []
for i in range(len(new)):
    align.append((','.join(new[i])).replace(',',''))
```


이제 alignment와 name을 모아 list로 정리하자.

```python
new_align = []
names = []
for i in range(len(align)):
    a = align[i]
    p = a.find('p')
    new_align.append(align[i][p+2:])
    names.append(align[i][:12])

for i in range(len(new_align)):
    new_align[i] = new_align[i].replace('\n','')
new_align = new_align[1:]
names = names[1:]


m = new_align[0].find('M')
for i in range(len(new_align)):
    new_align[i] = new_align[i][m:]

```


dataframe으로 저장하자.

```python
df_a = pd.DataFrame(data = {'align' : new_align, 'name':names})
a = pd.DataFrame(data = {'name': a_name, 'alleles': hla_a})
hla_a = pd.merge(a,df_a)
hla_a = hla_a.drop(['name'], axis=1)
```


또한 HLA-A-0101은 365bp이므로, 이를 기준으로 365개를 추리자.

이를 위해서는 불필요한 **-** 를 잘라내야 한다.

```python
aligns = list(hla_a['align'])
align = aligns[0]
indices = []
for i in range(len(align)):
    if align[i] =='-':
        indices.append(i)

new_align = []
for i in range(len(aligns)):
    align = aligns[i]
    a = []
    for j in range(len(align)):
        if j not in indices:
            a.append(align[j])
    new_align.append(('').join(a))
    new_align[i] = new_align[i].replace('-','*')
```



<br>


## step 6) make dataframe & save


이제 만든 data를 dataframe으로 만들어서 저장하자.


```python
hla_a = pd.DataFrame(data = {'alleles':list(hla_a['alleles']), 'align': new_align})
hla_a.to_csv('/home/data/HLA_A_prot.txt', index=False, header=None, sep='\t')
```

![df](/static/assets/img/blog/python/df3.png){: width="400" height="300"}




<br>





<br>
