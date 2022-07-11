---
title: "Finding the Replication Origin, Part IV"
author: "Dr. Gilbert and Dr. Duryea"
format: html
execute:
  keep-md: true
---





## Preliminary Thoughts

In all of our work so far, we've taken a fairly naive approach to discovering the replication origin. We haven't leveraged what biology can tell us about our search. Let's remember some biology and use it to our advantage.

### The Machinery of Genome Replication

Recall from gen-bio that DNA replication is asymmetric, meaning there is a leading strand and a lagging strand. The leading strand is replicated as one continuous section because it is oriented in the correct direction for the replication machinery. However, the lagging strand is pointed in the "wrong" direction, so it is replicated more slowly in chunks that are called Okazaki fragments.

One thing we did not mention in gen-bio is that this actually exposes the lagging strand to more potential errors during replication. This is because the lagging strand is actually signal stranded for more time and is more vulnerable to mutation during this time. One type of mutation that can occur is called deamination. This is where cytosine (C) mutates into thymine (T). Take a minute to review the chemical structure of both of these nucleotides. Can you see how cytosine could turn into T?

After this first mutation occurs (C mutating into T), we would have T across from G at this position of the double helix. When this genome is replicated again, the DNA polymerase would correct this "mismatch" so that the G would be replaced by A, to follow the rules of complementary base pairing. Thus, the overall result of deamination is a decrease in the frequency of G in that section of the genome.

Now that we know about deamination, let's see if we can use it to our advantage. It turns out that looking at the cumulative difference in observed frequencies of guanine and cytosine along the genome can be really helful in identifying the location where we should begin searching for the replication origin.

**Definition:** Let's define `SKEW(genome)` to be a function evaluated along a given genome, with a resulting value reported at each nucleotide. We select an arbitrary starting nucleotide (position 0), and report the *skew* at nucleotide *i* along the genome as the difference between the total number of observed guanine and total number of observed cytosine between position *0* and position *i*. We set $SKEW_0\left(genome\right) = 0$ and can compute $SKEW_{i+1}\left(genome\right)$ as follows:



$$SKEW_{i+1} = \left\{\begin{array}{l} SKEW_{i}\left(genome\right) + 1, \text{ if } genome_{i} = G\\
SKEW_{i}\left(genome\right) - 1, \text{ if } genome_{i} = C\\
SKEW_{i}\left(genome\right), \text{ otherwise} \end{array}\right.$$

***

**Challenge 1:** Build a function to compute `SKEW()`.  
> **Input:** A `genome`  
> **Output:** A list, `skew` such that `skew[i]` contains the value of the `SKEW()` function at nucleotide position *i* in `genome`.

***

::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::

Does your `SKEW()` function work? How do you know? If you are confident, use `SKEW()` to compute the *skew* along the E. Coli genome. Store the results into a list called `e_coli_skew`

::: {.cell}

```{.r .cell-code}
#Compute e_coli_skew here
```
:::

Use the code below to plot thew skew along the E. Coli genome.

::: {.cell}

```{.r .cell-code}
ggplot() +
  geom_line(mapping = aes(x =1:length(e_coli_skew), y = e_coli_skew))
```
:::

Thats interesting and quite informative. The biological theory suggests that we should look for the replication origin near the minimum of the skew function.

***

**Challenge 2:** Create a new function `SKEW_min()` so that it returns the location(s) of the minimum *skew* along the genome rather than returning a full list of skew values.

***

::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::

Use your new `SKEW_min()` function to solve the [Minimum Skew Problem](http://rosalind.info/problems/ba1f/) on Rosalind.

***

**Challenge 3:** Solve the Minimum Skew Problem on Rosalind.

***

::: {.cell}

```{.r .cell-code}
#Complete the challenge here
```
:::


## Summary and Debrief

Similar to the last notebook, I'll need to come back and write this section. We may need some more scaffolding here as well.

