# Computer Algorithms: Dijkstra Shortest Path in a Graph

## Introduction

We already know how we can find the shortest paths in a graph starting from a given vertex. Practically we modified breadth-first search in order to calculate the distances from s to all other nodes reachable from s. We know that this works because BFS walks through the graph level by level.

[![BFS Shortest Paths](/wp-content/uploads/2012/10/1.-BFS-Shortest-Paths.png)](/wp-content/uploads/2012/10/1.-BFS-Shortest-Paths.png)BFS is often used to find shortest paths between a starting node (s) and all other reachable nodes in a graph!

Some sources give a very simple explanation of how BFS finds the shortest paths in a graph. We must just think of the graph as a set of balls connected through strings. 

[![The Graph as Balls and Strings](/wp-content/uploads/2012/10/2.-The-Graph-as-Balls-and-Strings.png)](/wp-content/uploads/2012/10/2.-The-Graph-as-Balls-and-Strings.png)We can think of a graph as a set of balls connected through strings!

As we can see by lifting the ball called “S” all other balls fall down. The closest balls are directly connected to “s” and this is the first level, while the outermost balls are those with longest paths.

[![The Graph as Balls and Strings Levels](/wp-content/uploads/2012/10/3.-The-Graph-as-Balls-and-Strings-Levels.png)](/wp-content/uploads/2012/10/3.-The-Graph-as-Balls-and-Strings-Levels.png)Breadth-first search works much like the image above – it explores the graph level by level, thus we’re sure that all the paths are the shortest!

Clearly edges like those between A and B doesn’t matter for our BFS algorithm because they don’t make the path from S to C through B shorter. This is also known as the triangle inequality, where the sum of the lengths of two of the sides of the triangle is always greater than the length of the third side.

[![Triangle inequality](/wp-content/uploads/2012/10/4.-Triangle-inequality.png)](/wp-content/uploads/2012/10/4.-Triangle-inequality.png)What the triangle inequality says us is that if we have a direct edge between two nodes – that must be the shortest path between them!

We must only answer the question is BFS the best algorithm that finds the shortest path between any two nodes of the graph? This is a reasonable question because as we know by using BFS we don’t find only the shortest path between given vertices i and j, but we also get the shortest paths between i and all other vertices of G. This is an information that we actually don’t need, but can we find the shortest path between i and j without that info?

The answer is simply “no”! Practically depth-first search can’t help us. Even worse – we can find paths that are far not the shortest ones.

[![DFS and shortest path](/wp-content/uploads/2012/10/5.-DFS-and-shortest-path.png)](/wp-content/uploads/2012/10/5.-DFS-and-shortest-path.png)DFS actually can find the longest path in some cases and can’t be used for finding shortest path!

In the image above using DFS the distance between 1 and 7 is 7 while practically there is an edge between them.

So BFS is the optimal algorithm for finding shortest paths in a graph. But there’s a catch! This algorithm works fine when we assume that all the edges are the same length. In the examples so far each edge has the value of 1. So N edges between s and i made the distance between them of a length N.

## Overview

As we know in practice different edges can have different values. Exactly that was the case in weighted graphs. Going back to the road map example the distances between different cities are commonly evaluated in miles or kilometers. Of course we can associate any other meaningful value to this edges. This can be either time in hours to travel between cities, money for fuel or anything else.

[![Weighted Graphs in Practice](/wp-content/uploads/2012/10/6.-Weighted-Graphs-in-Practice.png)](/wp-content/uploads/2012/10/6.-Weighted-Graphs-in-Practice.png)In practice is more common to use weighted graphs than non-weighted graphs!

Now BFS can’t help us any more. Why? Because using non-equal values for the edges the triangle inequality is no longer true. Now the edge (the direct path) between A and B can be greater than the sum of the two edges (A, C) + (C, B)!

