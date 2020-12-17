---
layout: post
title: About NLP, twitter and sentiment analysis
subtitle: Following a hard time on the subject
tags: [nlp, twitter]
comments: true
---

## Preprocessing in NLP
- Stemming / lemmatization : taking the root of the word (walked --> walk). The difference lies in the fact that the stem might not be an actual word, whereas lemma is an acutal language word ( see [here](https://www.datacamp.com/community/tutorials/stemming-lemmatization-python))
- Stopword removal, but can be hurtful
- Casefolding : everything in lowercase. In practice, not a good idea when the dataset is large


### Preprocessing tweets 
Following [this kaggle post](https://www.kaggle.com/raenish/tweet-sentiment-insight-eda)  
To know whether a feature of the text is worth keeping, a simple analysis can be carried out using the training data. Ask yourselves these questions. "What sentiment do tweet tend to have when they contain / have the following characteristic ?" :
- a URL
- some sort of punctuation ('!' may be more likely in positive tweets, '*' in negative ones, ...)
- a given length (neutral tweets can tend to be longer)


Other unanswered questions : 
- How to deal with emojis ?

Helper [regex](https://www.kaggle.com/raenish/cheatsheet-text-helper-functions)

## GloVe 
In a few words :
- Stands for *global vector*
- Thanks to a well chosen NN and the co-occurence matrix, the GloVe yields a vector for each word of the dataset, with a given number of features. 
- It generally reflects what is going on in the neighborhood of a vector
- Pre-trained vectors are available [here](https://nlp.stanford.edu/projects/glove/) 

## Typical python libraries
```python
#NLP libraries
import spacy, nltk, gensim, sklearn
import pyLDAvis.gensim

#Vader
import vaderSentiment
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
```