# Computer Algorithms: Data Compression with Relative Encoding

## Overview

Relative encoding is another data compression algorithm. While [run-length encoding](/2012/01/09/computer-algorithms-data-compression-with-run-length-encoding/), [bitmap encoding](/2012/01/16/computer-algorithms-data-compression-with-bitmaps/) and [diagram and pattern substitution](/2012/01/23/computer-algorithms-data-compression-with-diagram-encoding-and-pattern-substitution/) were trying to reduce repeating data, with relative encoding the goal is a bit different. Indeed run-length encoding was searching for long runs of repeating elements, while pattern substitution and bitmap encoding were trying to “map” where the repetitions happen to occur. 

The only problem with these algorithms is that not always the input stream of data is constructed out of repeating elements. It is clear that if the input stream contains many repeating elements there must be some way of reducing them. However that doesn’t mean that we cannot compress data if there are no repetitions. It all depends on the data. Let’s say we have the following stream to compress.

```php
1, 2, 3, 4, 5, 6, 7
```

We can hardly imagine how this stream of data can be compressed. The same problem may occur when trying to compress the alphabet. Indeed the alphabet letters the very base of the words so it is the minimal part for word construction and it’s hard to compress them.

Fortunately this isn’t true always. An algorithm that tryies to deal with non repeating data is relative encoding. Let’s see the following input stream – years from a given decade (the 90’s).

```php
1991,1991,1999,1998,1991,1993,1992,1992
```

Here we have 39 characters and we can reduce them. A natural approach is to remove the leading “19” as we humans often do.

```php
91,91,99,98,91,93,92,92
```

Now we have a shorter string, but we can go even further with keeping only the first year. All other years will as relative to this year.

```php
91,0,8,7,0,2,1,1
```

Now the volume of transferred data is reduced a lot (from 39 to 16 – more than 50%). However there are some questions we need to answer first, because the stream wont be always formatted in such pretty way. How about the next character stream?

```php
91,94,95,95,98,100,101,102,105,110
```

We see that the value 100 is somehow in the middle of the interval and it is handy to use it as a base value for the relative encoding. Thus the stream above will become:

```php
-9,-6,-5,-5,-2,100,1,2,5,10
```

The problem is that we can’t decide which value will be the base value so easily. What if the data was dispersed in a different way.

```php
96,97,98,99,100,101,102,103,999,1000,1001,1002
```

Now the value of “100” isn’t useful, because compressing the stream will get something like this:

```php
-4,-3,-2,-1,100,1,2,3,899,900,901,902
```

To group the relative values around “some” base values will be far more handy.

```php
(-4,-3,-2,-1,100,1,2,3)(-1,1000,1,2)
```

However to decide which value will be the base value isn’t that easy. Also the encoding format is not so trivial. In the other hand this type of encoding can be useful in som specific cases as we can see bellow.

## Implementation

The implementation of this algorithm depends on the specific task and the format of the data stream. Assuming that we’ve to transfer the stream of years in JSON from a web server to a browser, here’s a short PHP snippet.

```php
// JSON: [1991,1991,1999,1998,1999,1998,1995,1997,1994,1993]
$years = array(1991,1991,1999,1998,1999,1998,1995,1997,1994,1993);
 
function relative_encoding($input)
{
	$output = array();
	$inputLength = count($input);
 
	$base = $input[0];
 
	$output[] = $base;
 
	for ($i = 1; $i [![San Francisco map with full lat and lon markers](/wp-content/uploads/2012/01/FullLatLononSanFrancisco.png)](/wp-content/uploads/2012/01/FullLatLononSanFrancisco.png)Map markers can be relative to the (0, 0) point on Earth, which can be sometimes useless.

Far more useful may be to encode those markers, relative to the center of the city, thus we can save some space.

[![San Francisco map with relative encoded markers](/wp-content/uploads/2012/01/SanFranciscoMap.png)](/wp-content/uploads/2012/01/SanFranciscoMap.png)Relative encoding can be useful for map markers on large zoom level!

However this type of compression can be tricky, for example when dragging the map and updating the marker array. In the other hand we must group markers if we have to load more than one city. That’s why we must be careful when implementing it. But in the other hand it can be very useful – for instance on initial load of the map we can reduce data and speed up the load time. 

The thing is that with relative encoding we can save only changes to base value (data) – something like version control systems and thus reducing data transfer and load. Here’s a graphical example. In the first case on the diagram bellow we can see that each item is stored on its own. It doesn’t depend on the adjacent items and it can be completely independent of them.

[![Non-relative encoding](/wp-content/uploads/2012/01/chart_11.png)](/wp-content/uploads/2012/01/chart_11.png) 

However we can keep full info only for the first item and any other item will be relative to it, like on the diagram bellow.

[![Relative encoding](/wp-content/uploads/2012/01/chart_21.png)](/wp-content/uploads/2012/01/chart_21.png)