---
layout: post
title: "Twitter sentiment analysis"
subtitle: CS-433
tags: [nlp, twitter]
comments: true
---

[Gensim and word embeddings](https://machinelearningmastery.com/develop-word-embeddings-python-gensim/)

# Abstract
The project consists in predicting the general sentiment of a tweet, which can be either positive or negative. To achieve this ask, we have been given a training set of ~1M tweets. First beginning with the analysis of the important components of a tweet to properly pre-process the data, we tried to apply several models, gradually increasing the complexity of the model. Finally, the model yielding the best results is a **describe model** based on **what** whose parameters have been optimized through **How**

# Preprocessing
Tweets cannot be treated as sentenced from a book. They contain grammar mistakes, uncommon abbreviations and twitter specific punctuations, that have strong influence on the final sentiment of the tweet. We thus first analyse the 