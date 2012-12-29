---
title: Item based recommendations with Mahout
author: Tomasz Korzeniowski
layout: post
categories:
  - Information Retrieval
tags:
  - java
  - machine learning
  - mahout
---
MyNewsdesk is all about to help journalists to, as effectively as possible, gain access to the press information and PR material they REALLY want. The goal of our development team is to implement a platform that meets this requirement.

Our platforms constantly changes and evolves, we implement new solutions, rebuild existing components and run experiments to make sure we can deliver new functionality that gives value and helps our customers to find all needle in the haystack they are looking for …or even better recommend them needle they are not fully aware that exists.

Some time ago we moved from Ferret to [Solr][1] and [Sunspot][2] with some custom components. Now we do experiments with more advanced machine learning algorithms and information retrieval methods. There are several interesting solutions and available libraries, but at this stage we are focused on putting [Mahout][3] to effective use in our scenario.

In this series of posts I would like to share our findings, the result of experiments, help you to use Mahout and this way give back to the community.

Let’s get the ball rolling!

Today, I would like to share with you how to make item-based recommendations with Mahout.

First of all, what is a recommendation. Well… recommendations are all about digging out preferences, and using them to suggest things you didn’t already know about.

In the case of MyNewsdesk we would like to recommend to our customers press information and PR material that is relevant to them and is in the scope of their interest.

There are several types of recommendations. In this post I would like to give a short introduction to an item-based recommender.

Please be aware that we are still in the phase of experiments and don’t know the all the answers and how our experiments will turn into functionality within our platform.

The first experiment will be to check how much effort and code is needed to get Mahout up and running. In order to implement the first recommendation experiment we will need to create some seed data.

I will use comma-separated data file format:

<pre>1,1,5.0
1,2,3.0
1,3,2.5
2,1,2.0
2,2,2.5
2,3,5.0
2,4,2.0
3,1,2.5
3,4,4.0
3,5,4.5
3,7,5.0
4,1,5.0
4,3,3.0
4,4,4.5
4,6,4.0
5,1,4.0
5,2,3.0
5,3,2.0
5,4,4.0
5,5,3.5
5,6,4.0</pre>

The first column defines a user id (e.g. journalist with account at MyNewsdesk). In the second column I will store the item id (e.g. database id of press release at MyNewsdesk) and in the third column there will be preference of given user for an item.

The data is now prepared to build our first recommendations engine. Now let’s write some code to get Mahout recommend some items for one of our users.



Wow… that was easy :) In a few lines of code we built a scalable recommendations engine. That’s the beauty of Mahout.

In the first line of code we load our data model. Please be aware that we could load data from any other source: the database, a webservice or our custom implementation. We just need to make sure that our class implements the DataModel interface. Next we build an item based recommender. We use euclidean distance metrics but there are other metrics available: pearson correlation, pearson correlation with weighting, euclidean distance with weighting, tanimoto coefficient, log likelihood.

Finally we build a recommender with our data model and ask for recommended items for user with id 1

We will get two new recommended items (6 and 4) with the preference values for user 1.

<pre>2 [main] INFO org.apache.mahout.cf.taste.impl.model.file.FileDataModel - Creating FileDataModel for file model.csv
48 [main] INFO org.apache.mahout.cf.taste.impl.model.file.FileDataModel - Reading file info...
50 [main] INFO org.apache.mahout.cf.taste.impl.model.file.FileDataModel - Read lines: 21
67 [main] INFO org.apache.mahout.cf.taste.impl.model.GenericDataModel - Processed 5 users
RecommendedItem[item:6, value:3.8309176]
RecommendedItem[item:4, value:3.558496]</pre>

In the code we do also do quick evaluation of precision and recall but I would like to touch this topic in one of the coming posts.

Please be aware of that this is only a quick introduction and we take a lot of shortcuts, but if you are interested in the next posts I will dive into more details and describe different types of recommenders, similarity metrics, and describe how to measure quality of the recommendations engine e.g. precision, recall.

That’s all folks! In the upcoming posts I will share more stuff about Mahout, try to give you a short introduction to the wonderful world of text mining and also describe a little more about our transition from Ferret to Solr and Sunspot.

Stay tuned. There is more to come.

 [1]: http://lucene.apache.org/solr/
 [2]: http://github.com/outoftime/sunspot
 [3]: http://mahout.apache.org/