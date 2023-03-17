---
layout: post
title:  "How Games Simulate Destruction"
date:   2023-03-17 10:05:00 +1000
categories: gamedevelopment
---
<div align="middle" margin="20" style="
   margin-bottom: 20px;
   margin-top: 20px;
">
<img src="{{site.baseurl}}/assets/images/mid_explosion.png" width="50%" height="50%">
</div>

Interaction in a virtual environment feels stiff when your push doesn't shove. Objects need to move when pushed and if pushed hard enough, break. Destruction gives a user an extra-sensory depth that encourages attention to the environment. The kind of destruction seen in games like Red Faction: Guerrilla and Rainbow 6 Seige inspires many players to ask "why isnt this in more games?". It's challenging and fascinating topic that I've spent a lot of time exploring. This post is a collection of my thoughts and experiments in real time destruction.

### Types

A destruction effect can be loosely grouped into one or more of the following types:

1. Fracture
   Anything made up of smaller pieces fractures. In a way everything fractures and the scale of the fracture is what this system from the other two. In voxel engines every break is a fracture as everything is made up of the smaller, same sized, things.
2. Cut
   A cut is a unique piece of something. A cutter is a plane or any geometry that performs the cut. If simulating sharp objects a cut is required.
3. Tear
   Malleable materials like cloth, skin, and paper all break with a tear in simulation. A tear is like a fracture but with loose constraints between neighboring pieces.

The groups loosely separate the way your simulation will be implemented to accommodate its functionality. This is why the tear type has its own group despite being similar to a fracture in definition, implementation varies.

In this post I will focus only on fracture type destruction.

### Pre-baked vs Procedural vs Voxel

Whatever the break a new component is created. This component can be created in three ways:

1. Pre-baked
   An artist or algorithm puts together the pieces that make the object which are then removed at runtime.
2. Procedural
   An algorithm creates the pieces at runtime. This is super computationally expensive and most expressive destruction. I find that this is good for cutting verbs but provides limited control for fracturing verbs. You will also need a tight control over the complexity of your meshes and physics shapes. Maybe use a simplified model for both.
3. Voxel
   An artist puts together the pieces that make up the object out of a voxel primitive. This gives you the power to make everything destructible at the cost of making everything destructible. You could voxelise 3d models or make your own. This requires a lot of custom, special physics / optimization code and passion equal to Dennis Gustafsson (@tuxedolabs). This certainly restricts your art style choices as your environment will likely be cube-ish. You can use signed distance fields and marching cubes to smooth this out to some extent.

I have experience with both pre-baked and procedural systems but not voxel.

## Pre-baked fractures

### Art Pipeline

I like pre-baked system the most. I think it is the most general and art pipeline friendly. If you build a chair or desk or microwave you should leave the pieces you make the model from separate. If you build a single piece prop like a plate you can use a tool to fracture it procedurally. I use blender and use the cell fracture addon. When importing into the game engine each component in the model becomes a component which can break off.

<img src="/assets/images/glue_structure_editor_2.png">

Figure 1. The above image shows a building asset. Each point is a component of the model which can break off. The green lines are the connections between them.

### Method

Most physics engines will allow you to build compound physics shapes. These are physics bodies which get their overall shape from multiple shapes. Every breakable component has its own physics shape so when we break an object we create a new physics body with the components shapes. This functionality alone is not difficult. Our physics engine can tell us to which shape force is being applied. We can use a threshold to avoid small disturbances and a bond strength value to simulate damage over time.

The real challenge is to preserve the sub-structures created in the break. This requires our system to know the connections between components when a break occurs. For this purpose I automatically build a graph structure as seen in figure 1. Note the black pin icons. These are components are anchor points. If a destructible object has an anchor point I treat the object like a static body. This helps the simulation run much more stable.

|                       Before Impact                       |                        After Impact                        |
| :-------------------------------------------------------: | :---------------------------------------------------------: |
| <img src="/assets/images/glue_structure_in_game.png"> | <img src="/assets/images/glue_structure_in_game_2.png"> |

Figure 2. Blue are anchor points. Red are weaked points. Green a regular points. The purple point is the center of mass (CoM).

A structure can be broken in two ways: either the separating component is or isn't connected to other components. If it isn't, we simply do as above and seperate that individual component. If it is connected to other components we must add the connected components to the new physics body and remove them from the parent. Checking which components are connected at runtime is the most costly operation in this system.

This problem is a graph theory problem called finding components; a component is a connected subgraph. Instead of component (which I have already called pieces also) I will call these subgraphs islands. Each island is unconnected to other subgraphs and therefore unsupported and must become its own physics objects. Finding islands in a graph is simple. Perform a graph search on any node in the graph. When you cannot find anymore unvisited nodes you have a subgraph. If there are no more unvisited nodes then there is one island. Repeat this process until all islands are found, each island becomes its own physics body with its components. This is how my destruction system works and it is very slow! The following section describes some methods I use to speed up this system.

### Performance Improvements

<b>1.</b> Use an adjacency matrix. Finding a node's neighbours is a repeated function in graph search. If edges are kept in an array the lookup time for neighbours requires traversing every edge. Using an adjacency matrix I can quickly add or remove edges and finding neighbours will take N operations. N being the number of nodes.

<b>2.</b> Use time slicing. For real time applications time slicing is often used to perform bits of a function incrementally each frame to prevent blocking. The time sink in destruction is the graph search. We can create a search function which will run the search for x steps. Each frame we allow the search function to run for a maximum amount of steps. This will prevent one big stutter when a large graph is searched at the cost of responsiveness. If there is a big particle effect when things break a 3 frame delay may not be noticeable.

<b>3.</b> Use a separate thread for searching. Each search is done on a thread which can prevent blocking the main process thread.

I future posts I plan on discussing my procedural system for making holes in walls like in Rainbow 6 Seige.
