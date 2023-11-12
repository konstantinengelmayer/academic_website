---
title: "Assignment 02 - Data analysis"
author: "Konstantin Engelmayer"
date: 2023-11-07
categories: ["Data Analysis"]
tags: ["R Markdown", "R"]
---

------------------------------------------------------------------------

## Unmarked Assignment: Loop and Conquer

------------------------------------------------------------------------

#### 1. Implement an if-then-else statement which prints “larger” if the number provided as variable n is larger than three and “equal or smaller” otherwise.

    check_value <- function(n){
      if(n >3){
        print("large")
      }else{
        print("equal or smaller")
      }
    }
    check_value(2)

    [1] "equal or smaller"

------------------------------------------------------------------------

#### 2. Extent a copy of the above statement (i.e. copy the entire if-then-else statement and include it a second time in your script in order to preserve both versions) which returns “equal” and “smaller” explicitly in addition to “larger”.

    check_value <- function(n){
      if(n >3){
        print("large")
      }else if(n == 3){
        print("equal")
      }else if(n <3){
        print("smaller")
      }
    }
    check_value(2)

    [1] "smaller"

------------------------------------------------------------------------

#### 3. Implement a if-then-else statement which prints “even” if the number provided as variable n is even and which prints “odd” otherwise.

    even_or_odd <- function(n){
      if(n %% 2 == 0){
        print("even")
      }else{
        print("odd")
      }
    }
    even_or_odd(5)

    [1] "odd"

------------------------------------------------------------------------

#### 4. Copy the extended larger/equal/smaller if-then-else statement and include it into a for loop which shows that all three options are actually implemented in a correct manner by iterating over n from a number which is smaller 3, exactly 3 and larger than 3.

    check_value <- function(n){
      if(n >3){
        print("large")
      }else if(n == 3){
        print("equal")
      }else if(n <3){
        print("smaller")
      }
    }
    for(n in c(2,3,4)){
      check_value(n)
    }

    [1] "smaller"
    [1] "equal"
    [1] "large"

------------------------------------------------------------------------

#### 5. Extent a copy of the above loop and modify the loop and if-then-else statement in such a way, that the information on “larger” etc. is not printed on the screen but saved within a vector (i.e. a variable which will hold all three statements in the end). Print the content of this vector after the loop.

    v1 <- c()
    check_value <- function(n){
      if(n >3){
        print("large")
      }else if(n == 3){
        print("equal")
      }else if(n <3){
        print("smaller")
      }
    }
    for(n in c(2,3,4)){
      v1 <- c(v1,check_value(n))
    }

    [1] "smaller"
    [1] "equal"
    [1] "large"

    print(v1)

    [1] "smaller" "equal"   "large"  

------------------------------------------------------------------------

#### 6. Extent a copy of the above modified loop in such a way, that the results are not saved in a vector but a list. Print the content of this list after the loop.

    l1 <- list()
    check_value <- function(n){
      if(n >3){
        print("large")
      }else if(n == 3){
        print("equal")
      }else if(n <3){
        print("smaller")
      }
    }
    for(n in c(2,3,4)){
      l1[length(l1) + 1] <- check_value(n)
    }

    [1] "smaller"
    [1] "equal"
    [1] "large"

    print(l1)

    [[1]]
    [1] "smaller"

    [[2]]
    [1] "equal"

    [[3]]
    [1] "large"

------------------------------------------------------------------------

#### 7. Change the above modified loop in such a way, that the iteration is controlled by a lapply not by a for-loop. Save the returning information from the lapply function in a variable and print the content of this variable after the loop.

    l1 <- list()
    check_value <- function(n){
      if(n >3){# testesttest
        print("large")
      }else if(n == 3){
        print("equal")
      }else if(n <3){
        print("smaller")
      }
    }
    values <- c(2,3,4)
    result <- lapply(values, check_value)

    [1] "smaller"
    [1] "equal"
    [1] "large"

    print(result)

    [[1]]
    [1] "smaller"

    [[2]]
    [1] "equal"

    [[3]]
    [1] "large"

------------------------------------------------------------------------

#### 8. Finally change the above variable (i.e. do not modify the loop anymore but just include one more line) in such a way that the content is not printed as a nested list but a vector (i.e. flatten the list).

    check_value <- function(n){
      if(n > 3){
        return("larger")
      }else if(n == 3){
        return("equal")
      }else if(n < 3){
        return("smaller")
      }
    }
    values <- c(2,3,4)
    result <- lapply(values, check_value)
    result <- unlist(result)
    print(result)

    [1] "smaller" "equal"   "larger" 

------------------------------------------------------------------------
