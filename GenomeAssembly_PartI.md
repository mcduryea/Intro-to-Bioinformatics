---
title: "Genome Assembly, Part I"
author: 
  - "Dr. Gilbert" 
  - "Dr. Duryea"
format: html
execute:
  keep-md: true
---





## Preliminary Thoughts

In solving the newspaper problem, you developed some strategies for reconstructing a string from its overlapping fragments. The goal of this notebook is to build a function which will reproduce a genome string in an idealized case. We'll assume that all of the fragments are of the same length, the overlapping segments are of a constant length, and we already know the order in which the overlapping fragments should be arranged. These are some very strong assumptions, but we'll relax them once we've solved this idealized version of the genome assembly problem.

## Assembling the Genome

Start by rebuilding your `randGenome()` function, which takes in your set of `nucleotides` and also a `genome_length` as input parameters.


::: {.cell}

```{.r .cell-code}
#Define randGenome() here
```
:::


We'll be able to use our `randGenome()` function to generate a fake genome to test our reconstructions with. It order to be able to do this, we'll need another function which will be able to fracture an existing genome into segments. We'll start with a simple function, `compositionK()` which takes a `genome` and an integer `k` as arguments, and outputs the `k`-mer composition of the `genome`. 

***

**Challenge 1:** Build a function `compositionK()`, to generate the `k`-mer composition of a `genome` string.  

> **Input:** A string (`genome`) and an integer (`k`).  
> **Output:** The `k`-mers composing the `genome`, where the `k`-mers are arranged in lexicographic (dictionary) order.

***


::: {.cell}

```{.r .cell-code}
#Solve the challenge problem here
```
:::


Once you think you've built `compositionK()` correctly, use it to solve the [*Generate the k-mer Composition of a String*](https://rosalind.info/problems/ba3a/) problem on Rosalind.


::: {.cell}

```{.r .cell-code}
#Solve the Rosalind Problem here.
```
:::


Now that we have a deconstructed string, can we put it back together? Doing so would help us stitch the overlapping reads together to reconstruct the genome. Note, there are many simplifying assumptions here, including:

+ The reads span the entire genome
+ All reads are of the same length
+ There are no errors in nucleotide transcription

***

**String Reconstruction Problem:** Reconstruct a genome string from its k-mer composition.  

> **Input:** A collection of patterns of k-mers (`patterns`) and an integer (`k`).  
> **Output:** A reconstructed genome (`genome`) with k-mer composition equal to `patterns` if such a genome exists.

***

We're not ready to solve the *String Reconstruction Problem* yet. Let's detour through a few simpler scenarios. We'll start with one in which we know that the k-mers are all pre-arranged into the correct order.

**Note:** There is lots of great discussion in the textbook between the statement of the string reconstruction problem and the *String spelled by a path* problem. There are several illustrative examples which should be added to this notebook!

***

**Challenge 2:** Construct a function `string_from_path()` to reconstruct a genome string spelled by a genome path:  

> **Input:** A sequence of $n$ k-mers (`seq`) such that for two subsequent elements of `seq`, the last $k-1$ nucleotides of the earlier element match the first $k-1$ nucleotides of the latter.  
> **Output:** A string (`genome`) of length $k + n - 1$ reconstructed by combining the consecutive k-mers using their overlaps.

***


::: {.cell}

```{.r .cell-code}
#Solve the challenge problem here.
```
:::


Once you've solved Challenge 2, head over to Rosalind to solve the [*Reconstruct a String from its Genome Path*](https://rosalind.info/problems/ba3b/) problem.


::: {.cell}

```{.r .cell-code}
#Complete the Rosalind Challenge here 
```
:::


Now that we've solved a simplified version of the genome reconstruction problem, let's recap before working towards a more general solution in the next notebook.

## Summary

In this intro notebook, we've accomplished the following.  

+ We've learned how to use our `randGenome()` function and a new `compositionK()` function to generate test cases for our ability to reconstruct a genome from its `k`-mer composition.
+ We built a function to reconstruct a genome from a genome path. That is, we've solved the genome assembly problem in the idealized case where we have the $k$-mer composition of a genome, we know the order in which the overlapping $k$-mers appear in the genome, and our $k$-mers cover the entire genome.

While we've solved a very idealized, special case of the genome assembly problem, we should be proud of the work we've accomplished. The functions we've built in this notebook will come in handy for solving the genome assembly problem in general.

See you in the next notebook.