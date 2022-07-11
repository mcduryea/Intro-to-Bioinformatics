---
title: "Finding the Replication Origin, Part V"
author: "Dr. Gilbert and Dr. Duryea"
format: html
execute:
  keep-md: true
---





### Dealing with Imperfections

Unfortunately we've already seen that errors in genome replication can occur. Additionally, we know that technological imperfections are likely to result in misreads when we try to sequence a genome in the lab. These are issues we will have to deal with as we seek to identify the replication origin (and also as we continue our journey into different areas of bioinformatics). They are indeed issues, as can be seen if we attempt to find frequent 9-mers near the location of the minimum skew in the E. Coli genome -- we come up with none! We'll have better luck if we look for frequent near 9-mers instead -- that is, 9-mers with the potential for some discrepancies (mismatches).

We need a better definition of *near* k-mers. Let's start with a way to measure the distance between two strings. The simplest such measure is called the *Hamming Distance*. For two strings of equal length, `stringA` and `stringB`, the Hamming Distance counts the number of positions in which they disagree. For example, the Hamming Distance between the strings `"ACCAGGTA"` and `"ACGGGATA"` is 3, since they disagree in positions 3, 4, and 6. Now you are ready to write a function to compute the Hamming Distance between two strings.

***

**Challenge 1:** The Hamming Distance  
> **Input:** Two strings, `stringA` and `stringB`  
> **Output:** The Hamming Distance between `stringA` and `stringB`.

***


::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


***

**Challenge 2:** Use your Hamming Distance function to solve the [Hamming Distance Problem](http://rosalind.info/problems/ba1g/) on Rosalind.

***


::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


Now that we know how to compute the Hamming Distance, we can make well-defined claims like "*`stringA` is **near** `stringB` if `hamming_dist(stringA, stringB)` $\leq$ `d`*" for some predetermined value `d` . This will allow us to complete the approximate pattern matching we need to do in order to identify the likely DnaA boxes within our suspected replication region.

In a recent homework assignment, you built a function `find_pattern()` to identify the starting positions of a particular pattern within a genome string. Use your new `hamming_dist()` function to update that `find_pattern()` function to a new function called `find_near_pattern()` which is sensitive to *near* matches of the pattern with at most `d` mismatches.

***

**Challenge 3:** Create `find_near_pattern()`, an updated version of `find_pattern()`, which accommodates mismatches.  
> **Input:** A `genomeString`, a `pattern`, and a non-negative integer `d` denoting the number of mismatches allowed while still resulting in a *near*-match.  
> **Output:** All starting positions where `pattern` appears as a substring of `genomeString` with at most `d` mismatches.

***


::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


***

**Challenge 4:** Use your `find_near_pattern()` function to solve the [Approximate Pattern Matching Problem](http://rosalind.info/problems/ba1h/) on Rosalind.

***


::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


Okay, now we've *really* come a long way. We know how to search through a genome and identify its most-often occurring k-mers. We've identified that just because a k-mer is most frequent across the entire genome, this doesn't mean that it is a good candidate for a DnaA box -- we are more interested in frequently occurring k-mers within short segments of the genome (~500 nucleotides or so). We used biological theory to develop the notion of *skew* within a genome and identified the point of minimum skew as a good place to start looking for DNA-A boxes. Finally, we noted that DNA replication and transcription are imperfect on both the biological and human sides. We discovered the Hamming Distance as a way to measure the distance between two strings, allowing us to then build functionality which identifies near matches of a string -- helping us overcome this imperfection challenge.

Now we just need to put it all together to identify our final candidates for the DnaA boxes, which send the replication signal for our DNA polymerase to begin replication. We'll do this in two steps. First, we'll build a function that identifies *frequent k-mers with mismatches*, and then we will extend it to a function that also includes *reverse complements with mismatches* in the counts of frequent k-mers.

**Note:** This will be a heavy lift for the students. I am wondering if it is worth recapping efficient versions of the necessary functions for them here and then asking them to organize the existing functions as building-blocks into these larger functions to find the DnaA boxes.

***

This gets us through page 31, but the recent notebooks likely need more structure.

***

## Summary and Debrief

Similar to the last notebook, I'll need to come back and write this section. We may need some more scaffolding here as well.

