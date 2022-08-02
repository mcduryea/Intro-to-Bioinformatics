---
title: "Bioinformatics Software Setup"
author: 
  - "Dr. Gilbert"
  - "Dr. Duryea"
format: html
execute:
  keep-md: true
---



## Welcome

Welcome to BIO422, Bioinformatics. This is a course that will pull together your prior learnings in Biology, Mathematics, Computing, and Communication. If you don't feel as though you are *skilled* in all of these areas, don't worry! This course typically enrolls students from across the sciences, math, and computing (Computer Science, IT, CIS, etc.). Dr. Duryea and I will make sure to build teams in such a way that every team has at least one scientist and at least one person who has computing experience. You'll not only get to leverage each other's expertise, but also to learn from one another as well.

There's quite a bit of software to set up for our course. We hope that most of this setup can be done outside of class time, but Dr. Duryea and Dr. Gilbert will be available to help troubleshoot any issues that arise. Much of this document is summarized from the [Happy Git with R](https://happygitwithr.com/) book. As that book mentions, all of this work is well worth it in the end -- just stick with it and reach out for help when you need it.

Setup instructions for Windows, macOS, and linux follow. For convenience, if you prefer to follow video setup instructions, those are below.

+ Full Windows Setup (link to come, Adam will record)
+ Full mac setup (link to come, Katie will record)
+ Full (Ubuntu) linux setup (link to come, Adam will record)

## Software

Modern Biological datasets are large -- too large to process by hand. For this reason, we'll be using software to interact with data in our course. We'll be using a full suite of professional tools which will give you the ability to process biological and genetic data, create professional looking and \[more importantly\] reproducible reports, and to collaborate using version control systems.

### Technical Computing with R

The primary language we'll use to interact with data in our course is called R. It is a computing language which was developed *for* data analysis.

**Windows:**

-   Install R
    -   Navigate to [CRAN, the Comprehensive R Archive Network](https://cran.r-project.org/){target="_blank"}.
    -   Click on the *Download R for Windows* link.
    -   Click the link to *install R for the first time*.
    -   Click the link in the grey rectangle to download the most recent version of R. As of current writing, this is *R-4.2.0 for Windows*.
    -   Once the download has completed, open the setup file and complete the installation by accepting all of the default settings.
-   Install Rtools
    -   Navigate back to CRAN and click on the *Download R for Windows* link again.
    -   The fourth hyperlink in the list at the top of the page is for `Rtools` -- you'll need this to build some packages which depend on C++ (another laguage, but one which we won't directly use).
    -   Click the *Rtools* hyperlink.
    -   Click the *RTools4.2* link.
    -   Click the *Rtools42 installer* link in the center of the page.
    -   Once the download has finished, open the installer and complete the installation by accepting all of the default settings.

**macOS:**

