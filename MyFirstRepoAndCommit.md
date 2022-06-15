---
title: "My First Repo and Reproducible Report"
author: "Dr. Gilbert and Dr. Duryea"
format: html
execute:
  keep-md: true
---



## Activity Objectives

In this activity, we'll achieve the following goals.

1. Create a GitHub repository, initialized with a README file.
2. Create an Rproject which clones your remote (GitHub) repository to your local machine.
3. Create a Quarto Document containing an analysis on the `PalmerPenguins` data.
4. Render the Quarto Document into a reproducible report.
5. Stage, commit, and push changes in your local repository to the remote repository, hosted at GitHub.

### Create a GitHub Repository

+ Navigate to [GitHub](https://www.github.com) and log in, if necessary.
+ Identify the `+` icon in the top-right corner of the webpage. Click on it, and select *New Repository*.

  + In the *repository name* field, type `PalmerPenguinsAnalysis`.
  + Include a meaningful description.
  + You may choose to leave the repository as Public or switch it to Private.
  + Click the checkbox to *Add a README file*.
  + You can leave both `.gitignore` and `license` set to *None* for now.
  + Click the green button to *Create Repository*.
  + Once inside of your repository, click the green *Code* button and use the clipboard icon to copy the URL for the repository. We'll need it soon.

Congratulations; you've created your first GitHub repository! It's pretty boring right now -- all it contains is a README file with no useful information. That's fine. We'll start adding to the repository shortly.

### Create an Rproject From Your Repository

+ Open RStudio on your local machine.
+ Click `File -> New Project`
+ Choose to create a new project from *Version Control*.
  
  + Choose *Git* as the version control system.
  + Paste the URL to your *PalmerPenguinsAnalysis* repository into the URL. The project directory name field will auto-populate.
  + Choose a location on your computer to house the project. I have a directory (folder) on my computer called *GitHub* and all of my local clones of repositories reside inside of it. You should do the same.
  + Check the box to *open in new session*
  + Click the button to *Create Project*

Great! You've now built an Rproject inside of your repository. Among other things, this allows you to commit and push changes to your main repository on GitHub. We'll create a file and then push changes later on.

### Create a Reproducible Quarto Document

+ Now that you are in your new Rproject, click `File -> New File -> Quarto Document` to create a new Quarto Document in your local repository.
+ Give your document the title *Palmer Penguins Initial Analysis*.
+ Add yourself as the Author of the document.
+ If you want a convenient interface with familiar formatting buttons (bold, italics, lists, etc.) to work in, select the box that says *use the visual editor*. If you prefer to work directly in markdown, then leave that box unchecked.
+ In the YAML (Yet Another Markdown Language) header -- that's the chunk in between the triple hypens (\-\-\-) -- add the code below. This says that we'd like to keep the intermediate markdown file during the rendering process. The reason for this is that markdown files display nicely in GitHub.


    ::: {.cell}
    
    ```{.r .cell-code}
    execute:
      keep-md: true
    ```
    :::

  
+ Replace the "Quarto" header with *Palmer Penguins*
+ Replace the text below the header with an informative blurb.
+ Delete all of the text and code cells below -- it will be better to build the rest from scratch.
+ Type a forward slash ("//") and choose *R Code Chunk* from the dropdown list.

  + Inside the grey code chunk, type the three lines below and run each line either with `ctrl+Enter`/`cmd+Return`, or run the entire chunk using the green "play" button in the top-right corner of the code chunk.

    ::: {.cell}
    
    ```{.r .cell-code}
    library(tidyverse)
    ```
    
    ::: {.cell-output .cell-output-stderr}
    ```
    ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
    ```
    :::
    
    ::: {.cell-output .cell-output-stderr}
    ```
    ✔ ggplot2 3.3.5     ✔ purrr   0.3.4
    ✔ tibble  3.1.6     ✔ dplyr   1.0.9
    ✔ tidyr   1.2.0     ✔ stringr 1.4.0
    ✔ readr   2.1.2     ✔ forcats 0.5.1
    ```
    :::
    
    ::: {.cell-output .cell-output-stderr}
    ```
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ```
    :::
    
    ```{.r .cell-code}
    library(palmerpenguins)
    
    penguins %>% head()
    ```
    
    ::: {.cell-output .cell-output-stdout}
    ```
    # A tibble: 6 × 8
      species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
      <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    1 Adelie  Torge…           39.1          18.7              181        3750 male 
    2 Adelie  Torge…           39.5          17.4              186        3800 fema…
    3 Adelie  Torge…           40.3          18                195        3250 fema…
    4 Adelie  Torge…           NA            NA                 NA          NA <NA> 
    5 Adelie  Torge…           36.7          19.3              193        3450 fema…
    6 Adelie  Torge…           39.3          20.6              190        3650 male 
    # … with 1 more variable: year <int>
    ```
    :::
    :::

  
  + Below the code cell, use text to describe what you see. Make sure that you are typing in an area with a white background, not a grey one.