---
title: "Phylogenetic Analysis and Visualization"
author: "Dr. Duryea and Dr. Gilbert"
format: html
editor: visual
execute:
  keep-md: true
---




## Introduction

In this exercise, we will learn how to analyze genetic data to build phylogenies, or evolutionary trees. Evolutionary trees have been used to display relationships among organisms since Darwin first wrote *The Origin of Species*. They were originally made using morphological data (or physical traits) but are now typically made using genetic data and increasingly large genetic datasets. Once again, it's Bioinformatics to the rescue!

## Notebook Objectives

In this Notebook, you will:

-   create various formats of evolutionary trees using the packages `ape` and `treeio`

-   examine how `ggtree` can be used to create many different visualization for evolutionary trees

-   interpret the output of the programs and "read" an evolutionary tree

## New Document and Packages

Open a new Quarto document in your Bioinformatics project. Give your document a meaningful title like "Phylogenetic analysis and Visualization." Type a brief description and then start to install the new packages you will need. Remember to remove the "\#" the first time you run it and then replace it after the package downloads.



::: {.cell}

```{.r .cell-code}
#install.packages("ape")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("treeio")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("ggtree")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("SGSeq")
```
:::



Now, let's load all the packages we will need into our library. Some of them you have downloaded earlier.



::: {.cell}

```{.r .cell-code}
library(rbioinfcookbook)
library(SGSeq)
library(ape)
library(treeio)
library(ggplot2)
library(ggtree)
```
:::



## Reading and writing trees with ape and treeio

In this section, we will create trees from a mammal phylogeny from datasets available in the *R Bioinformatics Cookbook.* First, we will use `ape` to read in the tree in two different formats - Newick and Nexus.

Newick is a simple format where evolutionary trees are displayed in a text-based format. This is good for computers, but doesn't create a great visualization.

The Nexus format builds on the Newick format and adds in character data (the DNA sequences or traits used to build the tree). Neither formats are great for humans to interpret, but both are a good way to load thee data to start the analysis.



::: {.cell}

```{.r .cell-code}
newick_file_path <- fs::path_package("extdata", "mammal_tree.nwk", package = "rbioinfcookbook" )

nexus_file_path <- fs::path_package("extdata", "mammal_tree.nexus", package = "rbioinfcookbook")

newick <- ape::read.tree(newick_file_path)
nexus <- ape::read.nexus(nexus_file_path)
```
:::



These will load the trees into your environment for us to work with. Now, we will load the same mammal phylogeny in two formats that are used by `treeio` - BEAST and RAxML.

BEAST stands for Bayesian Evolutionary Analysis Sampling Trees and is one of the most common tree formats. It uses a Bayesian method to incorporate different rates of evolution and is therefore capable of more sophisticated analysis.

RAxML stands for Randomized Axelerated Maximum Likelihood. It is a popular program for phylogenetic analysis of large datasets using maximum likelihood. Maximum likelihood is used to give statistical confidence to evolutionary trees.



::: {.cell}

```{.r .cell-code}
beast_file_path <- fs::path_package("extdata", "beast_mcc.tree", package = "rbioinfcookbook")

raxml_file_path <- fs::path_package("extdata", "RAxML_bipartitionsBranchLabels.H3", package = "rbioinfcookbook")

beast <- read.beast(beast_file_path)
raxml <- read.raxml(raxml_file_path)
```
:::



Again, this loaded the trees into our environment. We can use the `class` function to check what type of objects each tree type is, so we can use the appropriate analysis function.



::: {.cell}

```{.r .cell-code}
class(newick)
```

::: {.cell-output .cell-output-stdout}

```
[1] "phylo"
```


:::

```{.r .cell-code}
class(nexus)
```

::: {.cell-output .cell-output-stdout}

```
[1] "phylo"
```


:::

```{.r .cell-code}
class(beast)
```

::: {.cell-output .cell-output-stdout}

```
[1] "treedata"
attr(,"package")
[1] "tidytree"
```


:::

```{.r .cell-code}
class(raxml)
```

::: {.cell-output .cell-output-stdout}

```
[1] "treedata"
attr(,"package")
[1] "tidytree"
```


:::
:::



This shows us that there are two formats, phylo and treedata. We can use functions in `treeio` to interconvert between these formats.



::: {.cell}

```{.r .cell-code}
beast_phylo <- treeio::as.phylo(beast)
newick_tidytree <- treeio::as.treedata(newick)
```
:::



Now, we are ready to write the output files!



::: {.cell}

```{.r .cell-code}
treeio::write.beast(newick_tidytree, file = "mammal_tree.beast")
ape::write.nexus(beast_phylo, file = "beast_mcc.nexus")
```
:::



