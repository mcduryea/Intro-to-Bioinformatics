---
title: "Genome Assembly, Part IV"
author: 
  - "Dr. Gilbert" 
  - "Dr. Duryea"
format: html
execute:
  keep-md: true
---





## Preliminary Thoughts

We've spent a lot of time in the previous three notebooks re-casting the Genome Assembly problem into different settings in order to put a general solution more within reach. We've identified that a problem from Graph Theory which was solved in 1735 and a problem in theoretical mathematics and computer science solved in 1946 hold the keys to our ability to solve the Genome Assembly problem. We actually aren't too far behind "cutting-edge" bioinformatics here. The deBruijn graph technique we are now developing was first brought to bioinformatics in 1989 and is still the dominant approach for genome assembly today!

In this notebook we'll continue our work towards recovering candidate genomes from Eulerian Paths in the deBruijn Graph corresponding to the $k$-mer composition of the genome. At the end of the last notebook we developed an algorithm and function `eulerCircuit()` to construct an Eulerian Circuit from a graph, provided that one exists. We'll continue where we left off by adapting our function to find Eulerian Paths, since those are what we truly seek.

## Assembling the Genome: A More General Solution

The culmination of this notebook will be *a* general solution to the Genome Assembly problem, but will still be accompanied by several simplifying assumptions. The first stop in this notebook will be to move from being able to discover Eulerian Circuits in a graph to being able to construct Eulerian Paths instead. Once we achieve this objective, we'll relax some of our remaining assumptions to accommodate real restrictions associated with the ability of bioinformaticians to generate long reads from a genome. At the close of the notebook, we'll highlight a few of the differences between the Genome Assembly scheme we've come up with and the current state of the art methods employed in labs today.

#### From Eulerian Circuits to Eulerian Paths

Consider a graph that has an Eulerian Path but not an Eulerian Circuit. Are you convinced that such a graph exists? By now you should at least be convinced of the existence of graphs with Eulerian Circuit and the conditions that are required for these structures to exist. We can construct a graph with an Eulerian Path but no Eulerian Circuit by simply removing a single edge from a graph with an Eulerian Circuit. 

***

**Challenge 1:** (No Code) How many *un-balanced* vertices can a graph with an Eulerian Path have? What must be true about those vertices? (Remember, a vertex in a directed graph is said to be *balanced* if its in-degree and out-degree match.)

***

Now that you've thought a bit more about Eulerian Paths, you're ready to tackle another challenge.

***

**Challenge 2:** Build a function, `eulerPath()`, which will find an Eulerian Path in a graph, if such a path exists.  

> **Input:** An edge-list corresponding to a directed graph which contains an Eulerian Path.
> **Output:** A sequence of vertices corresponding to an Eulerian Path in the input graph.

***


::: {.cell}

```{.r .cell-code}
#Solve the challenge problem here
```
:::


Once you've built your function and are satisfied that it works, head on over to Rosalind to solve the [*Find an Eulerian Path in a Graph problem*](https://rosalind.info/problems/ba3g/).


::: {.cell}

```{.r .cell-code}
#Solve the Rosalind problem here
```
:::


With that accomplishment in the books, you've now got all of the pieces you need in order to solve the Genome Assembly problem under the conditions that you are in possession of the $k$-mers which compose the genome, for some fixed value of $k$ and you have complete coverage of the genome. To do so, simply combine the functions you've written to do the following:

1. Construct the deBruijn Graph from the $k$-mer composition. (`deBruijnK()`)
2. Construct and Eulerian Path through the deBruijn Graph. (`eulerPath()`)
3. Reconstruct the genome from the Path Graph (`stringFromPath()`)

***

**Challenge 3:** Create a function `genomeReconstruction()` which solves the Genome Assembly problem with full-coverage $k$-mers.  

> **Input:** An integer (`k`) and a list of $k$-mers (`patterns`).
> **Output:** A string `genome` with $k$-mer composition equal to `patterns`.

***


::: {.cell}

```{.r .cell-code}
#Solve the challenge problem here
```
:::


Once you've solved that challenge, verify your solution by heading to Rosalind and solving the [*Reconstruct a String from its $k$-mer Composition* problem](https://rosalind.info/problems/ba3h/).


::: {.cell}

```{.r .cell-code}
#Solve the Rosalind problem here
```
:::


**NOTE:** I'M SKIPPING THE INCLUSION OF SOLVING THE $k$-UNIVERSAL BINARY STRING PROBLEM FOR NOW. I DON'T THINK ITS SOLUTION IS NECESSARY TO PROCEED.

#### Assembling the Genome From Read-Pairs

In our most recent solution to the Genome Assembly problem we assumed that we had obtained a set of $k$-mers with complete coverage of the genome. Unfortunately, this assumption is not possible given current read-technology in bioinformatics. Genomes can be millions of nucleotides long, but current read technology can produce reliable reads over only a few hundred consecutive nucleotides. The Genome Assembly problem is also harder than we've let on because real genomes exibit long patterns which repeat throughout the genome (remember those DNAa boxes from our first unit?). These repeating patterns make it possible for multiple distinct Eulerian Paths to exist within the deBruijn Graph of the $k$-mer composition of our genome. Since multiple distinct Eulerian Paths exist, we'll obtain multiple candidates for the assembled genome.

Unfortunately, there's not much we can do to combat the repeating patterns problem. There are, however, some strategies which are useful in getting around the limited ability to produce reads out of a genome.

**Definition (Read-Pairs):** Biologist have devised quite an ingenious approach to increasing the read length by generating *read-pairs*, which are pairs of reads separated by a fixed distance $d$ within the genome. These reads are long, gapped reads in which we obtain $k$ nucleotides, followed by a gap of $d$ nucleotides, and then another read of $k$ nucleotides. The result is a read over $k + d + k$ consecutive nucleotides in the genome.

Each $\left(k, d\right)$-read-pair will correspond to not one edge, but two edges in the corresponding deBruijn Graph. In particular, there is a path in the deBruijn Graph of length $k + d + 1$ nodes corresponding to each $\left(k, d\right)$-read-pair. If only one such path in the deBruijn Graph exists (or if all of these paths spell out the same string), then this single $\left(k, d\right)$-read-pair becomes a long *virtual read* of length $2k + d$ starting from the first $k$-mer in the read-pair and ending in the second $k$-mer of the read-pair.

PERHAPS INCLUDE A VISUAL EXAMPLE HERE...

While the discussion above is exciting, it makes a pretty strong assumption. That is, that the path of length $k + d + 1$ in the deBruijn Graph is either unique or that all such paths spell out the same string. When this isn't the case, we cannot transform a read-pair into a long virtual read. In those cases, we have an alternative approach.

#### From Composition to Paired Composition



The following ideas need to be included in this notebook:

+ Transforming read-pairs into *long virtual reads*
+ Paired composition and $\left(k, d\right)$-mers of the form $\left(\overbrace{\texttt{Pattern 1}}^{k} |\underbrace{\cdots}_{d} |\overbrace{\texttt{Pattern 2}}^{k}\right)$
+ Paired deBruijn Graphs
+ A Pitfall of paired deBruijn Graphs
+ [*String Reconstruction from Read-Pairs* problem](https://rosalind.info/problems/ba3j/)





## Summary

In this intro notebook, we've accomplished the following.  

+ Something we accomplished...

See you in the next notebook.