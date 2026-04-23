# Computer Algorithms: Boyer-Moore String Searching

## Introduction

Have you ever asked yourself which is the algorithm used to find a word after clicking Ctrl+F and typing something? Well I guess you know the answer from the title, but in this article you’ll find out how exactly this is done.

As we saw from the [Morris-Pratt string searching](/2012/04/09/computer-algorithms-morris-pratt-string-searching/) we don’t need to compare the text and the pattern character by character. Some comparisons can be skipped in order to improve the performance of the string searching. Indeed the [brute force string searching](/2012/03/27/computer-algorithms-brute-force-string-matching/) and the [Rabin-Karp algorithm](/2012/04/02/computer-algorithms-rabin-karp-string-searching/) are quite slow only because they compare the pattern and the text character by character.

In the other hand the Morris-Pratt algorithm is a very good improvement of the brute force string searching, but the question remains. Is there any algorithm that is faster than Morris-Pratt – is there any way to skip more comparisons and to move the pattern faster.

It’s clear that if we have to find whether a single character is contained into a text we need at least “n” steps, where n is the length of the text. Once we have to find whether a pattern with the length of “m” is contained into a text with length of “n” the case is getting a little more complex.

However the answer is that there is such algorithm that is faster and more suitable than Morris-Pratt. This is the Boyer-Moore string searching.

## Overview

Boyer-Moore is an algorithm that improves the performance of pattern searching into a text by considering some observations. It is defined in 1977 by [Robert S. Boyer](http://en.wikipedia.org/wiki/Robert_S._Boyer) and [J Strother Moore](http://en.wikipedia.org/wiki/J_Strother_Moore) and it consist of some specific features. 

First of all this algorithm starts comparing the pattern from the leftmost part of text and moves it to the right, as on the picture below.

[![Boyer-Moore Shifting Direction](/wp-content/uploads/2012/04/Boyer-MooreShiftingDirection.png)](/wp-content/uploads/2012/04/Boyer-MooreShiftingDirection.png)In Boyer-Moore the pattern is shifted from left to right!

Unlike other string searching algorithms though, Boyer-Moore compares the pattern against a possible match from right to left as shown below.

[![Boyer-Moore Comparison Model](/wp-content/uploads/2012/04/Boyer-MooreComparisonModel.png)](/wp-content/uploads/2012/04/Boyer-MooreComparisonModel.png)Unlike other algorithms the letters of the pattern are compared from right to left!

The main idea of Boyer-Moore in order to improve the performance are some observations of the pattern. In the terminology of this algorithm they are called good-suffix and bad-character shifts. Let’s see by the following examples what they are standing for.

## Good-suffix Shifts

Just like the Morris-Pratt algorithm we start to compare the pattern against some portion of the text where a possible match will occur. In Boyer-Moore as I said this is done from the rightmost letter of the pattern. After some characters have matched we find a mismatch.

[![Boyer-Moore a Mismatch](/wp-content/uploads/2012/04/Boyer-MooreAMismatch.png)](/wp-content/uploads/2012/04/Boyer-MooreAMismatch.png) 

So how can we move the pattern to the right in order to skip unusual comparisons. To answer this question we need to explore the pattern. Let’s say there is a portion of the pattern that is repeated inside the pattern itself, like it is shown on the picture below.

[![Boyer-Moore Good-suffix Shift 1](/wp-content/uploads/2012/04/Boyer-MooreGood-suffixShift1.png)](/wp-content/uploads/2012/04/Boyer-MooreGood-suffixShift1.png)The pattern may consist of repeating portions of characters!

In this case we must move the pattern thus the repeated portion must now align with its first occurrence in the pattern.

A variation of this case is when the portion from the pattern A overlaps with another portion that consists of the same characters.

[![Boyer-Moore Good Suffix Shift 2](/wp-content/uploads/2012/04/Boyer-MooreGoodSuffixShift2.png)](/wp-content/uploads/2012/04/Boyer-MooreGoodSuffixShift2.png)Sometimes these portions may overlap!

Yet again the shift must align the second portion with its first occurrence. 

Finally only a portion of A, let’s say “B”, can happen to occur in the very beginning of the pattern, as on the diagram below.

[![Boyer-Moore Good Suffix Shift 3](/wp-content/uploads/2012/04/Boyer-MooreGoodSuffixShift3.png)](/wp-content/uploads/2012/04/Boyer-MooreGoodSuffixShift3.png)Only a sub-string of the pattern may re-occur at its front!

Now we must align the left end of the pattern with the rightmost occurrence of “B”.

## Bad Character Shifts

Beside the good-suffix shifts the Boyer-Moore algorithm make use of the so called bad-character shifts. In case of a mismatch we can skip comparisons in case the character in the text doesn’t happen to appear in the pattern. To become clearer let’s see the following examples.

[![Boyer-Moore Bad Character 1](/wp-content/uploads/2012/04/Boyer-MooreBadCharacter1.png)](/wp-content/uploads/2012/04/Boyer-MooreBadCharacter1.png)If the mismatched letter of the text appears in the pattern only in its front we can align it easily!

In the picture above we see that the mismatched character “B” from the text appears only in the beginning of the pattern. Thus we can simply shift the pattern to the right and align both characters B, skipping comparisons. An even better case is described by the following diagram where the mismatched letter isn’t contained into the pattern at all. Then we can shift forward the whole pattern.

[![Boyer-Moore Bad Character 2](/wp-content/uploads/2012/04/Boyer-MooreBadCharacter2.png)](/wp-content/uploads/2012/04/Boyer-MooreBadCharacter2.png)In case the mismatched letter isn't contained into the pattern we move forward the pattern!

## Maximum of Good-suffix and Bad-Character shifts

Boyer-Moore needs both good-suffix and bad-character shifts in order to speed up searching performance. After a mismatch the maximum of both is considered in order to move the pattern to the right.

## Complexity

It’s clear that Boyer-Moore is faster than Morris-Pratt, but actually its worst-case complexity is O(n+m). The thing is that in natural language search Boyer-Moore does pretty well.

[![Boyer-Moore Complexity](/wp-content/uploads/2012/04/Boyer-Moore-Complexity.png)](/wp-content/uploads/2012/04/Boyer-Moore-Complexity.png)Worst-case scenario of Boyer-Moore - O(m+n)

## Implementation

Finally let’s see the implementation in [PHP](/category/php/), which can be easily “transcribed” into any other programming language. The only thing we need is the structures for bad-character shifts and good-suffixes shifts.

```php
= 0; --$i) {
      if ($i > $g && $suffixes[$i + $m - 1 - $f] = 0 && $pattern[$g] == $pattern[$g + $m - 1 - $f]) {
            $g--;
         }
         $suffixes[$i] = $f - $g;
      }
   }
}
 
/**
 * Fills in the array of bad characters.
 *
 * @param string $pattern
 * @param array  $badChars
 */
function badCharacters($pattern, &$badChars)
{
   $m = strlen($pattern);
 
   for ($i = 0; $i = 0; $i--) {
      if ($suff[$i] == $i + 1) {
         for ($j = 0; $j = 0 && $pattern[$i] == $text[$i + $j]; $i--);
      if ($i [![Boyer-Moore Application](/wp-content/uploads/2012/04/Boyer-MooreApplication.png)](/wp-content/uploads/2012/04/Boyer-MooreApplication.png)