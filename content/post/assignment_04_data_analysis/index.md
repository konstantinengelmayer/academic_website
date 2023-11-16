---
title: "Assignment 04 - Data analysis"
author: "Konstantin Engelmayer"
date: 2023-11-16
categories: ["Data Analysis"]
tags: ["R Markdown", "R"]
---

------------------------------------------------------------------------

## Unmarked Assignment: Cleaning Crops

------------------------------------------------------------------------

In this assignment we read and clean our data to prepare it for data
analysis. First we take a look at the data:
![Feldfruechte](../../data/feldfruechte.png)

### 1. Problems to solve

1.  Skip first 6 and last 5 rows of the file
2.  Set right encoding to read Umlaute properly
3.  Read file with the first 3 column names missing
4.  Identify the missing values and replace them with NA
5.  Divide column 3 into 3 different columns
6.  Fill up the 3 different columns with missing information like “Land”
    and “Landkreis”
7.  Set a new order of the columns and sort the data frame by id and
    year

------------------------------------------------------------------------

### 2. Problem solving

We try to read the data for the first time. Also we save `nrow(data)` so
that we know how many rows there are in the data frame to skip the last
5.

    data <- read.csv2(file = "../../data/115-46-4_feldfruechte.txt", header = TRUE, skip = 6, encoding = 'latin1', col.names = c("Year", "ID", "Location", "Winter_Wheat", "Rye_and_Winter_Mixed_Grain", "Winter_Barley", "Spring_Barley", "Oats", "Triticale", "Potatoes", "Sugar_Beets", "Winter_Rapeseed", "Silage_Maize"))
    nrows <- nrow(data)

Now we can open the data view tool of r studio and sort the column by
their values to find the `na.strings`. There you will find “.”, “-” and
“/”. Now we read the data again. With `str(data)` we check if r read the
data correctly.

    data <- read.csv2(file = "../../data/115-46-4_feldfruechte.txt", header = TRUE, skip = 6, encoding = 'latin1', na.string = c(".", "-", "/"), nrows = nrows - 5, col.names = c("Year", "ID", "Location", "Winter_Wheat", "Rye_and_Winter_Mixed_Grain", "Winter_Barley", "Spring_Barley", "Oats", "Triticale", "Potatoes", "Sugar_Beets", "Winter_Rapeseed", "Silage_Maize"))
    str(data)

    ## 'data.frame':    8925 obs. of  13 variables:
    ##  $ Year                      : int  2015 2015 2015 2015 2015 2015 2015 2015 2015 2015 ...
    ##  $ ID                        : chr  "DG" "01" "01001" "01002" ...
    ##  $ Location                  : chr  "Deutschland" "  Schleswig-Holstein" "      Flensburg, Kreisfreie Stadt" "      Kiel, Landeshauptstadt, Kreisfreie Stadt" ...
    ##  $ Winter_Wheat              : num  81.5 100.3 94 99.2 99.5 ...
    ##  $ Rye_and_Winter_Mixed_Grain: num  56.6 79 0 73.9 84.7 79.7 87.5 76.9 79.6 77.4 ...
    ##  $ Winter_Barley             : num  76.9 101.7 94.5 100.7 116.8 ...
    ##  $ Spring_Barley             : num  54.2 59.1 0 0 0 0 0 0 0 0 ...
    ##  $ Oats                      : num  45.1 60.5 0 0 0 0 0 0 0 0 ...
    ##  $ Triticale                 : num  64.7 80.4 0 0 0 0 0 0 0 0 ...
    ##  $ Potatoes                  : num  438 420 0 0 0 ...
    ##  $ Sugar_Beets               : num  722 716 0 701 661 ...
    ##  $ Winter_Rapeseed           : num  39.1 42.6 41.1 45.1 44.8 36 44.1 40 42.7 45.2 ...
    ##  $ Silage_Maize              : num  414 406 390 450 465 ...

-   `read.csv2()` reads files with following default settings (e.g
    seperator = “,”, dec = “,”)
-   `skip = 6` skips the first 6 rows
-   `na.strings = c(".", "-", "/")` replaces missing values with NA´s
-   setting the column names while reading the data prevents error while
    reading the data with 3 missing column names
