---
title: "[2018-19]Speech Recognition Error Identification"
date: 2019-03-01T21:09:57-06:00
---
**Project Title** : Speech-To-Text Error Analyzer 
**Skill**: Python, Flask
**Toolkit**: CMUSphinx, nltk, numpy
**Summary** :

- Built a Flask front end to corporate with recognition application for speech recognition and text analyzation by getting recognition files from user and posting classification result
- Combined the ASR application with n-gram and word error rate to do speech-driven text error classification with a goal to detect and improve common mistake<!--more--> 
---
### Purpose 

Speech to Text, or Automatic Speech Recognition, is a process to convert human speech to a corresponding set of words. Currently, there are some well-established speech recognition systems such as Microsoft, Wake-Up-Word, Google, and CMU Sphinx. 
Yet, there aren’t many evaluation tools to analyse and categorize different word errors. 

Taken the following word error rate as an example (from my current intern research):

> REF (reference) :  who had__vanished 
HYP (hypothesis) :    ___ would vanish   
EVA (evaluation) :    D   _S___S                             
WER(word error rate): 17.65%

In this case, "who" has been deleted from the reference sentences and "has" is substituted by "would", and "vanished" is substituted by "vanish". (D for delete, S for substitute) 

### Speech-to-Text Error Analyzer
- The **goal/function of this tool**: is to list types of errors. For instance, 
> D S combination cover \_\_ % of error from this recognizer performance.
> \_\_ % D S combination is due to two words mixing together
> S is \_\_ % of all the error
> \_\_ % is due to inflections, \_\_  % is a spelling mistake, and \_\_ % is determiner substitute.

### Error Analyzer Components

**Front End**: Python Flask WebUI
- User Interface
- Easier to use
- Data visualization

**Back End**: Python
- Speech Recognition
- Error Analyzing

### Requirements
- Python 3

**Packages Install**
- flask
- pyyaml
- nltk
- numpy
- SpeechRecognition
- metaphone

### Demo

![img4](/gui.png)

![img1](/visual.png)

![img2](/subclasses-error.png)

github project link: [sr-GUI](https://github.com/joyyyjen/sr-GUI)

### Common Errors
1. **Inflection, Homophone**
Inflection might be caused by the missing suffixes in the phonetics dictionary. 
And homophone can be either the speaker's error or recognition error.

2. **Split or merge**
- hyphens: sometimes when we recognize the word correctly, it was marked as incorrect because of a missing or extra hyphens.
- numbers to words: the ASR can recognize the number but only output number in words format. It can cause the wrong alignment during the evaluation and lower the word error rate. 

Link to more detail findings: [Error,Repetition,Hyphen](https://joyyyjen.github.io/notebook-web/posts/error-repetition-hyphen/)

### SR/TF-IDF Independent Study Proposal
I am working on a further improvement from the SR-ANLZ project. 
The goal is to combine text information with word error rate with TF-IDF approaches. Since errors on common words like "the" might not be as bad as errors on rare/important words, we can adjust our evaluation parameters.
Another factor is length normalizer. If two recognition text has the same amount of error word, the one which has a longer passage length should still have a higher performance score.

### Mentor Point of View
- adapt the best result from the different speech recognition
- adapt the word correction?
- grammar autocorrect?

### Question?
- Will it be useful?
I think it can be useful because it highlights the kind of mistake in details. It's like human learn from mistakes.

### Possible Output
- Adapt the formula, f(q,d) = &sum;w&in;q&and;d c(w,q)x (k+1)c(w,d)/(c(w,d)+k(1-b+b x|d|/avdl)) x log ((M+1)/df(w))
- Query is the error words 
- Output the following:

|term|df|idf|
|----|----|---|
|car|18165| 1.65|
|auto| 6725|2.08|
|..|..|..|

