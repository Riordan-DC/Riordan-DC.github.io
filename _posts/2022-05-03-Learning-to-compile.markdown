---
layout: post
title:  "Learning to compile"
date:   2022-04-28 10:05:00 +1000
categories: nlp, programming
---

## Learning to compile

I'd love to make a game where you control little robotic agents exclusivly with written language.
You write their little programs and they execute them. If an instruction does not work
you have to break it down into multiple detailed instructions they can understand. 
Then every week / month / year all the instructions are gathered and their instruction parser
learns to translate the text to generic robotic actions. A sort of parser-interpreter combo where
our interpreter runs when it is given parsed instructions it can execute. Over time this abstracts 
the little robots programming bit by bit. 

### Missing information
Abstraction is communication with memory. 

Imagine a kitchen environment similar to 

Questions:
* Will these robots be backwards compatitable with older versions of the language?
No, the development of human language is not deterministic and as such you wont be able
to use new language with an older robot or visa versa. Word sense disambiguation is an active
problem for humans and machines alike. However, lots of language does not
change. These atoms of language can form the foundation of the programming. 

The robots can learn and become better
at executing an action over time and with instruction.
Im imagining a topdown view like and RTS or overcooked and you can write an instruction into
each of your little automated golems and they will perhaps each instruction in a sequence before
restarting their programming.