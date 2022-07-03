---
layout: post
title:  "Slot filling & Intent detection"
date:   2022-04-28 10:05:00 +1000
categories: nlp, programming
---

### NLP Cornerstones
A short literature review on slot filling and intent detection as it relates to embodied instruction parsing.

If we classify every sentence by its slot and intent content we have a matrix of possibilities:
* Slots & Intent
* No Slots & Intent
* Slots & No Intent
* No Slots & No Intent

Is the only mechanism for understanding similarity to memory? How can we design a mechanism for understanding?
Maybe this is obvious? Id be interested to know.

#### Question finding
If we recognise an entity we can assume properties. if they cannot be recognised we can ask.


#### NLP + Search Queries
We store redundant information in language models. Im not impressed with
a %10 increase in a metric if it required millions of dollars, millions of GPU hours,
and billions of extra black-box parameters. The extra parameters are especially concerning
because we are constatly producing new data and a model that cannot quickly update its priors
is not practical. Im very proud to see more research in:
* learn to substitue like string formatting
* learn to write queries

note: i wonder if we can improve SFID generalisation by fuzzing the slots. 

#### Language Generalisation
Generalise the instruction: "go to the apples". Maybe "function (parameter)". Maybe "go to(parameter)". 
We cluster instructions to define a function set and cluster goal states to define results. 

#### Planning
A neural model generates a sequence in reverse, like neural backchaining. At each step using attention to
find relevant environment features.
To demonstrate we construct a list of entities and their properties. Lets start with three entities:
A fuit bowl. A table. An apple. and a robot.
We will have two seperate models. An instruction parser that inputs instructions and outputs functions and arguments which
are used to define a goal state. The other is a sequence model that outputs plans from the current state to the goal state.
Every forward pass of the planner model outputs an action. 

Issue: There can be no fuzzieness about state
The planner outputs a robots actions. For this demonstration lets say the robot can access actions: go(target), pick(target), put(target)
The history context is the RNN hidden state which should record a history of actions and its goal state.
Finding the right action will depend on the history context and environment state and the current and goal state.
Finding the right parameters will depend on the history context and current environment and the selected action

# Lit Review
### (2018) SNIPS
Snips Voice Platform: an embedded Spoken Language Understanding system for private-by-design voice interfaces

### (2020) Survey on NN Methods for SF & IC 
Recent Neural Methods on Slot Filling and Intent Classification for Task-Oriented Dialogue Systems: A Survey