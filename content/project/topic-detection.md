---
title: "[2018]Topic Modeling"
date: 2018-12-14T21:10:20-06:00
---

**Project Title** :  Topic Modeling on Patient Review 
**Skill**:  python, corpus
**Toolkit**: Gensim, NLTK
**Summary** :

- Built topic model on RateMD database by applying the Latent Dirichlet Allocation model from Gensim library. 
- Conducted and analyzed text cleaning procedures and lemmatization to find the optimal model 

----
Topic model is a type of statistical model for discovering the abstract "topics" in a collection of text documents.
It is often used for discovery of hidden semantics structures in a text.

Latent Dirichlet Allocation (LDA) is an example of a topic model: a generative statistical model. It is used to classify text in a document to a particular topic.<!--more-->  
### Cleaning Procedure
- text to lowercase
- stop words and punctuations removal
- stemming vs. lemmatization
- other data(image, tag, video, etc) cleaning steps 

1. converted corpus to lowercase can eliminate the number of unique words
2. Stop words can cause high frequency as in Zipf’s Law. Punctuation is not necessary for text analyzing. 
3. Lemmatization might be a better case because stemming create words that might not be a real word in the dictionary. And can cause confusion when we manually label the topics.

### Dictionary
- Create a dictionary from the processed file containing the number of times a word appears in the training set
- Filter out tokens that are not that useful
-- appear in less than x documents
-- more than half of the documents (might be a stop word!)
-- keep only y most frequent tokens

```
# Gensim doc2bow
dictionary = gensim.corpora.Dictionary(processed_docs)
dictionary.filter_extremes(no_below=15, no_above=0.5, keep_n=100000)
bow_corpus = [dictionary.doc2bow(doc) for doc in proc_docs]
```

### Models - LDA 

```
#Set 1: number of topics (k = 10), number of passes (pass = 20), and number of iterations (iterations = 2000)
lda_model1 = gensim.models.LdaMulticore(bow_corpus, num_topics=10, id2word=dictionary, passes=20, workers=1,iterations=2000)

#Set 2: number of topics (k = 20), number of passes (pass = 20), and number of iterations (iterations = 2000)
lda_model2 = gensim.models.LdaMulticore(bow_corpus, num_topics=20, id2word=dictionary, passes=20, workers=1,iterations=2000)

```

#### Lemmatization
```
wnl = WordNetLemmatizer()
process = []
for i in doc:
    pos = pos_tag([i])[0][1]
    if pos[0] == 'V': process.append(wnl.lemmatize(i,'v'))
    else: process.append(wnl.lemmatize(i))       

```
### Output
**LDA-Set1 Without Lemmatization**
![img1](/ldaset1.png)
**LDA-Set2 Without Lemmatization**
![img2](/ldaset2.png)
**LDA-Set3 With Lemmatization**
![img2](/ldaset3.png)
**LDA-Set4 With Lemmatization**
![img2](/ldaset4.png)

### Conclusion
I would say LDA model with lemmatization for k = 10 generates better topics.
From the generated top words in each topic for LDA with lemmatization, we will not see the same lemma. Lemmatization reduces the number of unique words by removing conjugation or inflections. 

As we increase the number of topics, the models are likely to generate ambiguous topics with fussy semantics category such as ‘unknown*’ set2.