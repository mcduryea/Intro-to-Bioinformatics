---
title: "Genome Assembly, Part III"
author: 
  - "Dr. Gilbert" 
  - "Dr. Duryea"
format: html
execute:
  keep-md: true
---





## Preliminary Thoughts

In the previous notebook, `GenomeAssembly_PartII`, we developed some mathematics that could help us solve the Genome Assembly problem in general. First, we learned about Graph Theory, an area of mathematics concerned with the study of objects consisting of *vertices* and *edges* connecting pairs of vertices. In particular, we noted that we were interested in directed graphs, and we saw several ways for encoding graph structures. At the end of our last notebook we had translated the Genome Assembly problem into the problem of finding an Eulerian Path in the deBruijn graph corresponding to the $k$-mer composition of a genome of interest. This notebook picks up where we left off. We'll work to develop an algorithm that will construct an Eulerian Path in a deBruijn Graph. 

## Assembling the Genome: Walking Through the deBruijn Graph

An Eulerian Path in a graph is a path which uses all edges in the underlying graph exactly once. We've come to the conclusion that these Eulerian Paths in the deBruin Graph corresponding to a $k$-mer composition of a genome can be used to identify candidate reconstructed genomes. We are fortunate to be seeking Eulerian Paths because  

+ There is a simple test to determine whether an Eulerian Path in a graph exists.
+ We can build an iterative algorithm to efficiently find an Eulerian Path in a graph.

In this notebook we'll discover and implement both the existence test and the algorithm to construct an Eulerian Path if it exists. While doing this we'll continue to connect the mathematics to the Genome Assembly problem.

#### Another Way to Construct deBruijn Graphs

In the previous notebook we built a function `deBruinK()` to construct the deBruin Graph from the Path Graph corresponding to a genome. Unfortunately, we were still working under idealized and unrealistic assumptions. Assuming that we know the Path Graph of the genome is equivalent to assuming that we know the original genome to begin with. If we already know the genome then there is no need to assemble it. Let's try to construct the deBruin Graph without knowing the Path Graph.

Consider the following:

1. If we have the $k$-mer composition of a genome, we can compute the `prefix()` and `suffix()` of each $k$-mer. 
2. We construct a graph in which the unique `prefix()`es and `suffix()`es are the vertices in the graph.
3. For each $k$-mer in the $k$-mer composition of our genome, we'll draw an edge from the vertex corresponding to its `prefix()` to the vertex corresponding to its `suffix()`.

The three steps above will build the deBruijn Graph of the $k$-mer composition of a genome without assuming that we know the Path Graph ahead of time!

***

**Challenge 1:** Construct a new function `deBruin()` which will construct a deBruin Graph from a collection of $k$-mers.  

> **Input:** A collection of $k$-mers (`patterns`).
> **Output:** The edge-list of the corresponding deBruijn Graph.

***


::: {.cell}

```{.r .cell-code}
#Solve the challenge problem here
```
:::


Once you've solved the challenge problem, head over to Rosalind and use your function to solve the [*deBruijn Graph of a Colection of $k$-mers* problem](https://rosalind.info/problems/ba3e/).


::: {.cell}

```{.r .cell-code}
#Solve the Rosalind problem here
```
:::


#### The Bridges of Könignsberg

Now that we have a method for constructing the deBruijn Graph from the $k$-mer composition of a genome alone, let's return to the notion of Eulerian Paths. We'll begin with a short field trip to the town of Königsberg (now Kaliningrad, Russia) in 1736.

![Bridges of Königsberg](images/Konigsberg_bridges.png)

The image above shows a drawing of the city with the River Pregel flowing through it. The green items in the image show the positions of seven bridges laid about the city. The townspeople wondered whether it was possible to tour the city by walking all seven bridges and without repeating a bridge.

**NOTE:** STUDENT GROUPS WILL TRY THIS ACTIVITY ON THE KöNIGSBERG MAP AS WELL AS A VARIETY OF OTHER "MAPS" TO GAIN INTUITION FOR THE PROBLEM.

Think about your analyses of the bridge touring problems. In which cases was such a tour possible? What properties did those maps have in common that maps where the bridge tour was impossible did not possess?

***

**Challenge 2:** Devise a simple test to determine when an Eulerian Cycle exists in a graph. Adapt your simple test to determine whether an Eulerian Path exists in a directed graph. (No coding required for this challenge)

***

#### Euler's Theorem

Now that you've devised a simple test to determine whether an Eulerian Path in the deBruijn graph exists (and therefore whether reassembly of a genome is possible from a set of $k$-mers), it would be really nice if we could construct such a path. We'll focus on doing that here.

