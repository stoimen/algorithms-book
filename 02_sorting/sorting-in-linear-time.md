# Computer Algorithms: Sorting in Linear Time

## Radix Sort

The first question when we see the phrase “sorting in linear time” should be – where’s the catch? Indeed there’s a catch and the thing is that we can’t sort just anything in linear time. Most of the time we can speak on sorting integers in linear time, but as we can see later this is not the only case. 

Since we speak about integers, we can think of a faster sorting algorithm than usual. Such an algorithm is the counting sort, which can be very fast in some cases, but also very slow in others, so it can be used carefully. Another linear time sorting algorithm is radix sort.

## Introduction

Count sort is absolutely brilliant and easy to implement. In case we sort integers in the range [n, m] on the first pass we just initialize a zero filled array with length m-n. Than on the second pass we “count” the occurrence of each integer. On the third pass we just sort the integers with an ease. 

![Image](https://docs.google.com/drawings/pub?id=1VOyJ9u_sp5YQB6gpt0bcWFOKjYTSugoQWJYRkFFZTLc&w=620&h=399)

However we have some problems with that algorithm. What if we have only few items to sort that are very far from each other like [2, 1, 10000000, 2]. This will result in a very large unused data. So we need a dense integer sequence. This is important because we must know in advance the nature of the sequence which is rarely sure.

That’s why we need to use another linear time sorting algorithm for integers that doesn’t have this disadvantage. Such an algorithm is the radix sort.

## Overview

The idea behind the radix sort is simple. We must look at our “integer” sequence as a string sequence. OK, to become clearer let me give you an example. Our sequence is [12, 2, 23, 33, 22]. First we take the leftmost digit of each number. Thus we must compare [_2, 2, _3, _3, _2]. Clearly we can assume that since the second number “2” is only a one digit number we can fill it up with a leading “0”, to become 02 or _2 in our example: [_2, _2, _3, _3, _2]. Now we sort this sequence with a stable sort algorithm.

## What is a Stable Sort Algorithm

A stable sort algorithm is an algorithm that sorts a list by preserving the positions of the elements in case they are equal. In terms of PHP this means that:

```php
array(0 => 12, 1=> 13, 2 => 12);
```

Will be sorted as follows:

```php
array(0 => 12, 2 => 12, 1 => 13);
```

Thus the third element becomes second following the first element. Note that the third and the first element are equal, but the third appears later in the sequence so it remains later in the sorted sequence.

In the radix sort example, we need a stable sort algorithm, because we need to worry about only one position of digit we explore.

So what happens in our example after we sort the sequence? 

![Image](https://docs.google.com/drawings/pub?id=10dVPfCVf8YI2sEJNuAujnrOx0g0RxWGsQdTJ0xqGt1k&w=620&h=399)

As we can see we’re far from a sorted sequence, but what if we proceed with the next “position” – the decimal digit?

Than we end up with this:

![Image](https://docs.google.com/drawings/pub?id=1oaKToHilxrKyGJzwm7NvmrSaL3uVRO3R7r0RCb0jrR4&w=621&h=264)

Now we have a sorted sequence, so let’s summarize the algorithm in a short pseudo code.

## Pseudo Code

The simple approach behind the radix sort algorithm can be described as pseudo code, assuming that we’re sorting decimal integers.

1. For each digit at position 10^0 to 10^n

   1.1. Sort the numbers by this digit using a stable sort algorithm; 

The thing is that here we talk about decimal, but actually this algorithm can be applied equally on any numeric systems. That is why it’s called “radix” sort. 

Thus we can sort binary numbers, hexadecimals etc.

It’s important to note that this algorithm can be also used to sort strings alphabetically.

[ABC, BBC, ABA, AC]
[__C, __C, __A, __C] => [ABA, ABC, BBC, AC]
[_B_, _B_, _B_, _A_] => [AC, ABA, ABC, BBC]
[___, A__, A__, B__] => [AC, ABA, ABC, BBC]

That is simply correct because we can assume that our alphabet is another 27 digit numeric system (in case of the Latin alphabet).

## Complexity

As I said in the beginning radix sort is a linear time sorting algorithm. Let’s see why. First we depend on the numeric system. Let’s assume we have a decimal numeric system – then we have N passes sorting 10 digits which is simply 10*N. In case of K digit numeric system our algorithm will be O(K*N) which is linear.

However you must note that in case we sort N numbers in an N digit numeric system the complexity will become O(N^2)!

We must also remember that in order to implement radix sort and a supporting stable sort algorithm we need an extra space.

## Application

Sorting integers can be faster than sorting just anything, so any time we need to implement a sorting algorithm we must carefully investigate the input data. And that’s also the big disadvantage of this algorithm – we must know the input in advance, which is rarely the case.