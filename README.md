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

The above code exports cencus data for Fargo-Mooorhead area, including geometry information such as block and block group data from the U.S. Census Bureau Database. 

#### 3. Getting data previous clients record. 

```r
client_data = read_excel("2020 Client List.xls")
head(client_data)
```
The code above export data from the previous client report. he file used in the above code is **"2020 Client List.xls"**, which includes the address of the clients, longitude and latitude of the address.  

### C. Running Code 
 ```r
fargo_filter = cbind(c( -97, -97,  -96.6,  -96.6,  -97 ),c( 47.00, 46.70, 46.70, 47.00,  47.00 ))
fargo_filter_polygon = st_polygon(list(fargo_filter))
fargo_filter_polygon_sfc = st_sfc(fargo_filter_polygon, crs='NAD83')
fargo_filter_polygon_sf = st_sf(data.frame(geom=fargo_filter_polygon_sfc))
fargo_filter_polygon_sf
ggplot()+
  geom_sf(data=fargo_filter_polygon_sf)
  
contains_cass = data.frame(st_within(cass_pop_block_group$geometry,fargo_filter_polygon_sf$geometry))
contains_clay = data.frame(st_within(clay_pop_block_group$geometry,fargo_filter_polygon_sf$geometry))
contains_cass_block = data.frame(st_within(cass_pop_block$geometry,fargo_filter_polygon_sf$geometry))
contains_clay_block = data.frame(st_within(clay_pop_block$geometry,fargo_filter_polygon_sf$geometry))
filtered_cass_pop = cass_pop_block_group[contains_cass$row.id,]
filtered_clay_pop = clay_pop_block_group[contains_clay$row.id,]
filtered_cass_pop_block = cass_pop_block[contains_cass_block$row.id,]
filtered_clay_pop_block = clay_pop_block[contains_clay_block$row.id,]
ggplot() +
  geom_sf(data=filtered_clay_pop, aes(fill=value), color='red') +
  geom_sf(data=filtered_cass_pop, aes(fill=value), color='green')
ggplot() +
  geom_sf(data=filtered_clay_pop_block, aes(fill=value), color='red') +
  geom_sf(data=filtered_cass_pop_block, aes(fill=value), color='green')
  
ggplot()+
  geom_sf(data = cass_pop_block, aes(fill=cass_pop_block$value, color='orange'))+
  geom_sf(data = clay_pop_block, aes(fill=clay_pop_block$value, color='orange'))+
  coord_sf(xlim = c( -96.75,  -96.85), ylim=c( 46.90,   46.80))
ggplot()+
  geom_sf(data = cass_pop_block_group, aes(fill=cass_pop_block_group$value, color='orange'))+
  geom_sf(data = clay_pop_block_group, aes(fill=clay_pop_block_group$value, color='orange'))+
  coord_sf(xlim = c( -96.75,  -96.85), ylim=c( 46.90,   46.80))

ggplot()+
  geom_sf(data = cass_pop_block, aes(fill=cass_pop_block$value, color='orange'))+
  geom_sf(data = clay_pop_block, aes(fill=clay_pop_block$value, color='orange'))+
  coord_sf(xlim = c( -96.9535927135086,  -96.67830703065269), ylim=c( 46.93434775796007,   46.78745013835379))
ggplot()+
  geom_sf(data = cass_pop_block_group, aes(fill=cass_pop_block_group$value, color='orange'))+
  geom_sf(data = clay_pop_block_group, aes(fill=clay_pop_block_group$value, color='orange'))+
  coord_sf(xlim = c( -96.9535927135086,  -96.67830703065269), ylim=c( 46.93434775796007,   46.78745013835379))
 
#Block Group Clustering Queen
cass_weights = queen_weights(filtered_cass_pop)
data = filtered_cass_pop['GEOID']
bound = filtered_cass_pop['value']
data = data.frame(st_coordinates(data$geometry))
model_maxp_queen = maxp_greedy(w=cass_weights, df=data, bound_variable = bound, min_bound=10000)
model_azp_queen = azp_greedy(10,w=cass_weights, df=data, bound_variable = bound, min_bound=1000)
filtered_cass_pop_clustered_queen = cbind(filtered_cass_pop, maxpNum = model_maxp_queen$Clusters)
filtered_cass_pop_clustered_queen = cbind(filtered_cass_pop_clustered_queen, azpNum = model_azp_queen$Clusters)
#Block Group Clustering Rook
cass_weights = rook_weights(filtered_cass_pop)
data = filtered_cass_pop['GEOID']
bound = filtered_cass_pop['value']
data = data.frame(st_coordinates(data$geometry))
model_maxp_rook = maxp_greedy(w=cass_weights, df=data, bound_variable = data.frame(bound$value), min_bound=10000)
bound
model_azp_rook = azp_greedy(85,w=cass_weights, df=data, bound_variable=data.frame(bound$value), min_bound=5000)
model_skater_rook = skater(17,cass_weights,data)
filtered_cass_pop_clustered_rook = cbind(filtered_cass_pop, maxpNum = model_maxp_rook$Clusters)
filtered_cass_pop_clustered_rook = cbind(filtered_cass_pop_clustered_rook, azpNum = model_azp_rook$Clusters)
filtered_cass_pop_clustered_rook = cbind(filtered_cass_pop_clustered_rook, skater = model_skater_rook$Clusters)
filtered_cass_pop_clustered_rook

#Block Clustering
cass_block_weights = rook_weights(filtered_cass_pop_block)
data = filtered_cass_pop_block['GEOID']
bound = filtered_cass_pop_block['value']
data = data.frame(st_coordinates(data$geometry))
model_maxp_block = maxp_greedy(w=cass_block_weights, df=data, bound_variable = bound, min_bound=1000000)
model_azp_block = azp_greedy(170,w=cass_block_weights, df=data, bound_vals = bound, min_bound=1000)
filtered_cass_pop_block_clustered = cbind(filtered_cass_pop_block, maxpNum = model_maxp_block$Clusters)
filtered_cass_pop_block_clustered = cbind(filtered_cass_pop_block_clustered, azpNum = model_azp_block$Clusters)

ggplot() +
  geom_sf(data=filtered_cass_pop_clustered_queen, aes(fill=factor(maxpNum)))
ggplot() +
  geom_sf(data=filtered_cass_pop_clustered_queen, aes(fill=factor(azpNum)))
ggplot() +
  geom_sf(data=filtered_cass_pop_clustered_rook, aes(fill=factor(maxpNum)))
ggplot() +
  geom_sf(data=filtered_cass_pop_clustered_rook, aes(fill=factor(azpNum)))
ggplot() +
  geom_sf(data=filtered_cass_pop_block_clustered, aes(fill=factor(maxpNum)))
ggplot() +
  geom_sf(data=filtered_cass_pop_block_clustered, aes(fill=factor(azpNum)))
  
sum(filtered_cass_pop_clustered_rook$value)
filtered_cass_pop_clustered_rook %>% 
  group_by(maxpNum) %>%
  summarise(sum=sum(value))
filtered_cass_pop_clustered_rook %>% 
  group_by(azpNum) %>%
  summarise(sum=sum(value))
```
