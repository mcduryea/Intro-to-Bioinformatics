---
title: "Accessing and Using the Group Repos"
author: "Dr. Gilbert and Dr. Duryea"
format: html
execute:
  keep-md: true
---


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
:::


## Preliminary Comments

Over the course of the first few activities in our course, we've come a long way. You've gained some familiarity with R, used Quarto to build reproducible notebooks, many of you have had a crash course in programming (we'll continue learning and practicing more), and you've used a GitHub repository to track changes to your work along the way. As mentioned at the end of the last activity, you should be proud of what you've accomplished so far.

Soon, we'll embark on our first real bioinformatics endeavor. Our goal will be to identify the section of a genome which sends the signal that genetic replication should begin. It is along this segment of the genome that DNA polymerase binds to DNA and begins unzipping and copying the sequence of nucleotides. We'll be asking questions about genetic code, searching for patterns, and finding clues about which sections of the genome signal for DNA polymerase to bind and begin replication. Each of you will benefit by having colleagues to bounce ideas off of, to test and share code, and to collaborate more generally. 

## Shared Group Repositories (*Repos*)

We've assigned groups and have built a shared GitHub repo for each group. You'll use this shared repository to collaborate with your group members. We won't be using the full capabilities of GitHub for now though. Below are some ground rules for use of the repositories at this point. 

+ Each group member will maintain their own Quarto Notebook(s) in the repository.
+ Please use the naming convention `Replication_FirstName_LastName.qmd` for your Quarto Notebook devoted to DNA Replication (Chapter 1 in our book).

  + Doing this for now ensures that everyone is working on their own notebook. There will be no conflicts due to two group members making dueling changes to the same section of the same file. This *will* happen later in our course, and Dr. Duryea and I will prepare you for it.

+ The advantage provided to you by the shared group repository for now is that you'll be able to see your group members' notebooks (as long as they *push* them to GitHub, and you *pull* in their changes). We'll remind you to do this often.

We'll make better use of collaboration later in our course, but we are still getting used to GitHub and want to continue avoiding some frustrating and scary-looking collisions.

### Accessing and Cloning Your Group Repo

Check Slack for a list of your group members and a link to your shared group repository. You'll have been added to a new restricted channel in Slack containing only your group members as well as Dr. Duryea and Dr. Gilbert.

1. Navigate to the shared repository. 
2. Hit the green *Code* button and copy the HTTPS link.
3. Head on over to R, click `File -> New Project`.
4. Choose `Version Control` and `Git`.
5. Paste the URL into the Repository URL field.
6. Click the Browse button next to the third field if you'd like to change where the project will be stored.
7. Click the check box to *Open in a new session*
8. Click the *Create Project* button.
9. Once the new RStudio window opens, use `File -> Open File` and choose the `StarterNotebook.qmd` file.
10. Immediately use `File -> Save As` to create your own copy of the file using the naming convention suggested above.
11. Use the blue *render* arrow to render your notebook.
12. Now it's time to Sync your local repository to "origin" (that's the Repo on GitHub).

  + Use the blue arrow pointing downwards in Git tab of the top-right pane of RStudio to *Pull* in any existing repository changes from "origin".
  + Click the check box next to each of the listed files, click the *commit* button.
  + Add a Commit message and then click the *Commit* button.
  + Use the green arrow pointing upwards to *Push* your local files to "origin". 

13. Confirm with your group-mates that everyone was able to push their files to "origin".
14. If everyone was successful, make one last *Pull* to bring everyone else's notebook files in to your local repository.

And now we're ready to collaborate! See you in the next notebook.