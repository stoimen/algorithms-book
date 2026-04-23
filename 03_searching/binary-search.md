# Computer Algorithms: Binary Search

## Overview

The binary search is perhaps the most famous and best suitable search algorithm for sorted arrays. Indeed when the array is sorted it is useless to check every single item against the desired value. Of course a better approach is to jump straight to the middle item of the array and if the item’s value is greater than the desired one, we can jump back again to the middle of the interval. Thus the new interval is half the size of the initial one.

[![Binary search basic implementation](/wp-content/uploads/2011/12/BinarySearchfig.1.png)](/wp-content/uploads/2011/12/BinarySearchfig.1.png)Basic implementation of binary search

If the searched value is greater than the one placed at the middle of the sorted array, we can jump forward. Again on each step the considered list is getting half as long as the list on the previous step, as shown on the image bellow.

[![Binary search - basic implementation](/wp-content/uploads/2011/12/BinarySearchfig.2.png)](/wp-content/uploads/2011/12/BinarySearchfig.2.png)Binary search - basic implementation

## Implementation

Here’s a sample implementation of this algorithm on [PHP](/category/php/). Obviously the nature of this approach is guiding us to a recursive implementation, but as we know, sometimes recursion can be dangerous. That’s why here we can see either the recursive and iterative solution.

## Recursive Binary Search

```php
$list = array(0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144);
$x = 55;
 
function binary_search($x, $list, $left, $right) 
{
	if ($left > $right)
		return -1;
 
	$mid = ($left + $right) >> 1;
 
	if ($list[$mid] == $x) {
		return $mid;
	} elseif ($list[$mid] > $x) {
		return binary_search($x, $list, $left, $mid-1);
	} elseif ($list[$mid] > 1;
 
		if ($list[$mid] == $x) {
			return $mid;
		} elseif ($list[$mid] > $x) {
			$right = $mid - 1;
		} elseif ($list[$mid] > 1) == n/2. That is not always true and it is very dependant from the programming language. Thus in PHP those operations are fairly similar as PHP is written in C. You’ve to be aware of the language specific features when optimizing code.

## Fibonacci Search

Every developer has heard of Fibonacci and his sequence. The Fibonacci search algorithm is practically a variation of the binary search algorithm. In fact the only difference is that the binary search algorithm divides the list into two equal parts, while the Fibonacci search divides it in two but not equal parts. In fact sometimes it is faster to search if you divide the list by such non equal sub-lists. However the length of the sub-lists is not random.

It is clear that the ratio of any two consecutive numbers in the Fibonacci sequence is practically forming the golden ratio. This can lead us to another variation of Fibonacci and binary search – the golden section search. The only different thing is that you’ve to divide the length of the list in two parts exactly by the golden ratio.

[![Golden Section Search](/wp-content/uploads/2011/12/GoldenRatioSearch.png)](/wp-content/uploads/2011/12/GoldenRatioSearch.png)The golden section search doesn't divide the array on two equal sub-lists!

The complexity both of the Fibonacci and the golden section search algorithm is identical with the complexity of the binary search. However these two algorithms are rarely used in practice. Also it is more difficult to implement these two algorithms than the binary search and their advantage depends on specifically dispersed data.

## Complexity

The complexity of the binary search algorithm is intuitively clear – O(log(n)), which makes it far more effective than the sequential search.

[![log(n)](/wp-content/uploads/2011/12/chart_1.png)](/wp-content/uploads/2011/12/chart_1.png)f(n) = log(n) compared to f(n) = n

## Application

It is useless to mention examples of its use. This algorithm is easy to implement and in the same times it is very fast. Yes, indeed, this algorithm is only possible on sorted lists and this is a limitation. Also, as I said, compared to the jump search here we have more than one jump back in most of the cases, which sometimes can be more expensive than jump forward. However is this the fastest search algorithm? I’ll try to answer this question in my next article.