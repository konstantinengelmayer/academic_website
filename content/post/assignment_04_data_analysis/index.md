---
title: "Assignment 04 - Data analysis"
author: "Konstantin Engelmayer"
date: 2023-11-13
categories: ["Data Analysis"]
tags: ["R Markdown", "R"]
---

------------------------------------------------------------------------

## Unmarked Assignment: Cleaning Crops

------------------------------------------------------------------------

In this assignment we read and clean our data to prepare it for data
analysis. First we take a look at the data:
![Feldfruechte](feldfruechte.png)

### 1. Problems to solve

1.  Skip first 6 and last 5 row of the file
2.  Set right encoding to read Umlaute properly
3.  Read file with the first 3 column names missing
4.  Convert the missing values (“\_” and “.” and “/”) to NA
5.  Divide column 3 into 3 different columns
6.  Fill up the 3 different columns with missing information like “Land”
    and “Landkreis”
7.  Set a new order of the columns and sort the data frame by id and
    year

------------------------------------------------------------------------

### 2. Problem solving

    data <- read.csv2(file = "../../data/115-46-4_feldfruechte.txt", header = TRUE, nrows = 8925, na.strings = c("." , "-", "/"), skip = 6, check.names = FALSE, encoding = 'latin1')

-   read.csv2() reads files with following default settings (e.g
    seperator = “,”, dec = “,”)
-   skip = 6 skips the first 6 rows
-   na.strings = c(“.”, “-”, “/”) repleaces missing values with NA´s
-   check.names = FALSE disables the checking of the column names so it
    doesn´t produces an error with 3 missing column names
-   encoding = ‘latin1’ allows Umlaute
-   nrow = 8925 ignores the last 5 rows

------------------------------------------------------------------------

    colnames(data)[1:3] <- c("Jahr", "ID", "Ort")

-   adds column names for the first 3 columns

------------------------------------------------------------------------

    source("functions.R")
    data$Administrative_Einheit <- NA
    data$Administrative_Besonderheit <- NA
    data <- cleaning_administrativ_area(data, data$Ort)
    data <- data[,c(2,1,3,14,15,4,5,6,7,8,9,10,11,12,13)]
    data <- data[order(data$ID,data$Jahr), ]
    rownames(data) <- 1:nrow(data)

-   reads r-script of the cleaning\_administrative\_are()-function i
    created
-   creates new columns for administrative variables
-   `cleaning_administrative_area()` seperates the “Ort”- column into 3
    columns
-   sort and order the dataframe

<details>
<summary>
Click here to view the code for the
<code>cleaning\_administrative\_area()</code> -function
</summary>

    # Define a function to clean administrative area data in a dataframe
    cleaning_administrativ_area <- function(dataframe, column){
      
      # Split the specified column on commas
      split_data <- strsplit(column, ",")
      
      # Extract the first, second, and third elements from each split string
      one <- sapply(split_data, function(x) x[1])
      two <- sapply(split_data, function(x) x[2])
      three <- sapply(split_data, function(x) x[3])
      
      # Create a new data frame from the extracted elements
      df <- data.frame(one = one, two = two, three = three)
      
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
      dataframe$Ort <- df$one
      dataframe$Administrative_Einheit <- df$two
      dataframe$Administrative_Besonderheit <- df$three
      
      # Return the modified dataframe
      return(dataframe)
    }

</details>

------------------------------------------------------------------------

