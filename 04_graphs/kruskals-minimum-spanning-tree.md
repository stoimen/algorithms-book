# Computer Algorithms: Kruskal’s Minimum Spanning Tree

## Introduction

One of the two main algorithms in [finding the minimum spanning tree](/2012/11/05/computer-algorithms-minimum-spanning-tree/) algorithms is the algorithm of Kruskal. Before getting into the details, let’s get back to the principles of the minimum spanning tree. 

We have a weighted graph and of all spanning trees we’d like to find the one with minimal weight. As an example on the picture above you see a spanning tree (T) on the graph (G), but that isn’t the minimum weight spanning tree!

[![A graph and a possible spanning tree](/wp-content/uploads/2012/11/1.-A-graph-and-a-possible-spanning-tree.png)](/wp-content/uploads/2012/11/1.-A-graph-and-a-possible-spanning-tree.png) 

We can think of a group of islands and the possible connections of bridges connecting them. Of course building bridges is expensive and time consuming, so we must be aware of what kind of bridges we want to build. Nevertheless there is an important question, what’s the minimum price we’d like to pay to build such set of bridges connecting all the islands. 

[![Islands and bridges](/wp-content/uploads/2012/11/2.-Islands-and-bridges.png)](/wp-content/uploads/2012/11/2.-Islands-and-bridges.png) 

Thus we practically need to build a minimum spanning tree, where the vertices will be the islands, while the edges will be the possible bridges between them. Every possible bridge has a weight (the price or the time we need to build it, etc.).

This scenario is only one of possible use cases of where minimum spanning trees can be used in practice.  

The two main approaches – the Kruskal’s and the Prim’s algorithms however differ. 

## Overview

The algorithm of Kruskal starts by initializing a set of |V| trees. 

[![A set of V trees](/wp-content/uploads/2012/11/3.-A-set-of-V-trees.png)](/wp-content/uploads/2012/11/3.-A-set-of-V-trees.png) 

During the process of building the final spanning tree we keep a forest. Obviously we start with a forest with |V| trees, where each tree is a single node tree.

[![A single node tree](/wp-content/uploads/2012/11/4.-A-single-node-tree.png)](/wp-content/uploads/2012/11/4.-A-single-node-tree.png) 

On some point we have a forest of “k” trees which are all a sub-trees of the minimum spanning tree. 

[![Growing forest](/wp-content/uploads/2012/11/5.-A-forest-out-of-K-sub-trees.png)](/wp-content/uploads/2012/11/5.-A-forest-out-of-K-sub-trees.png) 

Finally one step before building the final MST we have two trees and we connect them with the less weighted edge left that connects them.

It’s important to note that during the process of building the tree we sort the edges in ascending order by their weight.

[![Sorted edges](/wp-content/uploads/2012/11/6.-Sorted-Edges.png)](/wp-content/uploads/2012/11/6.-Sorted-Edges.png) 

Than we start getting edges and check whether their ends (the two vertices making the edge) belong to a different sub-trees.

[![Check edges](/wp-content/uploads/2012/11/7.-Check-edges.png)](/wp-content/uploads/2012/11/7.-Check-edges.png) 

## Pseudo Code

1. T (the final spanning tree) is defined to be the empty set;
2. For each vertex v of G, make the empty set out of v;
3. Sort the edges of G in ascending (non-decreasing) order;
4. For each edge (u, v) from the sored list of step 3.
      If u and v belong to different sets
         Add (u,v) to T;
         Get together u and v in one single set;
5. Return T

A great feature about the Kruskal’s algorithm is that it also work on disconnected graphs.

## History

Kruskal’s algorithm is named after [Joseph Kruskal](http://en.wikipedia.org/wiki/Joseph_Kruskal), who wasn’t only computer scientist, but also prominent mathematician and statistician. Although he is best known for its algorithm for computing the minimum spanning tree, described in this post, he’s also known with his work as a statistician and his contribution to the formulation of multidimensional scaling. 

Kruskal also explored the Indo-European languages contributing the studies of the linguistics along with other scientists. His “Indo-European Lexicographical List” (http://www.wordgumbo.com/ie/cmp/) is still widely used.