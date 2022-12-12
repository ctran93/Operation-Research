# Operation Research Project Instruction

### This section is designated as an instruction for JT Lawn Services & Landscape to use and operate the code properly and effectively. 

## A. R and RStudio Installations
Since the optimal program was built in **R**, installations of **R** and **RStudio** are needed before any further steps. Those who need could download R **[here](https://cran.r-project.org/)** and RStudio **[here](https://posit.co/download/rstudio-desktop/)**. 

## B. Code Explanation and Instructions

The code provided to JT Lawn Services & Landscape consists two separated parts, including **[Data Ingestion](https://github.com/ctran93/Operation-Research/blob/main/clean/Data%20Ingestion.Rmd)** and **[JT Optimization](https://github.com/ctran93/Operation-Research/blob/main/clean/JT%20Optimization.Rmd)**. 

The optimal programming could be downloaded and runned simply by using the combinations of **"Ctrl + Shift + Enter"**.

### 1. Data Ingestion

The purpose of **Data Ingestion** coding file is to export and process data from both the U.S. Census Bureau Database in order to use it for running the **JT Optimization** coding file. 

The data is collected for the Fargo-Moorhead area from the U.S. Census Bureau Database by using the library/package **tidycensus**. This package, however, requires obtaining a **Cencus API Key** before exporting any data from the database. 

**Cencus API Key** could be registered **[here](http://api.census.gov/data/key_signup.html)**. After activating the key, please supply it into the function **census_api_key()**, which could be found at **line 18** of the **Data Ingestion** file. 

For example, if the Cencus API Key is "1363e3bbjncdcsjbvggaacd", the function in line 18 then will be:

```r
census_api_key('1363e3bbjncdcsjbvggaacd')
```

After registering the Cencus API Key and supplying it into the file, the code should operate smoothly without error. 

### 2. JT Optimization

The purpose of **JT Optimization** is to use the data collected from the **Data Ingestion** file and operate an optimization program using **Clustering Algorithms** to find the optimal solution for the zones distributing problem. 

The **JT Optimization** code, in addition, would 

The code has been modified for minimizing the number of actions needed from JT to run the code, however, since clients data file might change, we might need to pay more attention into this. 

The following code in ***Data Ingestion*** is used for exporting data from an Excel file ***(2020 Client List.xls)***, which reports information, including names and addresses (longitude and latitude), of JT's clients in 2020. Depend on the need of JT, other clients data files might be used, for example data file for 2021, 2022, and so on. 

```r
client_data = read_excel("2020 Client List.xls")
```

In order to change the 




