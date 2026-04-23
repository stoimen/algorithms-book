# Friday Algorithms: JavaScript Merge Sort

## Merge Sort

This week I’m going to cover one very popular sorting algorithm – the merge sort. It’s very intuitive and simple as it’s described in [Wikipedia](http://en.wikipedia.org/wiki/Merge_sort):

- If the list is of length 0 or 1, then it is already sorted.  Otherwise:
- Divide the unsorted list into two sublists of about half the size.
- Sort each sublist recursively by re-applying merge sort.
- Merge the two sublists back into one sorted list.

## Here’s the Source

(JavaScript)

```javascript
var a = [34, 203, 3, 746, 200, 984, 198, 764, 9];
 
function mergeSort(arr)
{
    if (arr.length < 2)
        return arr;
 
    var middle = parseInt(arr.length / 2);
    var left   = arr.slice(0, middle);
    var right  = arr.slice(middle, arr.length);
 
    return merge(mergeSort(left), mergeSort(right));
}
 
function merge(left, right)
{
    var result = [];
 
    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }
 
    while (left.length)
        result.push(left.shift());
 
    while (right.length)
        result.push(right.shift());
 
    return result;
}
 
console.log(mergeSort(a));
```

It’s interesting to see what happens in the Firebug’s console:

```javascript
[34, 203, 3, 746] [200, 984, 198, 764, 9]
 
[34, 203] [3, 746]
 
[34] [203]
 
[3] [746]
 
[200, 984] [198, 764, 9]
 
[200] [984]
 
[198] [764, 9]
 
[764] [9]
 
[3, 9, 34, 198, 200, 203, 746, 764, 984]
```

Actually the tricky part in this algorithm is the merge function – it does all the work.