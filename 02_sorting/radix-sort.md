# Computer Algorithms: Radix Sort

## Introduction

Algorithms always depend on the input. We saw that general purpose sorting algorithms as insertion sort, bubble sort and [quicksort](/2012/03/13/computer-algorithms-quicksort/) can be very efficient in some cases and inefficient in other. Indeed [insertion](/2012/02/13/computer-algorithms-insertion-sort/) and [bubble sort](/2012/02/20/computer-algorithms-bubble-sort/) are considered slow, with best-case complexity of O(n2), but they are quite effective when the input is fairly sorted. Thus when you have a sorted array and you add some “new” values to the array you can sort it quite effectively with insertion sort. On the other hand quicksort is considered one of the best general purpose sorting algorithms, but while it’s a great algorithm when the data is randomized it’s practically as slow as bubble sort when the input is almost or fully sorted. 

Now we see that depending on the input algorithms may be effective or not. For almost sorted input insertion sort may be preferred instead of quicksort, which in general is a faster algorithm.

Just because the input is so important for an algorithm efficiency we may ask are there any sorting algorithms that are faster than O(n.log(n)), which is the average-case complexity for merge sort and quicksort. And the answer is yes there are faster, linear complexity algorithms, that can sort data faster than quicksort, merge sort and heapsort. But there are some constraints!

Everything sounds great but the thing is that we can’t sort any particular data with linear complexity, so the question is what rules the input must follow in order to be sorted in linear time.

Such an algorithm that is capable of sorting data in linear O(n) time is radix sort and the domain of the input is restricted – it must consist only of integers.

## Overview

Let’s say we have an array of integers which is not sorted. Just because it consists only of integers and because array keys are integers in programming languages we can implement radix sort. 

First for each value of the input array we put the value of “1” on the key-th place of the temporary array as explained on the following diagram.

[![Radix sort first pass](/wp-content/uploads/2012/03/RadixSortBasicIdea.png)](/wp-content/uploads/2012/03/RadixSortBasicIdea.png)Radix sort first pass

If there are repeating values in the input array we increment the corresponding value in the temporary array. After “initializing” the temporary array with one pass (with linear complexity) we can sort the input. 

[![Radix sort second pass](/wp-content/uploads/2012/03/RadixSortBasicIdea2ndpass.png)](/wp-content/uploads/2012/03/RadixSortBasicIdea2ndpass.png)Radix sort second pass

## Implementation

Implementing radix sort is very easy in fact, which is great. The thing is that old-school programming languages weren’t so flexible and we needed to initialize the entire temporary array. That leads to another problem – we must know the interval of values from the input. Fortunately nowadays programming languages and libraries are more flexible so we can initialize our temporary array even if we don’t know the interval of input values, as on the example bellow. PHP is somewhere in the middle – it’s flexible enough to build-up arrays in the memory without knowing their size in advance, but we still must ksort them.

```php
$list = array(4, 3, 5, 9, 7, 2, 4, 1, 6, 5);
 
function radix_sort($input)
{
    $temp = $output = array();
	$len = count($input);
 
    for ($i = 0; $i  0) 
			? ++$temp[$input[$i]]
			: 1;
    }
 
    ksort($temp);
 
    foreach ($temp as $key => $val) {
		if ($val == 1) {
			$output[] = $key; 
		} else {
			while ($val--) {
				$output[] = $key;
			}
        }
    }
 
    return $output;
}
 
// 1, 2, 3, 4, 4, 5, 5, 6, 7, 9
print_r(radix_sort($list));
```

The problem is that PHP needs ksort – which is completely foolish as we’re trying to sort an array using “another” sorting method, but to overcome this you must know the interval of values in advance and initialize a temporary array with 0s, as on the example bellow.

```php
define(MIN, 1);
define(MAX, 9);
$list = array(4, 3, 5, 9, 7, 2, 4, 1, 6, 5);
 
function radix_sort(&$input)
{
    $temp = array();
	$len = count($input);
 
	// initialize with 0s
    $temp = array_fill(MIN, MAX-MIN+1, 0);
 
    foreach ($input as $key => $val) {
    	$temp[$val]++;
    }
 
    $input = array();
    foreach ($temp as $key => $val) {
	if ($val == 1) {
		$input[] = $key;
	} else {
		while ($val--) {
			$input[] = $key;
		}
	}
    }
}
 
// 4, 3, 5, 9, 7, 2, 4, 1, 6, 5
var_dump($list);
 
radix_sort(&$list);
 
// 1, 2, 3, 4, 5, 5, 6, 7, 8, 9
var_dump($list);
```

Here the input is modified during the sorting process and it’s used as result.

## Complexity

The complexity of radix sort is linear, which in terms of omega means O(n). That is a great benefit in performance compared to O(n.log(n)) or even worse with O(n2) as we can see on the following chart.

[![Linear function compared to n.log(n) and n^2](/wp-content/uploads/2012/03/RadixSortComplexity.png)](/wp-content/uploads/2012/03/RadixSortComplexity.png)Linear function compared to n.log(n) and n^2

## Why using radix sort

## 1. It’s fast

Radix sort is very fast compared to other sorting algorithms as we saw on the diagram above. This algorithm is very useful in practice because in practice we often sort sets of integers.

[![Pros of radix sort](/wp-content/uploads/2012/03/Prosofradixsort.png)](/wp-content/uploads/2012/03/Prosofradixsort.png) 

## 2. It’s easy to understand and implement

Even a beginner can understand and implement radix sort, which is great. You need no more than few loops to implement it.

## Why NOT using radix sort

## 1. Works only with integers

If you’re not sure about the input better do not use radix sort. We may think that our input consists only of integers and we can go for radix sort, but what if in the future someone passes floats or strings to our routine.

[![Cons of radix sort](/wp-content/uploads/2012/03/Consofradixsort.png)](/wp-content/uploads/2012/03/Consofradixsort.png) 

## 2. Requires additional space

Radix sort needs additional space – at least as much as the input.

## Final Words

Radix sort is restricted by the input’s domain, but I must say that in practice there are tons of cases where only integers are sorted. This is when we get some data from the db based on primary keys – typically primary in database tables are integers as well. So practically there are lots of cases of sorting integers, so radix sort may be one very, very useful algorithm and it is so cool that it is also easy to implement.