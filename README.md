# Operation Research Project

## Code Explaining 

### Importing Data

Put your file location after "root.dir =" 

```r
knitr::opts_knit$set(root.dir = 'C:/Users/Surface/Downloads/Client')
```

### Loading Libraries

```r
# For getting data
library(readxl)
# For modifying data
library(dplyr)
# For getting map data
library(ggmap)
# For getting streets data
library(osmdata)
# For Encode Spatial Data
library(sf)
# For Mapping
library(mapview)
```

### Getting data

Put your Google Maps API after "key ="
```r
getwd()
list.files()
register_google(key = )
```

### Processing Data

#### 2018 Data

```r
## 2018
Data18 <- read_excel("2018 Client List.xls")
Data18_Clean <- Data18 %>%
  select(CustomerType,
         PhysicalCity, 
         Longitude, 
         Latitude)
# Moorhead Data
Data18_M <- Data18_Clean %>%
  filter(PhysicalCity == "Moorhead")%>%
  select(CustomerType, 
         Longitude, 
         Latitude)
Data18_M_New <- Data18_M %>%
  filter(CustomerType == "Client")
Data18_M_Old <- Data18_M %>%
  filter(CustomerType == "FormerClient")
# Mapping 
Addr18_M <- st_as_sf(Data18_M, 
                     coords = c("Longitude", "Latitude"), 
                     crs = 4326)
```

***The other 4 years would have the same code, just change the name for the variables***

### Mapping 

```r
Addr18_M <- st_as_sf(Data22_M, 
                     coords = c("Longitude", "Latitude"), 
                     crs = 4326)
mapview(Addr18_M)
```

Here's how the map having all clients in 2018 looks like

![M_Zo](https://user-images.githubusercontent.com/114312864/204166579-41bddf31-db8a-401f-adb0-821ce2e49337.jpg)

If you zoom it in, it should show the exact location of each dot. 

![M_Zo](https://user-images.githubusercontent.com/114312864/204166648-e74fe886-9752-47ae-9119-a305ce1bd7e7.jpg)

Here's the map for the final file without repeated clients. The map shows the data didn't focus on the F-M area so we need to figure out what was wrong with this file. 

![M_Zo](https://user-images.githubusercontent.com/114312864/204166810-fd5fc67d-50aa-44f0-b7a8-3e1515bf8935.jpg)

I based other step on the map we have for 2020, which is this one:

![M_Zo](https://user-images.githubusercontent.com/114312864/204167139-6a65e887-9bf1-4d6e-9a3c-388d0a0e53e7.jpg)

### Streets Data and Mapping 

```r
M_streets <- getbb("Moorhead Minnesota")%>%
  opq()%>%
  add_osm_feature(key = "highway", 
                  value = c("motorway", "primary","secondary", "tertiary", "residential" )) %>%
  osmdata_sf()
  ```
  
  Then I added the streets and the data points together to have this map. 
  
  ![M_Zo](https://user-images.githubusercontent.com/114312864/204167288-3555c2fb-4dd5-4f51-8aea-525c5b37d313.jpg)
  
  Again, zooming in, it tell you exactly which dot lays in which area. Like this
  
  ![M_Zo](https://user-images.githubusercontent.com/114312864/204167493-ae355cd6-1529-4ca6-a97a-e58ed098c1a9.jpg)
  
  So what we need next is how to add these microareas into zones. I'm trying to test some of the codes I have. But I don't know. Let him see tmr then we decide next. 
  
  ## Predicting 2023 map
  
  We don't need to work on this, I'm just give it a try to see if we can predict the map for 2023 based on the data we have for previous years. 
  
  I put both the code and the map below. 
  
 ```r 
 ### 2023 Data Prediction 
## Return Rate # Maybe not needed
# Previous Years
return_r19_M <- nrow(Data19_M_Old)/nrow(Data18_M)
return_r20_M <- nrow(Data20_M_Old)/nrow(Data19_M)
return_r21_M <- nrow(Data21_M_Old)/nrow(Data20_M)
return_r22_M <- nrow(Data22_M_Old)/nrow(Data21_M)
# Expected Return Rate
exp_r_r_M <- mean(c(return_r19_M, return_r20_M, return_r21_M, return_r22_M))
# Random Return Clients
return_M <- sample_n(Data20_M, round(exp_r_r_M*nrow(Data20_M)))
# Mapping Return Clients
Addr_Return_M <- st_as_sf(return_M, 
                     coords = c("Longitude", "Latitude"), 
                     crs = 4326)

## Growth Rate
# Previous Years
grow_r19_M <- nrow(Data19_M)/nrow(Data18_M)
grow_r20_M <- nrow(Data20_M)/nrow(Data19_M)
grow_r21_M <- nrow(Data21_M)/nrow(Data20_M)
grow_r22_M <- nrow(Data22_M)/nrow(Data21_M)
# Expected Growth Rate
exp_g_r_M <- mean(c(grow_r19_M, grow_r20_M, grow_r21_M, grow_r22_M))
# Expected Number of New Clients
n_new_M <- round((exp_g_r_M - exp_r_r_M)*nrow(Data22_M))
# Expected New Clients
New23_M <- rbind(Data18_M, Data19_M,Data20_M)
New23_M <- sample_n(Data20_M, n_new_M)
# Mapping New Clients
Addr_New_M <- st_as_sf(New23_M, 
                     coords = c("Longitude", "Latitude"), 
                     crs = 4326)
 ```
 
 ![M_Zo](https://user-images.githubusercontent.com/114312864/204167914-286bce26-aac8-47f5-8364-ce038f3e78a2.jpg)


                  

