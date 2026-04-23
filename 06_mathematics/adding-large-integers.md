# Computer Algorithms: Adding Large Integers

## Introduction

We know how to add two integers using a perfectly simple and useful algorithm learned from school or even earlier. This is perhaps one of the very first techniques we learn in mathematics. However we need to answer few questions. First of all do computers use the same technique, since they use binary representation of numbers? Is there a faster algorithm used by computers? What about boundaries and large integers?

## Overview

Let’s start by explaining how we humans add two numbers. An important fact is that by adding two single-digit numbers we get at most two digit number. This can be proven by simply realizing that 9+9 = 18. This fact lays down in the way we add integers. Here’s how.

![Image](https://docs.google.com/drawings/pub?id=11pxzTffU-mVas5OYWiFg2sdSUAXa-QvBD1pSewNao3A&w=620&h=399)

We just line-up the integers on their right-most digit and we start adding them in a column. In case we got a sum greater than 9 (let’s say 14) we keep only the right-most digit (the 4) and the 1 is added to the next sum.

Thus we get to the simple fact that by adding two n-digit integers we can have either an n-digit integer or a n+1 digit integer. As an example we see that by adding 53 + 35 (two 2-digit integers) we get 88, which is again 2-digit integer, but 53 + 54 result in 107, which is 3 digit integer. 

That fact is practically true, as I mentioned above, for each pair of n-digit integers.

## What about binary numbers?

In fact binaries can be added by using the exact same algorithm. At the example below we add two integers represented as binary numbers.

![Image](https://docs.google.com/drawings/pub?id=1E376ILBpXg5fUU8JbxiKwIcSa8DxSDtaIjqVsCJCjWk&w=620&h=399)

As a matter of fact this algorithm is absolutely wonderful, because it works not only on decimals and binaries but in any base B.

Of course computers tend to perform better when adding integers that “fit” the machine word. However as we can see later this isn’t always the case and sometimes we need to add larger numbers that exceed the type boundaries.

## What about big integers?

Since we know how to add “small” integers, it couldn’t be so hard to apply the same algorithm on big integers. The only problem is that the addition will be slower and sometimes (done by humans) can be error prone. 

So practically the algorithm is the same, but we can’t just put a 1 billion integer into a standard computer type INT, right? That means that the tricky part here is the way we represent integers in our application. A common solution is to store the “big” integer into an array, thus each digit will be a separate array item. Then the operation of addition will be simple enough to be applied.

## Complexity

When we talk about an algorithm that is so well known by every human being (or almost every) a common question is “is there anything faster” or “do computers use a different algorithm”. The answer may be surprising to someone, but unfortunately that is the fastest (optimal) algorithm for number addition. 

Practically there’s nothing to optimize here. We just read the two n-digit numbers (O(n)), we apply “simple” addition to each digit and we carry over the 1 from the sums greater than 9 to the next “simple” addition. We don’t have loops or any complex operation in order to search for an optimization niche.

## Application

It’s strange how often this algorithm is asked on coding interviews. Perhaps the catch is whether the interviewed person will start to look for a faster approach?! Thus is cool to know that this algorithm is optimal.

Sometimes we may ask ourselves why we humans use decimals. It’s considered because we have 10 fingers on our hands and this is perhaps true.

An interesting fact though, is that the Mayas (who barely predicted the end of the world a couple of weeks ago) used a system of a base 20. That is logical, since we have not 10, but total of 20 fingers considering our legs.

Finally, this algorithm may seem to easy to be explained but it lays down in more complex algorithms.