# Computer Algorithms: Graph Depth-First Search

## Introduction

Along with [breadth-first search](/2012/09/10/computer-algorithms-graph-breadth-first-search/), depth-first search is one of the two main methods to walk through a graph. This approach though is different. Breadth-first search (BFS) looks pretty much like starting from a vertex and expanding the searching process level by level. This means that first we get some information of all the successors of the given node and then we go further with the next level. In other words BFS is like a wave. Depth-first search is based on a different approach, which can be very useful in some specific algorithms.

[![DFS vs. BFS](/wp-content/uploads/2012/09/1.-DFS-vs.-BFS.png)](/wp-content/uploads/2012/09/1.-DFS-vs.-BFS.png)Depth-first and breadth-first search are the two main ways to explore a graph!

Both methods can be useful in solving different tasks.

## Overview

Depth-first search is an algorithm that by given starting and target node, finds a path between them. We can use DFS also to walk through all the vertices of a graph, in case the graph is connected.

[![DFS explained](/wp-content/uploads/2012/09/2.-DFS-explained.png)](/wp-content/uploads/2012/09/2.-DFS-explained.png)The algorithm frist goes in depth and then backtracks to all unvisited successors!

The whole idea of this algorithm is to go as far as possible from the given starting node searching for the target. In case we get to a node that has no successors, we get back (typically this is done recursively) and we continue with the last vertex that isn’t visited yet.

So basically we have 3 steps:

- Pick up a vertex that isn’t visited yet and mark it visited;
- Go to its first non-visited successor and mark it visited;
- If all the successors of the vertex are already visited or it doesn’t have successors – go back to its parent;

## Code

The following [PHP](/category/php/) code implements the depth-first search. The key point is the recursion in the method depthFirst.

```php
class Graph 
{
    protected $_len = 0;
    protected $_g = array();
    protected $_visited = array();
 
    public function __construct()
    {
        $this->_g = array(
            array(0, 1, 1, 0, 0, 0),
            array(1, 0, 0, 1, 0, 0),
            array(1, 0, 0, 1, 1, 1),
            array(0, 1, 1, 0, 1, 0),
            array(0, 0, 1, 1, 0, 1),
            array(0, 0, 1, 0, 1, 0),
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
 
    public function depthFirst($vertex)
    {
        $this->_visited[$vertex] = 1;
 
        echo $vertex . "\n";
 
        for ($i = 0; $i _len; $i++) {
            if ($this->_g[$vertex][$i] == 1 && !$this->_visited[$i]) {
                $this->depthFirst($i);
            }
        }
    }
}
 
$g = new Graph();
// 2 0 1 3 4 5
$g->depthFirst(2);
```

## Complexity

By using an adjacency matrix we need n2 space for a graph with n vertices. We also use an additional array to mark visited vertices, which requires additional space of n! Thus the space complexity is O(n2).

When it comes to time complexity since we have a recursion and we try visiting all the vertices on each step, the worst-case time is yet again O(n2)!

## Application

This graph-walk algorithm can be very useful when solving some specific tasks like finding the shortest/longest paths in a graph. Although [BFS](/2012/09/10/computer-algorithms-graph-breadth-first-search/) and DFS aren’t the only methods of walking through a graph, they are considered the two main algorithms of that kind. This is important in order to solve graph-based problems.