# Friday Algorithms: Quicksort – Difference Between PHP and JavaScript

Here’s some Friday fun. Let me show you one sorting algorithm, perhaps the most known of all them – the quick sort, implemented both on PHP and JavaScript. Although the code look similar between both languages, there are few differences, that show the importance of the syntax knowledge!

## 1. PHP

```php
<?php
 
    $unsorted = array(2,4,5,63,4,5,63,2,4,43);
 
    function quicksort($array)
    {
        if (count($array) == 0)
            return array();
 
        $pivot = $array[0];
        $left = $right = array();
 
        for ($i = 1; $i < count($array); $i++) {
            if ($array[$i] < $pivot)
                $left[] = $array[$i];
            else
                $right[] = $array[$i];
        }
 
        return array_merge(quicksort($left), array($pivot), quicksort($right));
    }
 
    $sorted = quicksort($unsorted);
 
    print_r($sorted);
```

## 2. JavaScript

```javascript
var a = [2,4,5,63,4,5,63,2,4,43];
 
function quicksort(arr)
{
    if (arr.length == 0)
        return [];
 
    var left = new Array();
    var right = new Array();
    var pivot = arr[0];
 
    for (var i = 1; i < arr.length; i++) {
        if (arr[i] < pivot) {
           left.push(arr[i]);
        } else {
           right.push(arr[i]);
        }
    }
 
    return quicksort(left).concat(pivot, quicksort(right));
}
 
console.log(quicksort(a));
```

Note that the first conditional statement is quite important! While in PHP the count function will return 0 either on a NULL value or an empty array and you can substitute it with something like count($array) < 2

```php
if (count($array) < 2)
	return $array;
```

in JavaScript you cannot use that because of the presence of the ‘undefined’ value when an “empty” array is passed as an argument. Thus you’ve the conditional above:

```javascript
// this will result with an error
if (arr.length < 2)
        return arr;
```

## Coming Up Next …

An iterative version of the algorithm next Friday!