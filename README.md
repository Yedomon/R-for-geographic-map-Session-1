
# R-for-geographic-map-Session-1

This is a tutorial for rendering geographic map using **R**

Welcome to the training module designed for postdocs and alumni fellows in the [**Climap Africa**](https://www.daad.de/en/the-daad/what-we-do/sustainable-development/funding-programmes/climapafrica/) programme. We will work today with the open data science tool **R**.

**Course objective**: Enhance R users for informative geographic maps rendering using R

**Notice**: Please visit the hyperlink (in blue) for installation and settings.

**Pre-requisites**: 

* Install [R](https://cran.r-project.org/bin/windows/base/) and [RStudio](https://rstudio.com/products/rstudio/download/) on Windows 7, 8 or 10. A tutorial for a beginner is [here](https://medium.com/@GalarnykMichael/install-r-and-rstudio-on-windows-5f503f708027).

* Install the following packages before the course: 
   * [rgdal](https://cran.r-project.org/web/packages/rgdal/index.html), 
   * [mapdata](https://cran.r-project.org/web/packages/mapdata/index.html),
   * [mapproj](https://cran.r-project.org/web/packages/mapproj/index.html),
   * [maps](https://cran.r-project.org/web/packages/maps/index.html), 
   * [ggplot2](https://cran.r-project.org/web/packages/ggplot2/index.html), 
   * [ggrepel](https://cran.r-project.org/web/packages/ggrepel/index.html), 
   * [legendMap](https://github.com/3wen/legendMap), 
   * [dplyr](https://cran.r-project.org/web/packages/dplyr/index.html), 
   * [scales](https://cran.r-project.org/web/packages/scales/index.html),  
   * [ggmap](https://cran.r-project.org/web/packages/ggmap/index.html),
   * [gpclib](https://cran.r-project.org/web/packages/gpclib/index.html), and
   * [rgeos](https://cran.r-project.org/web/packages/rgeos/index.html)

A tutorial for package installation in RStudio is [here](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781783980246/1/ch01lvl1sec11/installing-libraries-in-r-and-rstudio).

It is more easier to install CRAN packages at once by typing 

```ruby

install.packages(c("rgdal", "mapdata", "mapproj" ,"maps" ,"ggplot2", "ggrepel", "dplyr", "scales", "ggmap", "gpclib", "rgeos"))

```

For installation of the [legendMap](https://github.com/3wen/legendMap) package, you need to install the [remotes](https://cran.r-project.org/web/packages/remotes/index.html) package first.

```ruby

install.packages("remotes")

```

Then install [legendMap](https://github.com/3wen/legendMap)

```ruby

remotes::install_github("3wen/legendMap", force = TRUE)

```

>  Please skip this part if you do not encounter package installation troubles. Continue [here](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/README.md#1-working-directory-setting-data-preparation-and-shapefile-importation)



Based on our recent experience on the online session, some participants have some troubles regarding this warning message:


```ruby
Error in maptools::unionSpatialPolygons(cp, attr[, region]) : 
  isTRUE(gpclibPermitStatus()) is not TRUE
```

Don't worry at all!

It is easy to solve this issue.


Just run the following codes:


01. You need to install gpclib package by doing 

```ruby
install.packages("gpclib", dependencies=TRUE)
```

02. In addition you need to install an other package call rgeos

```ruby
install.packages("rgeos", dependencies=TRUE)
```

03. After you've run the previous codes, please check if the libraries are well installed:


```ruby
library(gpclib)
library(rgeos)
```

04. Then run your R code for a nice map.


* Download the data for exercise [here](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/datafiles.zip)

## Course outline


| #  | Section                                                               | Duration |
| -- | --------------------------------------------------------------------- | -------- |
| 1. | [Working directory setting, data preparation and shapefile importation](https://github.com/Yedomon/R-for-geographic-map-Session-1#1-working-directory-setting-data-preparation-and-shapefile-importation) | 5 min        |
| 2. | [Rendering a basic map in R using ggplot2](https://github.com/Yedomon/R-for-geographic-map-Session-1#2-rendering-a-basic-map-in-r-using-ggplot2)                              | 10 min   |
| 3. | [Rendering a choropleth map](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/README.md#3-rendering-a-choropleth-map)                                            | 20 min   |
| 4. | [Add a scale bar and North Arrow](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/README.md#4-add-scale-bar-and-north-arrow)                                       | 5 min    |
| 5. | [Tips](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/README.md#5-tips)                                                                  | 5 mn     |
| 6. | [Q & A](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/README.md#6-q--a)                                                                 | 15 min   |
|    | Total                                                                 | 60 min   |

# 1. Working directory setting, data preparation and shapefile importation

## 1.1. Set the working directory

The working directory is the folder named `R MAP`. Please put all the shapefiles and data in your working disrectory. To set your working directory, type:

```ruby

setwd("C:/Users/ANGE/Documents/R MAP")

```


 
## 1.2. Clean the R environment workspace 

```ruby

rm(list=ls())

```


## 1.3. Set shapefile path


```ruby

mySHP="C:/Users/ANGE/Documents/R MAP"

```


## 1.4. Import the shapefile 

```ruby

myFile=readOGR (mySHP, layer="SEN_adm1", stringsAsFactors=FALSE)

```

This result should appear


```ruby

OGR data source with driver: ESRI Shapefile 
Source: "C:\Users\ANGE\Documents\R MAP", layer: "SEN_adm1"
with 14 features
It has 9 fields
Integer64 fields read as strings:  ID_0 ID_1 

```



## 1.5. Check the class of the shapefile


```ruby
class(myFile)

```

The class should display as follows

```ruby
[1] "SpatialPolygonsDataFrame"
attr(,"package")
[1] "sp"

```



## 1.6. Check the variables names

```ruby
names(myFile)
```



The name will display as follows:

```ruby

[1] "ID_0"      "ISO"       "NAME_0"    "ID_1"      "NAME_1"    "TYPE_1"   
[7] "ENGTYPE_1" "NL_NAME_1" "VARNAME_1"


```


## 1.7. Check the regions names


```r

print(myFile$NAME_1)

```

The regions names should appear as follows:


```r

[1] "Dakar"       "Diourbel"    "Fatick"      "KÃ©dougou"   "Kaffrine"   
 [6] "Kaolack"     "Kolda"       "Louga"       "Matam"       "SÃ©dhiou"   
[11] "Saint-Louis" "Tambacounda" "ThiÃ¨s"      "Ziguinchor" 

```





## 1.8. Library loading

You can import the packages like this:


```r
library(rgdal)
library(mapdata)
library(mapproj)
library(maps)
library(ggplot2)
library(ggrepel)
library(legendMap)
library(dplyr)
library(scales)
library(ggmap)
library(gpclib)
library(rgeos)

```

Or, to load multiple packages at once, type:

```r
Packages = "rgdal", "mapdata", "mapproj" ,"maps" ,"ggplot2", "ggrepel", "legendMap", "dplyr", "scales", "ggmap", "gpclib","rgeos")

lapply(Packages, library, character.only = TRUE)

```

# 2. Rendering a basic map in R using ggplot2

## 2.1. Change in dataframe format for ggplot2

```r

myDF = fortify(myFile, region = "NAME_1")

```


## 2.2. Overview of the data myDF


Type: 

```r

head(myDF, 4)

```


| long       | lat      | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |


## 2.3. Change long to Longitude and lat to Latitude

```r
myDF = rename(myDF, Longitude = long, Latitude= lat)

```

## 2.4.Overview of the myDF


Type: 

```r
head(myDF, 4)

```


| Longitude  | Latitude | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |


## 2.5. Make the basic plot

```r

p <- ggplot() +
  geom_polygon(data = myDF, 
               aes(x = Longitude, y = Latitude, group = group), 
               color = "black", size = 0.25) +
  coord_map() +
  theme_minimal() +
  ggtitle("Basic map with ggplot2")
```

The map is in p.

```r
p

```

You should get this output:


![Fig.1](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/img/map%2001.png)



# 3. Rendering a choropleth map


## 3.1. Import the data we want to plot on the map.Here that is the production of pearl millet per region

```r

mydata = read.csv("production_data.csv", header=TRUE, sep=";")

```

## 3.2. Import the the regions names for annotation step

```r

mydata1 = read.csv("region_names.csv", header=TRUE, sep=";")

```



## 3.3. Overview of the data mydata

Type:

```r
head(mydata, 4)

```

| long    | lat   | id        | Production |
| ------- | ----- | --------- | ---------- |
| \-17.33 | 14.75 | Dakar     | 0          |
| \-16.25 | 14.75 | Diourbel  | 46231      |
| \-16.53 | 14.36 | Fatick    | 80000      |
| \-12.18 | 12.80 | KÃ©dougou | 152        |

## 3.4. Overview of the data mydata1

Type: 

```r
head(mydata1, 4)

```

| Region   | long    | lat   |
| -------- | ------- | ----- |
| DAKAR    | \-17.33 | 14.75 |
| DIOURBEL | \-16.25 | 14.75 |
| FATICK   | \-16.53 | 14.36 |
| KAOLACK  | \-16.00 | 14.00 |


## 3.4. Join the data and the shapefle 


```r
plotData <- left_join(myDF, mydata)
```


**Key point: Note that myDF and mydata has id as a common variable**


## 3.5. Overview of plotData

Type: 

```r
head(plotData)
```

You will get this table

| Longitude  | Latitude | order | hole  | piece | id    | group   | long    | lat   | Production |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- | ------- | ----- | ---------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 | \-17.33 | 14.75 | 0          |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 | \-17.33 | 14.75 | 0          |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 | \-17.33 | 14.75 | 0          |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 | \-17.33 | 14.75 | 0          |

## 3.6. Make the plot 

```r

p <- ggplot() +
  

# English: Plot the shapefile geographic information
# French: Projeter les contours geographiques du shapefile
  
  geom_polygon(data = plotData, 
               aes(x = Longitude, 
                   y = Latitude, 
                   group = group,
                   fill = Production), 
               color = "black", size = 0.25) +
  
  #-------------------------------------------------------------------
  
  #English: Map projection type. Default is "mercator". 
  #See help(coord_map() for other choice)
  
  
  coord_map()+
  
    
  #------------------------------------------------------------------
  
  
  # English: Color choice, direction = 1 means colored from lowest to highest value
  # French: Choix de couleur. direction=1 veut dire colorer par ordre croissant de valeur  
    
  scale_fill_distiller(palette = "Greens",direction=1) +
  
  
  #------------------------------------------------------------------


  # English: Plot the localization points corresponding to each region
  # French: Projeter les points geographiques de chaque region
  
  
  
  geom_point(data=mydata1, 
             aes(x=long, y=lat), 
             shape = 21,
             fill = "white",
             size = 3, 
             color = "black") +
  
  #---------------------------------------------------------------------
  
  # English: Avoid overlapping text for the points annotation
  # French: Eviter la superposition des noms de regions
  
  geom_label_repel(data=mydata1, 
                   aes(x=long, 
                       y=lat, 
                   label=Region),
                   fontface = 'bold', 
                   color = 'black',
                   box.padding = 0.35, 
                   point.padding = 0.5,
                   segment.color = 'grey10')+
  
  
 # English: Theme option
  # French: Mise forme de l'arriere plan
  
  theme_minimal() + # Simple background
  
  
  theme(panel.grid.major = 
          element_line(
            colour = "black", 
            size = 0.5, 
            linetype = "dotted")) + # Customize the grid line type, size and color
  
  theme(plot.background = 
          element_rect(
            colour = "white", 
            size = 1)) + # Customize the background line type, color and size
  
  # Add a title
  ggtitle("Map of Pearl Millet Production in Senegal (Rainy season 2017)")
  
  
  ```


### Map rendering

Just call the map variable p

```r
p
```


![Fig.2](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/img/map%2002.png)



That is it!

# 4. Add scale bar and north arrow 

```r
p <- ggplot() +
  
  
  
  geom_polygon(data = plotData, aes(x = Longitude, y = Latitude, group = group,
                                    fill = Production), color = "black", size = 0.25) +
  
  
  coord_map()+
  
  
  scale_fill_distiller(palette = "Greens",direction=1) +
  
  geom_point(
    data=mydata1, 
    aes(x=long, y=lat), 
    shape = 21,
    fill = "white",
    size = 3, 
    color = "black") +
  

  geom_label_repel(data=mydata1, aes(x=long, y=lat, label=Region),
                 fontface = 'bold', color = 'black',
                 box.padding = 0.35, point.padding = 0.5,
                   segment.color = 'grey10')+
  
       
           
  # English: Scale bar and north arrow
  # French: Ajouter le north geographique et l'echelle
  
  scale_bar(lon = -12, lat = 16, #--longitude and latitude of the scale bar position 
            
            distance_lon = 40, distance_lat = 10, #--legnth and width of each rectangle
            
            distance_legend = 25, #--distance between legend rectangles and legend texts
            
            dist_unit = "km", #-- Unit
            
            arrow_length = 10, #-- Arrow length
            
            arrow_distance = 50, #-- Arrow distance to the scale bar
            
            arrow_north_size = 6) + #-- Arrow size
  
  
  theme_minimal() +
  
  
  theme(panel.grid.major = 
          element_line(
            colour = "black", 
            size = 0.5, 
            linetype = "dotted")) +
  theme(plot.background = 
          element_rect(
            colour = "white", 
            size = 1)) +
  
  ggtitle("Map of Pearl Millet Production in Senegal (Rainy season 2017)")

```


### Map rendering

Just call the map variable p

```r
p

```





![Fig.3](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/img/map%2003.png)





**Export a high quality map** by typing:

**PDF format**

```r
ggsave(p, file = "carte.pdf", limitsize = FALSE, width = 12, height = 10.5, dpi=500 )

```

**PNG format**

```r
ggsave(p, file = "carte.png", limitsize = FALSE, width = 10, height = 6.5, type = "cairo-png", dpi=500)

```
# 5. Tips

To find out a desirable position for scale bar or any adjustment, it is possible to plot the map with the basic R by typing:

```r
plot(myFile, axes=T, col="aliceblue")
```


You will get this output





![Fig.4](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/img/map%2004.png)






and then type:



```r
locator(n=2)  

```


2 is just an example. You can define many number as much as possible.
Using your mouse, click on the position you want. You will get the coordinates.



# References

* Pebesma EJ, Bivand RS (2005). Classes and methods for spatial data in R. R News, 5(2), 9–13. https://CRAN.R-project.org/doc/Rnews/.
* Bivand RS, Pebesma E, Gomez-Rubio V (2013). Applied spatial data analysis with R, Second edition. Springer, NY. https://asdar-book.org/.
* Wickham H (2016). ggplot2: Elegant Graphics for Data Analysis. Springer-Verlag New York. ISBN 978-3-319-24277-4, https://ggplot2.tidyverse.org.






# 6. Q & A



> Scripts in this repository are licensed under General Public License v3.



> For information inquiry please email Yedomon Ange Bovys Zoclanclounon PhD Candidate (angez9914@gmail.com)