**NOTE:** NEED TO BETTER DISCUSS CYCLES AND DEFINE EULERIAN CYCLES IN THIS NOTE-SET. PROBABLY AT THE START OF THE EXPLANATION OF THE KöNIGSBERG BRIDGES PROBLEM.

In 1736, the mathematician, Leonard Euler noticed the same things you just did.Furthermore, he was able to prove exactly when an Eulerian Circuit exists in a graph *and* how to find it.

**Euler's Theorem:** Every *balanced*, *strongly-connected* directed graph is Eulerian.

**Definition (balanced):** A graph is said to be *balanced* if the *in-degree* of every vertex matches its *out-degree*.

**Definition (strongly-connected):** A graph is said to be *strongly-connected* if it is possible to get from any vertex to any other vertex in the graph by following a sequence of directed edges.

The proof of Euler's Theorem holds the key to constructing an Eulerian Circuit in a graph. Consider that we have a graph which is *balanced* and *strongly-connected*. We'll outline the main steps of Euler's Construction below.

+ Consider an ant placed at a vertex $v_0$ in the graph. 
+ Allow the ant to walk randomly throughout the graph under the condition that it must follow the directions of the edges and that no edge may be traversed more than once.
+ The ant will eventually get stuck at a vertex with no "out"-edges left to traverse.

  + If the ant got lucky, it has traversed every edge exactly once and has constructed an Eulerian Circuit and we are done.
  + More likely, the ant has gotten stuck without covering all of the edges and we'll need a way to restart.

+ In the case that our ant has gotten stuck without restarting, note that the ant must have gotten stuck back at our original starting vertex. Indeed, if the ant comes into a vertex via an "in"-edge, then the ant must always be able to leave via an "out"-edge because our underlying graph was balanced. The only time this isn't the case is for the starting vertex, since the first edge consumed there was an "out"-edge.
+ The ant has traced out an initial circuit, which we will label as $C_0$.
+ If $C_0$ does not include all of the edges in the graph, then there must exist a vertex in the underlying graph with edges still unused. Furthermore, such a vertex must exist along $C_0$ since the graph is strongly connected.
+ We drop our ant at any vertex along $C_0$ with unused edges. Call that vertex $v_1$ for convenience. 

  + Since the original graph was balanced, we know that the graph consisting of the same vertex set but only including the edges unused by $C_0$ is also balanced.
  
+ Again, we allow the ant to randomly walk a sequence of unused edges until it gets stuck. 
+ For the same reasons as those mentioned earlier, the ant must get stuck at this new starting vertex. We'll call this new circuit $C'$.
+ We create a new larger circuit by beginning at $v_1$, tracing out $C'$ and then tracing out $C_0$ (beginning at $v_1$) to create the larger circuit which we will call our new $C_0$.
+ If unused edges remain in the graph, then again there will be at least one vertex along our new $C_0$ with unused edges. We'll repeat the process of tracing out a new cycle from this vertex and replacing $C_0$ with a larger circuit which includes both $C_0$ and the new extension.
+ We continue this procedure until no unused edges remain. The result will be an Eulerian Circuit in the graph.

Let's close out this notebook by solving the Eulerian Circuit problem.

***

**Challenge 3:** Build an `eulerCircuit()` function to construct an Eulerian Circuit in a graph, if one exists.  

> **Input:** The edge-list of an Eulerian directed graph.
> **Output:** A list of vertices corresponding to the traversal of an Eulerian Circuit in the graph.

***


::: {.cell}

```{.r .cell-code}
#Solve the challenge problem here
```
:::


Once you've solved the challenge problem, use your new function to complete the [*Eulerian Circuit problem*](https://rosalind.info/problems/ba3f/) at Rosalind.


::: {.cell}

```{.r .cell-code}
#Solve the Rosalind challenge here
```
:::


## Summary

In this intro notebook, we've accomplished the following.  

+ We identified how to construct the deBruijn Graph from a $k$-mer composition of a genome without first knowing the Path Graph of the genome.
+ We solved the famous Bridges of Königsberg problem, and defined Eulerian Paths and Circuits as special substructures which exist in some graphs.
+ We discovered requirements for an Eulerian Circuit to exist within a directed graph.
+ We worked through a constructive proof of Euler's Theorem on the existence of Eulerian Circuits in graphs.
+ We used the proof as a model in building a function which will consume the edge-list of a directed graph and return a sequence of vertices corresponding to an Euler Circuit in the graph, provided that one exists.

We accomplished quite a bit of mathematics in this notebook. Remember though that we are doing all of this to solve the Genome Assembly problem. As a reminder, in Part II of this series of notebooks, we developed the notion of the deBruijn Graph corresponding to the $k$-mer composition of a genome. Also in that notebook we identified that Eulerian Paths in the deBruijn Graph will correspond to candidate reconstructed genomes. 

We've come a very long way, but there's still more to do! See you in the next notebook.