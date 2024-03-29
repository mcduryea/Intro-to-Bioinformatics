---
title: "Intro to R for Bioinformatics"
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
✔ ggplot2 3.3.5     ✔ purrr   0.3.4
✔ tibble  3.1.7     ✔ dplyr   1.0.9
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


## Activity Objectives

Now that we have some exposure to data analysis in R, let's get our first exposure to some Bioinformatics work. Our objective is simple -- count the number of each nucleotide within a genome. Through this notebook, we'll gain familiarity with the following:

### R for Bioinformatics Objectives

+ Use the arrow (`<-`) operator to store objects in variables.
+ Use R's `sample()` function to create a random "genome" for the purpose of testing functions.
+ Build functions to process and analyze genetic data.
+ Learn about `for` loops and implement them when necessary.
+ Read in a real bacterial genome from a text file.
+ Analyse the genome using the ideas and tools developed in the first part of the notebook.

### Getting Started with R for Bioinformatics

Let's start with lists of strings -- it will be really useful to be able to generate our own (fake) genomes. This will help us experiment with code on small examples, without the need to utilize an entire genome. Even a small, bacterial genome is typically over a thousand nucleotides long! The ability to generate strings of nucleotides will also help us to explore how efficient (or inefficient) our code is before we choose to run it on an entire genome.

In R, lists of values are defined using the `c()` function, which *c*ombines items into a list. For example, we can create a list of instructors, called `instructors`, for this course and then print out the list as follows.


::: {.cell}

```{.r .cell-code}
instructors <- c("Duryea", "Gilbert")

instructors
```

::: {.cell-output .cell-output-stdout}
```
[1] "Duryea"  "Gilbert"
```
:::
:::


Now it is time for your first challenge. Recall that the four nucleotides are Adenine (A), Cytosine (C), Guanine (G), and Thymine (T). Use the code block below to complete your first challenge.

***

**Challenge 1:** Create a list nucleotides containing the four nucleotides and print it out to the notebook.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 1
```
:::


Did you define the list without any errors? If so, great work!

Now that you've got a list containing the four nucleotides, we're ready to generate a random genome. First, lets see a method that Dr. Duryea and Dr. Gilbert could use to randomly split up the responsibilities of preparing for each of our 32 class meetings this semester.


::: {.cell}

```{.r .cell-code}
numClasses <- 32

schedule <- sample(instructors, size = numClasses, replace = TRUE)

schedule
```

::: {.cell-output .cell-output-stdout}
```
 [1] "Gilbert" "Duryea"  "Duryea"  "Gilbert" "Duryea"  "Duryea"  "Duryea" 
 [8] "Gilbert" "Duryea"  "Gilbert" "Duryea"  "Duryea"  "Gilbert" "Duryea" 
[15] "Duryea"  "Duryea"  "Duryea"  "Duryea"  "Gilbert" "Duryea"  "Duryea" 
[22] "Duryea"  "Duryea"  "Gilbert" "Gilbert" "Duryea"  "Gilbert" "Duryea" 
[29] "Gilbert" "Duryea"  "Duryea"  "Duryea" 
```
:::
:::


Now it's your turn. Try the challenge below.

***

**Challenge 2:** Adapt the code above so that you create a random string of 15 nucleotides. Call your string `randGenome` and print it to the notebook.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 2
```
:::


Two things -- first, this genome is kind of hard to read as a list. Try running this code in the codeblock below to collapse your genome into a single string: `paste(randGenome, collapse = "")` and, secondly, compare your results with a classmate. What happened? Are you surprised?

Okay, now that you've generated a tiny random genome, let's generate something larger.

***

**Challenge 3:** Generate a random genome which is 1500 nucleotides long and use the `paste()` method to collapse it from a list down to a single string. Print your new `randGenome` to the notebook. Be sure that your new version of `randGenome` contains just the single long string, and not 1500 individual elements.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 3
```
:::


Were you able to generate a new, longer random genome? Nicely done!

You'll notice that you and your classmates are all continuing to get different results. This is fine, but sometimes we'll all want to replicate the same results, especially since we are just starting to learn how computing and bioinformatics all works! Try running the code below, and compare the results of the first few nucleotides in the random genome with your neighbors.


::: {.cell}

```{.r .cell-code}
set.seed(215)
genomeLength <- 1500

randGenome <- sample(nucleotides, size = genomeLength, replace = TRUE)

