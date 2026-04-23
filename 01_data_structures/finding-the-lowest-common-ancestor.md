# Computer Algorithms: Finding the Lowest Common Ancestor

## Introduction

Here’s one task related to the tree data structure. Given two nodes, can you find their lowest common ancestor? 

In a matter of fact this task always has a proper solution, because at least the root node is a common ancestor of all pairs of nodes. However here the task is to find the lowest one, which can be quite far from the root. 

[![Finding the Lowest Common Ancestor](/wp-content/uploads/2012/08/1.-Finding-the-Lowest-Common-Ancestor.png)](/wp-content/uploads/2012/08/1.-Finding-the-Lowest-Common-Ancestor.png)Finding the Lowest Common Ancestor

We don’t care what kind of trees we have. However the solution, as we will see, can be very different depending on the tree type. Indeed finding the lowest common ancestor can have linear complexity for binary search trees, which isn’t true for ordinary trees.

## Overview

Let’s say we have a tree (not binary!) and two nodes from this tree. The task is to find their lowest common ancestor. The thing is that we don’t know much about where they appear to be in the tree. 

We can think of this tree as a DOM tree of any single HTML page online. It is not binary or balanced and can’t be sure where these nodes are. 

[![Lowest Common Ancestor](/wp-content/uploads/2012/08/2.-Lowest-Common-Ancestor.png)](/wp-content/uploads/2012/08/2.-Lowest-Common-Ancestor.png) 

First we must find both paths from the root to each one of the target nodes. Note that this requires additional memory! Then, in linear time we can pass through these “paths” and scan them from the root down to the nodes. We expect these to arrays to be equal at least in their first element (the root).  Using this scenario the lowest common ancestor is the last equal element in both arrays. 

[![Lowest Common Ancestor Paths](/wp-content/uploads/2012/08/3.-Lowest-Common-Ancestor.png)](/wp-content/uploads/2012/08/3.-Lowest-Common-Ancestor.png)Once we know the paths from the root down to the nodes, we can compare them in order to find the lowest common ancestor!

To see how this algorithm can be dramatically changed depending on the data structure, let’s see another example. Now let’s say we have a binary search tree (BST). We know that in a BST all the elements in the left sub-tree are smaller than the root and all the items on the right sub-tree are greater than the root. This is true also for the left and the right sub-trees.

Now because we’re searching the lowest common ancestor, we don’t need to collect the paths from the root to the nodes in two arrays. We just know that the greater items are on the right, while the smaller items are on the left. This can help us find the lowest ancestor starting directly from the root.

[![Lowest Common in a BST](/wp-content/uploads/2012/08/4.-Lowest-Common-in-a-BST.png)](/wp-content/uploads/2012/08/4.-Lowest-Common-in-a-BST.png)In a BST we compare both values with a given node (starting from the root). In case the node’s value is in between them – this is the lowest common ancestor. If not – we go either on the left or on the right!

What we do is to compare the two keys of the target nodes with the root key. If one of the keys are smaller, and the other is greater than the root’s key, then obviously the root is the lowest common ancestor. This is true because one of the items will be somewhere in the left sub-tree, while the other will be in the right sub-tree. 

In case both values are greater (or smaller) than the root, we can move to the right (or to the left) sub-tree and try again with the same procedure. Thus the first node which key is in between the two target values will be the lowest common ancestor.

## Code

Here’s a very simple PHP implementation showing us these two algorithms.

```php
class Tree
{
    public $node = null;
    public $id = null;
    public $parent = null;
    public $children = array();
 
    public function __construct($node, $id = null)
    {
        $this->node = $node;
        $this->id = $id;
    }
 
    public function addChild(Node &$n)
    {
        $n->parent = $this;
        $this->children[] = $n;
    }
 
    /**
     * Returns an element by its id
     * 
     * @param mixed $id
     * @return Node 
     */
    public function search($id)
    {
        if ($this->id == $id) {
            return $this;
        }
 
        $a = false;
 
        // search all the children starting from the left-most
        foreach ($this->children as $child) {
            $a = $child->search($id);
        }
 
        return $a;
    }
 
    /**
     * Finds a path from the root to the 
     * item and returns it as a list
     * 
     * @param mixed $id 
     * @return array
     */
    public function find_path($id, &$path)
    {
        array_push($path, $this->id);
 
        if ($this->id == $id) {
            return 1;
        }
 
        foreach ($this->children as $child)  {
            if (1 == $child->find_path($id, $path)) return 1;
            array_pop($path);
        }
    }
 
    public function __toString()
    {
        return $this->node . ' ' . $this->id . "\n";
    }
}
 
$dom = new Tree('DOM', 'ROOT');
 
$body = new Tree('BODY', 1);
$div1 = new Tree('DIV', 'div-1');
$div2 = new Tree('DIV', 'my-id');
 
$a = new Tree("A", 'some-link');
 
$dom->addChild($body);
$body->addChild($div1);
$body->addChild($div2);
$div2->addChild($a);
 
$path1 = $path2 = array();
$dom->find_path('div-1', $path1);
$dom->find_path('some-link', $path2);
```

## Finding Lowest Common Ancestor in a BST

```php
class Tree
{
    public $key;
 
    public $parent  = null;
    public $left    = null;
    public $right   = null;
 
    public function __construct($key) 
    {
        $this->key = $key;
    }
 
    public function insert(Tree $n) 
    {
        if ($this->key key) {
            if ($this->right == null) {
                // insert
                $this->right = $n;
                $n->parent = $this;
            } else {
                $this->right->insert($n);
            }
        }
        if ($this->key > $n->key) {
            if ($this->left == null) {
                // insert
                $this->left = $n;
                $n->parent = $this;
            } else {
                $this->left->insert($n);
            }
        }
    }
}
 
$t = new Tree(10);
 
$n1 = new Tree(20);
$n2 = new Tree(5);
$n3 = new Tree(7);
$n4 = new Tree(13);
 
$t->insert($n1);
$t->insert($n2);
$t->insert($n3);
 
function find_common($node1, $node2, $tree) 
{
    if ($node1->key key && $node2->key > $tree->key) {
        return $tree;
    } else if ($node1->key key && $node2->key key) {
        find_common($node1, $node2, $tree->left);
    } else if ($node1->key > $tree->key && $node2->key > $tree->key) {
        find_common($node1, $node2, $tree->right);
    }
}
 
$node = find_common($n3, $n4, $t);
```

## Application

A typical use-case of this algorithm is finding the lowest common ancestor of two nodes in a DOM tree. Sometimes we just need to attach an event listener to both items (even before they are attached to the DOM!). Although attaching this event to the “document” will work just fine, all the elements from the nodes up to the root will be “capturing” these events due to event bubbling. Thus attaching the event to the lowest common ancestor is a better solution.