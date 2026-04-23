# Computer Algorithms: Morris-Pratt String Searching

## Introduction

We saw that neither [brute force string searching](/2012/03/27/computer-algorithms-brute-force-string-matching/) nor [Rabin-Karp string searching](/2012/04/02/computer-algorithms-rabin-karp-string-searching/) are effective. However in order to improve some algorithm, first we need to understand its principles in detail. We know already that brute force string matching is slow and we tried to improve it somehow by using a hash function in the Rabin-Karp algorithm. The problem is that Rabin-Karp has the same complexity as brute force string matching, which is O(mn).

Obviously we need a different approach, but to come with a different approach let’s see what’s wrong with brute force string searching. Indeed by taking a closer look at its principles we can answer the question. 

In brute force matching we checked each character of the text with the first character of the pattern. In case of a match we shifted the comparison between the second character of the pattern and the next character of the text. The problem is that in case of a mismatch we must go several positions back in the text. Well in fact this technique can’t be optimized. 

[![Morris-Pratt brute force string matching](/wp-content/uploads/2012/04/Morris-Pratt-brute-force-string-matching.png)](/wp-content/uploads/2012/04/Morris-Pratt-brute-force-string-matching.png)In brute force string matching in case of a mismatch we go back and we compare characters that has been compared already!

As you can see on the picture above the problem is that once there is a mismatch we must rollback and start comparing from a position in the text that has been explored already. In our case we have checked the first, second, third and fourth letters, where there is a mismatch between the pattern and the text and then … we go back and start comparing from the second letter of the text.

This is completely useless, because we already know that the pattern begins with the letter “a” and no such letter happens to be between positions 1 and 3. So how can we improve this redundancy?

## Overview

The answer of the question came to [James H. Morris](http://en.wikipedia.org/wiki/James_H._Morris) and [Vaughan Pratt](http://en.wikipedia.org/wiki/Vaughan_Pratt) in 1977 when they described their algorithm, which by skipping lots of useless comparisons is more effective than brute force string matching. Let’s see it in detail. The only thing is to use the information gathered during the comparisons of the pattern and a possible match, as on the picture below.

[![Morris-Pratt basic principles](/wp-content/uploads/2012/04/Morris-Pratt-basic-principles.png)](/wp-content/uploads/2012/04/Morris-Pratt-basic-principles.png)Morris-Pratt skips some comparisons by moving ahead to the next possible position of a match!

To do that first we have to preprocess the pattern in order to get possible positions for next matches. Thus after we start to find a possible match in case of a mismatch we’ll know exactly where we should jump in order to skip unusual comparisons.

## Generating the Table of Next Positions

This is the tricky part in Morris-Pratt and that is how this algorithm overcomes the disadvantages of brute force string searching. Let’s see some pictures.

[![Morris-Pratt with no repeating letters in the pattern](/wp-content/uploads/2012/04/Morris-Pratt-with-no-repeating-letters-in-the-pattern.png)](/wp-content/uploads/2012/04/Morris-Pratt-with-no-repeating-letters-in-the-pattern.png)It is clear that if the pattern consists only of different letters in case of a mismatch we should start comparing the next character of the text with the first character of the pattern!

However in case of repeating character in the pattern if we have a mismatch after that character a possible match must begin from this repeating character, as on the picture bellow.

[![Morris-Pratt with one repeating letter in the pattern](/wp-content/uploads/2012/04/Morris-Pratt-with-one-repeating-letter-in-the-pattern.png)](/wp-content/uploads/2012/04/Morris-Pratt-with-one-repeating-letter-in-the-pattern.png)The next table is slightly different if the pattern has repeating character! 

Finally if there are more than one repeating character in the text the “next” table will consist show their position.

[![Morris-Pratt more than one repeating letter in the pattern](/wp-content/uploads/2012/04/Morris-Pratt-more-than-one-repeating-letter-in-the-pattern.png)](/wp-content/uploads/2012/04/Morris-Pratt-more-than-one-repeating-letter-in-the-pattern.png)The next table contains the positions of repeating letters!

After we have this table of possible “next” positions we can start exploring the text for our pattern.

[![Morris-Pratt](/wp-content/uploads/2012/04/Morris-Pratt.png)](/wp-content/uploads/2012/04/Morris-Pratt.png) 

## Implementation

Implementing Morris-Pratt isn’t difficult. First we have to preprocess the pattern and then perform the search. The following [PHP](/category/php/) code shows you how to do that.

```php
/**
 * Pattern
 * 
 * @var string
 */
$pattern = 'mollis';
 
/**
 * Text to search
 * 
 * @var string
 */
$text = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque eleifend nisi viverra ipsum elementum porttitor quis at justo. Aliquam ligula felis, dignissim sit amet lobortis eget, lacinia ac augue. Quisque nec est elit, nec ultricies magna. Ut mi libero, dictum sit amet mollis non, aliquam et augue';
 
/**
 * Preprocess the pattern and return the "next" table
 * 
 * @param string $pattern
 */
function preprocessMorrisPratt($pattern, &$nextTable)
{
	$i = 0;
	$j = $nextTable[0] = -1;
	$len = strlen($pattern);
 
	while ($i  -1 && $pattern[$i] != $pattern[$j]) {
			$j = $nextTable[$j];
		}
 
		$nextTable[++$i] = ++$j;
	}
}
 
/**
 * Performs a string search with the Morris-Pratt algorithm
 * 
 * @param string $text
 * @param string $pattern
 */
function MorrisPratt($text, $pattern)
{
	// get the text and pattern lengths
	$n = strlen($text);
	$m = strlen($pattern);
	$nextTable = array();
 
	// calculate the next table
	preprocessMorrisPratt($pattern, $nextTable);
 
	$i = $j = 0;
	while ($j  -1 && $pattern[$i] != $text[$j]) {
			$i = $nextTable[$i];
		}
		$i++;
		$j++;
		if ($i >= $m) {
			return $j - $i;
		}
	}
	return -1;
}
 
// 275
echo MorrisPratt($text, $pattern);
```

## Complexity

This algorithm needs some time and space for preprocessing. Thus the preprocess of the pattern can be done in O(m), where m is the length of the pattern, while the search itself needs O(m+n). The good news is that you can do the preprocess only once and then perform the search as many times as you wish!

The following chart shows the complexity O(n+m) compared with O(nm) for 5 letter patterns.

[![Morris-Pratt complexity](/wp-content/uploads/2012/04/Morris-Pratt-complexity.png)](/wp-content/uploads/2012/04/Morris-Pratt-complexity.png)After pre-processing with O(m) the complexity of searching is O(n+m). You can see on the chart how effective is Morris-Pratt string searching compared to brute force string searching!

## Application

## Why it’s cool

- Its searching complexity is O(m+n) which is faster than brute force and Rabin-Karp
- It’s fairly easy to implement

## Why it isn’t cool

- It needs additional space and time – O(m) for pre-processing
- It can be optimized a bit (Knuth-Morris-Pratt)

## Final Words

Obviously this algorithm is quite useful because it improves in some very elegant manner the brute force matching. In the other hand you must know that there are faster string searching algorithms like the Boyer-Moore algorithm. However the Morris-Pratt algorithm can be quite useful in many cases, so understanding its principles can be very handy.