-   Install R
    -   Navigate to [CRAN, the Comprehensive R Archive Network](https://cran.r-project.org/){target="_blank"}.
    -   Click on the *Download R for macOS* link.
    -   Look for the *Latest release* heading and download/install the most recent version of R. At the time of writing this is *R-4.2.0.pkg*.

**Linux:**

-   Install R
    -   Navigate to [CRAN, the Comprehensive R Archive Network](https://cran.r-project.org/){target="_blank"}.
    -   Click on the link for your Linux Distribution next to *Download R for Linux*.
    -   Open a terminal and follow the instructions provided on the page.

### Reproducible Reporting with RStudio and Quarto

We'll be using RStudio to help us use R. *RStudio is to R as Microsoft Word is to English* -- that is, we can write R code in RStudio, just as you may have written a paper in Microsoft Word prior to this course. Follow the steps below to install RStudio.

-   Install RStudio
    -   Navigate to [RStudio.com](https://www.rstudio.com/products/rstudio/download/)
    -   Choose *Rstudio Desktop*
    -   The website will automatically detect your Operating System. Click the *Download RStudio for \[Your OS\]* button.
    -   Open the installer and complete the installation while accepting all of the defaults.

In RStudio, we'll be building markdown documents which allow us to mix R code and text into a reproducible report document. The flavor of markdown we'll be using is called *Quarto*. Follow the steps below to install Quarto notebooks.

-   Install Quarto
    -   Navigate to [Quarto.org](https://quarto.org/docs/get-started/)
    -   Click on the download link which corresponds to your Operating System.
    -   Once the download has completed, open the installer and complete the installation by accepting all of the defaults.
-   Obtain and Test the Hello Quarto notebook
    -   Click the RStudio icon under Step 2.

    -   Skip Step 1 on this page -- you've already installed RStudio.

    -   Open RStudio on your computer -- the icon is a light blue orb with a white "R" in the center.

    -   In the console (the left or lower-left pane of RStudio), type `install.packages("tidyverse")` and hit `Enter/Return` to run the command.

        -   Lots of black, blue, and red text will splash to the console. The installation will be complete once your blue prompt ([ \> ]{style="color:blue;"}) returns to the bottom of the console pane.

    -   In the console, type `install.packages("palmerpenguins")` and hit `Enter/Return` to run the command. Again, wait for the blue prompt to return.

    -   Back on the Quarto website, click to `Download hello.qmd` under item 3.

    -   Open the `hello.qmd` file in RStudio.

    -   Click the blue arrow button above the text editor in the top-left pane of RStudio to render the document.

        -   The rendered notebook should now be visible in the viewer pane -- the lower-right pane in RStudio.

Let Dr. Duryea or Dr. Gilbert know if you are having trouble rendering the notebook.

### Collaborative Version Control with git/GitHub

Have you ever found yourself working on a project and keeping files like `myBioProject_version1.doc`, `myBioProject_version2.doc`, `myBioProject_final_version.doc`, `myBioProject_RealFinal_version.doc`, etc? If so, you've been using a rudimentary version control system. This method is okay when you're working on a project by yourself, but it gets very messy once you start collaborating with other team members on a group project. 

In working with groups, you either end up e-mailing lots of files back and forth (very messy), or switching to a collaborative platform such as Google Docs. It is easier to collaborate on Google Docs, but it is also pretty dangerous to do so -- someone can accidentally remove an important section of a report, or you may want to revert back to a version of the report after receiving some feedback. Unless you've explicitly saved the relevant earlier version of the file, you may not be able to return to it.

Collaborative version control with `git` is a more advanced and more reliable strategy for version control than the Google Docs method. We'll be hosting our projects on gitHub, and we'll mostly use RStudio for pushing changes to our group gitHub repositories. 

**Register a GitHub Account:** Complete the steps below to register a GitHub account.

+ Navigate to [GitHub.com](https://github.com/)
+ Add your preferred e-mail address and hit the green *Sign Up for GitHub* button.  
+ Follow the prompts to obtain an account.  
  + [Here's some advice](https://happygitwithr.com/github-acct.html) from *Happy Git with R* on choosing a GitHub username.


**Install git (Windows):** Complete the following steps to install `git` on your local machine.  

+ Navigate to [gitforwindows.org](https://gitforwindows.org/)
+ Click the blue *Download* button.
+ Follow the prompts, accepting all of the default settings. Be sure that “Git from the command line and also from 3rd-party software” is selected when asked about *Adjusting your PATH environment*.

**Install git (MacOS):** Complete the following steps to install `git` on your local machine.

+ Open RStudio on your computer.
+ Click on the Terminal tab in the bottom left window to switch from the Console to the Terminal.
+ Into the Terminal, enter the command `xcode-select --install` and hit enter.
+ Agree to installing Command Line Developer Tools and the Xcode license agreement.
+ After the install is complete, type `git --version` into the Terminal and hit enter. If the install was successful, it will display your version of git.

**Linking git, GitHub, and RStudio:** Complete the following steps to allow `git`, `GitHub` and `RStudio` to work together.  

+ Open up RStudio
+ In the console, type and run `install.packages("usethis")`
+ Once the installation has completed, run `library(usethis)` and then `use_git_config(user.name = "INPUT_YOUR_USER_NAME_HERE", user.email = "INPUT_YOUR_EMAIL_ADDRESS_HERE")` -- be sure to use the username and e-mail associated with your GitHub account.
+ Run `create_github_token()` from the console.  
  + This opens up a GitHub webpage -- fill in the form. Change the token expiration to *No Expiration*.
  + Click *Generate Token*.
  + Use the clipboard button to copy the PAT token -- you may want to save this in a simple text file somewhere on your computer.
+ In the console, run `install.packages("gitcreds")`
+ Once the install has finished, run `gitcreds::gitcreds_set()`  
  + Paste the PAT token from GitHub into the console, next to the prompt, and hit `Enter/Return`.

**Check that Everything Worked!** We're finally ready to check that everything has worked. Complete the following steps.  

+ Open RStudio
+ Click `File -> New Project`
+ Do you see an option to create from `Version Control`? If so, excellent! Don't click on it just yet though.
+ Click on the `New Directory` button and then click `New Project`.  
  + On this next page do you see a checkbox to *Create a git Repository*? If so, we're all set!
  + Don't follow through with creating the new project. Instead, just close out of this process.
  
## Summary

That was a lot of work, and a lot of software to install! Good news though, the only software you'll *notice* interfacing with on a regular basis is RStudio. All of the other items we've installed will be controlled there.

Next time, you'll create your first GitHub repository, clone it to your local machine (create a copy of it on your computer), attach an R project within that repository, create a quarto document, conduct an analysis, build your report, and push updates out to your main repository on GitHub.