randGenome <- paste(randGenome, collapse = "")
```
:::


The additional line, `set.seed(215)`, in the code block above initializes our random number generator with the seed 215 (by the way, there's nothing special about the number 215 here -- it is Dr. Gilbert's office number). Everyone's computer is running the same random number generator, and so, if we start with the same initial value, we'll all generate the same random values!

Let's try an experiment. Much of our initial work in bioinformatics will be to read and analyse a genome. One reasonable first task would be to count the frequency of each nucleotide along a genome or a substring of a genome. Let's start by just counting the frequency of Adenine (`A`) in the random genome substring below.

***

**Challenge 4:** Set a seed using `set.seed(215)` and use it to generate a random genome (`randGenome`) consisting of 100 nucleotides, collapsed down to a single string. Once you've done this, count the frequency of Adenine (A) in the resulting "genome" by hand.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete challenge 4
```
:::


Did everyone's answer agree? We all generating the same "genome", but its unlikely that we all counted accurately. We've run into the issue of human error -- now imagine if this was a full genome, on the order of millions of nucleotides in length!

Computers are much more accurate and dependable than humans (as long as we provide them with meaningful and consistent instructions). Wouldn't it be great if we could just get the computer to count the appearances of Adenine for us?

### Looping with `for` Loops

Its time for us to introduce a programming technique called the `for` loop. In programming, a for loop allows us to run a set of instructions over and over for some predetermined number of iterations. There are many ways to run a `for` loop -- we'll encounter several throughout our course. The basic idea is this.

1. Set an iterator (this may just be a counter or it may be floating over elements in an existing list).
2. Provide the instructions to be run each time through the loop.

When the code is run, the instructions will be run once for each value of the iterator. Let's build a simple for loop that will just add up the values of the iterator. We'll set the iterator to run through the range of integer values from 1 through 10.


::: {.cell}

```{.r .cell-code}
mySum <- 0

for(i in 1:10){
  mySum <- mySum + i
  print(mySum)
}
```

::: {.cell-output .cell-output-stdout}
```
[1] 1
[1] 3
[1] 6
[1] 10
[1] 15
[1] 21
[1] 28
[1] 36
[1] 45
[1] 55
```
:::
:::


There are a few things to note here. First, we initialize the container `mySum` with a starting value of 0. Then we initialized the `for` loop with the keyword `for` and provided the condition on the iterator within a set of parentheses, ending this line with an opening bracket `{`. Once we do this, the next line becomes indented -- these indented lines are the instructions to be run each time through the loop. An additional thing to notice is that we didn't actually define `i` before using it to initialize our for loop. We just said that `i` should be `in` `1:10` -- short hand for all of the integers between 1 and 10 (including both endpoints).

***

**Challenge 5:** Initialize a variable called `myProduct` with the value 1 in the code chunk below. Write your own for loop, using an iterator `j`, which takes the integer values from 1 to (and including) 15. Each run through the loop should update `myProduct` by multiplying `myProduct` by `j` and printing out the result.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 5!
```
:::


Notice that there is nothing special about the name we give to the iterator. You could just as easily have called your iterator `steve` instead of `j` in the previous challenge -- as long as you change all of the occurrences of `j` to `steve` in the loop you wrote, you will get the same results! This is helpful because, whenever possible, we would like to use informative variable and iterator names in our code.

Alright, now that you know how to write and execute a `for` loop, let's see if we can work towards our goal of analysing a genome. If we have any hope of analysing the genome we'll need to be able to access individual nucleotides. This will allow us to access particular points in the genome to determine nucleotide frequency. Let's see if we can write a for loop to print out the individual nucleotides in a random genome.

***

**Challenge 6:** In the code block below, generate a random genome substring consisting of 10 nucleotides and use the `paste()` method to collapse your genome to a single string rather than a list. Then write a `for` loop to print out each individual nucleotide. 

*Hint.* You'll find the `str_sub()` (string subset) function to be useful here -- it takes three arguments, the `string` to be subset, the position to `start` subsetting, and the position to `end` subsetting the string. If `start` and `end` are the same, then you'll extract a single character. 

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 6
```
:::


### Programmatic Flow Control with `if`, `else if`, and `else`

Now that we are able to access and print out all of the nucleotides in our sample genome, let's pretend that we are only interested in the appearances of Adenine (for now). It's time to be introduced to another super useful programming concept -- flow control. Its common that we want to execute code (like printing out a value) only when certain conditions are satisfied. We can achieve this with `if`, `else if`, and `else` statements.

Let's generate a new random genome to work with.


::: {.cell}

