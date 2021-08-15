---
layout: post
title:	Topic Modeling of Venetian Ambassador Reports
tags: NLP
---

In this work we conducted stylometry analysis and topic modeling on 22 reports from [Venetian Ambassador in England](http://www.british-history.ac.uk/cal-state-papers/venice/vol1/cxxii-cxxix). The 22 reports explored a time range from 1489 to 1763, so language style and topics could vary during the timespan.

<!-- more -->

## Stylometry

[Stylometry](https://en.wikipedia.org/wiki/Stylometry) is a quantitative study of linguistic style. We did a quick stylometry check with *Stylo* package from R on the 22 reports. 

![stylo](http://veniceatlas.epfl.ch/wp-content/uploads/2016/03/DH_MDS_100_MFWs_Culled_0____001.png){: .center-image}

The isolated report by Giustinian was indeed different from the others; it was confirmed by my Italian teammates to be written with a mixture of Italian vernacular and Latin.


## Topic Modeling


### Tools

To start with TM we decide to proceed with the whole dataset. *Mallet* was used instead of *StanfordTM* and *Gensim* but all of them are excellent tools under specific conditions.

### Stopwords

The stopwords were mainly assembled by my Italian teammates. First they took reference from two websites:
- [snowball.tartarus.org](snowball.tartarus.org)
- [http://www.ranks.nl/stopwords/](http://www.ranks.nl/stopwords/)

Which could be useful in future cases. With the inaugural stopwords, we ran topic modeling in Mallet and came up with 20 raw topics.

![original](http://veniceatlas.epfl.ch/wp-content/uploads/2016/03/Immagine2.png){: .center-image}

As we can see above (well, if you understand Italian), for example, Topic 5 are filled with meaningless words. My teammates then put them to the stopwords list, and ran the TM model in an iterative approach. In the first iterations, the generated topics were rather miserable. It took them 30-some iterations to reach a extensive list of 500 Italian words, including pronouns, conjugatives, and some strangely stylished words.

### Topic trend over time


The first analysis we did was the trend of topics. We ran the model on the whole dataset for another time and generated 30 topics. Then we divided the reports into five chunks according to the time they were written. We can see significant shift over time on specific topics (i.e. Topic 9 and 20).

![30topics](http://veniceatlas.epfl.ch/wp-content/uploads/2016/06/AvarageProportion30.png){: .center-image}

Then we focused on a single topic to visualize its trend across time chunks. Different trends could be detected as follows.

![timeseries](http://veniceatlas.epfl.ch/wp-content/uploads/2016/06/Benchmark-grouped-for-Wpress.png){: .center-image}

Topics about a famous person will be popular when they are at prime, or at least alive. Whenever they were not born, the trend was non-existent. When they were already dead, the trend would fade away.

The topic 11 about continential wars became more and more significant over the 200 years. That could be a dangerous signal for the reigning class if they knew NLP hundreds years ago that Europe was about to shake.

![worddistribution](http://veniceatlas.epfl.ch/wp-content/uploads/2016/04/word-topic-distribution.jpg){: .center-image}

## Wordnet

The wordnet is based on [td.idf score](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) if you were interested. Top 400 frequent words were kept to represent the most important connections.

![wordnet](http://veniceatlas.epfl.ch/wp-content/uploads/2016/06/graph-full-dataset.jpg){: .center-image}

We could detect a couple of shortcomings in the output networks, namely the large number of nodes per cluster and the difficulties in identifying a general trend among words in the same group. The excessive homogeneity between texts and the limited size of the dataset might be the main reason here.

![wordnet2](http://veniceatlas.epfl.ch/wp-content/uploads/2016/06/graph-first-interval.jpg){: .center-image}

## Credits

This work is part of my EPFL DH101 Project. The project is collaborated with my teammates Stafano, Francesco and Jiacheng. My Italian friends did most of the job in the analysis part since I speak just a *poco* Italian.


You can find our [full report here](http://veniceatlas.epfl.ch/topic-modeling-of-ambassadors-relations-from-england-final-report/).
