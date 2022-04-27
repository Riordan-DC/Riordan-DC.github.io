---
layout: post
title:  "Semantic Graph Convolutional Networks"
date:   2020-05-16 10:05:00 +1000
categories: paper 
---
In this post I want to write about Graph Convolutional Networks. More specifically I 
want to write about Niko Suenderhauf's paper: "Where are the Keys? – Learning Object-Centric Navigation Policies on Semantic Maps with Graph Convolutional Networks". This
paper exists in the sub-genre of SLAM known as semantic SLAM. To understand
semantic SLAM it is important to survey common approaches to the SLAM 
(Simultaneous Localisation and Mapping) problem.


Existing methods often incorporate a camera (or two, in stereo-vision) sensor into their pipeline. Their maps primarily consist of landmarks seen by the camera. The objective of landmarks is to represent interesting parts of an environment such that it can be uniquely identified. Feature based landmarks are produced from finding
patterns in an image such as lines or corners, often these are sparse.
Another method uses every image's pixels as landmarks, producing a very dense map.
The underlying problem with these methods is that they produce maps where little meaning beyond shape can be inferred from landmarks. Shape is enough for navigation
but contributes little to actually understanding the environment. Semantic SLAM
uses semantic landmarks and would resemble how our minds represent an environment. For instance you can recall exactly how to navigate your kitchen and cook despite having an imperfect memory of its appearance. Furthermore you can often orient yourself in a new kitchen by understanding the context of your environment regardless of the shape/architecture of the room. 

Niko's paper focuses on navigating and constructing these semantic maps. Niko uses a graph to represent a semantic map and a GCN (Graph Convolutional Network) to explore it but more on this later. First I want to discuss the way landmarks are represented in a semantic map. Landmarks are described as any objects detected by an object detector (such as YOLO or AlexNet). The details of the detection mechanism are not important. What is important is how these landmarks are represented and how they are structured in the semantic map. Firstly, each node in the graph is represented by a word2vec embedding of its label. This is the key to preserving the semantics of the environment.  A word2vec vector is a representation of a word which has been learnt as a consequence of training a model to perform a language understanding task. Wether the tasks we chose are proper tests of actual understanding is under debate. Nevertheless we can observe that these vectors are useful in modelling the sense (meaning) of words in language. In this paper the fastText vectors are used.
In the semantic map the nodes can be divided into two groups:
- <b>Pose nodes</b>. This node is a place the agent has visited and currently contains no object. It is assigned an embedding of all zeros.
- <b>Landmark nodes</b>. A landmark node is a node of an object observed from a pose node. Its value will be the word2vec embedding of its label. 


As each landmark node is observed via a pose node an edge is drawn between them to represent its locality. 

or a landmark node. each object has a label in English.  


References
--------
<link to Nikos paper>
