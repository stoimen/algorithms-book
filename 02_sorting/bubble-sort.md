# Computer Algorithms: Bubble Sort

## Overview

It’s weird that bubble sort is the most famous sorting algorithm in practice since it is one of the worst approaches for data sorting. Why is bubble sort so famous? Perhaps because of its exotic name or because it is so easy to implement. First let’s take a look on its nature.

Bubble sort consists of comparing each pair of adjacent items. If the item on the left is larger than the item on the right, the two items swap places. During one full pass through the list, larger items keep moving to the right, so the largest unsorted item ends the pass in its final position.

## 1. Each pair of adjacent elements is compared

[![In bubble sort we compare two adjacent elements](../images/BubbleSortStep1CompareTwoElements1.png)](../images/BubbleSortStep1CompareTwoElements1.png)In bubble sort we compare each pair of adjacent elements.

Here `4` is larger than `2`, so if `4` appears on the left side of `2`, the two items must swap.

## 2. Swap larger elements to the right

[![If larger elements appear before smaller elements they should swap](../images/BubbleSortStep2AnElementStartstoBubble.png)](../images/BubbleSortStep2AnElementStartstoBubble.png)If a larger element appears before a smaller element, they should swap.

When a larger item is followed by a smaller item, the pair is out of order. Swapping the pair moves the larger item one position to the right.

## 3. Move forward and keep comparing adjacent pairs

[![Swapping is slow and that is the main reason not to use bubble sort](../images/BubbleSortStep3ALighterElementStartstoBubble.png)](../images/BubbleSortStep3ALighterElementStartstoBubble.png)Swapping is slow and that is the main reason not to use bubble sort.

The problem with bubble sort is that you may have to swap a lot of elements.

## 4. The largest unsorted item keeps moving right

[![On each pass the largest unsorted element moves toward the end](../images/BubbleSortStep4ALighterElementStartstoBubble.png)](../images/BubbleSortStep4ALighterElementStartstoBubble.png)On each pass the largest unsorted element moves toward the end.

If another even larger item appears later in the pass, that item continues moving right instead. By the end of the pass, the largest item in the unsorted part reaches the end.

## 5. Finally the largest item is in its place

[![Finally the list begins to look sorted](../images/BubbleSortStep5Themostlightelementisonitsplace.png)](../images/BubbleSortStep5Themostlightelementisonitsplace.png)Finally the list begins to look sorted.

At the end of each pass we can be sure that the largest unsorted item is in the right place: at the end of the unsorted part of the list.

The problem is that this algorithm needs a tremendous number of comparisons and swaps, and as we know already this can be slow.

We can easily see how ineffective bubble sort is. Now the question remains: why is it so famous? Maybe indeed the answer lies in the simplicity of its implementation. Let’s summarize the algorithm in pseudocode.

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

With the early-exit flag, the best-case complexity is `O(n)`: if the list is already sorted, the algorithm performs one pass, makes no swaps, and stops.

[![Bubble sort compared to quicksort, merge sort and heapsort in the average case](../images/BubbleSortComparedToOthers.png)](../images/BubbleSortComparedToOthers.png)Bubble sort compared to quicksort, merge sort and heapsort in the average case

Even for small values of n, the number of comparisons and swaps can be tremendous.

[![stats](../images/BubbleSortStats-2.png)](../images/BubbleSortStats-2.png)Bubble sort is three times slower than quicksort even for n = 100, but it's easier to implement

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