These will save the files, but again, nothing pop ups. These trees are now in a format that can be read by many different phylogenetic analysis packages. One way to view the file is to use the free resources on the Interactive Tree of Life Project (<https://itol.embl.de/>).

Go to this website an click on "Upload a Tree" below Annotate on the bottom of the page. When the page loads, click on "Choose File" and navigate to the "mammal_tree.beast" file that you just created. It should be in your GitHub project folder. You can leave the other fields blank or add it a name or description, if you like.

What do you notice about the tree? Do the relationships make sense to you? Look up any of the names that are unfamiliar and comment on your findings in your Quarto document.

In the next section, we will learn how to visualize and format trees in R using `ggtree` and data from the Interactive Tree of Life project.

## Visualizing trees using ggtree

In this section, we will learn how to visualize trees using `ggtree.` This allows us to look at our trees in R and is useful for bioinformatics because we can visualize trees created by different genes and easily compare and edit them.

First, we will load in some trees from the `rbioinfcookbook` package. This is a Newick format of tree from the Interactive Tree of Life project (<https://itol.embl.de/>).



::: {.cell}

```{.r .cell-code}
tree_file <- fs::path_package("extdata", "itol.nwk", package = "rbioinfcookbook")

itol<- ape::read.tree(tree_file)
```
:::



Now, we will use `ggtree` tot create a basic tree plot.



::: {.cell}

```{.r .cell-code}
ggtree(itol)
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-9-1.png){width=672}
:::
:::



We are able to see the tree! However, it might still be unintelligible. Let's add labels to our tree!



::: {.cell}

```{.r .cell-code}
ggtree(itol) +
  geom_tiplab(color = "blue", size = 1)
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-10-1.png){width=672}
:::
:::



We have labels, but this is still hard to read! Let's try some different options for displaying the tree. We can also make a circular plot.



::: {.cell}

```{.r .cell-code}
ggtree(itol, layout = "circular") +
  geom_tiplab(color = "blue", size = 2)
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-11-1.png){width=672}
:::
:::



This makes it easier to read the labels. You now may be able to tell that we are working with different species of bacteria.

You can also add annotations to the tree. We can add a strip of color to highlight a particular clade.



::: {.cell}

```{.r .cell-code}
ggtree(itol, layout = "circular") +
  geom_tiplab(color = "blue", size = 2) +
  geom_strip(13, 14, color = "red", barsize = 1)
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-12-1.png){width=672}
:::
:::



You can also change the way the tree is displayed by inverting it. You may remember these commands from ggplot!



::: {.cell}

```{.r .cell-code}
ggtree(itol) +
  coord_flip() +
  scale_x_reverse()
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-13-1.png){width=672}
:::
:::



You can also display your tree as unrooted. This displays relationships of organisms without including a time component and is useful when you are unsure of the origins of your species of interest.



::: {.cell}

```{.r .cell-code}
ggtree(itol, layout = "unrooted")
```

::: {.cell-output .cell-output-stderr}

```
"daylight" method was used as default layout for unrooted tree.
```


:::

::: {.cell-output .cell-output-stderr}

```
Average angle change [1] 0.174910612627287
```


:::

::: {.cell-output .cell-output-stderr}

```
Average angle change [2] 0.16164519138077
```


:::

::: {.cell-output .cell-output-stderr}

```
Average angle change [3] 0.129304375923281
```


:::

::: {.cell-output .cell-output-stderr}

```
Average angle change [4] 0.0825706767962521
```


:::

::: {.cell-output .cell-output-stderr}

```
Average angle change [5] 0.100056259084231
```


:::

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-14-1.png){width=672}
:::
:::



Now that we know the basics, let's see if we can display our mammal phylogeny from earlier.

Load in the mammal Newick file and use `ape` to read the tree.



::: {.cell}

```{.r .cell-code}
mammal_file <- fs::path_package("extdata", "mammal_tree.nwk", package = "rbioinfcookbook" )

mammal<- ape::read.tree(mammal_file)
```
:::



Now, let's use `ggtree` to create a basic tree plot.



::: {.cell}

```{.r .cell-code}
ggtree(mammal) +
  geom_tiplab(color = "blue", size = 2)
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-16-1.png){width=672}
:::
:::



Let's display our tree as circular for comparison.



::: {.cell}

```{.r .cell-code}
ggtree(mammal, layout = "circular") +
  geom_tiplab(color = "blue", size = 2)
```

::: {.cell-output-display}
![](Phylogenetic_Analysis_Vis_files/figure-html/unnamed-chunk-17-1.png){width=672}
:::
:::



We have come full circle and our now able to display trees in R!

## Summary and debrief

Reflect on what you learned in this exercise and what questions you still have in your Quarto document.

Don't forget to annotate your notebook and pull, commit, and push your notebook to GitHub before moving on!

## References

*R Bioinformatics Cookbook*, 2nd edition, Dan MacLean, Packt Publishing, 2023.
