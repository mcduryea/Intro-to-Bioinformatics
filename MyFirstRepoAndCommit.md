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

  + In the *repository name* field, type `BioStatisticsAnalysis`.
  + Include a meaningful description.
  + Please leave the repository as Public -- you can talk to Dr. Gilbert or Dr. Duryea if you have concerns about this.
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
  + Paste the URL to your *BioStatisticsAnalysis* repository into the URL. The project directory name field will auto-populate.
  + Choose a location on your computer to house the project. I have a directory (folder) on my computer called *GitHub* and all of my local clones of repositories reside inside of it. You should do the same.
  + Check the box to *open in new session*
  + Click the button to *Create Project*

Great! You've now built an Rproject inside of your repository. Among other things, this allows you to commit and push changes to your main repository on GitHub. We'll create a file and then push changes later on.

### Create a Reproducible Quarto Document

+ Now that you are in your new Rproject, click `File -> New File -> Quarto Document` to create a new Quarto Document in your local repository.
+ Give your document the title *Palmer Penguins Initial Analysis*.
+ Add yourself as the Author of the document.
+ If you want a convenient interface with familiar formatting buttons (bold, italics, lists, etc.) to work in, select the box that says *use the visual editor*. If you prefer to work directly in markdown, then leave that box unchecked.
+ Click the *Create* button.
+ In the YAML (Yet Another Markdown Language) header -- that's the chunk in between the triple hyphens (\-\-\-) -- add the code below to the bottom of the header (before the second set of hyphens). This says that we'd like to keep the intermediate markdown file during the rendering process. The reason for this is that markdown files display nicely in GitHub.


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
    #Load the tidyverse
    library(tidyverse)
    ```
    
    ::: {.cell-output .cell-output-stderr}
    ```
    ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
    ```
    :::
    
    ::: {.cell-output .cell-output-stderr}
    ```
    ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ✔ tibble  3.1.8     ✔ dplyr   1.0.9
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
    #Read the penguins_samp1 data file from github
    penguins <- read_csv("https://raw.githubusercontent.com/mcduryea/Intro-to-Bioinformatics/main/data/penguins_samp1.csv")
    ```
    
    ::: {.cell-output .cell-output-stderr}
    ```
    Rows: 44 Columns: 8
    ```
    :::
    
    ::: {.cell-output .cell-output-stderr}
    ```
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (3): species, island, sex
    dbl (5): bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g, year
    
    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ```
    :::
    
    ```{.r .cell-code}
    #See the first six rows of the data we've read in to our notebook
    penguins %>% head()
    ```
    
    ::: {.cell-output .cell-output-stdout}
    ```
    # A tibble: 6 × 8
      species   island bill_length_mm bill_depth_mm flipper_le…¹ body_…² sex    year
      <chr>     <chr>           <dbl>         <dbl>        <dbl>   <dbl> <chr> <dbl>
    1 Adelie    Dream            43.2          18.5          192    4100 male   2008
    2 Adelie    Dream            44.1          19.7          196    4400 male   2007
    3 Chinstrap Dream            49            19.5          210    3950 male   2008
    4 Gentoo    Biscoe           48.4          14.4          203    4625 fema…  2009
    5 Gentoo    Biscoe           59.6          17            230    6050 male   2007
    6 Adelie    Biscoe           37.9          18.6          172    3150 fema…  2007
    # … with abbreviated variable names ¹​flipper_length_mm, ²​body_mass_g
    ```
    :::
    :::

  
  + Below the code cell, use text to describe what you see. Make sure that you are typing in an area with a white background, not a grey one.
  + Click the blue arrow labeled *Render* at the top of the markdown editor to render your document. You'll be prompted to save the file first, if you haven't done so already. I named my file `PalmerPenguins_Initial.qmd` -- you should choose something similar.
  
    + The rendered document will appear in the viewer tab of the lower-right pane of your RStudio window.
    + If you have any errors that prevent the document from rendering, be sure to fix them. Ask for help if you need it!
    
Excellent -- you now have a Quarto Document which loads R packages, and displays a data set on various characteristics of penguins. This is quite an achievement. Now lets show the world!
  
### Commit and Push Changes to GitHub

Now your local repository has some files that your GitHub repository doesn't have! It's time to push those changes to GitHub so that they are reflected in the main repository.

+ Click the `Git` tab in the top-right pane of RStudio.
+ Click the blue *Pull* icon to pull in the most up-to-date version of all files in the main GitHub repository.

  + This seems to be a silly step now (we know there are no changes there), but *Pulling* before you *Commit/Push* will save you significant headaches later -- especially once you are collaborating with others in a shared repository.
+ Click the check boxes next to all of the files listed.
+ Click the *Commit* button.
  
  + Add a relevant Commit Message -- something like "Created Quarto Document -- PalmerPenguins_Initial".
  + Click the *Commit* button below the Commit Message dialogue box.
+ Close the message box that summarizes the Commit tasks.
+ Click the green *Push* arrow (it's pointing upwards) to push your committed changes to the main GitHub repository.
+ Navigate to your GitHub repository on the web and click refresh to see that the new files now appear there.

Fantastic! You've now updated your main repository on GitHub with the new files you've created. Both your local and main/remote GitHub repository are up to date and hold the same versions of all the project files.

## Summary

Nice work! You've accomplished a lot in this activity. You created a GitHub repository, used RStudio to clone the repository and created an Rproject to manage it, you created a Quarto Document which contained some text as well as your first few lines of R code, you rendered the document, and you reconciled all of the differences between the "origin" repository on GitHub and your local clone of it by first *Pulling* in any changes present on GitHub then *Committing* your local changes and *Pushing* them back out to GitHub.

The major takeaways from this activity are summarized below.

+ GitHub is used to host repositories, which track the current state (as well as the history) of all files involved in a project.
+ Typically a project will involve an *origin* repository located on GitHub as well as one or more "remote" clones which are located on the machines of the project contributors.
+ *Pulling* in changes from *origin* ensures that your cloned version of the repository contains the most up-to-date version of all files in the project repository hosted on GitHub.
+ *Committing* and then *Pushing* changes from your local clone ensures that the changes you've made to files in your local clone are reflected in the *origin* on GitHub. 
+ Once you've *committed* and *pushed* your changes, your project collaborators will get your updated files as soon as they *pull* in changes from the *origin* repository on GitHub.

  + No more e-mailing files back and forth or trying to remember which version of a file is the most recent one! Git and GitHub are now managing all of that for you.

+ We can manage all of these *pull/commit/push* tasks from the `Git` tab in RStudio.

  + Be sure to make *pull*, then *commit*, then *push* your standard workflow for committing changes to the *orign* repository. Doing so will save lots of headaches associated with "*merge conflicts*" (when two people make changes to the same part of a file) later on.

+ We can use Quarto Documents in RStudio to mix R code, text, and more into a reproducible analysis (more on this later on).

  + GitHub can be used to host much more than just Quarto Documents -- in fact, your `BioStatisticsAnalysis` repository already contains lots of types of files.

+ GitHub nicely displays markdown (`.md`) files directly within your repository. This means that people viewing your repository can see your work without needing to download and run all of your files. Try opening one of your `.rmd` or `.html` files from GitHub and notice how difficult they are to read when compared to the `.md` files.


