---
title: "Performing Quantitative RNA-seq, Part I"
author: "Dr. Duryea and Dr. Gilbert"
format: html
execute:
  keep-md: true
---




## Quantitative RNA-seq

In the next few notebooks, we will start our Bioinformatics analysis with an introduction to RNA-seq. This is one of the most popular of the new genetic techniques used by scientists to better understand organisms. RNA-seq uses a high-throughput sequencing method to quantify all RNA or all genes that are being expressed in a sample. Samples can be whole organisms (usually small organisms, like insects), tissues, or even single cells. In most approaches, two or more treatments are compared to determine how genes are differentially expressed, or what genes are "turned on" during different conditions. However, in any given cell or tissue, there are hundreds or thousands of genes that are expressed at once - Bioinformatics to the rescue!

## Notebook Objectives

In this notebook, you will:

-   Get introduced to `edgeR`, one of the most common R packages for analyzing RNA-seq data.

-   Import a small RNA-seq data set

-   Analyze the RNA-seq data set for differential expression and interpret the output

## RNA-seq Project

At this point it would be a good idea to open a New Project in R Studio. Make sure it is connected to your GitHub by using Version Control and give it a meaningful name like Bioinformatics. Once you have your New Project, open a new Quarto Document and give it a meaningful title, like "RNA-seq Part I." Once you have your document set up, type a description (you can update this later), and then we are ready to start loading the packages we need!

Let's start by installing the packages we will need to do this analysis. First, wee will install a package called `rbioinfocookbook`. This is from the book *R Bioinformatics Cookbook* by Dan MacLean. This book is a great resource for those who want to continue on with Bioinformatics. We will install `rbioinfocoobook` from Dan MacLean's GitHub repository using `devtools.`



::: {.cell}

```{.r .cell-code}
#install.packages("devtools")
library(devtools)
```

::: {.cell-output .cell-output-stderr}

```
Loading required package: usethis
```


:::

```{.r .cell-code}
#devtools::install_github("danmaclean/rbioinfcookbook")
library(rbioinfcookbook)
```
:::



Now, let's install the other packages we will use to analyze the data. We will install `forcats` the usual way and then two other packages using Bioconductor. This is a package installer that helps to streamline the installation of Bioinformatics packages.



::: {.cell}

```{.r .cell-code}
#install.packages("forcats")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("edgeR")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("Biobase")

library(forcats)
library(edgeR)
```

::: {.cell-output .cell-output-stderr}

```
Loading required package: limma
```


:::

```{.r .cell-code}
library(Biobase)
```

::: {.cell-output .cell-output-stderr}

```
Loading required package: BiocGenerics
```


:::

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'BiocGenerics'
```


:::

::: {.cell-output .cell-output-stderr}

```
The following object is masked from 'package:limma':

    plotMA
```


:::

::: {.cell-output .cell-output-stderr}

```
The following objects are masked from 'package:stats':

    IQR, mad, sd, var, xtabs
```


:::

::: {.cell-output .cell-output-stderr}

```
The following objects are masked from 'package:base':

    anyDuplicated, aperm, append, as.data.frame, basename, cbind,
    colnames, dirname, do.call, duplicated, eval, evalq, Filter, Find,
    get, grep, grepl, intersect, is.unsorted, lapply, Map, mapply,
    match, mget, order, paste, pmax, pmax.int, pmin, pmin.int,
    Position, rank, rbind, Reduce, rownames, sapply, saveRDS, setdiff,
    table, tapply, union, unique, unsplit, which.max, which.min
```


:::

::: {.cell-output .cell-output-stderr}

```
Welcome to Bioconductor

    Vignettes contain introductory material; view with
    'browseVignettes()'. To cite Bioconductor, see
    'citation("Biobase")', and for packages 'citation("pkgname")'.
