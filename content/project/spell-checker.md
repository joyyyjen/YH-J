---
title: "[2017]Spell Checker"
date: 2017-11-27T21:11:28-06:00
---

**Project Title** :  Spell Checker on Yupik Language
**Skill**:  python, greedy algorithm
**Summary** :

• Normalized the text by tokenizing the consonant clusters using the greedy algorithm and nltk library
• Utilized phonetics, phonology, and spelling rules of Yupik language to enforce spell checks<!--more--> 

----
There are several approaches to do the spell check. 
1. Compare with a correctly spelled words dictionary + morphological analysis on inflections
2. Create n-grams models to recognize errors statistically
3. Have phonetic and phonology information for the target language to identify patterns

In this application, we are given the phonetics, phonology information, and spelling rules. Thus, this project is targeted at the #3 method.

### Phonetics information

For every language, they have consonants and vowels (alphabets). 
A portion of Yupik's phonetics: 

- consonant = {'p','t','k','kw','q','qw','f','ll','s','rr','gg','wh','ghh','ghhw','h','mm','nn','ngng','ngngw'}
- nasal = {'m':'mm','n':'nn','ng':'ngng','ngw':'ngngw'}
- fricative =  {'l':'ll','r':'rr','g':'gg','gh':'ghh','ghw':'ghhw'}

This matters when we are doing tokenization. 
A yupik words 'mayughwaaghmi' need to be separated into phonemes.
['m', 'a', 'y', 'u', 'ghw', 'a', 'a', 'gh', 'm', 'i'] 

In English, 'phoneme' has 7 characters but its pronunciation 'fōnēm' has only 5 characters.
### Phonological Rule

```
# If an undoubled (but doubleable) nasal or fricative occurs immediately before the unvoiced fricative ll
#     the grapheme for the doubleable voiced consonant is replaced with its voiceless counterpart.
# If an undoubled (but doubleable) nasal occurs immediately after an unvoiced consonant
#     (where that other consonant is not doubleable), 
#     the grapheme for the doubleable voiced nasal is replaced with its voiceless counterpart.
```
In general, phonological rules start with the underlying representation of a sound (the phoneme that is stored in the speaker's mind) and yield the final surface form, or what the speaker actually pronounces.
We grab the surface form and convert it back to underlying representation in order to see the phonological rules. 
Yupik have voicing assimilation when there are two adjacent consonants, if one of them is voiceless, the other becomes voiceless.
```
nasal = {'m':'mm','n':'nn','ng':'ngng','ngw':'ngngw'}

if index+1 < max  and graphemes[index+1] == "ll":
                graphemes[index] = nasal[graph]
                index += 1
elif index+1 < max and graphemes[index+1] in C:
                graphemes[index] = fricative[graph]
                index += 1
                #print("I replace something before unvoiced c")
```

I check the adjacent consonant by iterating through each graph in the graphene. 
If it matches the rule, it is substituted with its corresponding voiceless graph.
After this process, we convert them to IPA.

### Spell Rules

```
# Yupik syllables must be of the form:
#
# * CV
# * CVC
# * CVV
# * CVVC
#
# Additionally, the first syllable of a word may be of the form:
#
# * V
# * VC
# * VV
# * VVC

if i == "C":
    if len(candi) == 1 and candi[-1] == "C": return(word)
    elif len(candi) == 1 and candi[-1] == "V": candi = candi + i

```
After converted the IPA to 'C', and 'V', we check the spelling or syllable rules. 
If the word violates the rules, it is an error.
From the code above, if there are double C in a syllable, it violates those rules. 
If we have 'CV', then we continue to see what the next character is.

Note: If the syllable candidate we have is 'CVCC', we could not just say it's an incorrect spelling. If the next letter is 'V', the syllable candidate will be split and still follows the syllable rules. This is where we need the greedy algorithm.