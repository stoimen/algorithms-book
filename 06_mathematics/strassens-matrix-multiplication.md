# Computer Algorithms: Strassen’s Matrix Multiplication

## Introduction

The Strassen’s method of matrix multiplication is a typical divide and conquer algorithm. We’ve seen so far some divide and conquer algorithms like [merge sort](/2012/03/05/computer-algorithms-merge-sort/) and the [Karatsuba’s fast multiplication](/2012/05/15/computer-algorithms-karatsuba-fast-multiplication/) of large numbers. However let’s get again on what’s behind the divide and conquer approach.

Unlike the dynamic programming where we “expand” the solutions of sub-problems in order to get the final solution, here we are talking more on joining sub-solutions together. These solutions of some sub-problems of the general problem are equal and their merge is somehow well defined.

A typical example is the merge sort algorithm. In merge sort we have two sorted arrays and all we want is to get the array representing their union again sorted. Of course, the tricky part in merge sort is the merging itself. That’s because we’ve to pass through the two arrays, A and B, and we’ve to compare each “pair” of items representing an item from A and from B. A bit off topic, but this is the weak point of merge sort and although its worst-case time complexity is O(n.log(n)), quicksort is often preferred in practice because there’s no “merge”. [Quicksort](/2012/03/13/computer-algorithms-quicksort/) just concatenates the two sub-arrays. Note that in quicksort the sub-arrays aren’t with an equal length in general and although its worst-case time complexity is O(n^2) it often outperforms merge sort.

This simple example from the paragraph above shows us how sometimes merging the solutions of two sub-problems actually isn’t a trivial task to do. Thus we must be careful when applying any divide and conquer approach.

## History

[Volker Strassen](http://en.wikipedia.org/wiki/Volker_Strassen) is a German mathematician born in 1936. He is well known for his works on probability, but in the computer science and algorithms he’s mostly recognized because of his algorithm for matrix multiplication that’s still one of the main methods that outperforms the general matrix multiplication algorithm.

Strassen firstly published this algorithm in 1969 and proved that the n^3 algorithm isn’t the optimal one. Actually the given solution by Strassen is slightly better, but his contribution is enormous because this resulted in many more researches about matrix multiplication that led to some faster approaches, i.e. [the Coppersmith-Winograd algorithm](http://en.wikipedia.org/wiki/Coppersmith%E2%80%93Winograd_algorithm) with O(n^2,3737).

## Overview

The general algorithm on multiplying two matrices A[NxN] and B[NxN] is fairly simple. Although it’s more difficult than multiplying two numbers and also it is not commutative it’s still very simple – but slow.

Let’s first define what’s a matrix A[NxN]. As we speak about matrices NxN we usually think of a square grid with N rows and N columns. In each row and column A[i][j] we’ve a value. 

[![Square matrix](/wp-content/uploads/2012/11/1.-Square-matrix.png)](/wp-content/uploads/2012/11/1.-Square-matrix.png) 

Of course, as developers, we can think of a matrix as a two-dimensional array.

```php
// PHP two-dimensional array
$a = array(
    0 => array($v1, $v2, $v3, $v4),
    1 => array($v5, $v6, $v7, $v8),
    2 => array($v9, $v10, $v11, $v12),
);
```

Don’t forget that a NxN matrix is just a private case for a matrix. We can equally likely have any other size of a matrix NxM (N <> M). 

However the size of a matrix is crucial in order to multiply it with another matrix. Why is that? 

As I said above multiplying matrices isn’t the same as multiplying numbers. First of all this operation isn’t commutative.

[![Commutative problem](/wp-content/uploads/2012/11/2.-Commutative-problem.png)](/wp-content/uploads/2012/11/2.-Commutative-problem.png) 

And the second problem is the way you multiply two matrices A with B.

[![Matrix Multiplication](/wp-content/uploads/2012/11/3.-Matrix-Multiplication.png)](/wp-content/uploads/2012/11/3.-Matrix-Multiplication.png) 

Just because this works with NxN matrices we can see the problem with multiplying rectangular matrices. Indeed, this wouldn’t be possible unless the second dimension of A isn’t exactly equal to the first dimension of B. 

[![Rectangular Matrix Multiplication](/wp-content/uploads/2012/11/4.-Rect-Matrix-Multiplication.png)](/wp-content/uploads/2012/11/4.-Rect-Matrix-Multiplication.png) 

Hopefully we are now talking about square matrices with exactly the same dimensions.

OK, so now we know how to multiply two square matrices (with the same dimensions NxN) and now let’s evaluate the time complexity for the general purpose algorithm.

As we know A.B = C only when:

C[i][j] = sum(A[i][k] * B[k][j]) for k = 0 .. n

Thus we have n^3 operations. Let’s try to find out a divide and conquer approach.

Indeed this isn’t difficult in case of matrices because as we know we can divide in matrix in smaller sub-matrices.

[![Divide and Conquer](/wp-content/uploads/2012/11/5.-Divide-and-Conquer.png)](/wp-content/uploads/2012/11/5.-Divide-and-Conquer.png) 

Now what do we have?

[![Divide and Conquer Result](/wp-content/uploads/2012/11/6.-Divide-and-Conquer-Result.png)](/wp-content/uploads/2012/11/6.-Divide-and-Conquer-Result.png) 

Again – the same complexity – we have 8 products and 4 sums. Where’s the catch? 

Of course in order to get faster solution we’ve to be looking as Strassen did in 1969. He defined P1, P2, P3, P4, P5, P6 and P7 as defined on the image below.

[![Strassen's Algorithm](/wp-content/uploads/2012/11/7.-Strassens-Algorithm.png)](/wp-content/uploads/2012/11/7.-Strassens-Algorithm.png) 

## Complexity

As I mentioned above the Strassen’s algorithm is slightly faster than the general matrix multiplication algorithm. The general algorithm’s time complexity is O(n^3), while the Strassen’s algorithm is O(n^2.80).

You can see on the chart below how slightly faster is this even for large n.

[![Strassen's Complexity](/wp-content/uploads/2012/11/Strassens-Complexity.png)](/wp-content/uploads/2012/11/Strassens-Complexity.png) 

## Application

Although this algorithm seems to be more close to pure mathematics than to computer practically everywhere we use NxN arrays we can benefit from matrix multiplication.

In the other hand the algorithm of Strassen is not much faster than the general n^3 matrix multiplication algorithm. That’s very important because for small n (usually n  100 the difference can be very big.

In the same time typically NxN arrays are used always when we talk about adjacency matrix of graphs |V| = n and some graph algorithms practically depend on matrix multiplication.