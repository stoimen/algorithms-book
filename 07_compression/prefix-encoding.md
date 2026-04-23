# Computer Algorithms: Data Compression with Prefix Encoding

## Overview

Prefix encoding, sometimes called front encoding, is yet another algorithm that tries to remove duplicated data in order to reduce its size. Its principles are simple, however this algorithm tend to be difficult to implement. To understand why, first let’s take a look of its nature.

Please, have a look on the following dictionary.

```php
use
used
useful
usefully
usefulness
useless
uselessly
uselessness
```

Instead of keeping all these words in plain text or transferring all them over a network, we can compress (encode) them with prefix encoding. 

[![Prefix Encoding](/wp-content/uploads/2012/02/Prefixencoding.png)](/wp-content/uploads/2012/02/Prefixencoding.png) 

It’s clear that each of these words begin with the prefix “use” which is also the first word from the list. So we can easily compress them into the following array.

```php
$data = array(
0 => 'use',
1 => '0d',
2 => '0ful',
3 => '0fully',
4 => '0less',
5 => '0lessly',
6 => '0lessness',
);
```

It’s clear that this is not the best compression and we can go even further by using not only the first word as prefix.

```php
$data = array(
0 => 'use',
1 => '0d',
2 => '0ful',
3 => '2ly',
4 => '0less',
5 => '4ly',
6 => '4ness',
);
```

Now the compression is better and the good news is that decompression is a fairly simple process. However the tricky part is compression itself. The problem is that it is quite difficult to chose an appropriate prefix. In our first example this is simple, but most of the times in practice we can have more heterogeneous data. Indeed the process of compression can be very difficult for randomly generated data and the algorithm will be not only slow, but difficult to implement.

The good thing is that this algorithm can be used in many cases once we know the data format in advance. So let’s see three examples where this algorithm can be very handy.

## Application

Here are three examples of prefix encoding. As I said above the process of compression can be very difficult for random data, so it is a good practice to use only it if you know in advance the format of the input data.

## Date and time prefixes

We humans often skip the first two digits of an year, so for instance we don’t always write 1995 or 1996, but we use the shorter – ‘95 and ‘96. Thus years can be encoded with shorter strings.

```php
input: 	(1991, 1992, 1993, 1994, 1995, 1996)
output:	(91, 92, 93, 94, 95, 96)
```

The problem is that with small changes of the input stream we can confuse the decoder. Thus if we add years from the 21st century we lose the uniqueness of the data.

```php
input:	(1998, 1992, 1999, 2011, 2012)
output: (98, 92, 99, 11, 12)
```

Now the decoder can decode the last two values as (1911, 1912) as “19” is considered to be the prefix. So we must know in advance that our prefix is absolutely equal for each of the values. If not the encoding format must be different. For instance we can encode also the prefix, with some special maker.

```php
input:	(1998, 1992, 1932, 1924, 2001, 2012)
output:	(#19, 98, 92, 32, 24, #20, 01, 12)
```

Once the decoder reads the # character it will know to decode the following number as prefix.

This can be used in practice for date and time formats. Let’s say we have some datetime values, but we know that all of them are in the same day.

```php
2012-01-31 15:33:45
2012-01-31 16:12:11
2012-01-31 17:32:35
2012-01-31 18:54:34
```

Obviously we can omit the date part of these strings and send (keep) only the time. Once again, we must be absolutely sure that all these values are in the same day. If not, we can use the encoding strategy of the previous example.

## Phone numbers

Phone numbers are the typical case of prefix encoding. Not only the international code, but also the mobile network operators use prefixes for their phone numbers. Thus if we have to transfer phone numbers from, let’s say the UK, we can replace the leading “+44” with something shorter. 

If you happen to code a phone book for a mobile device you can spend some space by compressing the data using prefix encoding and thus the user will have more space and will store more phone numbers on his mobile.

Phone number prefixes can be also used for database normalization. Thus you can store them in a separate db table and leave only the unique numbers from the phonebook.

## Geo Coordinates

Using the same example from [my previous post](/2012/01/30/computer-algorithms-data-compression-with-relative-encoding/) we can send GEO coordinates by removing a common prefix, for large levels of zoom. Indeed when you’ve to send lots of markers to your map application you can expect all of these markers to be fairly close to each other in large zoom level.

[![NY Subway Map](/wp-content/uploads/2012/02/NY-map.png)](/wp-content/uploads/2012/02/NY-map.png)On large zoom levels we can expect markers to be with the same prefix.

Now the coordinates of those points can have a common prefix, like the example bellow with the Subway stations.

```php
LatLon(40.762959,-73.985989)
LatLon(40.761886,-73.983629)
LatLon(40.762861,-73.981612)
LatLon(40.764616,-73.98056)
```

We can see that all of these GEO points have the same prefix (40.76x, -73.98x), so we can send the prefix only once.

```php
Prefix: (40.76, -73.98)
Data: 
LatLon(2959,5989)
LatLon(1886,3629)
LatLon(2861,1612)
LatLon(4616,056)
```

These are only three examples of prefix encoding and this algorithm must be considered as very useful when transferring homogeneous data. 

## Suffix Encoding

Suffix encoding practically the same algorithm as prefix encoding, with the small difference that we use to encode duplicating suffixes. Like the examples bellow suffix encoding can be useful is replacing repeating last name suffixes.

```php
Johnson
Clarkson
Jackson
```

Or company names.

```php
Apple Inc.
Google Inc.
Yahoo! Inc.
```

Here we can replace “ Inc.” with something else, but shorter.