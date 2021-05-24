# HacRED

Dataset for HacRED: A Large-Scale Relation Extraction Dataset Toward Hard Cases in Practical Applications

We first analyze the performance gap between popular datasets and practical applications, the underlying reason of which is that practical applications intrinsically have more hard cases.

To make RE models more robust on such practical hard cases, we propose the **Ha**rd **C**ase **R**elation
**E**xtraction **D**ataset (**HacRED**).

The HacRED consists of 65,225 relational facts annotated from 9,231 wiki documents with sufficient and diverse hard cases.

Notably, HacRED is one of the largest Chinese document-level RE datasets and achieves a high 96% F1 score on data quality and a more reasonable data distribution.

## Statistic

| Common Statistics | HacRED |
| ----------------- | ------ |
| \# Text           | 9,231  |
| \# Relation       | 26     |
| \# Triple         | 67,047 |
| \# Fact           | 65,225 |
| Avg. Sentences    | 5.0    |
| Avg. Words        | 126.6  |
| Avg. Chars        | 204.2  |
| Avg. Entities     | 10.8   |
| Avg. Triples      | 7.4    |

| Data Distribution                   | HacRED |
| ----------------------------------- | ------ |
| Ratio of Duplicated Triples         | 2.72%  |
| Ratio of Biased Relations           | 0.00%  |
| Ratio of Top-20% Relational Triples | 49.96% |

## File List & Download

main files:

- **train.json**: with 6231 json lines
- **dev.json**: with 1500 json lines
- **test.json**: with 1500 json lines, without ground-truth such as `labels_char` and `labels_word` values 
- **rel2id.json**: a dict mapping relation to id

meta-data files:

- **char2id.json**

- **word2id.json**

- **ner2id.json**

- **vec_100d.npy**: pretrained word embeddings from the paper **Analogical Reasoning on Chinese Morphological and Semantic Relations** (https://github.com/Embedding/Chinese-Word-Vectors)

- **vec_300d.npy**: similar to vec_100d.npy

- **schema.json**


1. Download from google drive:

https://drive.google.com/drive/folders/1T6QUfDV_ILAr6UJ_fROYQd4-NaFxIzqN?usp=sharing

2. 复旦大学知识工场实验室主页下载:

http://kw.fudan.edu.cn/

## Data Format

The format of data files is JSONL, including `train.json`, `dev.json`, `test.json`.

Each line contains relevant annotations of a document. The overall format is similar to the format of DocRED. Alternatively, we provided two kinds of data considering the characteristic of the Chinese.

If using the data at character-level in Chinese, `sent_char`, `vertex_char`, `labels_char` in HacRED are corresponding to `sents`, `vertexSet`, `labels` in DocRED, respectively.

If using the data at character-level in Chinese, `sent_word`, `vertex_word`, `labels_word` in HacRED are corresponding to `sents`, `vertexSet`, `labels` in DocRED, respectively.

- **id**: unique key can be used to identify a certain document
- **text**: text in a document
- **sents_char**: sentences in the document, each sentence is a list of tokens at character-level vocabulary fitting to the Chinese PLMs like `bert-base-chinese` etc.
- **sents_word**:  sentences in the document, each sentence is a list of tokens at word-level vocabulary, which means “中国”(China), two characters in Chinese, is regarded as one word.
- **vertex_char**: list of entities in each document. one entity may have multiple mentions in a document. each mention contains following values (`sent_id` and `pos` are computed based on `sents_char`):
    - name: entity mention
    - sent_id: mention in which sentences, index start from 0
    - type: ner type
    - pos: entity boundaries in certain sentence, for example sents_char[sent_id][pos[0]: pos[1]] would get the mention
- **vertex_word**: similar to `vertex_char`, but `sent_id` and `pos` are computed based on `sents_word`.
- **labels_char**: list of triples in each document. each triple contains following values:
    - r: relation label
    - h: head mention/subject index in `vertex_char`
    - t: tail mention/object index in `vertex_char`
- **labels_word**: similar to `labels_char`, but `h` and `t` are computed based on `vertex_word`.

```
{
    "id": 6020,   # unique key can be used to identify a certain document
    "text": "......"
    "sents_char": [
        ["token_1", "token_2", "token_3",..., "token_m"],   # tokens in sentence at char-level vocabulary
        [],	  # tokens in sentence 1
        [],
        ...
        []
    ],
    "vertex_char": [
        [
            {
                "name":"<a BOOK_NAME mention>",   # entity mention
                "sent_id":0,       # mention in which sentences, index start from 0
                "type":"WORK",     # ner type of entity
                "pos":[8,13]       # entity boundaries in certain sentence, for example sents_char[sent_id][pos[0]: pos[1]] would get the mention
            }, 
            {}, 
            {}   # one entity with multiple mentions in a document, name, sent_id, pos may be different
        ],
        [{}, {}],	# another entity
        ...,
        [{}] 
    ],
    "labels_char":[
        {
            "r":"<a RELATION label>",	# description of a relation label, not relation label_id
            "h":0,	# head entity (subject, e1) index in vertex_char, index start from 0
            "t":1	# tail entity (object, e2) index in vertex_char
        },
        {},
        ...
    ],
    "sents_word":[
        [],
        [],
        ...,
        []
    ], # same as sents_char format, but tokens in sentences at word-level vocabulary
    "vertex_word":[
        [{}, {}],
        [{}]
        ...
    ], # same as vertex_char format, but sentence index and boundaries are computed based on sents_word
    "labels_word":[
        {}, 
        {},
        ...
    ]	# same as labels_char format, but subject/object index are computed based on vertex_word
}
```

## Cite

Findings of ACL 2021