[![Triangle Inequality Problem](/wp-content/uploads/2012/10/7.-Triangle-Inequality-Problem.png)](/wp-content/uploads/2012/10/7.-Triangle-Inequality-Problem.png)In a weighted graph the edges aren’t equal for our BFS algorithm so we can’t use it!

In other words, assuming the same abstraction with balls and wires the hanging wires can’t be discarded so easily.

[![Weighted Graph as Balls and Strings](/wp-content/uploads/2012/10/8.-The-Graph-as-Balls-and-Strings.png)](/wp-content/uploads/2012/10/8.-The-Graph-as-Balls-and-Strings.png)On weighted graphs BFS is no longer useful!

So now how can we solve this problem? A very dummy approach is to break apart each edge with dummy vertices in order to make BFS work again.

[![Breaking apart edges](/wp-content/uploads/2012/10/9.-Breaking-apart-edges.png)](/wp-content/uploads/2012/10/9.-Breaking-apart-edges.png)Since the graph is weighted we can decompose its edges to more “dummy” edges!

However this approach has several weak points. The major one is that we’ll have to keep much more information, which means more memory usage, for even small graphs. This is done in case we break each edge on too many parts.

The solution of this problem was given by [Edsger Dijkstra](http://en.wikipedia.org/wiki/Edsger_W._Dijkstra) in 1956 and published in 1959. The only thing we should do now is to be sure that even discarding the triangle inequality we have the shortest paths. The first thing to do is to keep information for the distance from s to the parent (previous) node of i in the graph in order to calculate which distance is shorter.

In BFS we used a queue in order to walk through all the ancestors of a node. This was made consecutively. Thus for the graph G on the next image the order of enqueuing the ancestors of S was A, B, C.

[![Order of enqueuing](/wp-content/uploads/2012/10/10.-Order-of-enqueuing.png)](/wp-content/uploads/2012/10/10.-Order-of-enqueuing.png)The order of enqueuing in BFS is consecutive – something that isn’t working for weighted graphs!

The Dijkstra’s algorithm make use of a priority queue, also know as a heap. This fact combined by the fact we keep info for the shortest path so far help us find shortest paths in a weighted graphs.

Why this works? To answer this question let’s see the next very basic example, assuming the graph G from the next image. As we can see the triangle inequality isn’t true.

[![Weighted graph](/wp-content/uploads/2012/10/11.-Weighted-graph.png)](/wp-content/uploads/2012/10/11.-Weighted-graph.png)A weighted graph that doesn’t follow the triangle inequality!

OK, we see that the path [S, B, A] is shorter than [S, A] although the edge (S, A) exists. How the Dijkstra algorithm overcomes this problem.

First we have no information about the distances (S, A) and (S, B), the only thing we know is that S is the starting point, its distance is 0 and its path so far is the empty set. So first we enqueue in a priority the distances from S to A and B.

[![Dijkstra Priority Queue](/wp-content/uploads/2012/10/12.-Dijkstra-Priority-Queue.png)](/wp-content/uploads/2012/10/12.-Dijkstra-Priority-Queue.png)The algorithm of Dijkstra make use of a priority queue!

Now we dequeue the minimum (first in the heap) element from the queue – the closest node to S, which is B. Then all the nodes adjacent to S in the queue are tested for adjacency to B, thus if we have already the distance between S and A now we can test if its longer than (S, B) + (B, A) – the triangle inequality!

So far we know that we must change a bit BFS to get the Dijkstra algorithm. The only thing to do is to keep info for each node for the path through its parent and to use a priority queue.

## Code

Implementing this algorithms isn’t much more difficult than BFS, so here’s the code in [PHP](/category/php/). However this example make use of the standard php library SPL and the PriorityQueue data structure, but any developer can code [his own heap](/2012/08/07/computer-algorithms-heap-and-heapsort-data-structure/).

Here’s the graph from the code:

[![The Graph from the Code](/wp-content/uploads/2012/10/0.-Graph.png)](/wp-content/uploads/2012/10/0.-Graph.png)The graph!

```php
class vertex
{
    public $key         = null;
    public $visited     = 0;
    public $distance    = 1000000;  // infinite
    public $parent      = null;
    public $path        = null;
 
    public function __construct($key) 
    {
        $this->key  = $key;
    }
}
 
class PriorityQueue extends SplPriorityQueue
{
    public function compare($a, $b)
    {
        if ($a === $b) return 0;
        return $a > $b ? -1 : 1;
    }
}
 
$v0 = new vertex(0);
$v1 = new vertex(1);
$v2 = new vertex(2);
$v3 = new vertex(3);
$v4 = new vertex(4);
$v5 = new vertex(5);
 
$list0 = new SplDoublyLinkedList();
$list0->push(array('vertex' => $v1, 'distance' => 3));
$list0->push(array('vertex' => $v3, 'distance' => 1));
$list0->rewind();
 
$list1 = new SplDoublyLinkedList();
$list1->push(array('vertex' => $v0, 'distance' => 3));
$list1->push(array('vertex' => $v2, 'distance' => 7));
$list1->rewind();
 
$list2 = new SplDoublyLinkedList();
$list2->push(array('vertex' => $v1, 'distance' => 7));
$list2->push(array('vertex' => $v3, 'distance' => 8));
$list2->push(array('vertex' => $v4, 'distance' => 12));
$list2->rewind();
 
$list3 = new SplDoublyLinkedList();
$list3->push(array('vertex' => $v0, 'distance' => 1));
$list3->push(array('vertex' => $v2, 'distance' => 8));
$list3->rewind();
 
$list4 = new SplDoublyLinkedList();
$list4->push(array('vertex' => $v2, 'distance' => 12));
$list4->push(array('vertex' => $v5, 'distance' => 3));
$list4->rewind();
 
$list5 = new SplDoublyLinkedList();
$list5->push(array('vertex' => $v4, 'distance' => 3));
$list5->rewind();
 
$adjacencyList = array(
    $list0,
    $list1,
    $list2,
    $list3,
    $list4,
    $list5,
);
 
function calcShortestPaths(vertex $start, &$adjLists)
{
    // define an empty queue
    $q = new PriorityQueue();
 
    // push the starting vertex into the queue
    $q->insert($start, 0);
    $q->rewind();
 
    // mark the distance to it 0
    $start->distance = 0;
 
    // the path to the starting vertex
    $start->path = array($start->key);
 
    while ($q->valid()) {
        $t = $q->extract();
        $t->visited = 1;
 
        $l = $adjLists[$t->key];
        while ($l->valid()) {
            $item = $l->current();
 
            if (!$item['vertex']->visited) {
                if ($item['vertex']->distance > $t->distance + $item['distance']) {
                    $item['vertex']->distance = $t->distance + $item['distance'];
                    $item['vertex']->parent = $t;
                }
 
                $item['vertex']->path = array_merge($t->path, array($item['vertex']->key));
 
                $q->insert($item["vertex"], $item["vertex"]->distance);
            }
            $l->next();
        }
        $q->recoverFromCorruption();
        $q->rewind();
    }
}
 
calcShortestPaths($v0, $adjacencyList);
 
// The path from node 0 to node 5
// [0, 1, 2, 4, 5]
echo '[' . implode(', ', $v5->path) . ']';
```

## Complexity

The complexity of that code is based on the complexity of BFS with the main difference that we keep a priority queue. For BFS we knew that the complexity was O(|V| + |E|), while Dijkstra’s algorithm has running time of O((|V| + |E|).log(|V|)). That is quite natural since the heapsort’s complexity is O(n.log(n))!

## Application

Since the basic BFS can’t help us for weighted graphs and there are plenty of problems designed with weighted graphs obviously Dijkstra’s algorithm can be very handy. The only thing we should be aware of is the positive values of the edges.