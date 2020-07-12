# R-for-geographic-map-Session-1
This is a tutorial for rendering geographic map using R

Welcome to the training module designed for postdocs and alumni fellows in the ***Climap Africa***

programme. We will work today with the open data science tool **R**.

**Course objective**: Enhance R users for informative geographic maps rendering using R

**Notice**: Please visit the hyperlink (in blue) for installation and settings.

**Pre-requisites**: 

* Install [R](https://cran.r-project.org/bin/windows/base/) and [RStudio](https://rstudio.com/products/rstudio/download/) on Windows 7, 8 or 10. A tutorial for a beginner is [here](https://medium.com/@GalarnykMichael/install-r-and-rstudio-on-windows-5f503f708027).

* Install the following packages before the course: rgdal, mapdata, mapproj,
maps, ggplot2, ggrepel, legendMap, dplyr, scales, and ggmap. A tutorial for package installation in RStudio is [here](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781783980246/1/ch01lvl1sec11/installing-libraries-in-r-and-rstudio).

* Download the data for exercise [here](https://github.com/Yedomon/R-for-geographic-map-Session-1/blob/master/datafiles.zip)

## Course outline


1.  Working directory setting, data preparation and shapefile importation (5 mn)

2.  Rendering a basic map in R using ggplot2 (10 min)

3.  Rendering a choropleth map (20 min)

4.  Add a scale bar and North Arrow (5 min)

5.  Tips (5 min)

6.  Q & A (15 min)


# 1. Working directory setting, data preparation and shapefile importation

## 1.1. Set the working directory

The working directory is the folder named `R MAP`. Please put all the shapefiles and data in your working disrectory. To set your working directory, type:

```
setwd("C:/Users/ANGE/Documents/R MAP")
```


 
## 1.2. Clean the R environment workspace 

```
rm(list=ls())
```


## 1.3. Set shapefile path
```
mySHP="C:/Users/ANGE/Documents/R MAP"
```


## 1.4. Import the shapefile 
```
myFile=readOGR (mySHP, layer="SEN_adm1", stringsAsFactors=FALSE)
```

## 1.5. Check the class of the shapefile
```
class(myFile)
```

## 1.6. Check the variables names
```
names(myFile)
```

## 1.7. Check the regions names
```
print(myFile$NAME_1)
```



## 1.8. Library loading
```
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
```


# 2. Rendering a basic map in R using ggplot2

## 2.1. Change in dataframe format for ggplot2

```
myDF = fortify(myFile, region = "NAME_1")
```


## 2.2. Overview of the data myDF


Type: `head(myDF, 4)`


| Longitude  | Latitude | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |


## 2.3. Change long to Longitude and lat to Latitude

```
myDF = rename(myDF, Longitude = long, Latitude= lat)
```

## 2.4.Overview of the myDF


Type: `head(myDF, 4)`


| Longitude  | Latitude | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |


## 2.5. Make the basic plot

```
p <- ggplot() +
  geom_polygon(data = myDF, 
               aes(x = Longitude, y = Latitude, group = group), 
               color = "black", size = 0.25) +
  coord_map() +
  theme_minimal() +
  ggtitle("Basic map with ggplot2")
```

The map is in p

```
p

```


# 3. Rendering a choropleth map


## 3.1. Import the data we want to plot on the map.Here that is the production of pearl millet per region

```
mydata = read.csv("production_data.csv", header=TRUE, sep=";")
```

## 3.2. Import the the regions names for annotation step

```
mydata1 = read.csv("region_names.csv", header=TRUE, sep=";")
```



## 3.3. Overview of the data mydata

Type:`head(mydata, 4)`

| Longitude  | Latitude | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |

## 3.4. Overview of the data mydata1

Type: `head(mydata1, 4)`

| Longitude  | Latitude | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |


## 3.4. Join the data and the shapefle 
```
plotData <- left_join(myDF, mydata)
```


**Key point: Note that myDF and mydata has id as a common variable**


## 3.5. Overview of plotData

Type: `head(plotData)`

You will get this table

| Longitude  | Latitude | order | hole  | piece | id    | group   |
| ---------- | -------- | ----- | ----- | ----- | ----- | ------- |
| \-17.16056 | 14.89375 | 1     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16004 | 14.89333 | 2     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.16000 | 14.89335 | 3     | FALSE | 1     | Dakar | Dakar.1 |
| \-17.15683 | 14.89042 | 4     | FALSE | 1     | Dakar | Dakar.1 |



## 3.6. Make the plot 

```

p <- ggplot() +
  
  
  geom_polygon(data = plotData, 
               aes(x = Longitude, 
                   y = Latitude, 
                   group = group,
                   fill = Production), 
               color = "black", size = 0.25) +
  
  
  coord_map()+
  
  
  
  scale_fill_distiller(palette = "Greens",direction=1) +
  
  
  geom_point(data=mydata1, 
             aes(x=long, y=lat), 
             shape = 21,
             fill = "white",
             size = 3, 
             color = "black") +
  
  
  geom_label_repel(data=mydata1, 
                   aes(x=long, 
                       y=lat, 
                   label=Region),
                   fontface = 'bold', 
                   color = 'black',
                   box.padding = 0.35, 
                   point.padding = 0.5,
                   segment.color = 'grey10')+
  
  
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

```
p

```

That is it!

# 4. Add scale bar and north arrow 

```
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
  
  scale_bar(lon = -12, lat = 16, 
            distance_lon = 40, distance_lat = 10, 
            distance_legend = 25, dist_unit = "km", 
            arrow_length = 10, arrow_distance = 50, arrow_north_size = 6)+
  
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

```
p

```


**Export a high quality map** by typing:

**PDF format**

```
ggsave(p, file = "carte.pdf", limitsize = FALSE, width = 12, height = 10.5, dpi=500 )

```

**PNG format**
```
ggsave(p, file = "carte.png", limitsize = FALSE, width = 10, height = 6.5, type = "cairo-png", dpi=500)

```
# 5. Tips

To find out a desirable position for scale bar or any adjustment, it is possible to plot the map with the basic R by typing:

```
plot(myFile, axes=T, col="aliceblue")
```

and then typing:



```
locator(n=2)  # 2 is just an example. You can define many number as much as possible

```

and using your mouse, click on the position you want. You will get the coordinates.


# 6. Q & A

