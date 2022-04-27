---
layout: post
title:  "Reasoning: Abstract Graphs"
date:   2020-06-10 11:48:00 +1000
categories: learning
---

## Teaching and Learning
Something that I always enjoy is making comparisons from Machine Learning to teaching. The more I teach the more I understand the difficulty of learning. I begin by believing each student can learn the concept I am teaching and then my job is to frame the problem in such a way they can learn with ease. Some students need to have the information bootstrapped to their existing understanding, or maybe they need a diagram, a story. The only constant among these approaches is the need to abstract the concepts and let the student build connections between them. 


The biggest difference between a student and any ML model is prior experience. Over the course of a semester I am building, at the moment, islands of fundamental information. This is the hardest part for an eager student who can't see the content's connection to the course. But it is all part of the learning process, trying my hardest to encourage all my students to make a connection I have quietly led them to. After weeks of this I have a good record of what they know and what they do not, its always a good time to test. To test my model, I mean students, have generalised I will ask questions that beg one or more of my islands to be bridged. These questions are great puzzles because they might be simple but very intentionally draw together weeks of learning and really demonstrate a students understanding. Ideally I want my students to feel as if they are discovering this for the first time and I am their muse. This kind of learning is a uniquely human technique and it seems to have begun to be integrated into machine learning.

## Representation Learning
Often machine learning doesn't strive to learn like humans because they only work on humans with required prior experience. The work that goes into encapsulating this experience is the job of representation learning. Wherein the objective is "How can we fit more human experience into some vector". In fact at the heart the most important machine learning advancements is a new way to represent information. Convolutional features now represent images, and distributional semantics (word vectors) now represent text. I believe we are just beginning to see graphs used to represent concepts. Graphs can represent structure and information together in a way that is explainable and easily interpreted by machines and humans. 

 Graph Convolutional Networks 

it is very difficult to have a model perform reasoning.

## Semantic Maps
In a semantic map the landmarks are objects and so the concept of 3d space isn't very valuable. Overall the value of the semantic map is conceptual, accessible, human-like understanding of the environment. I.e. We don't need to know where the toaster's position is relative to the centre of the universe, we need to know it is in the kitchen.

Recently I have been reading literature on graphs, or more specifically, Google's knowledge graph. In the paper A2N they build a model which is capable of performing a knowledge graph task known as "completion". Much like mobile keyboard autocomplete, this task is concerned with completing an unseen relation between entities in the graph. An example query might be: (Obama, Nationality, ?). The graph may be constructed from text which provided the relation (Obama, lives_in, America), which is a candidate response to this query. It is the task of completion, of the A2N model, to score that candidate highly. 

I bring up knowledge graph completion because I believe it has a place in one of my approaches to natural language navigation. Currently I find my problem at the intersection of semantic scene understanding and natural language understanding. The bridge between them I believe is graphs. Both fields use graphs to represent information and hopefully this means easier interoperation. 


### References
<link to A2N paper>
<Link to Chistopher Manning videos>
<Link to Neural State Machines Paper>
<Graph Convolution Blog>