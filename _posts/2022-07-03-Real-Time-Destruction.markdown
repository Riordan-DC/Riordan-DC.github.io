---
layout: post
title:  "Real Time Destruction"
date:   2022-07-03 10:05:00 +1000
categories: programming, game dev, simulation
---

## Real Time Destruction
Interaction in a virtual environment feels stiff when your push doesn't shove. Objects need to move when pushed and if pushed hard enough, break. Destruction gives a user an extra-sensory depth that encourages attention to the environment. This post is a collection of my thoughts and experiments in real time destruction.

### Types
Things break differently and we use different verbs to describe the break. I'll group these into three:
1. Fracture
	Anything made up of smaller pieces fractures. In a way everything fractures and the scale of the fracture is what this system from the other two. In voxel engines every break is a fracture as everything is made up of the smaller, same sized, things.
2. Cut
	A cut is a unique piece of something. A cutter is a plane or any geometry that performs the cut. If simulating sharp objects a cut is required.
3. Tear
	 Malleable materials like cloth, skin, and paper all break with a tear in simulation. A tear is like a fracture but with loose constraints between neighboring pieces. 

The groups loosely separate the way your simulation will be designed to accommodate its functionality. This is why tear has its own group, although similar to a fracture in definition, implementation varies.

### Pre-baked vs procedural vs voxel
Whatever the break a new component is created. This component can be created in three ways:
1. Pre-baked
	An artist or algorithm puts together the pieces that make the object which are then removed at runtime.
2. Procedural
	An algorithm creates the pieces at runtime. This is super computationally expensive and most expressive destruction. I find that this is good for cutting verbs but provides limited control for fracturing verbs. You will also need a tight control over the complexity of your meshes and physics shapes. Maybe use a simplified model for both.
3. Voxel
	An artist puts together the pieces that make up the object out of a voxel primitive. This gives you the power to make everything destructible at the cost of making everything destructible. You could voxelise 3d models or make your own. This requires a lot of custom, special physics / optimization code and passion equal to Dennis Gustafsson (@tuxedolabs). This certainly restricts your art style choices as your environment will likely be cube-ish. You can use signed distance fields and marching cubes to smooth this out to some extent.

I have experience with both pre-baked and procedural systems but not voxel.

## Pre-baked fracture
I like pre-baked system the most. I think it is the most general and art pipeline friendly. If you build a chair or desk you should leave the pieces you make the model from separate. If you build a wall you can use a tool to fracture it procedurally. I use blender and use the cell fracture addon. When importing into the game engine each component in the model becomes a component which can break off. 

Most physics engines will allow you to build compound physics shapes. These are physics bodies which get their overall shape from multiple shapes. Every breakable component has its own physics shape so when we break an object we create a new physics body with the components shapes. This functionality alone is not difficult. Our physics engine can tell us which components shape is being separated and we remove it from the original physics body and give it its own. The challenge is to preserve the sub-structures created in the break. This requires our system to know the connections between components when a break occurs.

A structure can be broken in two ways. Either the separating component is or isn't connected to other components. If it isn't, we simply do as above. If it is we must add the connected components to the new physics body and remove them from the parent. Checking which components are connected at runtime is the most costly operation in this system. This problem is a graph theory problem called finding components; a component is a connected subgraph. Instead of component (which I have already called pieces also) I will call these subgraphs islands. Each island is unconnected to other subgraphs and therefore unsupported and must become its own physics objects. Finding islands in a graph is simple. Perform a graph search (depth or breadth) on any node in the graph. When you cannot find anymore unvisited nodes you have a subgraph. If there are no more unvisited nodes then there is one island. If you search and find a subgraph and have more unvisited nodes then begin another search on an unvisited node to find another subgraph. Do this until no nodes are unvisited. The result is for each time the search runs you find 1 island. 1 set of components which need to be their own physics body. This is how my destruction system works and it is very slow!

### Speedups
0) Use a while loop instead of recursion for graph search
1) Use Breadth-First Search for cache coherence
2) Use an adjacency matrix data structure to speed up connection lookup
The graph search checks nodes adjacency very often and we can speed this up. If edges are stored in a list, checking adjacency requires one enumeration of the list, O(n) complexity (n being the number of nodes in the graph). An adjacency matrix is a 2-dimension binary square matrix where both dimensions are equal to the number of nodes in the graph. Each element at x,y in the matrix represents the presence of an edge between nodes x and y. This matrix will be triangular as its upper and lower sides will be identical. Its diagonal will be the matrix identity as all nodes are connected to themselves.
3) Use time slicing. For real time applications time slicing is often used to perform bits of a function incrementally each frame to prevent blocking. The time sink in destruction is the graph search. We can create a search function which will run the search for x steps. Each frame we allow the search function to run for a maximum amount of steps. This will prevent one big stutter when a large graph is searched at the cost of responsiveness. If there is a big particle effect when things break a 3 frame delay may not be noticeable.  
3) Use a separate thread for searching. Each search is done on a thread which can prevent blocking and speed up graph search.
4) Use multiple threads. Same as above but more. Maybe a thread for each search job. I think threads might be more trouble than they are worth.
5) Sparsification. Sparse graph search. We cluster our graph and connect these clusters to form a graph on a graph. Why? When we search if a link between nodes exists we first traverse the smaller cluster graph quickly. For example in an 100 node graph we search 100 nodes to check connectivity. If we divide our graph into clusters of 20 we have a 5 node graph. If node 1 belongs to cluster A and node 2 belongs to cluster B and there exists a path from cluster A to B then we know a connection has not been broken. If both the nodes are in the same cluster we don't get this benefit but the nodes being close together should imply a short search. The only issue with this system is that now the node clusters have to be updated every time and edge is broken. If an edge between cluster A and B is broken we must recompute the connectivity of the clusters. If an edge between nodes in the same cluster is broken we must still recompute connectivity as a cluster by definition must be fully connected.

Note that this quickly becomes overkill. There is a lot of research on finding connected components in massive social media graphs but implementing them could hurt the long term maintainability of the codebase. If it becomes too slow consider trying A* or changing from BFS to DFS. 