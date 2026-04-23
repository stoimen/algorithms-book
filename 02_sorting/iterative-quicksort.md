# Friday Algorithms: Iterative Quicksort

## Sorting Algorithms

Last Friday I wrote a quick [post](/2010/06/11/friday-algorithms-quicksort-difference-between-php-and-javascript/) showing the recursive version of quicksort on both PHP and JavaScript, but this time let me begin with some breve history. Actually sorting and searching algorithms are widely used in the computer science and are developed and improved many times in the last 60 years or so. As the world goes so wired, all the information around is stored in data bases and the need of fast search methods is critical. But as we all know from our experience, it is far more easy to search into a sorted data structure. Such are all the dictionaries we use. Can you, actually, imagine to search for a word in a dictionary where all the words don’t follow the alphabetical order and are dispersed chaotically instead?

That’s why from the early years of computer science both sorting and searching algorithms go together. I’ll cover more on search algorithms in the future, but as I started with quicksort – one of the many sorting algorithms, let me mention that this algorithm is not anonymous! What I mean is that his author is well known and he’s recognized in the computer science community. His name is …

## C. A. R. Hoare

![Sir C. A. R. Hoare](../images/Hoare.jpg)

Sir Charles Antony Richard Hoare is born on the 11 of January 1934 in Ceylon, now Sri Lanka, and after finishing his studies in the University of Oxford and being in the Royal Navy for two years, he went to … Moscow where at the age of 26 (in 1960) he developed the famous quicksort algorithm. That’s not everything! Beside the sorting algorithm he’s well recognized with the Hoare logic and the invention of the formal language CSP (Communicating Sequential Processes).

Here’s what Sir C. A. R. Hoare wrote in his lecture “The Emperor’s Old Clothes” published by Communications of the ACM in 1981:

My first task was to implement for the new Elliot 803 computer, a  library subroutine for a new fast method of internal sorting just  invented by Shell. I greatly enjoyed the challenge of maximizing  efficiency in the simple decimal-addressed machine code of those days.  My boss and tutor, Pat Shackleton, was very pleased with my completed  program. I then said timidly that I thought I had invented a sorting  method that would usually run faster than SHELLSORT, without taking much  extra store. He bet me sixpence that I had not. Although my method was  very difficult to explain, he finally agreed that I had won my bet.

The quicksort was born!

## Recursive vs. Iterative

There are two main directions in solving such problems as the sorting algorithms. Recursive way and the iterative way. Every developer knows what is recursive and what’s iterative. We like to say that recursive is more “elegant” solution, than the iterative, but it’s interesting to say that both are completely interchangeable. Yeah, every recursion can be modeled with a stack (as the recursion is always converted into a stack on the machine level), and every loop can be converted to a recursion. It’s interesting to say that the second direction is easier, while the switch from recursion to stack it’s not always obvious and easy!

Here’s a code snippet from the recursive version of the quicksort, I’ve [posted](/2010/06/11/friday-algorithms-quicksort-difference-between-php-and-javascript/) last Friday:

```javascript
return quicksort(left).concat(pivot, quicksort(right));
```

What actually happens here is that the second call of quicksort() is always delayed for a later execution. Until the first quicksort() doesn’t finish his job, the second is never used! Why than we need a recursion? Why don’t we put the right part of the array in a stack for later execution? In fact this is the solution! Every right part will be put into a stack. Thus while the stack has some elements we pop an element from it and than split it again on two – left and right: the right part enters the stack and the left is yet again sorted.

In that scenario we end with an element split when the left part is with only one element. Here’s important to say that this might not be the perfect solution and there might be some quite good optimizations!

Assuming we’ve the following array – [8,4,6,2,1,9,5], here’s what happens, note that in JavaScript array.pop() pops the rightmost element, in this case 5:

```javascript
stack = [[8,4,6,2,1,9,5]]
sorted = [];
 
1. step
stack = [5,8,6,9][4,2,1]
sorted = []
 
2. step
stack = [5,8,6,9][4][2,1]
sorted = []
 
3. step
stack = [5,8,6,9][4][2][1]
sorted = []
 
4. step
stack = [8,6,9][5]
sorted = [1,2,4]
 
5. step
stack [8,9][6]
sorted = [1,2,4,5]
...
```

## Code

Here’s the implementation of the iterative version, again on both PHP and JavaScript:

JavaScript:

```javascript
var a = [8,4,6,2,1,9,5,5,4,3,4,3];
 
function qsort(arr)
{
    var stack = [arr];
    var sorted = [];
 
    while (stack.length) {
 
        var temp = stack.pop(), tl = temp.length;
 
        if (tl == 1) {
            sorted.push(temp[0]);
            continue;
        }
        var pivot = temp[0];
        var left = [], right = [];
 
        for (var i = 1; i < tl; i++) {
            if (temp[i] < pivot) {
                left.push(temp[i]);
            } else {
                right.push(temp[i]);
            }
        }
 
        left.push(pivot);
 
        if (right.length)
            stack.push(right);
        if (left.length)
            stack.push(left);
 
    }
 
    console.log(sorted);
}
qsort(a);
```

PHP:

```php
$unsorted = array(8,4,6,2,1,9,5,5,4,3,4,3);
 
function qsort($array)
{
    $stack = array($array);
    $sorted = array();
 
    while (count($stack) > 0) {
 
        $temp = array_pop($stack);
 
        if (count($temp) == 1) {
            $sorted[] = $temp[0];
            continue;
        }
 
        $pivot = $temp[0];
        $left = $right = array();
 
        for ($i = 1; $i  $temp[$i]) {
                $left[] = $temp[$i];
            } else {
                $right[] = $temp[$i];
            }
        }
 
        $left[] = $pivot;
 
        if (count($right))
            array_push($stack, $right);
        if (count($left))
            array_push($stack, $left);
    }
 
    return $sorted;
}
 
$sorted = qsort($unsorted);
print_r($sorted);
```

First of all this is more code than the recursive solution, and besides that it’s perhaps not the optimal solution – I’ll cover in the future more on how to optimize it and where it can be useful.

Till next Friday!