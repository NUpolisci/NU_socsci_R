# Working with Existing Data + Making Homebases for R {#ch7}

In this chapter: 

- Setting a working directory 

- Importing data and understanding different file types 

- Developing a document workflow and conventions for (re)saving data 

It is very likely that you will not create every dataset that you work with in R from scratch. In almost all cases, you will want to interact with your home computer or Cloud environments to import data into R. Doing so requires that you tell R what directory you want to pull from and that you have at least a basic understanding of what type of data file you are working with to be able to properly import the file. Furthermore, once you have manipulated data, run your analyses, or created data visualizations, you will want to save these created objects in recognizable and safe places and according to naming conventions that make sense. This chapter will show you how to do these things, as well as discuss some of the key issues to consider around file management. 


## The Working Directory and Basic Directory Commands

When working in R, your computer has the ability to pull and save files based on file pathways that you have indicated in your coding environment. The call `getwd()` will tell you where exactly this is, however you can also check this (and change it) by going to the RStudio toolbar and selecting *Preferences*. As you can see in the evaluation of the below code, the current working directory is in a file pathway related to this project. What is your base working directory when you try to run this code? 


```r
# print your current working directory 

getwd()
```

```
## [1] "/Users/sarahmoore/Library/CloudStorage/OneDrive-NorthwesternUniversity/Teaching/R_textbook_v1/nu_socscir"
```

While you could change your working directory in the Preferences menu, the call `setwd()` offers you more flexibility to change the working directory as needed. The set-up for `setwd()` will differ on the basis of your operating system, as MacOS and Windows write file paths a little differently. However, the general call is the same in both cases and very straightforward. You merely write out the file path corresponding to the directory that you would like to work in and run the call as you would any other R function. 

The command `setwd()` will maintain that working directory until you close that R session or specify otherwise. Unless you have created an R project, which we cover in more detail below, it is best practice to begin every R script with defining your working directory with the `setwd()` call and then double checking that the pathway is correct with `getwd()`. It's also best practice to double check your file path anytime you try to change the directory in the middle of a script. 

### Special Case: R Projects 

RStudio has additional features which allow for some more sophisticated project management and directory tools. Specifically, you may have seen the option to *Create a project* or simply *File $\rightarrow$ New Project...* in the File drop-down menu. An R project is a means to create a project directory within an existing local directory on your computer that automatically sets the working directory to the chosen project home folder. 
For example, when authoring this textbook, we created an R project within an existing folder for Math Camp and Political Science 403 so that all of our classroom documents and corresponding data would be readily available for compilation into the project.

Another example might be if you already have an existing dataset on your computer and you would like to run some analysis on data in that dataset. You can choose to create an R project in the same folder as the existing dataset so that you both import data, save code, and export any data or results into the same folder more seamlessly than otherwise. 

An R project is a pretty attractive option, but what are some cases that it might not be a great idea? R projects probably don't make sense if your data files are all coming from various sources outside of a single path (this sounds messy...) or if you are working in a shared folder where you don't yet want to make your code accessible to others. That being said, R projects are really flexible and allow for multiple source files (such as scripts or .Rmd files) all in one easy to locate place, so long as you remember where your original R project is stored. So all that to say, if you know that you will be working with some data and a script more than once, it might be advantageous to just make an R project out of it. 

## Importing Data of All Kinds

You are most likely used to dealing with data in the form of tables, particularly in Excel with the file extension .xlsx or .csv. However, in the world of data analysis these are only two of the many types of files that you might encounter.

Other file types may come from other statistical programs, such as .sas, .sav, or .dta from SAS, SPSS, and STATA respectively. Each of these proprietary data file types cannot be merely opened on your computer unless you find some way to read them in through a program like R. There are various ways to do this, each of them having their own advantages and we'll go over some of your options here. 

Let's take a [real dataset](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-08-30/pell.csv) from the tidy tuesdays Subreddit posted on 8/30/2022. This dataset named `pell.csv` is a list of school level Pell grant award information in the United States. For those of you unfamiliar, the Pell Grant is an award to students to pursue secondary education that make less than a set threshold amount. I've gone on to save this file from online into my current working directory. 

First, I'll double check that my working directory is where I think it is. In the case that the data is not in my immediate directory, and is maybe in a subfolder or a just an alternative location for some reason, I would need to specify this information in the file path in the call to load in the data. In this case, my working directory and the place where the data are is the sample place. So, I don't have to worry about the file path in the call. Then, I'll think of some name to assign my data, here I have chosen `pell_grants` (very boring I know, but you should pick names that are intuitive and easy to remember!), followed by the assignment operator `<-` and then begins the command. 

The command we will use here is base R's `read.csv()`. As mentioned there are all different kinds of file types, so I have just chosen a basic command that deals with the file I have. In the call, I have also specified that R should treat the top row as the header, or the variable names, given the argument `header = T`. Under this initial call, I have also re-written the call to include the file path, which I would use in the case that I was using some other working directory. 

I then can double check that the data has loaded in as planned by looking at the head of the data. 


```r
# specified working directory is the same file as the data 
getwd()
```

```
## [1] "/Users/sarahmoore/Library/CloudStorage/OneDrive-NorthwesternUniversity/Teaching/R_textbook_v1/nu_socscir"
```

```r
# call without file path
pell_grants <- read.csv("pell.csv", header= T)

# call with file path 
pell_grants <- read.csv("/Users/sarahmoore/OneDrive - Northwestern University/Teaching/R_textbook_v1/nu_socscir/pell.csv", header= T)

head(pell_grants)
```

