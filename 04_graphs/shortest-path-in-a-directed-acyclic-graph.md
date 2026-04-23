# Computer Algorithms: Shortest Path in a Directed Acyclic Graph

## Introduction

We saw how to find the shortest path in a graph with positive edges using the [Dijkstra’s algorithm](/2012/10/15/computer-algorithms-dijkstra-shortest-path-in-a-graph/). We also know how to find the shortest paths from a given source node to all other nodes even when there are negative edges using [the Bellman-Ford algorithm](/2012/10/22/computer-algorithms-bellman-ford-shortest-path-in-a-graph/). Now we’ll see that there’s a faster algorithm running in linear time that can find the shortest paths from a given source node to all other reachable vertices in a directed acyclic graph, also known as a DAG.

Because the DAG is acyclic we don’t have to worry about negative cycles. As we already know it’s pointless to speak about shortest path in the presence of negative cycles because we can “loop” over these cycles and practically our path will become shorter and shorter.

[![Negative Cycles](/wp-content/uploads/2012/10/1.-Negative-Cycles.png)](/wp-content/uploads/2012/10/1.-Negative-Cycles.png)The presence of a negative cycles make our atempt to find the shortest path pointless!

Thus we have two problems to overcome with Dijkstra and the Bellman-Ford algorithms. First of all we needed only positive weights and on the second place we didn’t want cycles. Well, we can handle both cases in this algorithm.

## Overview

The first thing we know about DAGs is that they can easily be topologically sorted. [Topological sort](/2012/10/01/computer-algorithms-topological-sort-of-a-graph/) can be used in many practical cases, but perhaps the mostly used one is when trying to schedule dependent tasks.

[![Topological Sort](/wp-content/uploads/2012/10/2.-Topological-Sort.png)](/wp-content/uploads/2012/10/2.-Topological-Sort.png)Topological sort is often used to “sort” dependent tasks!

After a topological sort we end with a list of vertices of the DAG and we’re sure that if there’s an edge (u, v), u will precede v in the topologically sorted list.

[![Topological Sort (part 2)](/wp-content/uploads/2012/10/3.-Topological-Sort-part-2.png)](/wp-content/uploads/2012/10/3.-Topological-Sort-part-2.png)If there’s an edge (u,v) then u must precede v. This results in the more general case from the image. There’s no edge between B and D, but B precedes D!

This information is precious and the only thing we need to do is to pass through this sorted list and to calculate distances for a shortest paths just like the algorithm of Dijkstra.

OK, so let’s summarize this algorithm:

–	First we must topologically sort the DAG;

–	As a second step we set the distance to the source to 0 and infinity to all other vertices;

–	Then for each vertex from the list we pass through all its neighbors and we check for shortest path;

It’s pretty much like the Dijkstra’s algorithm with the main difference that we used a priority queue then, while this time we use the list from the topological sort.

## Code

This time the code is actually a pseudocode. Altough all the examples so far was in PHP, perhaps pseudocode is easier to understand and doesn’t bind you in a specific language implementation. Also if you don’t feel comforatable with the given programming language it can be more difficult for you to understand the code than by reading pseudocode.

1. Topologically sort G into L;
2. Set the distance to the source to 0;
3. Set the distances to all other vertices to infinity;
4. For each vertex u in L
5.    - Walk through all neighbors v of u;
6.    - If dist(v) > dist(u) + w(u, v) 
7.       - Set dist(v) 

## Application

It’s clear why and where we must use this algorithm. The only problem is that we must be sure that the graph doesn’t have cycles. However if we’re aware of how the graph is created we may have some additional information if there are cycles or not – then this linear time algorithm can be very applicable.