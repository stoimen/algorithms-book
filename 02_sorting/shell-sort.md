# Computer Algorithms: Shell Sort

## Overview

[Insertion sort](/2012/02/13/computer-algorithms-insertion-sort/) is a great algorithm, because it’s very intuitive and it is easy to implement, but the problem is that it makes many exchanges for each “light” element in order to put it on the right place. Thus “light” elements at the end of the list may slow down the performance of insertion sort a lot. That is why in 1959 [Donald Shell](http://en.wikipedia.org/wiki/Donald_Shell) proposed an algorithm that tries to overcome this problem by comparing items of the list that lie far apart.

[![Insertion Sort vs. Shell Sort](/wp-content/uploads/2012/02/Insertion-Sort-vs.-Shell-Sort.png)](/wp-content/uploads/2012/02/Insertion-Sort-vs.-Shell-Sort.png)Insertion sort compares every single item with all the rest elements of the list in order to find its place, while Shell sort compares items that lie far apart. This makes light elements to move faster to the front of the list.

In the other hand it is obvious that by comparing items that lie apart the list can’t be sorted in one pass as insertion sort. That is why on each pass we should use a fixed gap between the items, then decrease the value on every consecutive iteration.

[![Shell Sort](/wp-content/uploads/2012/02/Shell-Sort.png)](/wp-content/uploads/2012/02/Shell-Sort.png)We start to compare items with a fixed gap, that becomes lesser on each iteration until it gets to 1.

However it is intuitively clear that Shell sort may need even more comparisons than insertion sort. Then why should we use it? 

The thing is that insertion sort is not an effective sorting algorithm at all, but in some cases, when the list is almost sorted it can be quite useful. Here’s the answer of the question above. With Shell sort once the list is sorted for gap = i, it is sorted for every gap = j, where j k), then for N = 1000, we get the following sequence: [500, 250, 125, 62, 31, 15, 7, 3, 1]

[![Shell Gap Sequence](/wp-content/uploads/2012/02/Shell-Gap-Sequence.png)](/wp-content/uploads/2012/02/Shell-Gap-Sequence.png)Shell sequence for N=1000: (500, 250, 125, 62, 31, 15, 7, 3, 1)

## Pratt Sequence

[Pratt](http://en.wikipedia.org/wiki/Vaughan_Ronald_Pratt) proposes another sequence that’s growing with a slower pace than the Shell’s sequence. He proposes successive numbers of the form 2p3q or [1, 2, 3, 4, 6, 8, 9, 12, …].

[![Pratt Gap Sequence](/wp-content/uploads/2012/02/Pratt-Gap-Sequence.png)](/wp-content/uploads/2012/02/Pratt-Gap-Sequence.png)Pratt sequence: (1, 2, 3, 4, 6, 8, 9, 12, ...)

## Knuth Sequence

Knuth in other hand proposes his own sequence following the formula (3k – 1) / 2 or [1, 4, 14, 40, 121, …]

[![Knuth Gap Sequence](/wp-content/uploads/2012/02/Knuth-Gap-Sequence.png)](/wp-content/uploads/2012/02/Knuth-Gap-Sequence.png)Knuth sequence: (1, 4, 13, 40, 121, ...)

Of course there are many other gap sequences, proposed by various developers and researchers, but the problem is that the effectiveness of the algorithm strongly depends on the input data. But before taking a look to the complexity of Shell sort, let’s see first its implementation.

## Implementation

Here’s a Shell sort implementation on [PHP](/category/php/) using the Pratt gap sequence. The thing is that for this data set other gap sequences may appear to be better solution.

```php
$input = array(6, 5, 3, 1, 8, 7, 2, 4);
 
function shell_sort($arr)
{
	$gaps = array(1, 2, 3, 4, 6);
	$gap  = array_pop($gaps);
	$len  = count($arr);
 
	while($gap > 0)
	{
		for($i = $gap; $i = $gap && $arr[$j - $gap] > $temp) {
				$arr[$j] = $arr[$j - $gap];
				$j -= $gap;
			}
 
			$arr[$j] = $temp;
		}
 
		$gap = array_pop($gaps);
	}
 
	return $arr;
}
 
// 1, 2, 3, 4, 5, 6, 7, 8
shell_sort($input);
```

It’s easy to change this code in order to work with Shell sequence.

```php
$input = array(6, 5, 3, 1, 8, 7, 2, 4);
 
function shell_sort($arr)
{
        $len  = count($arr);
	$gap  = floor($len/2);
 
	while($gap > 0)
	{
		for($i = $gap; $i = $gap && $arr[$j - $gap] > $temp) {
				$arr[$j] = $arr[$j - $gap];
				$j -= $gap;
			}
 
			$arr[$j] = $temp;
		}
 
		$gap = floor($gap/2);
	}
 
	return $arr;
}
 
// 1, 2, 3, 4, 5, 6, 7, 8
shell_sort($input);
```

## Complexity

Yet again we can’t determine the exact complexity of this algorithm, because it depends on the gap sequence. However we may say what is the complexity of Shell sort with the sequences of Knuth, Pratt and Donald Shell. For the Shell’s sequence the complexity is O(n2), while for the Pratt’s sequence it is O(n*log2(n)). The best approach is the Knuth sequence where the complexity is O(n3/2), as you can see on the diagram bellow.

[![Complexity of Shell Sort](/wp-content/uploads/2012/02/Complexity-of-Shell-Sort.png)](/wp-content/uploads/2012/02/Complexity-of-Shell-Sort.png)Complexity of Shell sort with different gap sequences.

## Application

Well, as insertion sort and bubble sort, Shell sort is not very effective compared to quicksort or merge sort. The good thing is that it is quite easy to implement (not easier than insertion sort), but in general it should be avoided for large data sets. Perhaps the main advantage of Shell sort is that the list can be sorted for a gap greater than 1 and thus making less exchanges than insertion sort.