```
##   STATE     AWARD RECIPIENT                               NAME SESSION YEAR
## 1    AK  197232.9       109          Alaska Pacific University 1999-00 1999
## 2    AK  133148.0        69 Alaska Vocational Technical Center 1999-00 1999
## 3    AK  107287.0        72            Sheldon Jackson College 1999-00 1999
## 4    AK 3425148.8      1920   University of Alaska - Anchorage 1999-00 1999
## 5    AK 2441864.0      1256  University of Alaska at Fairbanks 1999-00 1999
## 6    AK  353170.0       221  University of Alaska at Southeast 1999-00 1999
```

### Using other packages to load in data 

If you would like to retain some of the meta-data that files from programs like STATA contain, there are other options beyond base R's functions. The packages `haven`, `readr`, `rio`, or `readxl` are some examples of some of the packages that might help with loading in data. Visit [this article](https://www.computerworld.com/article/2921176/great-r-packages-for-data-import-wrangling-visualization.html) from **ComputerWorld** for a great rundown of some other packages that might be helpful toward this end. Furthermore, it's possible that you can load in data directly from a webpage, specifically somewhere like Github where people are hosting stable versions of their data.

One example of this possibility is in the package `readr`, where the `read_csv` call allows you to input the url of a dataset as the file path. Looping back to the Pell Grant data from the previous section, we can easily load in the original .csv file with the data from its original location that it was posted, on Github. As a note, here we will want to be sure to use `read_csv()`. I've opted for the name `pell_grants2` here instead just so we can ensure the data are the same.  


```r
library(readr)

pell_grants2<- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-08-30/pell.csv')
```

```
## Rows: 100474 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): STATE, NAME, SESSION
## dbl (3): AWARD, RECIPIENT, YEAR
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Here, we receive and error message about the specifications of the column types, which is a fancy part of the `readr` commands for `read_csv()`, but here we don't care so much and we can look past the error for now. Otherwise, the data have loaded in as expected and we now have some tools for putting other data into our machine. 

Ultimately, R users all have different preferences over packages that they find most efficient and reliable for loading in data. For now, what will matter is that you pick the right tool for the file type that you have. If all else fails, a quick Google search of *what package can I use to load X type of data into R?* will probably return an abundance of possibilities. 

## Workflow and Data Management Conventions 

Reliably saving data and R scripts necessitates that you come up with a transparent management system to save and update your files. Just like when we advised that you should comment on your R script so that your future self remembers what you are doing, you should name and manage your files such that your future self can distinguish certain file iterations. As you continue to add certain skills to your toolbox and amass more files, naming different subsets from totally different source datasets `dat` or `data1` is not helpful so that you or other people can meaningfully access your files at a later time. Instead, as you start to update and save files that you have tinkered with, here are some considerations that you should have in naming files for data storage: 

1. **Date:** Beginning your file name with a date is not necessary, but it does make things easier as naming with dates solves two related problems. First, files with later dates can already be identified as the latest versions on the basis of the date. Second, when you are actually searching for files, you can order your files on the basis of their name. Doing so, you can identify which files are the most recently updated. Sometimes this is more advantageous than naming version numbered because little changes don't necessitate version numbering updates or the numbering process can become convoluted.  

2. **Clear, concise names that identify the project and data type:** Names like `data_2020.dta` are not helpful if you had multiple projects that happened in 2020. Instead, name your files along the lines of the type of data you're storing (e.g. survey, election), pertinent location or entity (e.g. Colombia, Mississippi, UN), and the relevant time horizon of the data (e.g. 2016 election, panel, post-2001). 

3. **Skip the strange characters and spaces in file names:** Characters aside from the underscore, hyphen or dash, and plus sign are not widely savable by different data management systems and cloud services. So just don't use them! Instead of spaces between words, opt for _ or -. 

4. **Version control:** The first, second, or third time that you update something will not always be the last. And you will find that not all potential co-authors or colleagues are managing version control with Github. Therefore, creating naming conventions that identify the version of your data is necessary. Ideally, your naming conventions will also be accompanied by a .txt file or Word document that detail the updates made to each version if you are not also working in Github, along with comments in the R code that detail certain changes. Version control can also be further managed by the date features in your naming covered in (1). 

These points cover the basic of some considerations you will make when naming files. Whatever the case, pick a system and stick to it. Then, remember to implement it EVERY TIME that you save a new file or updated an existing one. 

Given these pointers, can you identify which of the following file names are suitable for proper data management and those that need some work? 

- 2018 Survey from Afrobarometer.csv(download)

- 2021_july21_media_survey_PANEL.csv 

- 22-08-31_ColombiaStateCapacity_Project2020v2.dta

- ANES_2018_v3.dta 

- 2022-subset: anes.csv 

### Codebooks!!!

One of the most important features of data are not even the data themselves. It's the codebooks! Codebooks are additional documents that *should always* be authored alongside a dataset. Good codebooks tell the reader about each variable: the variable's name, its labels, how the variable is measured, the variable type , and what the values of the variable mean. Without this information, data are really meaningless and not interpretable. Some codebooks will even provide frequency tables of variable values or summary statistics for each variable.

When you are downloading data that are not your own, it is always best practice to download the codebook alongside the data and then store it in the same location. Even more importantly, if you find yourself in the position that you are authoring a dataset of your own, always ensure that you write a codebook alongside it too-- even if you are the only person that will ever use the data! Making your data accessible and interpretable is important for data management and transparency in scientific decision-making, two related principles of modern scientific discovery that will be important both in and out of academia. 

If you are more interested in this discussion, especially with regard to workflow we encourage you to check out [Coding for Economists by Arthur Turrell](https://aeturrell.github.io/coding-for-economists/wrkflow-rap.html#introduction). The linked section talks about workflows and tools for transparent and reproducible analyses. 
