# Computer Algorithms: Bubble Sort

## Overview

It’s weird that bubble sort is the most famous sorting algorithm in practice since it is one of the worst approaches for data sorting. Why is bubble sort so famous? Perhaps because of its exotic name or because it is so easy to implement.

Bubble sort consists of comparing each pair of adjacent items. If the item on the left is larger than the item on the right, the two items swap places. During one full pass through the list, larger items keep moving to the right, so the largest unsorted item ends the pass in its final position.

![Optimized bubble sort infographic](../images/bubble-sort-infographic.png)

The infographic shows the optimized version of bubble sort. Each pass scans the unsorted part of the array from left to right. Adjacent elements are compared, and if the left element is larger than the right element, they swap. This pushes larger elements toward the end of the array.

After a pass finishes, the largest element from the unsorted part is already in its final position. The next pass can therefore ignore that sorted zone and scan one fewer element. If a complete pass performs no swaps, the algorithm stops early because the array is already sorted.

Even with these optimizations, bubble sort can still require a tremendous number of comparisons and swaps. That is the main reason it is rarely a good practical choice for large lists. Still, the algorithm is useful for learning because the idea is simple and the movement of elements is easy to visualize.

## Pseudo Code

This version sorts the list in ascending order. It mutates the input list in place, stops early when a complete pass makes no swaps, and shortens the scan range after each pass because the largest unsorted item has already reached its final position.

```text
procedure bubbleSort(A)
  n = length(A)

  repeat
    swapped = false

    for i = 0 to n - 2
      if A[i] > A[i + 1]
        swap A[i] and A[i + 1]
        swapped = true
      end if
    end for

    n = n - 1
  until swapped = false

  return A
end procedure
```

## Complexity

The average and worst-case complexity of bubble sort is `O(n^2)`. This is slow compared to algorithms like quicksort, merge sort, and heapsort, which run in `O(n log n)` on average.

With the early-exit flag, the best-case complexity is `O(n)`: if the list is already sorted, the algorithm performs one pass, makes no swaps, and stops. Even for small values of `n`, however, the number of comparisons and swaps can be high when the list is far from sorted.

Another problem is that most of the languages (libraries) have built-in sorting functions, that they don’t make use of bubble sort and are faster for sure. So why a developer should implement bubble sort at all?

## Application: 3 Cool Reasons To Use Bubble Sort

We saw that bubble sort is slow and ineffective, yet it is used in practice. Why? Is there any reason to use this slow, ineffective algorithm with weird name? Yes and here are some of them that might be helpful for any developer.

## 1. It is easy to implement

Definitely bubble sort is easier to implement than other “complex” sorting algorithms as quicksort. Bubble sort is easy to remember and easy to code and that’s great instead of learning and remembering tons of code.

## 2. Because the library can’t help

Let’s say you work with JavaScript. Great, there you get array.sort() which can help for this: [3, 1, 2].sort(). But what would happen if you’d rather like to sort more “complex” structures like … some DOM nodes. Here we have three LI nodes and we want to sort them in some order. Obviously you can’t compare them with the “<" operator, so we've to come up with some custom solution.

```html4strict
[Click here to sort the list](#)
 
- node 3
- node 1
- node 2
```

Here sort() can’t help us and we’ve to code our own function. However we have only few elements (three in our case). Why not using bubble sort?

## 3. The list is almost sorted

One of the problems with bubble sort is that it consists of too much swapping, but what if we know that the list is almost sorted?

```javascript
// almost sorted
[1, 2, 4, 3, 5]
```

We have to swap only 3 with 4. 

Note that the naive implementation of bubble sort remains `O(n^2)` even on sorted input because it still performs the same nested passes. Bubble sort has a best-case complexity of `O(n)` only when implemented with an early-exit flag that stops after a pass with no swaps.
