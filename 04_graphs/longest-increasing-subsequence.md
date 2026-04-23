# Computer Algorithms: Longest Increasing Subsequence

## Introduction

A very common problem in computer programming is finding the longest increasing (decreasing) subsequence in a sequence of numbers (usually integers). Actually this is a typical dynamic programming problem.

Dynamic programming can be described as a huge area of computer science problems that can be categorized by the way they can be solved. Unlike divide and conquer, where we were able to merge the fairly equal sub-solutions in order to receive one single solution of the problem, in dynamic programming we usually try to find an optimal sub-solution and then grow it.

Once we have an optimal sub-solution on each step we try to upgrade it in order to cover the whole problem. Thus a typical member of the dynamic programming class is finding the longest subsequence.

However this problem is interesting because it can be related to graph theory. Let’s find out how.

## Overview

We already know various ways to calculate the shortest paths in a graph. Indeed finding the single-source shortest path is a typical graph problem. To model such kind of solutions we definitely need a graph represented in our solution. 

However the single-source shortest path isn’t a straight-forward problem. It depends on many factors. Thus for positive edges [the algorithm of Edsger Dijkstra](/2012/10/15/computer-algorithms-dijkstra-shortest-path-in-a-graph/) can be a perfect solution, but when the graph contains negative edges his algorithm is no longer useful. 

In the presence of negative edges the Dijkstra’s algorithm doesn’t work and we’d better use the [Bellman-Ford algorithm](/2012/10/22/computer-algorithms-bellman-ford-shortest-path-in-a-graph/). It is interesting to note, that it was exactly [Richard Bellman](http://en.wikipedia.org/wiki/Richard_E._Bellman) who first introduced the term “dynamic programming” in the 1940s.

In fact the Bellman-Ford algorithm was able to detect negative cycles. That’s too important, because in presence of negative cycles the shortest path problem is no longer well defined. 

In the other hand when we’re talking about [shortest paths in a DAG](/2012/10/28/computer-algorithms-shortest-path-in-a-directed-acyclic-graph/) (Directed Acyclic Graph) we can find a faster (linear) solution. That’s because we’re sure that there are no cycles (not even negative cycles)! 

[![Toplogical Sort](/wp-content/uploads/2012/12/1.-Toplogical-Sort.png)](/wp-content/uploads/2012/12/1.-Toplogical-Sort.png) 

Finding the shortest paths in a DAG is closely related to the topological sorting of the DAG. This gives us a linear representation of the vertices of the DAG and we can clearly calculate the distances from the starting node to all other nodes. Note that in a DAG we have one or more nodes that can be considered as starting nodes – which means they don’t have predecessors (incoming edges).

[![Toplogical Sort 2](/wp-content/uploads/2012/12/2.-Toplogical-Sort-2.png)](/wp-content/uploads/2012/12/2.-Toplogical-Sort-2.png) 

Following the words above is pretty hard to find what all these graph algorithms have to do with finding the longest increasing (decreasing) subsequence. Actually this problem is very closely related to the toplogical sort of the DAG and the problem of finding shortest paths in a DAG.

That’s because we can represent our sequence as a DAG. The only thing we must care about is to “connect” with directed edges those elements that form an increasing (decreasing) pair. 

[![Integer Sequence as a DAG](/wp-content/uploads/2012/12/3.-Integer-Sequence.png)](/wp-content/uploads/2012/12/3.-Integer-Sequence.png) 

Thus the sequence from our example [1, 8, 2, 7, 3, 4, 1, 6] is going to look like this.

[![Longest subsequence](/wp-content/uploads/2012/12/4.-Longest-subsequence.png)](/wp-content/uploads/2012/12/4.-Longest-subsequence.png) 

Another important thing to note is that we don’t search for shortest, but for longest path, since our task is to find the longest subsequence.

## Pseudo Code

Our pseudo code for finding the shortest paths in a DAG was something like that.

1. Get the toplogically sorted list L of the DAG;
2. The starting node is s;
3. The distance to s equals to 0;
4. All other distances are initialized to ∞
5. For each node (v) in L\{s} do:
5.1. If dist(v) > dist(u) + w(u, v) then
5.1.1.  dist(v) := dist(u) + w(u, v)

In the above pseudo code “u” is every predecessor of “v”!

Now we must “reverse” the solution above in order to find the longest increasing subsequence. Note that we don’t care any more about the weight of the edges, thus we can simply substitute them with 1.

1. Get the sequence (L) as a toplogically sorted DAG;
2. For each (i) in L do:
2.1. S(i) := 1 + max(S(j), where (i, j) is an edge from the DAG);

## Application

Finding the longest increasing subsequence can be very useful not only at the Google/Yahoo/Facebook interview, but also in various fields of statistics.