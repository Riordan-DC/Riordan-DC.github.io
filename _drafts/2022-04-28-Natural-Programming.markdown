---
layout: post
title:  "Natural Programming"
date:   2022-04-28 10:05:00 +1000
categories: nlp, programming
---

## Natural Programming

The focus of my research lies at the intersection of Natural Language Programming (NLP) and robotics. It is a small but quickly growing field. Where I study, at QUT, 
robotics research is concerned with fine motor control tasks like grasping, packing, walking, and other fine verbs. Control is a wide word
and encompasses a heirarchy of control. At the foundation of the control pyramid are fine motor controls. Things we as humans do abscent mindedly
but we train every second. As we move towards the tip of the pyramid we find programs of lower control tasks. The ascent of the pyramid
measures: 

* Infrequency of the task, and therefore how often we can learn from it. 
* The conscienceness or awareness required to complete the task.
* My interest in it.

I am primarily concerned with high level planning. Controlling a system using as many verbs in the pyramid. This area I want to call
Natural programming. This is not original and the concept of Natural Language Programming already exists (confusingly also called NLP).
I suppose my interest is in the missing information inherent to all natural communication. 

 It is a level of abstract planning that we as humans do all the time when we act. When carrying out a task as described
to us. To emulate this ability in machines requires understanding of natural language and an ability to translate that language to programming.
Some(citation needed) have begun to to use large language models to perform tasks by describing the program in its prompt. So called prompt engineering
is an inconsistant method for coercing a large neural network to produce impressive results and is a promising step towards a more mature system of research.
I believe Natural programming will operate as a symbolic algebra with learnt representations that will give us the ability to control any machine with the 
same programming humans use. 

## Translation techniques
Sparsity? 

## Slot filling & Intent detection
Slot filling and intent detection classify and predict functions and arguments we define in language. For instance the phrase: "go to the kitchen"
defines a function "go to" with arguments "kitchen" (goto(kitchen)). Slotfilling and intent detection works well for chatbots and virtual assistants.
These disembodied systems reside in static environments and ultimately perform the task of translating natural language to a static API. Their
environment interface has a one-to-one mapping with its natural language input. If a task requires multiple interactions then its interface will
prompt a user with a branching question-answer list until every parameter for the function has been filled and the task can execute. For example:
"Play song X" maps to the function call playSong(X). With underspecified intent: "play song" maps to playSong(whichSong()), where whichSong() is
a request for the user to specify a song. With embodied agents almost every instruction requires multiple steps and extra specification because we
rely on implicit information and environments are dynamic. If we imagine the question: "get me the fork" we can blindly map this to the grossly
underspecified function getMe(fork). This function requires arguments 

### Reinforcement Learning Policy Gradient
RL Policy Gradient methods struggle the higher up the heirarchy of control. 
* Compounding dynamics
* Partial observability
* Discontinuous optimisation problems that reduce gradient descent methods to random initalisation tasks.
	Additionally, policy gradients methods WILL find some minima regardless of if it correctly aligns with
	your objective function. Perhaps your loss is misaligned with your goals or more common your training data.
	The solution to these issues is not to engineer the loss function or the sanatise the training data, both of which
	make your model more rigid. The solution is to optimise your own algorithm before adding optimisation methods.
	The real success we have seen in RL is in algorithms where it stays in its lane and works with, not agaisnt, an
	explict symbolic method.
	If your gradient method is sensitive to initalisation then perhaps you are misusing gradient methods.

