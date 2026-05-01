# Computer Algorithms: Binary Search Tree

## Introduction

[Linked lists](/2012/06/14/computer-algorithms-linked-list-data-structure/) are easy to build and modify, but searching one is slow: in the worst case we have to visit every item, giving `O(n)` complexity. The same is true of a sequential scan over an array.

[![Search over Linked Lists and Arrays](../images/1.-Search-over-Linked-Lists-and-Arrays.png)](../images/1.-Search-over-Linked-Lists-and-Arrays.png)Sequential search over arrays seems much like searching in linked lists and it is a basically ineffective opration!

Arrays at least support binary search by jumping to the middle index — but linked lists have no direct access, so that trick is unavailable. To search a dynamic structure faster than `O(n)` we need a different data structure altogether.

### Tree terminology

A *tree* is a structure where each node holds some data and points to its parent and its children. Two pieces of vocabulary cover almost everything we need:

- **Root** — the only node with no parent.
- **Leaf** — a node whose children are all `NIL`.
- A node together with everything reachable below it forms a **sub-tree**, which is itself a tree. This recursive definition is what makes most tree algorithms naturally recursive.

[![A tree](../images/2.-A-tree.png)](../images/2.-A-tree.png)A tree data structure. Each item points to its parent and its children. The root’s parent is NIL.

[![Root and Leafs](../images/3.-Root-and-Leafs.png)](../images/3.-Root-and-Leafs.png)Root and leaves.

[![Trees](../images/4.-Trees.png)](../images/4.-Trees.png)Possible trees.

[![Sub-trees](../images/5.-Sub-trees.png)](../images/5.-Sub-trees.png)Every node is the root of its own sub-tree.

## Binary Search Tree

A **binary tree** restricts each node to at most two children — conventionally called *left* and *right*.

[![Binary Tree](../images/6.-Binary-Tree.png)](../images/6.-Binary-Tree.png)In the binary tree each node has at most two sub-trees – left and right!

A plain binary tree, however, is no better for search than any other tree: without an ordering rule we still have to inspect every node. The fix is to enforce the **BST property**:

> For every node `x`, all keys in `x.left` are smaller than `x.key`, and all keys in `x.right` are larger.

[![Binary search tree](../images/7.-Binary-search-tree.png)](../images/7.-Binary-search-tree.png)Binary search tree – BST.

That single rule makes both insert and search trivial: at each node, compare the key and pick a side.

[![Insert in BST](../images/8.-Insert-in-BST.png)](../images/8.-Insert-in-BST.png)Inserting in a binary search tree is fairly easy.

## Implementation

The BST is built from one passive struct (the node) and three recursive routines (insert, search, in-order traversal). Each routine is short on its own.

### The node

```
Node:
    key
    parent  ← NIL
    left    ← NIL
    right   ← NIL
```

### Inserting a key

Walk down the tree comparing keys; attach the new node when an empty slot is found.

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

The BST property guarantees that the target — if present — lies on exactly one side at every step.

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

### Deleting a key

Deletion is the only BST operation with real subtlety. After locating the node `z` we want to remove, we have to splice it out without breaking the BST property. There are three cases:

1. **`z` is a leaf** — just detach it from its parent.
2. **`z` has one child** — replace `z` with that child.
3. **`z` has two children** — `z` cannot simply be removed, because its slot has to keep ordering both subtrees. We replace `z`'s key with its **in-order successor** (the smallest key in `z.right`), then delete that successor from `z.right`. The successor is the leftmost node of `z.right`, so by construction it has no left child and falls into case 1 or 2.

A helper finds the minimum of a sub-tree:

```
MIN(node):
    while node.left ≠ NIL do
        node ← node.left
    return node
```

Then `DELETE` dispatches on the three cases:

```
DELETE(root, key):
    z ← SEARCH(root, key)
    if z = NIL then
        return                            // key not in tree

    if z.left = NIL and z.right = NIL then
        REPLACE(z, NIL)                   // case 1: leaf
    else if z.left = NIL then
        REPLACE(z, z.right)               // case 2a: only right child
    else if z.right = NIL then
        REPLACE(z, z.left)                // case 2b: only left child
    else
        s ← MIN(z.right)                  // case 3: in-order successor
        z.key ← s.key                     // copy successor's key into z
        REPLACE(s, s.right)               // splice out the successor
```

`REPLACE(old, new)` rewires `old`'s parent to point at `new` (or makes `new` the tree's root if `old` was the root) and sets `new.parent` accordingly:

```
REPLACE(old, new):
    if old.parent = NIL then
        root ← new
    else if old = old.parent.left then
        old.parent.left ← new
    else
        old.parent.right ← new
    if new ≠ NIL then
        new.parent ← old.parent
```

Each of these steps walks at most one root-to-leaf path, so deletion is `O(h)` where `h` is the height of the tree — the same bound as search.

### Traversing in order

Visiting *left – root – right* yields the keys in ascending order — useful for printing and a building block for rebalancing (see [Balancing a Binary Search Tree](./balancing-a-binary-search-tree.md)).

```
IN_ORDER(node):
    if node = NIL then
        return
    IN_ORDER(node.left)
    visit(node)
    IN_ORDER(node.right)
```

## Search Complexity

Each step of `SEARCH` descends one level, so the work is proportional to the height of the tree.

- **Best / average case** — a roughly balanced tree has height `O(log n)`, so search is `O(log n)`.
- **Worst case** — if keys are inserted in sorted order every node ends up on one side, and the tree degenerates into a linked list of height `n`, giving `O(n)`.

[![Tree or a Linked list](../images/9.-Tree-or-a-Linked-list.png)](../images/9.-Tree-or-a-Linked-list.png)Inserting only greater keys produces a tree with no left children — indistinguishable from a linked list.

[![O(n) vs. O(log n) search cost](../images/BST-Chart.png)](../images/BST-Chart.png)Worst-case comparisons as `n` grows. At `n = 1000`, an unbalanced tree needs ≈1000 comparisons while a balanced one needs ≈10 — the gap widens as `n` grows.

## Further Optimization

Because the worst case is so bad, in practice we either trust that input is unordered or we maintain the **balanced BST** invariant: the heights of the left and right sub-trees of every node differ by at most one. The full procedure is covered in the [Balancing a Binary Search Tree](./balancing-a-binary-search-tree.md) article.

[![Balanced or not](../images/10.-Balanced-or-not.png)](../images/10.-Balanced-or-not.png)Searching in a balanced tree is significantly faster than in some binary search trees!

## Application

Binary search trees are cheap to build and maintain, and they give `O(log n)` search, insert, and delete whenever the input is reasonably unordered — without the bookkeeping cost of a self-balancing variant. Beyond lookup, the in-order traversal defined above is useful whenever data needs to be processed in key order: printing a sorted dump, range queries, or feeding a rebalance routine.
