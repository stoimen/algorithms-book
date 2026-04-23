# Computer Algorithms: Linear Search in Sorted Lists

## Overview

The expression “linear search in sorted lists” itself sounds strange. Why should we use this algorithm for sorted lists when there are lots of other algorithms that are far more effective? As I mentioned in

[my previous post](/2011/11/24/computer-algorithms-sequential-search/) the sequential search is very ineffective in most of the cases and it is primary used for unordered lists. Indeed sometimes it is more useful first to sort the data and then use a faster algorithm like the binary search. On the other hand the analysis shows that for lists with less than ten items the linear search is much faster than the binary search. Although, for instance, binary search is more effective on sorted lists, sequential search can be a better solution in some specific cases with minor changes. The problem is that when developers hear the expression “sorted list” they directly choose an algorithm different from the linear search. Perhaps the problem lays in the way we understand what an ordered list is?

## What is a sorted list?

We used to think that this list (1, 1, 2, 3, 5, 8, 13) is sorted. Actually we think so because it is … sorted, but the list (3, 13, 1, 3, 3.14, 1.5, -1) is also sorted, except that we don’t know how. Thus we can think that any array is sorted, although it is not always obvious how. There are basically two cases when sequential search can be very useful. First when the list is very short or when we know in advance that there are some values that are very frequently searched. Let’s say we have a very large list, with hundreds of thousands of items, but actually most of the searches in that list always find the same ten values. This additional information tells us that using a binary search will be quite ineffective in this case. A possible approach, of course, is to place those values at the front of the list and to perform a sequential search. 

[![You should choose a search algorithm by carefully examining the data you search.](../images/search.jpg)](../images/search.jpg)You should choose a search algorithm by carefully examining the data you search. Unfortunately the search will be slow when we search for some value missing from the front of the list, but then we can use another algorithm on sorted lists. Still there is one question that should be answered. We do know that in most of the cases we search for the same values, but we do not know exactly those values. So the question is how to put the most frequently accessed values at the front of the list, since we don’t know them. Here we need some sort of auto adjustment of the list.

## Self-Organization

Self-organization practically means that every time we search and find the desired value, we somehow change the list so the next search will be far more effective. There are basically two approaches to do that.

- To move the item one position forward to the front of the list;
- To move the item directly at the front of the list; 

Of course it depends on your case which approach you’ll choose, but it is assumed that the second option, the one that we choose to move the item directly at the front of the list, is better. Indeed if we choose the first option and the list is (…, 24, 31) after constantly searching for those two values the array will be changing from (…, 24, 31) to (…, 31, 24) and once again to (…, 24, 31) and so on and so on. Thus a better solution is to move the desired item directly to the front of the list. Now if we look for the value of “5” in the list

(1, 2, 4, …, 5, …, 398) it will become (5, 1, 2, …, 398) after the value is found. By choosing this approach we can be sure that as the number of searches increases, the most frequently searched values are placed at the front of the list. Now the sequential search is quite a good solution! Here’s an example of sequential search from my previous article. The only change is that after we find the desired value we need to move it to the front of the list.

## Application

Using sequential search in sorted lists can be very useful and fast, the only thing is that we need to know in advance that there are some values that are frequently searched. A typical example of this case is the contact list on your phone. Perhaps you have lots of names in there, but most of the times you search in it is to find your best friends’ and family phone numbers. That is why most of the cell phone manufacturers add to their phones the ability to predefine shortcut keys for the most frequently dialed numbers. Here’s another use case. Let’s say that we have the same scenario as in my previous

[post](/2011/11/24/computer-algorithms-sequential-search/), where username/name pairs are stored into a CSV file. We can fetch those values in a PHP array.

Every time a user enters the site we search for his name by his username and a welcome message is displayed. We know that some users enter the site very frequently while others do that once per month so we cannot only perform a sequential search but also we can use self-organization for the array and change the CSV file at the end.

The result is:

Hello, Darth Vader 
Found after 5 iterations!
Hello, Darth Vader
Found after 1 iterations!

Now every time Darth Vader tries to sign in, you won’t bother him to wait a lot for sure. However I bet nobody uses CSV files to store such information, but this is only an example.