-   `encoding = 'latin1'` allows Umlaute
-   `nrows = nrows - 5`ignores the last 5 rows

Now we have to divide the third column into 3. We want one column to
contain the name of the place. The second column should contain
information about the administrative unit. The third column should
contain additional information like “Hansestadt”. For that we create 2
new empty rows. To dived the column we use the
`clean_administrativ_area()` function, we created in the
function-script. Lastly we set the column in the right order and sort
the rows by the ID and year.

    source("functions.R")
    data$administrative_unit <- NA
    data$additional_info <- NA
    data <- cleaning_administrativ_area(data, data$Location)
    data <- data[,c(2,1,3,14,15,4,5,6,7,8,9,10,11,12,13)]
    data <- data[order(data$ID,data$Year), ]
    rownames(data) <- 1:nrow(data)

<details>
<summary>
Click here to view the code for the
<code>cleaning\_administrative\_area()</code> -function
</summary>

    #A function to clean administrative area data in a data frame
    cleaning_administrativ_area <- function(dataframe, column){
      
      # Split the specified column on commas
      split_data <- strsplit(column, ",")
      
      #create a matrix out of the split_data
      extracted <- t(sapply(split_data, function(x) x[1:3]))
      
      # Convert the matrix to a data frame
      df <- as.data.frame(extracted)
      names(df) <- c("one", "two", "three")

      
      # Identify rows where the third column is not NA for swapping
      swap_idx <- !is.na(df$three)
      
      # Perform the swap of 'two' and 'three' where the third column is not NA
      temp <- df$two[swap_idx]
      df$two[swap_idx] <- df$three[swap_idx]
      df$three[swap_idx] <- temp
      
      # Fill missing values in 'two' based on how far the place is idented
      df$two[is.na(df$two) & regexpr("\\S", df$one)==1] <- "Land"
      df$two[is.na(df$two) & regexpr("\\S", df$one)==3] <- "Bundesland"
      df$two[is.na(df$two) & regexpr("\\S", df$one)==7] <- "Landkreis"
      
      # Trim leading whitespace from 'one', 'two', and 'three'
      df$one <- sub("^\\s+", "", df$one)
      df$two <- sub("^\\s+", "", df$two)
      df$three <- sub("^\\s+", "", df$three)
      
      # Update the original dataframe with the new columns
      dataframe$Location <- df$one
      dataframe$administrative_unit <- df$two
      dataframe$additional_info <- df$three
      
      # Return the modified dataframe
      return(dataframe)
    }

</details>

------------------------------------------------------------------------

### 3. Resulting data frame


    head(data)

    ##   ID Year           Location administrative_unit additional_info Winter_Wheat
    ## 1 01 1999 Schleswig-Holstein          Bundesland            <NA>         91.9
    ## 2 01 2000 Schleswig-Holstein          Bundesland            <NA>         96.5
    ## 3 01 2001 Schleswig-Holstein          Bundesland            <NA>         98.4
    ## 4 01 2002 Schleswig-Holstein          Bundesland            <NA>         81.6
    ## 5 01 2003 Schleswig-Holstein          Bundesland            <NA>         86.4
    ## 6 01 2004 Schleswig-Holstein          Bundesland            <NA>         90.7
    ##   Rye_and_Winter_Mixed_Grain Winter_Barley Spring_Barley Oats Triticale Potatoes
    ## 1                       67.6          86.8          56.3 59.2      67.2    376.6
    ## 2                       67.1          81.7          54.9 53.7      71.4    379.6
    ## 3                       73.2          87.2          49.6 56.1      77.4    370.4
    ## 4                       64.9          74.4          44.4 50.2      67.2    328.9
    ## 5                       67.1          79.6          53.0 61.7      73.2    347.7
    ## 6                       69.7          84.4          51.3 61.4      72.6    402.0
    ##   Sugar_Beets Winter_Rapeseed Silage_Maize
    ## 1       543.7            39.7        378.4
    ## 2       555.3            39.5        356.8
    ## 3       538.3            41.1        385.1
    ## 4       533.7            32.0        372.3
    ## 5       546.3            37.9        343.9
    ## 6       572.1            44.2        354.5
