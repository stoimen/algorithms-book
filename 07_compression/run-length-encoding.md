# Computer Algorithms: Data Compression with Run-length Encoding

## Introduction

No matter how fast today’s computers and networks are, the users will constantly need faster and faster services. To reduce the volume of the transferred data we usually use some sort of compression. That is why this computer sciences area will be always interesting to research and develop.

There are many data compression algorithms, some of them lossless, others lossy, but their main goal aways will be to spare storage space and traffic. These algorithms are very useful when talking about data transfer between two distant places. Perhaps the best example is the transfer between a web server and a browser.

In the last few years a lot of research has been done on compressing files, executed on the client side. Such files are javascript, css, htmls and images. In fact servers and clients already have some techniques to compress data, like using [GZIP](http://www.gzip.org/) for instance, that can dramatically decrease the transfer. In the other hand there are lots of tools and tricks in order to decrease the size of the data.

Actually when a file is executed by the client’s virtual machine, it doesn’t matter how “beautifully” it is formatted from a programmer’s point of view. Thus the spaces, tabs and the new lines don’t bring any significant information for the environment. That is why such compressing tools like [YUI Compressor](http://developer.yahoo.com/yui/compressor/), [Google Closure Compiler](http://code.google.com/closure/compiler/), etc. remove those symbols. Well, they can achieve even more in order to improve the compression rate. In this post I won’t cover this, but this shows how important data compression algorithms are.

It would be great if we could just compress data with some tool. Unfortunately this is not the case and usually the compression rate depends on the data itself. It is obvious that the choice of data compression algorithm depends mainly on the data and first of all we must explore the data.

Here I’ll cover one very simple lossless data compression algorithm called “run-length encoding” that can be very useful in some cases.

[![Run-length Encoding](/wp-content/uploads/2012/01/Run-lengthEncoding1.png)](/wp-content/uploads/2012/01/Run-lengthEncoding1.png) 

## Overview

This algorithm consists of replacing large sequences of repeating data with only one item of this data followed by a counter showing how many times this item is repeated. To become clearer let’s see a string example.

```php
aaaaaaaaaabbbaxxxxyyyzyx
```

This string’s length is 24 and as we can see there are lots of repetitions. Using the run-length algorithm, we replace any run with shorter string followed by a counter.

```php
a10b3a1x4y3z1y1x1
```

The length of this string is 17, which is approximately 70% of the initial length. Obviously this is not the optimal way to compress the given string. For instance we don’t need to use the digit “1” when the character is repeated only once. In some cases this approach can increase the length of the initial string which is exactly the opposite of what we need. In this case we’ll get the string bellow.

```php
a10b3ax4y3zyx
```

Now the length of the resulting string is 13, which is 54% of the initial length! A variation of the example above is not to keep a counter of the repetitions of the character, but their position instead. Thus the initial string will be compressed as follows.

```php
a0b10a13x14y18z21y22x23
```

Which of these two approaches you’ll use depends on the goal. In the second case we can achieve a good optimization of [binary search](/2011/12/26/computer-algorithms-binary-search/).

It is clear that this algorithm is not only applicable on strings. We can achieve very good results on arrays. A typical example is the transfer of [JSON](http://www.json.org/) from a server to a client. Then if there are large sequences of repeating data we can achieve great results.

## Implementation

The implementation bellow is assuming that we’re compressing a string and it’s written on PHP. However the nature of this algorithm doesn’t restrict us to use only strings. As I said before with slight modifications we can use it with other data structures. It is important only to understand that the run-length algorithm is very useful on large sequences of repeating elements, no matter characters or array items.

```php
$message = 'aaaaaaaaaabbbaxxxxyyyzyx';
 
function run_length_encode($msg)
{
	$i = $j = 0;
	$prev = '';
	$output = '';
 
	while ($msg[$i]) {
		if ($msg[$i] != $prev) {
 
			if ($i) 
				$output .= $j;
 
			$output .= $msg[$i];
 
			$prev = $msg[$i];
 
			$j = 0;
		}
		$j++;
		$i++;
	}
 
	$output .= $j;
 
	return $output;
}
 
// a10b3a1x4y3z1y1x1
echo run_length_encode($message);
```

And slightly optimized.

```php
$message = 'aaaaaaaaaabbbaxxxxyyyzyx';
 
function run_length_encode($msg)
{
	$i = $j = 0;
	$prev = '';
	$output = '';
 
	while ($msg[$i]) {
		if ($msg[$i] != $prev) {
 
			if ($i && $j > 1) 
				$output .= $j;
 
			$output .= $msg[$i];
 
			$prev = $msg[$i];
 
			$j = 0;
		}
		$j++;
		$i++;
	}
 
	if ($j > 1)
		$output .= $j;
 
	return $output;
}
 
// a10b3ax4y3zyx
echo run_length_encode($message);
```

Finally a small change – now we store the position of the character.

```php
$message = 'aaaaaaaaaabbbaxxxxyyyzyx';
 
function run_length_encode($msg)
{
	$i = 0;
	$prev = '';
	$output = '';
 
	while ($msg[$i]) {
		if ($msg[$i] != $prev) {
 
			$output .= $msg[$i] . $i;
 
			$prev = $msg[$i];
 
		}
 
		$i++;
	}
 
	return $output;
}
 
// a0b10a13x14y18z21y22x23
echo run_length_encode($message);
```

## Complexity and Data Compression

We’re used to talk about complexity of an algorithm measuring time and we usually try to find the fastest implementation, like in search algorithms. Here it is not so important to compress data quickly, but to compress as much as possible so the output is as small as possible without lossing data. A great feature of run-length encoding is that this algorithm is easy to implement.

## Application

We can use run-length encoding in many cases. It is commonly used to compress images and is very successful when we deal only with black and white images. Here I’ll cover another use case that I only mentioned above. Let’s say we have to transfer a very large array of data to our AJAX-powered application using JSON. Let’s say also that the data are some years, for instance the years of the premiere of a movie. There are lots of movies with a premiere in the same year, thus although the data is sorted, we actually can’t have any benefit. More important is that we have large sequences of data. Here we can use run-length encoding.

```php
$data = array(
	0 	=> 1991,
	1 	=> 1991,
	...
	2223 	=> 1991,
	2224 	=> 1992,
	...
	19298 	=> 1995,
	19299 	=> 1996,
	...
);
```

As you can see to transfer the whole array can be a nightmare, especially on slow networks. It is better to compress it (i.e. with PHP’s [json_encode](http://php.net/manual/en/function.json-encode.php)).

```php
// {"0":1991,"1":1991, ..., "2223":1991,"2224":1992, ..., "19298":1995,"19299":1996, ...}
echo json_encode($data);
```

After running run-length encoding we can receive something like the following array (note that these are only sample data and it’s up to you to decide which is the best format to store data).

```php
$data = array(
	0 => array(1991, 2224),
	1 => array(1992, 3948),
	2 => array(1995, 2398),
	3 => array(1996, 3489),
);
```

And the JSON output.

```php
// [[1991,2224],[1992,3948],[1995,2398],[1996,3489]]
echo json_encode($data);
```

Note that if the data is sorted we can achieve great success compressing it!!! This approach can be used for images, graphics or map coordinates.

This is only one example of how data compression can be useful in our daily work. Although the communication between the server and the client can be optimized and compressed, we can improve it. In other words we’re not always sure that the opposite side supports compression.

Well, it’s true that the client has to decompress the data, which can also be slow. Now in the first case we have only the time to transfer, as on the diagram bellow.

[![Data Transfer Without Compression](/wp-content/uploads/2012/01/DataTransferWithoutCompression.png)](/wp-content/uploads/2012/01/DataTransferWithoutCompression.png)Time to transfer data without compression!

In the second case, we should sum the time for compression, transfer and decompression.

[![Data Transfer with Compression](/wp-content/uploads/2012/01/DataTransferwithCompression.png)](/wp-content/uploads/2012/01/DataTransferwithCompression.png)Time to send data with compression!

All this is important, but in general data compression can be handy in many cases in our daily work.