# Computer Algorithms: Order Statistics

## Introduction

We know that [finding the minimum in a list of integers](/2012/05/21/computer-algorithms-minimum-and-maximum/) is a fairly simple task, but what about finding the i-th smallest element? Then the task isn’t that trivial and we have to think for a different approach. 

First of all there are some very basic and intuitive approaches. Since finding the minimum is so easy, can we just find the minimum, than exclude it from the list and then search the minimum again until we find the i-th smallest element.

[![Finding the Minimums](/wp-content/uploads/2012/05/1.-Finding-the-Minimums.png)](/wp-content/uploads/2012/05/1.-Finding-the-Minimums.png) 

That is a pure brute-force-like algorithm and it is extremely slow. In this case if we’re looking for the 99-th smallest element into an array of 100 items it will be quite inefficient. In other words this isn’t the best approach.

Another fairly intuitive approach is to sort the list in first place and then search the i-th element. 

[![Sort and Seach](/wp-content/uploads/2012/05/2.-Sort-and-Seach.png)](/wp-content/uploads/2012/05/2.-Sort-and-Seach.png)First we can sort the list and then search for the i-th element!

This is better than the our first attempt because we’ll need the time to sort the array and then search (in linear time) the i-th element.

In this case we need to find out which of the sorting algorithms we will use. Will it be [merge sort](/2012/03/05/computer-algorithms-merge-sort/) (with constant O(n.lg(n)) complexity) or [quicksort](/2012/03/13/computer-algorithms-quicksort/) (with O(n2) in the worst case, but O(n.lg(n)) average complexity) or [bubble sort](/2012/02/20/computer-algorithms-bubble-sort/) (O(n^n) in the best-case scenario) it’s a developer choice.

However there is one very clever and yet more efficient approach, based on some observations.

## Overview

If we’re looking for the i-th element and we decided that the list must be sorted first, we don’t need to fully sort it in order to find the desired element. 

In case the list is sorted it’s easy to find which is the i-th element. However if the i-th element is in its place, the only thing we need to know is that the items on the left side of the i-th element are smaller and the items on the right side are greater. We don’t need the left and the right side ordered.

[![Don't need ordered sub-lists](/wp-content/uploads/2012/05/3.-Dont-need-ordered-sub-lists.png)](/wp-content/uploads/2012/05/3.-Dont-need-ordered-sub-lists.png) 

In the other hand this approach looks very much like quicksort. There during the sorting process we put the items smaller than the “pivot” on its left and the items greater than the pivot on its right. After that partitioning we executed quicksort on the left and on the right sub-lists.

Here the approach is similar with very small changes. First we choose a pivot. Then we make two partitions of the list – one left sub-list with all the elements with smaller values than the pivot and one right sub-list with all the elements with a greater value than the pivot. 

[![Choose a pivot and partition](/wp-content/uploads/2012/05/4.-Choose-a-pivot-and-partition.png)](/wp-content/uploads/2012/05/4.-Choose-a-pivot-and-partition.png)Just like quicksort we chose a pivot and then we partition the list into two sub-lists!

Now we check the length of the left sub-list. If it is greater than i we continue recursively with the left sub-list and again we’re searching for the i-th element.

In case the length of the left sub-list is smaller than i, we continue with the right sub-list. However this time we don’t search for the i-th element, but for the i – length(LEFT). 

## Implementation

The following implementation is in [PHP](/category/php/). It’s important to note that at each step we need two non-empty sub-list. That is why we take the pivot (by extracting the last item of the list) and then making two sub-lists. In case one of the sub-lists is empty we append the pivot in it. Thus we’re always partitioning the list into two non-empty sub-lists.

```php
$list = array(3,4,5,7,8,2,5,6,9,0,1);
 
function partition($list, $pivot)
{
	$left = $right = array();
 
	$len = count($list);
	for ($i = 0; $i = $i) {
		return order_statistic($left, $i);
	} else {
		return order_statistic($right, $i - count($left));
	}
}
 
// 4
echo order_statistic($list, 5);
```

## Application

Finding the minimum and maximum is easy, however sometimes we don’t search for them, but for the second, third or i-th smallest element. Then our task becomes a bit more difficult. This algorithm can be useful in many practical cases and shows us how different kind of algorithms may be related – exactly as this algorithm is related to quicksort in its principles.