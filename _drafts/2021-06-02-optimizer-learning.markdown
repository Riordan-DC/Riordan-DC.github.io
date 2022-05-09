---
layout: post
title:  "Reinforcement Learning is Optimizer Learning"
date:   2021-06-02 16:40:30 +1000
categories: rl
---

An optimiser carefully explores a space for a minimum.
An RL agent carefully explores a space for a maximum.
An optimiser effects parameters to move towards an objective.
An RL agent effects (acts on) the environment to move towards an objective. (Including itself the definition of environment)

In policy gradient methods we often use an optimiser to find parameters
for an agent to find actions that move the environment towards an objective.

RL agent acts -> Evaluate action -> Optimise parameters -> RL agent acts
Optimise acts -> Evaluate action -> Optimise acts