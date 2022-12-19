---
title: "Generating Hypotheses"
author: "You, Scientist"
format: html
execute:
  keep-md: true
---





## Activity Objectives

This is your first in a series of notebooks which will build to a completed analysis and inference regarding a topic of interest to you. Dr. Duryea and Dr. Gilbert have identified several biological data sets and have made them available to you. Throughout this activity, you'll practice the following skills.

+ Split an available data set into an initial exploratory set and a *test* set with which you'll test inferential claims.
+ Conduct an initial exploratory analysis using your exploratory set and use those explorations to generate hypotheses.


## Your Tasks

Please complete the following tasks using what you've learned about R and exploratory analyses so far. 

1. Look through the data sets that Dr. Duryea and Dr. Gilbert have posted. Identify the one that you'd like to work with over the next two weeks.
2. Go to GitHub and create a new repository for your analyses -- title it accordingly.
3. Initialize that repository with a `README` file and leave all of the defaults selected.
4. Click on the green *Code* icon and copy the URL for the repository.
5. Open RStudio and go to `File -> New Project`, choose to create a new project from Version Control and use *Git*. Paste your repository's URL into the URL field and then click the *Create Project* button.
6. Open a new Quarto Notebook in your project and save it using a meaningful file name.
7. Read your data into your quarto notebook.
8. Split your data into exploratory and test data using code of the following form:

::: {.cell}

```{.r .cell-code}
#install.packages("tidymodels")
library(tidymodels)

my_data_splits <- initial_split(data, prop = 0.5)

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)
```
:::


9. Look at your `exploratory_data` identify some interesting questions after you understand the variables included in your data set.

10. Conduct exploratory analyses to generate some hypotheses about the answers to your interesting questions. Write those hypotheses down.

11. Use the *Git* tab in the top-right of the RStudio window to `Pull -> Commit -> Push` your changes to your repository.