```{.r .cell-code}
nucleotides <- c("A", "C", "G", "T")
genomeLength <- 10

randGenome <- paste(
  sample(nucleotides, size = genomeLength, replace = TRUE),
                   collapse = "")
print(randGenome)
```

::: {.cell-output .cell-output-stdout}
```
[1] "GTATCCATTT"
```
:::
:::


Now, consider the following loop which will only print out the occurrences of Adenine.


::: {.cell}

```{.r .cell-code}
for(i in 1:nchar(randGenome)){
  if(str_sub(randGenome, start = i, end = i) == "A"){
    print(str_sub(randGenome, start = i, end = i))
  }
}
```

::: {.cell-output .cell-output-stdout}
```
[1] "A"
[1] "A"
```
:::
:::


Notice that `print(str_sub(randGenome, start = i, end = i))` is only run *if* the current nucleotide in `randGenome` is `A`.

Its time for another challenge. Can you adapt the for loop above to count the number of occurrences of Adenine rather than printing out all of the occurrences?

***

**Challenge 7:** Adapt the for loop provided above so that it counts the number of occurrences of Adenine (A) in randGenome.

*Hint:* You'll want to initialize a counter variable to 0 before the start of your loop and increment it by one every time the nucleotide is Adenine, rather than simply printing out the nucleotide.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 7
```
:::


If you got that, then you are ready to compute the frequencies of all of the nucleotides in the genome string.

***

**Challenge 8:** Adapt the loop you just created so that it will count the frequencies of each of the four individual nucleotides.

*Hint:* You'll need a separate counter for each of the nucleotides and some additional flow control (`if/else if/else`) statements.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 8
```
:::


Now let's try this out on a real genome! 

+ Use [this link](http://bioinformaticsalgorithms.com/data/realdatasets/Replication/Vibrio_cholerae.txt) to access a full copy of the *Vibrio Cholerae*  chromosome DNA, consisting of 1,108,250 nucleotides. 
+ Use `ctrl+a` or `cmd+a` to select all of the text in the file. Then use `ctrl+c` or `cmd+c` to copy the text.
+ Open up a simple text editor, like `Notepad` or `TextEdit` and use `ctrl+v` or `cmd+v` to paste the genome into a text file.
+ In the text editor, use `File -> Save As` to save the file with a `.txt` extension.
+ You may want to create a Bioinformatics Data folder on your Desktop to keep this file and others in.
+ Once the file is saved, you can read it into R with the `scan()` function below. Be sure to update the file path to reflect the location of the file on your computer.


::: {.cell}

```{.r .cell-code}
vib_c <- scan("C:/Users/agilb/Desktop/Bioinformatics Data/VibrioCholerae.txt", what = "character", sep = NULL)
```
:::


***

**Challenge 9:** Use your code from Challenge 8 to count the frequency of each nucleotide in the *Vibrio Cholerae* chromosome.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete Challenge 9
```
:::


***

**Challenge 10:** As a final challenge, complete the following:

+ Navigate to [rosalind.info](https://rosalind.info/) and create a free user account.
+ Navigate to the [Counting DNA Nucleotides problem](https://rosalind.info/problems/dna/)
+ Make sure that your code from Challenge 9 produces the expected *Sample Output* when you use the *Sample Dataset* as your genome string.
+ Use the blue *Download dataset* button to download your challenge dataset, read it into R like you did with the *Vibrio Cholerae* genome, use your code to count the number of each nucleotide, and submit your solution.

***


::: {.cell}

```{.r .cell-code}
#Use this code chunk to complete the final challenge
```
:::


Did you get the Rosalind Challenge correct? Congratulations if you did! If you didn't, try troubleshooting your code or asking for help. 

Nice work. Although the culmination of this notebook is quite simple -- count the frequency of each nucleotide within a genome -- you've accomplished a lot (especially if this is your first exposure to computing) and you should be proud!

## Summary

In this notebook you've learned how to

+ Create a list of `nucleotides`
+ Use the `sample()` function to create a random "genome" using the characters found in the `nucleotides` list.
+ Use `paste()` with the parameter setting `collapse = ""` to collapse your random genome into a single long string of nucleotides.
+ Use `set.seed()` to set a seed for a random number generator to make your results reproducible over time and across different machines (and different researchers too).
+ Use a for loop to repeat a set of simple instructions for a pre-determined number of iterations.
+ Use programmatic flow control in the form of `if/else if/else` statements to run code only when a particular condition is satisfied.
+ Construct a `for` loop and use `if` statements to count the frequency of each of the four nucleotides in a genome string.
+ Solve a bioinformatics problem on the *rosalind* platform.