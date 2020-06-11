---
title: "[2018]Text Classification: Language Identification"
date: 2018-03-02T16:44:43-06:00
---

**Project Title** : Language Identification on English, French, and Spanish
**Skill**:  python
**Toolkit**: nltk
**Summary** :

• Implemented statistical letter and word-based n-gram models with smoothing for English, Italian, and Spanish 
• Achieved above 95% for both letter and word-based language models for test sets between 300 to 2000 sentences <!--more--> 
### Language Model
- A statistical model
- a probability distribution over sequences of words
- Given a string with a length of m, it assigns a probability to the whole sequence

My note on <a href="/content/language-model.md">language model</a>

Estimating the relative likelihood of different phrases is useful in many natural language processing applications, especially those that generate text as an output. 
We can find language model in **speech recognition**, **machine translation**, **part-of-speech tagging**, **parsing**, **handwriting recognition**, **information retrieval** and other applications.

The program can be used when we want to **detect current keyboard language in multi-language computer**. 
For instance, 
> "Il sait pertinemment que notre Reine est un chef d ' État apolitique . " is **French**
> "Il processo verbale della seduta di ieri è stato distribuito ." is **Spanish**

### Models
1. Letter Bigram: find the probability distribution over sequences of characters.
-- Language Model &rarr; P(n|a), P(g|n)
2. Word Bigram: find the probability distribution over sequences of words.
-- Language Model &rarr; P(model|Language)

### Advantage of the Letter Model:
- the size of the bigram matrixes are much smaller compare to those word model. 
-- For a single language, the matrix is around 100*100. 
- The bigram matrixes are not as sparse as those in the word model

### Disadvantages of the Letter Model: 
- might not work for short strings, because the possible combination of Latin alphabet is limited and our training language is similar. 

### Advantage of the Word Model
- After smoothing and considering out-of-vocabulary words, the performance of #2 is better than the performance of #1
- It can deal with short strings

### Disadvantages for the Word Model: 
- The size of bigram matrixes are much bigger than those in letter model
-- The types of words are much more than types of character 
-- From all three languages, the matrix is at least 7000*7000. 
- It requires smoothing since the bigram is very sparse, otherwise, the performance will be lower than 80%. Moreover, there are out-of-vocabulary words. 

### Outcome

- Comparing the **letter model*'s output to the solution file, the output answer matches 99.6% of the solution file.
-- The letter model can be implemented without any kind of smoothing and still have above 85% correctness.
-- However, it will cause some sentence to be **unidentifiable**, because there are some sentences' possibility is zero for all three languages. 
-- Some letter sequences are not in bigram because it’s **proper noun**.
-- Better to implement the basic **add-one smoothing**.

- Comparing the **word model**'s output with the solution file, the output answer matches 100% of the solution file
-- also required OOV and smoothing