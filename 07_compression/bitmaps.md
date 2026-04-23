# Computer Algorithms: Data Compression with Bitmaps

## Overview

In [my previous post](/2012/01/09/computer-algorithms-data-compression-with-run-length-encoding/) we saw how to compress data consisting of very long runs of repeating elements. This type of compression is known as “run-length encoding” and can be very handy when transferring data with no loss. The problem is that the data must follow a specific format. Thus the string “aaaaaaaabbbbbbbb” can be compressed as “a8b8”. Now a string with length 16 can be compressed as a string with length 4, which is 25% of its initial length without loosing any information. There will be a problem in case the characters (elements) were dispersed in a different way. What would happen if the characters are the same, but they don’t form long runs? What if the string was “abababababababab”? The same length, the same characters, but we cannot use run-length encoding! Indeed using this algorithm we’ll get at best the same string.

In this case, however, we can see another fact. The string consists of too many repeating elements, although not arranged one after another. We can compress this string with a bitmap. This means that we can save the positions of the occurrences of a given element with a sequence of bits, which can be easily converted into a decimal value. In the example above the string “abababababababab” can be compressed as “1010101010101010”, which is 43690 in decimals, and even better AAAA in hexadecimal. Thus the long string can be compressed. When decompressing (decoding) the message we can convert again from decimal/hexadecimal into binary and match the occurrences of the characters. Well, the example above is too simple, but let’s say only one of the characters is repeating and the rest of the string consists of different characters like this: “abacadaeafagahai”. Then we can use bitmap only for the character “a” – “1010101010101010” and compress it as “AAAA bcdefghi”. As you can see all the example strings are exactly 16 characters and that is a limitation. To use bitmaps with variable length of the data is a bit tricky and it is not always easy (if possible) to decompress it.

[![Bitmap Compression](/wp-content/uploads/2012/01/Run-lengthvs.BitmapCompression.png)](/wp-content/uploads/2012/01/Run-lengthvs.BitmapCompression.png)Basically bitmap compression saves the positions of an element that is repeated very often in the message!

In the other hand bitmap compression  is not only applicable on strings. We can compress also arrays, objects or any kind of data. The example from my previous post is very suitable. Then we had to transfer a large array from a server to the client (browser) using [JSON](/tag/json/). The data then was very suitable for “run-length encoding”. Now let’s assume we have the same data – a set of different years, which this time are dispersed in a different way.

```php
$data = array(
	0 	=> 1991,
	1 	=> 1992,
	2 	=> 1993,
	3 	=> 1994,
	4 	=> 1991,
	5 	=> 1992,
	6 	=> 1993,
	7 	=> 1992,
	8 	=> 1991,
	9 	=> 1991,
	10 	=> 1991,
	11 	=> 1992,
	12 	=> 1992,
	13 	=> 1991,
	14 	=> 1991,
	15 	=> 1992,	
	...
);
```

The JSON will encoded message will be the following (a simple but yet very large javascript array).

```php
[1991,1992,1993,1994,1991,1992,1993,1992,1991,1991,1991,1992,1992,1991,1991,1992, ...]
```

However if we use bitmap compression we’ll get a “shorter” array.

```php
$data = array(
	0 => array(1991, '1000100011100110'),
	1 => array(1992, '0100010100011001'),
	2 => array(1993, '0010001000000000'),
	3 => array(1994, '0001000000000000'),
);
```

Now the JSON is:

```php
[[1991,"1000100011100110"],[1992,"0100010100011001"],[1993,"0010001000000000"],[1994,"0001000000000000"]]
```

It is obvious that the compression ratio is getting better and better as the uncompressed data grows. In fact, most of us know bitmap compression from images, because this algorithm is largely used for image compression. We can imagine how successful it can be when compressing black and white images (as black and white can be represented as 0 and 1s). Actually it is used for more than two colors (256 for instance) and again the level of compression is very high.

## Implementation

The following implementation on [PHP](/category/php/) aims only to illustrate the bitmap compressing algorithm. As we know this algorithm can be applicable for any kind of data structures.

```php
// too many repeating "a" characters
$msg = 'aazahalavaatalawacamaahakafaaaqaaaiauaacaaxaauaxaaaaaapaayatagaaoafaawayazavaaaazaaabararaaaaakakaaqaarazacajaazavanazaaaeanaaoajauaaaaaxalaraaapabataaavaaab';
 
function bitmap($message) 
{
	$i = 0;
	$bits = $rest = '';
 
	while ($v = $message[$i]) {
		if ($v == 'a') {
			$bits .= '1';
		} else {
			$bits .= '0';
			$rest .= $v;
		}
		$i++;
	}
 
	return number_format(bindec($bits), 0, '.', '') . $rest;;
}
 
echo bitmap($msg);
 
// uncompressed: 
acaaaaadaaaabalaaeaaaaganaaxakaavawamaasavajawaaaayaauaaadalanagaeaeamaarafalaazaaaiasaanaahaaazaraxaalaahaaawaaajasamahaajaakarapanaakaoakaanawalaacamauaamaal
// compressed:
152299251941730035874325065523548237677352452096zhlvtlwcmhkfqiucxuxpytgofwyzvzbrrkkqrzcjzvnzenojuxlrpbtvb
```

## Application

This algorithm is very useful when there is an element in our data that repeats very often, so you need to investigate the nature of the data you want to compress. Actually because of this fact this algorithm is used for image compression as [PNG8](http://en.wikipedia.org/wiki/Portable_Network_Graphics) or [GIF](http://en.wikipedia.org/wiki/Graphics_Interchange_Format).