---
title: "[2018] Sentiment Analysis"
date: 2018-05-10T21:10:45-06:00
---
**Project Title** : Sentiment Analysis on Movie Review
**Skill**:  Weka, Python
**Toolkit**: nltk
**Summary** :

- Perform a series of sentiment classification comparison on Naïve Bayes Classifier, Random Forest, SVM, and Naïve Bayes Multinomial, with advanced features such as stemming, part of speech tagging, n-gram, and feature extractions
- Achieved higher performance with 85% accuracy for Naïve Bayes Multinomial using text preprocessing, Weka’s Ranker, N-gram, and InfoGainAttributeEval

---

With the increasing information on the internet, there are more unstructured data and more useful content that could be classified. This kind of task is called **topic classification**. We identified a document’s topic, or even the languages it is written in. The other similar and important category of Natural Language Processing is **sentiment classification**, or sentiment analysis. 

Sentiment Analysis is an analysis to identify and categorize opinions of the public about things like movies, books, event, and etc, which usually determine whether one’s attitude towards a topic is positive, neutral, or negative. It is useful when we are looking for public opinion. 
<!--more--> 
#### Weka 
**Weka** with its Graphical User Interference (GUI) allows users to do data mining and to learn machine learning **without coding**. It contains a collection of machine learning algorithms and tools for data preprocessing, classification, clustering, and etc. 

#### Dataset
The movie review dataset consists of 1000 positive set and 1000 negative set. Each set contained user-created movies review from the IMDb archive of the rec.arts.movies.reviews newsgroup. Each review set is a plain text files (.txt) and saves in a corresponding class folder, “pos”, “neg”. This dataset is called polarity dataset v2.0.

#### Pre-processing

A series of text preprocessing is conducted before we load the text data into Weka. 
> Even if you are using other machine learning tools, it is still important to clean the text dataset.  
It include **punctuation removal**, **whitespace tokenization**, and **stop words removal**. 
Usually, characters like punctuations appear very frequently in a text document but useless in text classification. 
For stop words like “after”, “the”, “a” are also useless in sentiment analysis

### Attribute and Relation
Weka provides an unsupervised attribute filter StringtoWordVector. It transfers the text data into **bag-of-words**, or in other words, this approach coverts a sequence of words to numerical feature vectors. 
Each word is corresponding to a relation with 2 attributes: word, sentiment.
![img1](/weka.png)

### Machine Learning Model
 
After preprocessing, the dataset has 2000 instances and 1202 attributes/features. For the movie review sentiment analysis, I am testing four different machine learning models: **Naive Bayes Classifier, Random Forest, SVM, and Naive Bayes Multinomial.**
This sentiment analysis is negative and positive classification. In another word, it’s a **two class classification**.

#### [Support Vector Machines (SVM)](https://en.wikipedia.org/wiki/Support-vector_machine)
- find the boundary that separates classes. 
- In two-class SVM, it can be done by using a **straight line**
- suitable to make a prediction of two possible outcomes 
- good for large features set

#### Decision Tree
- commonly used in classification problems
- find the best features depends on the **information gain** 
- easily **overfit** the training data
- Random decision forests is a method that can correct decision trees’ tendency on overfitting
- Random forests differ from the original way by **selecting a random subset of the features**

#### Naive Bayes classifier
- A **Naive Bayes model** assumes that each of the features is **conditionally independent** of one another given some class
- good when there are many equally important features
- **Multinomial Naive Bayes** considers the **word count** and the **distribution is multinomial** 

### Outcome1


|Model|Accuracy| Precision|Recall |F-measure|
|---|---|---|---|---|
|Naive Bayes|0.8265|0.827| 0.827|0.826|
|Naive Bayes Multinomial| 0.837|0.837|0.837|0.837| 
|Random Forest|0.795|0.795|0.795|0.795|
|SVM|0.808|0.808|0.808|0.808|

### Advance features
To improve the baseline model, I would like to do stemming, part of speech tagging, and try bigram, and then feature extraction.
#### Stemming - Weka
- A process to approximated lemmatization of a word.
--  dog and dogs derive from the same lemma "dog"
- **Approximated lemmatization**: removing the suffix of the word
> In Weka, as we are doing the bag-of-words approach with *StringToWordVector*, there is an option for *stemmer*.

#### N-gram - Weka
- Multiple-words-phrase: “very good”, “good”, “not good at all”
-- Affect the positive and negative sentiments
> N-grams can also be done in Weka. Under the filter of *StringToWordVector*, there is an option for *tokenizer*.

#### AttributeSelection - Weka
- not all of the attributes are important or useful
> In this case, Weka has supervised attribute AttributeSelection filter. This filter allows us to choose an attribute evaluation method and a search strategy. 

**Option1** : CfsSubsetEval works when the attribute subset is highly correlated with the class attribute while not strongly correlated with one another
**Option2**:  InfoGainAttributeEval + Ranker to manually choose the top ranked attributes. 

#### Part of Speech Tagging - Python NLTK POS
- can help differentiate words
- “I love this movie”, and “It’s a love story” has different meanings
-- “I love this movie” can represent a positive opinion
-- “It’s a love story” is only a statement.
 
|Attribute Selection|Model|Naive Bayes Multinomial|Random Forest|SVM|
|---|---|---|---|---|
|InfoGain + Ranker(56)|Accuracy|0.8135|0.7985|0.8115|
|InfoGain + Ranker(101)|Accuracy|0.84|0.815|0.8345|
|InfoGain+ Ranker(151)|Accuracy|**0.8505**|0.8275|0.8385|
|InfoGain+ Ranker(1111)|Accuracy|0.843|0.8025|0.8045|

### Conclusion
After a series of experiment, according to the result, the most beneficial features might be **attribute selection and N-gram**, and the most accurate model is Naive Bayes Multinominal.  