```


:::
:::



## Using edgeR to estimate differential expression

The `edgeR` package is one common tool for estimating differential expression, or how two or more samples differ in what genes are being expressed (turned on or off). We will practice this using a sample data set from the R Bioinformatics Cookbook - `modencodefly`. This data set contains data on the model organsim, *Drosophila melanogaster*. Even this sample data set contains 147 different samples and about 15,000 genes. You can read more about this data set at <http://data.modencode.org/>.

To estimate differential expression, we will use two tools available in `edgeR`:

-   Trimmed mean of M-values (TMM) method for normalizing data - this technique uses the mean of log-transformed gene expression values to help scale the data and account for differences in data collection between samples (the size of the library, or amount of data collected for each sample).

-   Generalized linear model (GLM) - this statistical method is a more general form of a linear model that works well for count data. We will use it to compare differences between treatments or the types of samples.

### Using edgeR from a count table

Here, we will start by loading a transcript count table. This is a raw data file which has the RNA transcripts that were recorded in our samples and a count for each transcript.

We will start by loading a data set from the `rbioinfcookbook` package called `count_dataframe` and converting it to a matrix, or data table that is readable by R. This dataframe has the RNA transcript counts from the *Drosophila* experiment.



::: {.cell}

```{.r .cell-code}
genes <- count_dataframe[['gene']]
count_dataframe[['gene']] <- NULL
count_matrix <- as.matrix(count_dataframe)
rownames(count_matrix) <- genes
```
:::



You can check the Environment tab to see what this did. You should have a data table called `count_dataframe` with 14,869 observations (these are the different gene transcripts) and 147 variables (these are the different fly samples).

Now, we will use another data set from `rbioinfcoockbook` called `pheno_data`. This has the phenotypic data or physical information about the samples. We will specify that we want to work with two experiments about the larvae and their larval stage, L1 (first instar) and L2 (second instar).



::: {.cell}

```{.r .cell-code}
experiments_of_interest <- c("L1Larvae", "L2Larvae")
columns_of_interest <- which(pheno_data[['stage']] %in% experiments_of_interest)
```
:::



Now we will set stage as the grouping factor, or categorical variable that we will use to compare between samples.



::: {.cell}

```{.r .cell-code}
grouping <- pheno_data[["stage"]] [columns_of_interest] |> forcats::as_factor()
```
:::



Now, we will form a subset of the `count_matrix` data table that only includes our columns of interest.



::: {.cell}

```{.r .cell-code}
counts_of_interest <- count_matrix[,counts = columns_of_interest]
```
:::



Now, we will use the `DGEList` function in `edgeR` to assemble all our data into one object called `count_dge`.



::: {.cell}

```{.r .cell-code}
count_dge <- edgeR::DGEList(counts = counts_of_interest, group = grouping)
```
:::



Our data is now assembled in the format that we need and we can finally perform our differential expression analysis! To do this, we will use a series of functions from the `edgeR` package. These functions are outline below:

-   `model.matrix` tells `edgeR` our experimental design or what variables we are testing with our model

-   `estimateDisp` is used to estimate the dispersion (or spread) of our count data

-   `glmQLFit` is used to fit a generalized linear model to our data

-   `glmQLFTest` is used to perform a likelihood ratio test to identify differentially expressed genes

-   `topTags` displays the top differentially expressed genes

Run these now with the code below.



::: {.cell}

```{.r .cell-code}
design <- model.matrix(~grouping)
eset_dge <- edgeR::estimateDisp(count_dge, design)
fit <- edgeR::glmQLFit(eset_dge, design)
result <- edgeR::glmQLFTest(fit, coef=2)
topTags(result)
```

::: {.cell-output .cell-output-stdout}

```
Coefficient:  groupingL2Larvae 
               logFC    logCPM        F       PValue          FDR
FBgn0027527 6.318477 11.148756 43306.39 8.921836e-36 1.326588e-31
FBgn0037430 6.557811  9.109132 37053.47 4.458944e-35 2.269743e-31
FBgn0037424 6.417995  9.715826 36957.31 4.579479e-35 2.269743e-31
FBgn0037414 6.336991 10.704514 32230.76 1.878703e-34 6.983608e-31
FBgn0029807 6.334533  9.008720 30679.42 3.125484e-34 9.294565e-31
FBgn0037429 6.623754  8.525136 26529.63 1.399656e-33 3.468581e-30
FBgn0037224 7.056029  9.195077 25539.20 2.072124e-33 4.064613e-30
FBgn0030340 6.176240  8.500866 25406.05 2.186892e-33 4.064613e-30
FBgn0029716 5.167029  8.977840 24890.80 2.700890e-33 4.462171e-30
FBgn0243586 6.966860  7.769756 24146.95 3.698699e-33 5.499595e-30
```


:::
:::



Let's interpret this output! The first column shows us our gene transcript of interest (e.g. FBgn0037430), the logFC stands for log Fold Change, which is a measure of the differences between the two samples. The logCPM column which stands for log Counts Per Million, this is similar to the average expression value of the gene across samples but accounts for the large number of gene transcripts. The F value, P value, and false detection rate (FDR) columns are the results of our statistical test. FDR is the most relevant to us because it is a p-value that has been corrected for the number of tests we're conducting, or adjusted to decrease the probability of false detection.

Using FDR, we can see that a number of gene transcripts are differentially expressed between our two treatments. This is showing us how gene epression differs between the first and second larval instar. However, at this point we may be left with more questions than answers! This is typical of bioinformatics analysis. It will take more research to better understand this data!

## Summary and Debrief

In this notebook, we got introduced to `edgeR`, a popular package for analyzing RNA-seq data. We used `edgeR` to compare RNA-seq data from two treatments of *Drosophila* larva and see what genes were differentially expressed between treatments. In the next notebook, we will learn how to visualize this data.

Don't forget to annotate your notebook and pull, commit, and push your notebook to GitHub before moving on!

## References

*R Bioinformatics Cookbook*, 2nd edition, Dan MaClean, Packt Publishing, 2023.
