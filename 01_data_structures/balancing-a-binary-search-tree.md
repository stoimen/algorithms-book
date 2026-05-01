# Computer Algorithms: Balancing a Binary Search Tree

## Introduction

The [binary search tree](/2012/06/22/computer-algorithms-binary-search-tree-data-structure/) is a very useful data structure, where searching can be significantly faster than searching into a linked list. However in some cases searching into a binary tree can be as slow as searching into a linked list and this mainly depends on the input sequence. Indeed in case the input is sorted the binary tree will seem much like a linked list and the search will be slow. 

[![Inserting into a binary search tree](../images/1.-Inserting-into-a-binary-search-tree.png)](../images/1.-Inserting-into-a-binary-search-tree.png)A binary search tree may seem much like a linked lists if the input is nearly sorted!

To overcome this we must change a bit the data structure in order to stay well balanced. It’s intuitively clear that the searching process will be better if the tree is well branched. This is when finding an item will become faster with minimal effort.

[![Balanced tree](../images/2.-Balanced-tree.png)](../images/2.-Balanced-tree.png)Searching into a balanced tree is significantly faster than searching into a non-balanced tree!

Since we know how to construct a binary search tree the only thing left is to keep it balanced. Obviously we will need to re-balance the tree on each insert and delete, which will make this data structure more difficult to maintain compared to non-balanced search trees, but searching into it will be significantly faster.

## Overview

In order to balance a tree we can go for the very basic and intuitive approach. First let’s take a look of one non-balanced tree.

[![Balanced vs. Non-Balanced](../images/3.-Balanced-vs.-Non-Balanced.png)](../images/3.-Balanced-vs.-Non-Balanced.png)

Compared to the balanced tree on the right from the image above with the same items we see that the root is approximately equal to its middle item. I.e. 4 is the middle item of the sequence [1,2,3,4,5,6,7]!

If we take a look of the sequence [2 3 4], clearly by building a binary tree it will look like a linked list. However if we choose the middle item for a root – we’ll easy build a balanced tree. So the only thing to do is to get the middle item out of a list.

We now see that building a balanced binary tree out of a sorted linked list isn’t that difficult. In the other hand, as I said above, on each insert we’ll have to rebalance the tree. You can think of the tree out of the values [1,2,3,4,5] and the same tree after inserting [44,45,46,47,48]. Clearly the root of the resulting tree will no longer be 3. 

So we need to implement the re-balancing in three basic operations. First we need to build a linked list out of a balanced binary tree. On the second place we’ll have to find the middle item and on the third place we’ll have to build again a balanced search tree. 

Hopefully the first two tasks are easy to implement, because making out a sorted list out of a binary search tree is very easy. We need just to walk through the tree from left-root-right recursively. Because smaller items are in the left sub-tree and greater items are on the right we’re sure that the resulting list will be sorted. Then finding the middle item is as easy as finding the middle index of an array know its length.

## Balancing Optimization

Of course the main problem of re-balancing a tree on each insert/delete is that this operations will be slow and soon or later we’ll have problems. That can happen if we change often our data structure. That’s why we should think of some optimization. 

Normally we insert and re-balance on each step, which is slow. In the other hand we can do bulk insert forgetting about the re-balancing for a while. Only after the inserts are done we can go for re-balancing the entire tree.

[![Bulk Insert with Only one Balance](../images/4.-Bulk-Insert-with-Only-one-Balance.png)](../images/4.-Bulk-Insert-with-Only-one-Balance.png)Doing bulk insert/delete and only one balancing will make the data structure faster!

The same approach we can use with bulk delete. We can just set to NIL the items we want to delete, but we can keep them in memory for a while. Thus the search will stay relatively fast without rebalancing the tree. However this approach can be used carefully because we’ll keep some data in the memory without actually using it. 

[![Bulk Delete](../images/5.-Bulk-Delete.png)](../images/5.-Bulk-Delete.png)We can NULL items without actually removing the pointers (links) and the structure of the tree!

## Implementation

Implementing a balanced binary tree is harder than implementing a plain BST, but the rebalancing algorithm splits cleanly into three independent steps:

1. **Flatten** the tree into a sorted list (in-order traversal).
2. **Pick the middle** element of that list as the new root.
3. **Recurse** on the left and right halves to rebuild the rest of the tree.

Below is the pseudocode for each piece. Each routine is short on its own; the trick is to compose them.

### The node

Each node stores a key, a payload, and links to its parent and children.

```
Node:
    key
    data
    parent  ← NIL
    left    ← NIL
    right   ← NIL
```

### Inserting a key

A standard BST insert: walk down the tree comparing keys and attach the new node when an empty slot is found.

```
INSERT(root, new):
    if root = NIL then
        root ← new
        return

    if new.key < root.key then
        if root.left = NIL then
            root.left  ← new
            new.parent ← root
        else
            INSERT(root.left, new)
    else
        if root.right = NIL then
            root.right ← new
            new.parent ← root
        else
            INSERT(root.right, new)
```

### Searching by key

Because the tree is ordered, we descend left when the target key is smaller and right when it is larger. The work done is proportional to the height of the tree — `O(log n)` when balanced, `O(n)` in the degenerate case.

```
SEARCH(node, key):
    if node = NIL then
        return NIL
    if node.key = key then
        return node
    if key < node.key then
        return SEARCH(node.left,  key)
    else
        return SEARCH(node.right, key)
```

### Flattening the tree (in-order traversal)

Visiting the tree in *left – root – right* order produces the keys in sorted order — exactly what we need before rebalancing.

```
IN_ORDER(node):
    if node = NIL then
        return [ ]
    return IN_ORDER(node.left)
         ++ [ node ]
         ++ IN_ORDER(node.right)
```

### Rebalancing

Since the list returned by `IN_ORDER` is sorted, its middle element makes a perfect root: roughly half of the keys are smaller and half are larger. Recursively applying the same idea to each half builds a balanced tree.

```
BALANCE(tree):
    list ← IN_ORDER(tree.root)
    tree.root ← NIL                  // discard the old structure
    BUILD(tree, list)

BUILD(tree, list):
    if list is empty then
        return
    m ← ⌊length(list) / 2⌋           // middle index
    INSERT(tree.root, list[m])
    BUILD(tree, list[0 .. m-1])      // left half
    BUILD(tree, list[m+1 .. end])    // right half
```

The traversal is `O(n)`. Each `BUILD` step performs an `INSERT` that walks an `O(log n)` path, giving `O(n log n)` overall — the cost we pay once per rebalance.

## Complexity of Searching

Compared to non-balanced binary search trees we’re sure that searching into a balanced trees is quick enough. The maximum height of the tree is log(n) so the worst-case searching is O(log(n)).

[![BST Chart](../images/BST-Chart.png)](../images/BST-Chart.png)Compared to searching in linked lists in O(n) time, searching into a balanced binary tree is O(log(n)) in the worst-case scenario!

## Application

Searching into a balanced binary tree is fast. What is more important is that we’re sure that in the worst-case scenario the search is O(log(n)). The only problem is that keeping a tree balanced is a slow operation that consumes too much resources and must be performed carefully.
