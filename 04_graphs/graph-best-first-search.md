# Computer Algorithms: Graph Best-First Search

## Introduction

So far we know how to implement graph [depth-first](/2012/09/17/computer-algorithms-graph-depth-first-search/) and [breadth-first](/2012/09/10/computer-algorithms-graph-breadth-first-search/) search. These two approaches are crucial in order to understand graph traversal algorithms. However they are just explaining how we can walk through in breadth or depth and sometimes this isn’t enough for an efficient solution of graph traversal.

In the examples so far we had an undirected, unweighted graph and we were using adjacency matrices to represent the graphs. By [using adjacency matrices](/2012/08/31/computer-algorithms-graphs-and-their-representation/) we store 1 in the A[i][j] if there’s an edge between vertex i and vertex j. Otherwise we put a 0. However the value of 1 gives us only the information that we have an edge between two vertices, which is not always enough when designing graphs.

Indeed graphs can be weighted. Sometimes the path between two vertices can have a value. Thinking of a road map we know that distances between cities are represented in miles or kilometers. Thus often representing a road map as a graph, we don’t put just 1 between city A and city B, to say that there is a path between them, but also we put some meaningful information – let’s say the distance in miles between A and B. 

Note that this value can be the distance in miles, but it can be something else, like the time in hours we’ve to walk between those two cities. In general this value is a function of A and B. So if we keep the distance between A and B we can say this function is F(A, B) = X, or distance(A, B) = X miles.

Of course in this particular example F(A, B) = F(B, A), but this isn’t always true in practice. We can have a directed graph where F(A, B) != F(B, A).

Here I talk about distance between two cities and it is the edge that brings some additional information. However sometimes we have to store the value of the vertices. Let’s say I’m playing a game (like chess) and each move brings me some additional benefit. So each move (vertex) can be evaluated with some particular value. Thus sometimes we don’t have a function of and edge like F(A, B), but function of the vertices, like F(A) and F(B).

In breadth-first search and depth-first search we just pick up a vertex and we consecutively walk through all its successors that haven’t been visited yet.

[![Walk Through an Unweithed Graph](/wp-content/uploads/2012/09/1.-Unweithed-Graph-Walkthrough.png)](/wp-content/uploads/2012/09/1.-Unweithed-Graph-Walkthrough.png)In order to walk through an unweithed graph using DFS, we chose consecutively each successor of node i!

So in DFS in particular we started from left to right in the array above. So the first node that has to be explored is vertex “1”.

```php
0: [0, 1, 0, 0, 1, 1]
```

However sometimes, as I said above, we have weighted graphs, so the question is – is there any problem, regarding to the algorithm speed, if we go consecutively through all successors. The answer in general is yes, so we must modify a bit our code in order to continue not with the first but with the best matching successor. By best-matching we mean that the successor should match some criteria like – minimal or maximal value.

## Overview

In the following example we see that some of the successors of vertex 0 are very far from it, while others are closer. Thus 4 has the value of 5, while node 1’s value is 2 and 5 is 1.

[![DFS and Weighted Graph](/wp-content/uploads/2012/09/2.-BFS-and-Weighted-Graph.png)](/wp-content/uploads/2012/09/2.-BFS-and-Weighted-Graph.png)Weithed graph brings us more information about the successors of a given vertex. Thus we have to chose carefully which one to get first in our path exploration!

```php
0: [0, 2, 0, 0, 5, 1]
```

In this case if we’re searching for the shortest path between 1 and 3, although 1 and 4 are the first two successors in the adjacency matrix of the “start” vertex, we don’t choose them since there’s a better solution – going through node 5.

[![Best-First Search](/wp-content/uploads/2012/09/3.-Best-First-Search.png)](/wp-content/uploads/2012/09/3.-Best-First-Search.png)In best-first search we continue the path to the target through the best-matching successor!

## Problems

The question is – are we sure that by choosing node 5, we’ll find the best path? Even more! Is there a path through node 5? As we see on the image below both cases are possible.

[![BFS problems](/wp-content/uploads/2012/09/4.-BFS-problems.png)](/wp-content/uploads/2012/09/4.-BFS-problems.png)Somtimes best-first search doesn’t find the “best” (shortest/longest/cheapest) path to the target!

Practically best-first search is identical with depth-first search, with the main difference that we choose the best-matching successor instead of choosing the first matching successor. So we’re sure that we’re going through all the successors but in some particular order, different from DFS. Thus we know that if there’s a path we’ll find it.

However even if we find the path between A and B, we can’t be sure that there is not a better path. We only know that this path is the best so far. 

Another question is – how can we find the best matching successor effectively. Well if we’re looking for the minimal or maximal value one possible solution is to sort the array of successors.

[![Using Priority Queues](/wp-content/uploads/2012/09/5.-Using-Priority-Queues.png)](/wp-content/uploads/2012/09/5.-Using-Priority-Queues.png)The difference between depth-first and best-first is that we change the order of chosing the next successor!

```php
0: [0 => 0, 1 => 2, 2 => 0, 3 => 0, 4 => 5, 5 => 1]
// sorted by value
0: [5 => 1, 1 => 2, 4 => 5, 0 => 0, 2 => 0, 3 => 1]
```

Another good approach will be to use priority queues or heaps.

Thus on every step we’ll get the best matching successor.

## Code

In general best-first search uses the ground of depth-first search, so its implementation isn’t more difficult! The following PHP code snippet shows the very small difference between these two algorithms.

```php
class Graph 
{
    protected $_len = 0;
    protected $_g = array();
    protected $_visited = array();
 
    public function __construct()
    {
        $this->_g = array(
            array(0, 2, 0, 0, 5, 1),
            array(1, 0, 3, 0, 0, 0),
            array(0, 2, 0, 8, 0, 0),
            array(0, 0, 3, 0, 5, 0),
            array(1, 0, 0, 8, 0, 1),
            array(1, 0, 0, 0, 5, 0),
        );
 
        $this->_len = count($this->_g);
 
        $this->_initVisited();
    }
 
    protected function _initVisited()
    {
        for ($i = 0; $i _len; $i++) {
            $this->_visited[$i] = 0;
        }
    }
 
    public function bestFirst($vertex)
    {
        $this->_visited[$vertex] = 1;
 
        echo $vertex . "\n";
 
        asort($this->_g[$vertex]);
 
        foreach ($this->_g[$vertex] as $key => $v) {
            if ($v > 0 && !$this->_visited[$key]) {
                $this->bestFirst($key);
            }
        }
    }
}
 
$g = new Graph();
// 2 1 0 5 4 3
$g->bestFirst(2);
```

## Application

Best-first search is a typical greedy algorithm. In its principles lies the main greedy approach of chosing the best possible solution so far. It is important to note that depth-first search and breadth-first search are the very basic graph walk through approaches, but they can be also widely extended in order to solve more complex problems.