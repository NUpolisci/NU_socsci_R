# Beyond Base R: Exploring the `tidyverse` {#ch8}

In this chapter: 

- Why `tidyverse`?

- The pipe `%>%` to chain functions and saving intermediate objects

- `dplyr` for data manipulation and wrangling

- Additional `dplyr` functions for joining and transforming data 

- An alternative to `tidyverse`: `data.table`

This chapter will largely rely on [Wickham and Grolemund's R for Data Science](https://r4ds.had.co.nz/transform.html) and Northwestern's Research and Computing Service's workshop on [`tidyverse`](https://nuitrcs.github.io/r-tidyverse/html/intro.html#setup).for its material. These are great resources to revisit once you have mastered some of the basic concepts of programming in R. 


## What, why, how of `tidyverse`

Tidyverse is modern library of packages developed for modern data science. All of the accompanying packages and their underlying functions share similar design and grammar styles, meant for seamless interaction between functions among the library of packages. 

Packages in the core `tidyverse` within our scope: 

- `dplyr` is the modern toolkit for data manipulation. 

- `ggplot2` provides a framework and grammar for plotting data of different types and interacting with `dplyr` verbs to mutate data to make it more graph-able. 

- `tidyr` helps to consistently and tidily clean data. 

- `readr` a package for importing rectangular data of different file types. 

Other packages in the `tidyverse` you might encounter later: `forcats`, `stringr`, `tibble`, `purrr`, `lubridate`, `rvest`, `magrittr`. 

For more information on each of these and the greater power of the `tidyverse` check out the [webpage on these packages](https://www.tidyverse.org/packages/). 

So, why `tidyverse` over the tried and true base R? While base R is great for some things, it can also be really inefficient and difficult to combine different functions together in a meaningful workflow. The `tidyverse` package solves some of these inefficiencies given the relationships between packages in the library and the ways that `tidyverse` packages emphasize **piping**, or chaining together codes to form sophisticated chains of functions.  

Given that there is a similar language and interpretation across the `tidyverse` packages, it makes data analysis and data science a lot easier to manage, as opposed to having a multitude of different packages performing all these different steps in your code. However, the learning curve is high because there is a certain philosophy and grammar to the workflow. Navigating and employing this grammar is necessary to running `tidyverse` code that works. While this high learning curve can be confusing at times, you will thank yourself for putting the work at the front end of your programming journey because `tidyverse`'s efficiencies make up for it in the end. 

### Just one more thing! 
Before we move an inch further into this chapter, recall how to install and load packages from your R library as we went over in Chapter \@ref(ch6). If you did not install the `tidyverse` during those activities, do so now! The remainder of this chapter (and much of this text), will assume that you are working with `tidyverse` functions. After you make sure that you did install `tidyverse`, make sure that it is loaded via the `library()` function. 

Next, you will load the dataset `mtcars` that is already available in Base R. To do so merely run the function `data('mtcars')`. This is a play dataset with data from the 1974 *Motor Trend US* magazine that accounts for 11 different specifications across 32 different cars. We realize this has nothing to do with social science, **BUT** the dataset is really accessible and fairly small. There are also many examples out there using this dataset, so hopefully if you have any more questions you can find answers applied to the `mtcars` data, making it a bit more comfortable to follow along.


```r
library(tidyverse) # load the package
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
## ✔ readr   2.1.2     ✔ forcats 0.5.1
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
data("mtcars") # load the data from the package

glimpse(mtcars) # take a quick look at the data 
```

```
## Rows: 32
## Columns: 11
## $ mpg  <dbl> 21.0, 21.0, 22.8, 21.4, 18.7, 18.1, 14.3, 24.4, 22.8, 19.2, 17.8,…
## $ cyl  <dbl> 6, 6, 4, 6, 8, 6, 8, 4, 4, 6, 6, 8, 8, 8, 8, 8, 8, 4, 4, 4, 4, 8,…
## $ disp <dbl> 160.0, 160.0, 108.0, 258.0, 360.0, 225.0, 360.0, 146.7, 140.8, 16…
## $ hp   <dbl> 110, 110, 93, 110, 175, 105, 245, 62, 95, 123, 123, 180, 180, 180…
## $ drat <dbl> 3.90, 3.90, 3.85, 3.08, 3.15, 2.76, 3.21, 3.69, 3.92, 3.92, 3.92,…
## $ wt   <dbl> 2.620, 2.875, 2.320, 3.215, 3.440, 3.460, 3.570, 3.190, 3.150, 3.…
## $ qsec <dbl> 16.46, 17.02, 18.61, 19.44, 17.02, 20.22, 15.84, 20.00, 22.90, 18…
## $ vs   <dbl> 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0,…
## $ am   <dbl> 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0,…
## $ gear <dbl> 4, 4, 4, 3, 3, 3, 3, 4, 4, 4, 4, 3, 3, 3, 3, 3, 3, 4, 4, 4, 3, 3,…
## $ carb <dbl> 4, 4, 1, 1, 2, 1, 4, 2, 2, 4, 4, 3, 3, 3, 4, 4, 4, 1, 2, 1, 1, 2,…
```
**Based on this glimpse of the data, what can we discern about its dimensions and the data types?** 

## The Pipe `%>%` and Some Basic Conventions 

One of the most consequential features of programming with `tidyverse` is the piping feature. The pipe, `%>%` or `|>`, allows us to chain together functions to create meaningful function pipelines. The pipe is accessible with the `command+shift+M` on the Mac or `ctrl+shift+M` on Windows. What this means is that we can start with a basic data object, particularly a dataframe or tibble (`tidyverse`'s dataframe equivalent), and use different functions in a coordinated manner to achieve an efficient outcome with the same data. If we like a function to a sentence, we can liken a piped function to a paragraph.

While the pipe is more efficient than piecemeal function writing, it does require that we think a bit differently about *how* we program. In a pipe we think about the formula `data %>% function(...) -> output`, wherein the input is then on the LHS and our output is on the RHS, the functions we want to chain fill in the middle. In case this isn't clicking, take the example below.

Let's say we want to summarize the miles per gallon of the vehicles in the `mtcars` dataset, grouped by the number of cylinders in the car. This calls for using two functions: `group_by()` and `summarize()`. 


```r
mtcars %>% # start with the data input 
  group_by(cyl) %>%  # specify the grouping variable 
  summarize(mean_mpg=mean(mpg))-> mean_mpg # chain the function that summarizes and assign an output 

mean_mpg # print the resulting object 
```

```
## # A tibble: 3 × 2
##     cyl mean_mpg
##   <dbl>    <dbl>
## 1     4     26.7
## 2     6     19.7
## 3     8     15.1
```
This piped command assigns the calculated values to a tibble with the group means across each of the cylinder values. Also notice that we were able to name our column for the mean mpg *within* the summarize function.

Aside from managing multiple functions at once, as we go into further detail below, piping is a great option for saving intermediate objects. Intermediate objects might be subsets that have various parameters on the data, summarized parts of your data, or data that has otherwise been transformed that does not fit the existing dimensions of your data. It's likely that you will not want these auxiliary objects to be merged with your data, or it may be impossible to merge the two. The piping feature in `tidyverse` allows for a structured approach to saving objects like this for further use in your programming. 

## `dplyr` Functions for Data Wrangling 

### `filter` 

### `select`

### `mutate`

### `group_by`

### `summarize`

### Further use of %>% with `dplyr`'s manipulating functions 

## `dplyr` for merging and transforming data 

### `join` 

### What is "data transformation"? 

#### Data Transformation in `dplyr` 


