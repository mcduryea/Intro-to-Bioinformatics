---
title: "Finding the Replication Origin, Part II"
author: "Dr. Gilbert and Dr. Duryea"
format: html
execute:
  keep-md: true
---





## Preliminary Thoughts

Welcome back! We've done a lot of work and made quite a bit of headway so far. As a reminder, our goal through this first chapter is to identify the location of the replication origin within genome. Up to this point we've had a crash course in computing, encountering looping mechanisms (`for` loops), programmatic flow control (`if`/`else if`/`else` statements), and developed an ability to write reusable code blocks in the form of *functions*. We've applied all of these techniques to help us in analysing genomes (mostly fake/random genomes that we've generated, but we'll get to real bacterial genomes soon). Our work thus far has culminated in a pair of functions `generate_k_mers()`, which will generate all strings of `k` consecutive nucleotides within a genome, and `count_patterns()` which will count the number of occurrences of a particular `pattern` within a `genome`. Let's pick back up from here.

Open RStudio and open the project that is managing your group repository. Open the Quarto Notebook you were working on last time and re-run the code cells which build the `generate_k_mers()` and `count_pattern()` functions -- you can simply run all of the code chunks if you'd like. You'll be adding on to that notebook today.

## Towards Identification of the Replication Origin

Remember from General Biology that DNA replication begins at a certain position in the genome called the origin. The origin consists of a specific set of DNA sequences that signal to the cell's machinery the correct position to start replication. In bacteria, one of the proteins that helps with replication is called *DnaA*. This protein binds to sequences in the genome called *DnaA boxes*, which are strings of 9 nucleotides that are repeated more frequently around the origin of replication.

Armed with the knowledge that these DnaA boxes appear more frequently than one would expect a random sequence of 9 nucleotides to appear, we know that we are searching for frequently occurring 9-mers. Can you combine your `generate_k_mers()` and `count_patterns()` functions into a new function called `generate_frequent_k_mers()` that will build a list of the most frequent `k`-mers? Your function should take two parameters, `genomeString` and `k`.

***

**Challenge 1:** Find the most frequent words in a string

>***Input:*** A DNA string (`genomeString`) and an integer `k`  
>***Output:*** A list containing the most frequent `k`-mers in `genomeString`. Note that your list should contain each `k`-mer at most once.  
>*Hint*: Use `unique(frequent_k_mers)` on your list of `frequent_k_mers` before returning the object.

***


::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


Think you've got it? Try your function on the sequence `ACACAGACATCCCACCCC` and consider 3-mers. Your function should return a list of two 3-mers, `ACA` and `CCC`. Once you've done that, head back over to Rosalind and try solving the [Frequent Words Problem](http://rosalind.info/problems/ba1b/){target="_blank"}. Test your code on the sample data set first -- if it works, click the link to download the challenge data set and see if you can generate the correct output. As a reminder, the code to read the data into R is below.


::: {.cell}

```{.r .cell-code}
data <- scan("PATH_TO_FILE", what = character())

#The genomeString
genomeString <- data[1]
genomeString

#The value of k
k <- as.integer(data[2])
k
```
:::


To automatically generate the output in the format Rosalind wants it (all k-mers on one line, each separated by a space), you can use the following.


::: {.cell}

```{.r .cell-code}
freq_k_mers <- generate_frequent_k_mers(genomeString, k)

noquote(freq_k_mers)
```
:::


The code stores the list of most frequent k-mers into the object freq_k_mers and then says that we should print each of these k-mers out with no quotation marks around them.

Great work. Now that we are feeling good about ourselves, things are going to get messy. Unfortunately the nucleotides in these DnaA boxes we are looking for, don't always appear in the same way. For example, just like humans make mistakes while typing, DNA polymerase can make mistakes while it replicates the genome -- what if one of the base pairs wasn't copied correctly during the previous round of replication? Additionally, the replication signal we are looking for may appear as both the k-mer signaling that replication should begin, or as its reverse complement. Because of this, we'll need to build code that is flexible enough to count reverse complements as well as near k-mers (with some pre-defined number of discrepancies allowed). Let's get on it.

We'll start with the reverse complement problem. Recall from gen-bio that A and T are complementary base pairs, as well as G and C. This means that in double-stranded DNA, we should see across from each Adenine (A) a Thymine (T), across from each Cytosine (C) a Guanine (G), and vice-versa. This means that if we look across from a segment of DNA, we can see the reverse complement of that segment. The "reverse" refers to the fact that the complementary sequence is also anti-parallel, meaning 5' and 3' ends are oriented in opposite directions on each side of the double strand. Thus, we read the complementary sequence in reverse order. For example, the reverse complement of `ACCTGA` is `TCAGGT`.

Let's write a function to compute the `reverse_complement()` of a `genomeSubString`. While you're at it, solve your third Rosalind challenge.

***

**Challenge 2:** The [Reverse-Complement Problem](http://rosalind.info/problems/ba1c/){target="_blank"}  

> **Input:** A pattern of nucleotides called `genomeSubString`  
> **Output:** The reverse-complement of `genomeSubString`

***


::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


The efficiency of our code can make a really big difference in our ability to solve problems at the scale of real genomes (millions of nucleotides in length). We'll start paying a bit more attention to ways we might be able to speed up our functions (and clean up our code too). While running slow code on our examples so far hasn't been much of a problem, if we run a series of slow functions on a full genome (say 4 million nucleotides), we may end up with code that takes hours, days, or even weeks to run!

Let's see a way to speed up (and clean up) our `reverse_complement()` function below (we'll do this together).


::: {.cell}

```{.r .cell-code}
library(Dict)
#We'll update the function together
```
:::


*Note that we had left last year:* The fourth Rosalind problem ([Find all occurrences of a pattern in a string](https://rosalind.info/problems/ba1d/){target="_blank"}) may be a really good homework problem. It doesn't really follow the reverse complement problem -- what do you think about assigning it for them to complete outside of class? I wonder if we could made an assignment out of pages 12-13 in the book -- having them write about why pattern matching is important and how just because we have a frequently occurring k-mer/9-mer this doesn't necessarily mean we have found a DnaA box, and then having them try to solve the coding challenge.

## Summary and Debrief

We're a week in and we've made quite a bit of headway. We're able to search a genome for frequent "near"-k-mers and their reverse-complements. These are the genetric substrings which will signal replication to the DNA polymerase. Next time, we'll look at narrowing down the area we should be looking in for the replication origin. We'll exploit properties of genetic transcription and subtle mutation.

