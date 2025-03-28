---
title: "Performing Quantitative RNA-seq, Part II"
author: "Dr. Duryea and Dr. Gilbert"
format: html
editor: visual
execute:
  keep-md: true
---




## Displaying RNA-seq Data

In the last notebook, we learned how to analyze RNA-seq data and determine which genes in a dataset are differentially expressed or turned or off given our experiment. However, this left us with many potential gene candidates and perhaps a lack of the overall pattern that our data takes.

One way to visual RNA-seq data and get a better idea of the patterns in the data is by creating a heatmap. A heatmap is a graphical representation of data where values are represented by colors, typically with a color scale (warm vs. cool colors, for example). In Bioinformatics, heatmap plots are used to identify patterns in large genomics datasets. The colors in the heat map can be used to indicate the amount of gene expression for any given gene and the plot overall can help identify patterns in which genes are turned on or off for our experiment.

We will use another example dataset from the *R Bioinformatics Cookbook* to learn how to create a heatmap from RNA-seq data. This data is looking at the model plant species *Arabidopsis thaliana* and how its gene expression varies in different parts of the plant. We can use RNA-seq to see if gene expression varies by ecotype, which are naturally occurring variants of *Arabidopsis* that are adapted to different climates and environments.

## Notebook Objectives

In this notebook, you will:

-   Get introduced to `ComplexHeatmap` - an R package for creating heatmaps from large datasets.

-   Use an RNA-seq dataset from the model plant *Arabidopsis* to create a heatmap plot

-   Interpret the heatmap plot that you create.

## RNA-seq Part II

At this point, it is a good idea to open a fresh Quarto document within your RNA-seq project and give it a meaningful title like "RNAseq_PartII". Give your new document a useful description and then we will install and load the packages that you will need to analyze this data.

The main package we will be using is `ComplexHeatmap.` This can be downloaded from Bioconductor. Remeber, copy the code below and remove the "\#"'s to install `ComplexHeatmap.` Once the package is installed, you can replace the "\#"'s, so that you don't install it every time you render your document.



::: {.cell}

```{.r .cell-code}
#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("ComplexHeatmap")
```
:::



The other packages we will use allow for the data to be organized and visualized with a useful color scheme. Copy the code and remove the "\#"'s to install these packages. Then, replace the "\#"'s after they are installed.



::: {.cell}

```{.r .cell-code}
#install.packages("viridisLite")
#install.packages("stringr")
#install.packages("RColorBrewer")
#install.packages("circlize")
```
:::



Now, load all your packages using library, including `rbioinfcookbook`, which you downloaded in the last notebook.



::: {.cell}

```{.r .cell-code}
library(ComplexHeatmap)
library(viridisLite)
library(stringr)
library(RColorBrewer)
library(circlize)
library(rbioinfcookbook)
```
:::



## Loading the Data

Now that we have all the packages that we need, let's load the dataset from the `rbioinfcookbook.` The name of the dataset is `at_tf_gex`. We will pull out a few columns of interest and scale the data to make it more easy to interpret.

We will then use a function in the `stringr` package to split the data by ecotype.



::: {.cell}

```{.r .cell-code}
mat <- log(as.matrix(at_tf_gex[ , 5:55]))
ecotype <- stringr::str_split(colnames(mat), ",", simplify = TRUE)[,1]
part <- stringr::str_split(colnames(mat), ",", simplify = TRUE)[,2]
```
:::



## Color Palettes

We will use the `circlize` and `viridisLite` packages to create color palettes for the different types of data in the heatmap. We will create a unique color palette for each ecotype and plant part.



::: {.cell}

```{.r .cell-code}
data_col_func <- circlize::colorRamp2(seq(0, max(mat), length.out = 6), viridisLite::magma(6))

ecotype_colors <- c(RColorBrewer::brewer.pal(12, "Set3"), RColorBrewer::brewer.pal(5, "Set1"))
names(ecotype_colors) <- unique(ecotype)

part_colors <- RColorBrewer::brewer.pal(3, "Accent")
names(part_colors) <- unique(part)
```
:::



## Annotations and Formatting

Now, we will add annotations to our heatmap to allow us to interpret the results. The code below adds two annotation objects that will be displayed on the heatmap. Ecotype and plant part information are included in the `top_annot`. We use `annotation_name_side` to set the annotation to the left of the color that they represent.

The `side_annot` object is used to add annotation to the rows of the heatmap. In this case, we add the length information for the samples. The other functions are used to format the plot:

-   `anno_points()` specifies the location of the points to be plotted

-   `pch` specifies the shape of the points

-   `size` specifies the size of the points

-   `axis_param` specifies the locations of the ticks on the x-axis.



::: {.cell}

```{.r .cell-code}
top_annot <- HeatmapAnnotation("Ecotype" = ecotype, "Plant Part" = part, col = list("Ecotype" = ecotype_colors, "Plant Part" = part_colors), annotation_name_side = "left")

side_annot <- rowAnnotation(length = anno_points(at_tf_gex$Length, pch = 16, size = unit(1, "mm"), axis_param = list(at = seq(1, max(at_tf_gex$Length), length.out = 4)),))
```
:::



## The Heatmap!

Now that we have everything coded and formatted, we are ready to create our heatmap! We will use the `Heatmap()` function in `ComplexHeatmap` and specify `mat` as the dataset. The `data_col_func` uses the color palettes that we created above and `top_annot` and `side_annot` uses the annotations we made. The other functions adjust the display of the plot:

-   `row_km` function is used to set the number of clusters for the rows

-   `cluster_columns` is set to TRUE which causes the columns of the heatmap to cluster based on their similarity in gene expression

-   `column_split` is set to ecotype, which groups the columns by ecotype

-   `show_column_names` is set to FALSE, which hides the column names to make for a neater display

-   `column_title` is also left blank to allow the plot to look less cluttered

Copy over and run the code below to produce your heatmap!



::: {.cell}

```{.r .cell-code}
ht_1 <- Heatmap(mat, name="log(TPM)", row_km = 6, col = data_col_func, top_annotation = top_annot, right_annotation = side_annot, cluster_columns = TRUE, column_split = ecotype, show_column_names = FALSE, column_title = " ")

ComplexHeatmap::draw(ht_1)
```

::: {.cell-output-display}
![](RNAseq_PartII_files/figure-html/unnamed-chunk-7-1.png){width=672}
:::
:::



Congratulations - you made a heatmap of RNA-seq data! Are you able to interpret the output? The log(TPM) is looking at gene expression by transcript count. The lighter colors represent genes that are more highly expressed. Are you able to notice any trends in gene expression based on ecotype or plant part?

## Summary and debrief

To learn more about this project you can also check out the Expression Atlas for this experiment: <https://www.ebi.ac.uk/gxa/experiments/E-GEOD-53197/Results> or the original article that published it, cited below.

This is a complex analysis, so you might not understand it all, but see what you can learn from this. Reflect on what you learned and what questions you still have in your Quarto document.

Don't forget to annotate your notebook and pull, commit, and push your notebook to GitHub before moving on!

## References

*R Bioinformatics Cookbook*, 2nd edition, Dan MacLean, Packt Publishing, 2023.

Lei, L., Steffen, J.G., Osborne, E.J. *et al.* Plant organ evolution revealed by phylotranscriptomics in *Arabidopsis thaliana* . *Sci Rep* **7**, 7567 (2017). <https://doi.org/10.1038/s41598-017-07866-6>
