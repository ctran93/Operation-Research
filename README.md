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
census_api_key("1363e3bbjncdcsjbvggaacd")
```

After registering the Cencus API Key and supplying it into the file, the code should operate smoothly without error. 

### 2. JT Optimization

The purpose of **JT Optimization** coding file is to use the data collected from the **Data Ingestion** file and operate an optimization program using **Clustering Algorithms** to find the optimal solution for the zones distributing problem. The code is 

The **JT Optimization** code, in addition, would also uses data from an Excel file ***(2020 Client List.xls)***, which reports information, including names and addresses (longitude and latitude), of JT's clients in 2020. Depend on the need of JT, other clients data files might be used, for example data file for 2021, 2022, and so on. 

Command line for exporting clients data in 2020 could be found at **line 32** of the **JT Optimization** file:

```r
client_data = read_excel("2020 Client List.xls")
```

In order to change the client data file used, please just simply supply the new data file into the function **read_excel()**. For example, if JT would like to use data from 2021 instead of 2020, and the client information for 2021 is collected and saved with an Excel file named **2021 Client List.xls**, the **line 32** of the **JT Optimization** then should be:

```r
client_data = read_excel("2021 Client List.xls")
```

### Result

After running both the **Data Ingestion** and **JT Optimization** file, we should get the following map, which divides Fargo-Moorhead area into 12 different equally serviceable zones, each of them will have at least 120 houses for JT Lawn Services & Landscape to visit in a day long working shift, which equivalent to around 8 hours of work. The map will provide in detail the boundary of zones, along with name and number of clients (houses) in each. 

![mapdd](https://user-images.githubusercontent.com/114312864/206946728-58468630-cf1b-423a-88de-d4505307afa2.png)

If any problem occurs while running the code for this optimization program, or if having any question regarding of these instructions, please do not hesitate to contact us via our emails. 

Thank you! 




