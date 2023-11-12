---
title: "Assignment 01 - Data analysis"
author: "Konstantin Engelmayer"
date: 2023-10-31
categories: ["Data Analysis"]
tags: ["R Markdown", "R"]
---


## Marked Assignment: Hello R, Hello GitHub 

------------------------------------------------------------------------

#### 1. Assign the value of five to a variable called `a` and the value of two to a variable called `b`.

    a <- 5
    b <- 2

------------------------------------------------------------------------

#### 2. Compute the sum, difference, product and ratio of a and b (a always in the first place) and store the results to four different variables called `r1`, `r2`, `r3`, and `r4`.

    r1 <- a+b
    r2 <- a-b
    r3 <- a*b
    r4 <- a/b

------------------------------------------------------------------------

#### 3. Create a vector `v1` which contains the values stored within the four variables from step 2.

    v1 <- c(r1,r2,r3,r4)

------------------------------------------------------------------------

#### 4. Add a fifth entry to vector `v1` which represents `a` by the power of `b` (i.e. `a**b`).

    v1 <- c(v1,a**b)

------------------------------------------------------------------------

#### 5. Show the content of vector `v1` (e.g. use the `print` function or just type the variable name in a separate row).

    print(v1)

    ## [1]  7.0  3.0 10.0  2.5 25.0

------------------------------------------------------------------------

#### 6. Create a second vector `v2` which contains information on the type of mathematical operation used to derive the five results. Hence this vector should have five entries of values *sum, difference*,…

    v2 <- c("sum", "difference", "product", "ratio", "exponentiation")

------------------------------------------------------------------------

#### 7. Show the content of vector `v2`.

    print(v2)

    ## [1] "sum"            "difference"     "product"        "ratio"          "exponentiation"

------------------------------------------------------------------------

#### 8. Combine the two vectors `v1` and `v2` into a data frame called `df`. Each vector should become one column of the data frame so you will end up with a data frame having 5 rows and 2 columns.

    df <- data.frame(v1,v2)

------------------------------------------------------------------------

#### 9. Make sure that the column with the data of `v1` is named Results and `v2` is named Operation.

    colnames(df) <- c("Results", "Operation")

------------------------------------------------------------------------

#### 10. Show the entire content of `df`.

    print(df)

    ##   Results      Operation
    ## 1     7.0            sum
    ## 2     3.0     difference
    ## 3    10.0        product
    ## 4     2.5          ratio
    ## 5    25.0 exponentiation

------------------------------------------------------------------------

#### 11. Show just the entry of the cell in the second row and first column.

    print(df[2,1])

    ## [1] 3

------------------------------------------------------------------------
