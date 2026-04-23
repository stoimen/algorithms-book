# Computer Algorithms: Topological Sort of a Graph

## Introduction

Let’s assume we have a list of tasks to accomplish. Some of the tasks depend on others, so we must be very careful with the order of their execution. If the relationship between these tasks were simple enough we could represent them as a linked list, which would be great, and we would know the exact order of their execution. The problem is that sometimes the relations between the different tasks are more complex and some tasks depend on two or more other tasks, which in their turn depend on one or more tasks, etc.

Thus we can’t model this problem using linked lists or trees. The only rational solution is to model the problem using a graph. What kind of graph do we need? Well, we definitely need a directed graph, to desribe the relations, and this graph shouldn’t have cycles. So we need the so called directed acyclic graph (DAG).

[![Topological Sort. Directed Graph.](/wp-content/uploads/2012/10/1.-TS-Directed-Graph.png)](/wp-content/uploads/2012/10/1.-TS-Directed-Graph.png)In order to sort a graph using topological sort we need this graph to be acyclic and directed!

Why we don’t what a cycle in the graph? The answer of this question is simple and obvious. In case of cyclic graph, we wouldn’t be able to determine the priority of task execution, thus we won’t be able to sort the tasks properly.

Now the solution we want is to sort the vertices of the graph in some order so for each edge (u, v) u will precede v. Then we’ll have a linear order of all tasks and by starting their execution we’ll know that everything will be OK.

[![Topological Sort. Sort the vertices.](/wp-content/uploads/2012/10/2.-TS-Sort-the-vertices.png)](/wp-content/uploads/2012/10/2.-TS-Sort-the-vertices.png)The output of topological sort should be a list of vertices!

This kind of sort is also known as “topological” sort (or topsort) and it is one of the very basic graph algorithms.

## Overview

OK, so we have an acyclic directed graph, how do we proceed to get a linked list with all the vertices sorted? Since it’s an acyclic graph we know that there is at least one vertex without predecessor. Thus at first place, we can put all the vertices without predecessors into our linked list.

[![Topological Sort. First Step.](/wp-content/uploads/2012/10/3.-TS-First-Step.png)](/wp-content/uploads/2012/10/3.-TS-First-Step.png)Initially we get only the vertices without a predecessor!

This approach answers the question – is there a possibility to have more than one valid topological sort of a graph? Indeed, the only thing we’d like to do is to put all the vertices in the correct order, but since there might be vertices with no predecessors any combination of them will be a valid topological sort for a graph.

As we can see from the picture above even for vertices with predecessors our topological sort can vary. Thus [9, 6, 2, 7, 4, 1] is a valid topological sorted graph, but [6, 9, 2, 7, 4, 1] is also a valid topological sort out of the same graph!

Now we can generalize the algorithm in some basic steps.

1. Make an empty list L and an empty list S;

2. Put all the vertices with no predecessors in L;

3. While L has items in it;

    3.1. Pop an item from L – n, and push it to S;

    3.2. For each vertex m adjacent to n;

         3.2.1. Remove (n, m);

	 3.2.2. If m has no predecessors – push it to L;

[![Topological Sort. Second Step.](/wp-content/uploads/2012/10/4.-TS-Second-Step.png)](/wp-content/uploads/2012/10/4.-TS-Second-Step.png)The image above explains step 3.2. from the algorithm!

## Code

Here’s the very basic [PHP](/category/php/) implementation. As you can see the short implementation shows us how easy this algorithm is. However its importance to computer science and programming is enormous.

```php
class G
{
    protected $_g = array(
        array(0, 1, 1, 0, 0, 0, 0),
        array(0, 0, 0, 1, 0, 0, 0),
        array(0, 0, 0, 0, 1, 0, 0),
        array(0, 0, 0, 0, 1, 0, 0),
        array(0, 0, 0, 0, 0, 0, 1),
        array(0, 0, 0, 0, 0, 0, 1),
        array(0, 0, 0, 0, 0, 0, 0),
    );
    protected $_list = array();
    protected $_ts   = array();
    protected $_len  = null;
 
    public function __construct()
    {
        $this->_len = count($this->_g);
 
        // finds the vertices with no predecessors
        $sum = 0;
        for ($i = 0; $i _len; $i++) {
            for ($j = 0; $j _len; $j++) {
                $sum += $this->_g[$j][$i];
            }
 
            if (!$sum) {
                // append to list
                array_push($this->_list, $i);
            }
            $sum = 0;
        }
    }
 
    public function topologicalSort() 
    {
        while ($this->_list) {
            $t = array_shift($this->_list);
            array_push($this->_ts, $t);
 
            foreach ($this->_g[$t] as $key => $vertex) {
                if ($vertex == 1) {
                    $this->_g[$t][$key] = 0;
 
                    $sum = 0;
                    for ($i = 0; $i _len; $i++) {
                        $sum += $this->_g[$i][$key];
                    }
 
                    if (!$sum) {
                        array_push($this->_list, $key);
                    }
                }
                $sum = 0;
            }
        }
 
        print_r($this->_ts);
    }
}
 
$g = new G();
/*
Array
(
    [0] => 0
    [1] => 5
    [2] => 1
    [3] => 2
    [4] => 3
    [5] => 4
    [6] => 6
)*/
$g->topologicalSort();
```

## Application

As I already mentioned above this algorithm is practically used to sort the execution of different tasks that depend on each other. However this isn’t its only use. Actually any kind of objects that depend on each other can be modeled with a graph. Indeed sometimes these graphs may be a trees, but most of the cases that isn’t true.