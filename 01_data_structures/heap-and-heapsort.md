# Computer Algorithms: Heap and Heapsort

## Introduction

Heapsort is one of the general sorting algorithms that performs in O(n.log(n)) in the worst-case, just like [merge sort](/2012/03/05/computer-algorithms-merge-sort/) and [quicksort](/2012/03/13/computer-algorithms-quicksort/), but sorts in place – as quicksort. Although quicksort’s worst-case sorting time is O(n2) it’s often considered that it beats other sorting algorithms in practice. Thus in practice quicksort is “faster” than heapsort. In the same time developers tend to consider heapsort as more difficult to implement than other n.log(n) sorting algorithms.

In the other hand heapsort uses a special data structure, called heap, in order to sort items in place and this data structure is quite useful in some specific cases. Thus to understand heapsort we first need to understand what is a heap.

So first let’s take a look at what is a heap.

## Overview

A heap is a complete binary tree, where all the parents are greater than their children (max heap). If all the children are greater than their parents it is considered to call the heap a min-heap. But first what is a complete binary tree? Well, this is a binary tree, where all the levels are full, except the last one, where all the items are placed on the left (just like on the image below).

[![Complete Binary Tree](/wp-content/uploads/2012/08/1.-Complete-Binary-Tree.png)](/wp-content/uploads/2012/08/1.-Complete-Binary-Tree.png)A complete binary tree is a structure where all the levels are completely full, except the last level, where all the items are placed on the left!

Combined with the fact that each node contains a greater key than its children, a heap may look like the tree on the following diagram.

[![Heap](/wp-content/uploads/2012/08/2.-Heap.png)](/wp-content/uploads/2012/08/2.-Heap.png)In a max-heap each node contains a greater value than its children. Respectively in a min-heap each node contains a smaller value than its parent!

The thing is that if we put indices next to each node of this tree, starting from the root (index 1) and continuing from left to right on each level, we’ll get the following tree.

[![Heap Indices](/wp-content/uploads/2012/08/3.-Heap-Indexes.png)](/wp-content/uploads/2012/08/3.-Heap-Indexes.png)Putting indices right to each node reveals the secret of the heap. The i-th node has left child exactly with the index 2*i, and right child with index 2*i+1! This is a great opportunity to put this tree into an array!

Now if we take a closer look to the picture above we can see that the indices of a node and its children are closely related. Thus for a node of an index i we see that its left child has the index 2*i, while its right child’s index is 2*i + 1.

This particular order gives us the possibility to store each heap in an array.

[![Heap as an Array](/wp-content/uploads/2012/08/4.-Heap-as-an-Array.png)](/wp-content/uploads/2012/08/4.-Heap-as-an-Array.png)The heap tree can be easily represented as an array!

Since in a heap its greater element is in the root of the tree (for max-heap, respectively in a min-heap its smallest element is the root) we need to answer two questions. 

- How to build a heap out of an ordinary array?
- After extracting the root, which is the greatest (smallest) item, how can we rebuild the heap in order to keep it a heap again?

First let’s try to answer the first question. How to build a heap? Well, let’s forget about the array for a while and let’s take a look on a ordinary binary tree with only three nodes.

[![Heapify](/wp-content/uploads/2012/08/5.-Heapify.png)](/wp-content/uploads/2012/08/5.-Heapify.png)Fixing a node and its children in order to form a valid Heap is often called heapify!

We see that the three green nodes destroy the structure of our heap, because the root (1) is smaller than its children (4) and (5). Thus we need to fix this problem and what we’re going to do is to swap the root with its biggest child. 

[![Heapify Part 2](/wp-content/uploads/2012/08/6.-Heapify-Part-1.png)](/wp-content/uploads/2012/08/6.-Heapify-Part-1.png)We first need to know which is the greatest out of the three items, than in case it is not the root, swap its value with the root!

As you can see on the picture above the i-th item is first compared to its left child. The greater of these two items is compared to the right child. Note that we don’t swap them – we just compare them to get which one is greater. Once we find the greatest of these three values we swap them with the root in case it’s not the root value.

Although now these three elements form a heap, by swapping the root with one of its children may destroy the heap constructed out of this child. That is why we continue the same procedure with it.

This actually gives us the procedure to heapify the three nodes constructed out of the i-th item and its children. However to build a heap from an arbitrary array we should perform this operation starting from floor(len[A] / 2) down to the first item in the array.

[![Random Array to Heap](/wp-content/uploads/2012/08/7.-Random-Array-to-Heap.png)](/wp-content/uploads/2012/08/7.-Random-Array-to-Heap.png)Building a random array into a heap isn’t that difficult since we know that half of the complete tree items lay in it’s lowest level! Thus we start from floor(len[A] / 2)!

Why? Well, a complete binary tree with a full last level contains n/2 + 1 nodes in it. They don’t have children, thus we don’t need to check them – they are “sorted”. Indeed if we start from an item on the right of floor(len[A] / 2) there won’t be items with indices 2*i and 2*i + 1.

## Code

So far we know how to build the heap. Next thing is to swap the first and the last element of the array and rebuild the heap. Here’s the PHP code of how to do this.

```php
$a = array(1, 6, 3, 8, 2, 5, 4);
 
function heapify(&$a, &$i, &$heap_size)
{
    $l = $i*2 + 1;
    $r = $i*2 + 2;
 
    if ($l  -1; $i--) {
        heapify($a, $i, $heap_size);
    }
}
 
function heapsort(&$a)
{
    $heap_size = count($a);
    build_heap($a, $heap_size);
 
    while ($heap_size--) {
        $t = $a[$heap_size];
        $a[$heap_size] = $a[0];
        $a[0] = $t;
        build_heap($a, $heap_size);
    }
}
 
// 1 2 3 4 5 6 8
heapsort($a);
```

## Complexity

OK, the last question is – how do we know that this algorithm sorts in place in n.log(n) time? Let’s explore the algorithm one more time. The heapify worst-case is when we start from the root down to the lowest level of the tree. In these terms if the tree height is h, the time is O(h), but because the tree is balanced (complete) the time in terms of n is O(log(n)). 

In the other hand to build a heap we walk from floor(len[A] / 2) to 0, which makes it run in O(n.log(n)). However there is only one case when the heapify may run in log(n), and that is when it starts from the root, so it’s not absolutely true that building the heap runs in n.log(n).

Indeed heapify depend on the level it has been started. It doesn’t run for the last ceil(n/2) items and it runs in O(1) for another 2h-1. Thus in practice we can build a heap in O(n). 

Once we have the heap built, the only thing to do is to extract its first element and rebuild – heapify from the first item. This makes the sorting algorithm run in O(n.log(n)) – just like quicksort and mergesort.

## Application

As I said in the beginning of this post quicksort is often the fastest general purpose algorithm in practice. This makes both merge sort and heapsort not so popular. However heapsort introduces an interesting data structure which can help us in many other cases. 

It’s initially used to implement priority queues. What is great about a heap is that after we build it, which we know how to do in linear time, we can extract the greatest value – thus taking the highest priority task. Then with rebuilding the heap we can extract the next priority and so on, without fully sorting the array. 

This makes the heapsort the only sorting algorithm that can sort the first k items out of a set of n items without sorting the whole set.

Indeed let’s say we have a set of positive integers and we’d like to get the biggest sum out of three items. Obviously we can sort the array and take the greatest three numbers, but this will cost us n.log(n) time, while using heapsort we can do it much faster! And all this without extra space – in place!