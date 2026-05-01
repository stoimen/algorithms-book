# Computer Algorithms: Balancing a Binary Search Tree

## Introduction

The [binary search tree](/2012/06/22/computer-algorithms-binary-search-tree-data-structure/) is a very useful data structure, where searching can be significantly faster than searching into a linked list. However in some cases searching into a binary tree can be as slow as searching into a linked list and this mainly depends on the input sequence. Indeed in case the input is sorted the binary tree will seem much like a linked list and the search will be slow. 

[![Inserting into a binary search tree](../images/1.-Inserting-into-a-binary-search-tree.png)](../images/1.-Inserting-into-a-binary-search-tree.png)A binary search tree may seem much like a linked lists if the input is nearly sorted!

To overcome this we must change a bit the data structure in order to stay well balanced. It’s intuitively clear that the searching process will be better if the tree is well branched. This is when finding an item will become faster with minimal effort.

[![Balanced tree](../images/2.-Balanced-tree.png)](../images/2.-Balanced-tree.png)Searching into a balanced tree is significantly faster than searching into a non-balanced tree!

Since we know how to construct a binary search tree the only thing left is to keep it balanced. Obviously we will need to re-balance the tree on each insert and delete, which will make this data structure more difficult to maintain compared to non-balanced search trees, but searching into it will be significantly faster.

## Overview

The key observation is simple: **the middle element of a sorted sequence makes an ideal root.** Half of the remaining keys are smaller and half are larger, so each subtree gets roughly the same number of nodes.

[![Balanced vs. Non-Balanced](../images/3.-Balanced-vs.-Non-Balanced.png)](../images/3.-Balanced-vs.-Non-Balanced.png)

For example, given `[1, 2, 3, 4, 5, 6, 7]`, choosing `4` as the root produces a balanced tree, while inserting the values in their original order produces a tree that degenerates into a linked list.

That gives us a straightforward rebalancing recipe in three steps:

1. **Flatten** the tree into a sorted list. An in-order (*left – root – right*) traversal of a BST already visits keys in sorted order, so this is essentially free.
2. **Pick the middle item** as the new root — trivial once we know the list’s length.
3. **Rebuild** by applying the same procedure to the left and right halves.

The catch is that the tree’s middle changes whenever we insert or delete. A tree built from `[1,2,3,4,5]` has root `3`; after inserting `[44,45,46,47,48]` the middle shifts, so a strict definition of "balanced" would force us to rebalance after every modification.

## Balancing Optimization

Rebalancing on every insert and delete is expensive, so in practice we batch the work.

**Bulk insert.** Insert many keys without rebalancing in between, then rebalance once at the end. The intermediate tree may be skewed, but we pay the rebalance cost only once instead of once per insert.

[![Bulk Insert with Only one Balance](../images/4.-Bulk-Insert-with-Only-one-Balance.png)](../images/4.-Bulk-Insert-with-Only-one-Balance.png)Doing bulk insert/delete and only one balancing will make the data structure faster!

**Lazy delete.** Instead of physically unlinking a node, mark its data as `NIL` and leave the structure intact. Searches remain fast because the shape of the tree is unchanged; the deleted slots are reclaimed during the next rebalance. The trade-off is memory: tombstoned nodes occupy space until then, so this only pays off when deletes are frequent and rebalances are rare.

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
