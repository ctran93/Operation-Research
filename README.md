# Operation Research Project

### This section is designated as an instruction for the clients to use and operate the code properly and effectively. 

## R and RStudio Installations
Since the optimal program was built in R, installations of R and RStudio are needed before any further steps. Clients could download R [here](https://cran.r-project.org/) and RStudio [here](https://posit.co/download/rstudio-desktop/). 

After installing R and RStudio, the optimal programming could be runned simply by pasting the code into a new RStudio markdown file. 

Code could be runned entirely by using the combinations of "Ctrl + Shift + Enter"

## Code Explanation and Instructions

### Libraries Loading
```r
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
library(rvest)
library(magrittr)
library(stringi)
library(sf)
library('ggplot2')
library('ggspatial')
library("rnaturalearth")
library("rnaturalearthdata")
library("dplyr")
library("rgeoda")
library("readxl")
```
The code above loads the necessary libraries for the optimization program. Some of them require to be installed manually into R, simply by following these steps:

1, In the lower right pane of RStudio, click the **Packages** tab and then the **Install** button 
