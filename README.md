# Operation Research Project Instruction

### This section is designated as an instruction for the clients to use and operate the code properly and effectively. 

## R and RStudio Installations
Since the optimal program was built in **R**, installations of **R** and **RStudio** are needed before any further steps. Clients could download R **[here](https://cran.r-project.org/)** and RStudio **[here](https://posit.co/download/rstudio-desktop/)**. 

After installing R and RStudio, the optimal programming could be runned simply by pasting the code into a new RStudio markdown file. 

Code could be runned entirely by using the combinations of **"Ctrl + Shift + Enter"**.

## Code Explanation and Instructions

### A. Libraries Loading
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

![Pic](https://user-images.githubusercontent.com/114312864/206927176-7b2bd78a-7b95-46a9-9d7f-299ae47f2f09.jpg)

2, After a pop up box appears, type the name of the package/library into the **"Packages (separate multiple packages with a space or comma):"** section, click **Install*

![Pic](https://user-images.githubusercontent.com/114312864/206927109-28540b71-3d6b-42b9-ad13-1d725616e33a.jpg)

After these two steps, the package/library will be installed into R, and the code is ready to operate. 

### B. Getting data 

#### 1. API Key Registration
```r
library(tidycensus)
options(tigris_use_cache = TRUE)
census_api_key('[API Key]', install = TRUE, overwrite = TRUE)
readRenviron("~/.Renviron")
```

The code above exports data from the U.S. Cencus Database by using the **Tidycencus** package. An API Key is needed at this step. API Key could be registered [here](https://walker-data.com/tidycensus/reference/census_api_key.html)

After activating the API key, insert in into the following command line:

```r
census_api_key('[API Key]', install = TRUE, overwrite = TRUE)
```

For example, using **"123456789"** as an API Key, the command line then would be:

```r
census_api_key('123456789', install = TRUE, overwrite = TRUE)
```

#### 2. Getting data from U.S. Cencus Database

```r
cass_pop_block <- get_decennial(
  geography = "block", 
  variables = "P1_001N",
  state = "ND", 
  year = 2020,
  geometry = TRUE,
  county="cass",
  show_call = TRUE
)
clay_pop_block <- get_decennial(
  geography = "block", 
  variables = "P1_001N",
  state = "MN", 
  year = 2020,
  geometry = TRUE,
  county="clay"
)
cass_pop_block_group <- get_decennial(
  geography = "block group", 
  variables = "P1_001N",
  state = "ND", 
  year = 2020,
  geometry = TRUE,
  county="cass"
)
clay_pop_block_group <- get_decennial(
  geography = "block group", 
  variables = "P1_001N",
  state = "MN", 
  year = 2020,
  geometry = TRUE,
  county="clay"
)
summary(clay_pop_block)
```
