---
title: "[2018-19]Spin Intern: Speech-to-Text Auto-Captioning"
date: 2019-03-01T21:09:03-06:00
---

### Job Description

**Employer** : National Center For Supercomputing Application
**Project Title** : Speech-To-Text Auto Captioning
**Skill**: python, bash, C++ <br>
**Toolkit**: CMUSphinx
**Summary** :

- Researching frameworks and workflow for Automatic Speech Recognition (ASR) in order to facilitate live auto-captioning and create caption files
- Developing an ASR application based on CMUSphinx Open Source Speech Recognition system
- Building domain-specific speech and text corpus for phonetics dictionary and acoustics model
- Comparing and evaluating the models' difference to get better training result <!--more--> 
CMUSphinx contains a number of packages for different tasks and applications. 
Here is a list: 

- Pocketsphinx — lightweight recognizer library written in C.
- Sphinxbase — support library required by Pocketsphinx
- Sphinx4 — adjustable, modifiable recognizer written in Java
- Sphinxtrain — acoustic model training tools

### 1. Speech Recognition

ASR matches utterances combinations using models. The utterance combination is the speech structure. 

Speech is a continuous audio stream 

- A phone is any distinct speech sound/diphones, triphones …
- Senone (detectors) is a  context-dependent unit / “spe” in speech → s_e 
- Include some context about neighboring phones when modeling a certain phone
- p(a| c_n)? 

The three models in ASR are:

- Acoustic Model : acoustic properties for each senone (HMM)
- Phonetic Dictionary: a mapping from words to phones
- Language Model: to restrict word search
- What is the next words in this example: In spite __

#### CMUSphinx

Speech Recognition with CMUSphinx is straight forward. There are multiple versions for Pocketsphinx includes Go, Python, Android and etc.
The basic usage looks like the following: 

```
from os import environ, path

from pocketsphinx.pocketsphinx import *
from sphinxbase.sphinxbase import *

MODELDIR = "pocketsphinx/model"
DATADIR = "pocketsphinx/test/data"

# Create a decoder with certain model
config = Decoder.default_config()
config.set_string('-hmm', path.join(MODELDIR, 'en-us/en-us'))
config.set_string('-lm', path.join(MODELDIR, 'en-us/en-us.lm.bin'))
config.set_string('-dict', path.join(MODELDIR, 'en-us/cmudict-en-us.dict'))
decoder = Decoder(config)

# Decode streaming data.
decoder = Decoder(config)
decoder.start_utt()
stream = open(path.join(DATADIR, 'goforward.raw'), 'rb')
while True:
  buf = stream.read(1024)
  if buf:
    decoder.process_raw(buf, False, False)
  else:
    break
decoder.end_utt()
print ('Best hypothesis segments: ', [seg.word for seg in decoder.seg()])

``` 

### 2. Evaluation

To evaluate the performance, we use Word Error Rate.

Word Error Rate: ( I + D + S) / N
Given an original text, a recognition text with a length of N words, 

- I is the number of inserted words
- D is the number of deleted words
- S is the number of substituted words

I test the performance with: 

- Input: 63 16kHz Audio Files
- Output: recognition text
- Models: 
-- Acoustic Model: CMU en-us hmm 
-- Language Model: en-us.lm.bin
-- Phonetics Dictionary: cmudict-en-us-dict
- Word Error Rate: 31.9%

### PROBLEM: How to improve?
My hypothesis to improve the recognition is by...
1. <a href="/project/asr-error-identification">Identify common errors</a>
2. Build an domain-specific text and speech corpus

Currently, I am working on followings to build the corpus: 

- [Building a domain-specify dictionary] (https://joyyyjen.github.io/notebook-web/posts/creating-text-corpus-from-wiki/)
- [Adapting an existing acoustic model] (https://joyyyjen.github.io/notebook-web/posts/adapting-acoustics-models-steps/)

I am also keeping my model training result in a table here [Current Models Comparison ] (https://joyyyjen.github.io/notebook-web/posts/sr-training-chart/)
