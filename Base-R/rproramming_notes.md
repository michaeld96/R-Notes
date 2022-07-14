# R-Notes

## Arithmetic in R

Arithmetic in `R` is quite simple. You can run it in the Console or run it line by line in the source file.
R supports $+,-,/,\cdot,$ and even modular arithmetic. Below are some examples.

```R
1+2 #Addition.
5-2 #Subtraction.
5*2 #Multiplication.
5/2 #Division.
5 %% 2 #Modular Arthimetic.
(5*2) + (5-3) #Order of operations.
```

## Variables

When it comes to variables in R there is a different assignment operator. This is the arrow operator `<-`
Below is how to use this operator on assigning `var` the value `5`.

```r
var <- 5 #5 is now stored as the variable "var."
```

The naming convention in R is variables are either camel case, or the space between words is represented as a `.`

```r
bank.account <- 20 #"period" notation.
bankAccount <-872 #camel case.
```

## Basic Data Types

In R we have a variety of data types like `numberic, logical, character`, etc. We can find out what variables data type is by calling a function called `class()`

```r
var <- 100
trueVar <- TRUE
falseVar <- FALSE
myName <- "Michael Dick"
class(var) #outputs: numeric
class(trueVar) #outputs: logical
class(falseVar) #outputs: logical
class(myName) #outputs: character
```

## Vector Basics

Vectors in R are 1-D arrays. The unique thing about these vectors is that they can act as a dictionary in Python. To initialize a vector in R we use the syntax: `vec <- c(10, 20 ,30)` where `c` is a a function that combines all of these variables to be a part of this array. Now we can associate meaning to certain variable types. Look at the following:

```r
days <- c("Mon", "Tues", "Weds", "Thurs", "Fri", "Sat", "Sun") #Array of characters.
temps <- c(72, 55, 79, 60, 80, 70, 69) #Array of numerics.
names(temps) <- days #names assigns each item in temps a name from days
temps
```

`temps` will display the following:

```r
 Mon  Tues  Weds Thurs   Fri   Sat   Sun 
   72    55    79    60    80    70    69 
```

## Vector Indexing and Slicing

With R it is VERY IMPORTANT to remember that indexing in R starts at `1` rather than `0`. There is a few unique things we can perform on vectors. Let's look at sample code:

```r
v1 <- c(100,200,300)
v2 <- c(a,b,c) 
v1[1] #Returns 100
temps[5] #Returns  Fri 
#                   80 
temps[2:4] #Splicing
#Returns: Tues  Weds Thurs 
#           55    79    60 
temps[c(1,7)] #Grab index 1 and index 7
temps["Mon"] #Returns the number assoicated with "Mon"
temps[c("Tues", "Fri")]
#Filter out values less than 2
vnum <- c(1,2,3,4)
vchar <- c('a', 'b', 'c', 'd')
names(vnum) <- vchar
vnum[vnum > 2] #Returns 3 and 4
vnum > 2
###################################
myFilter <- vnum > 2
vnum[myFilter]
###################################
```

For the `#` section of this code block you see we assign an array of logical operations. We can call this `myFiler`. `myFilter` will contain `{FALSE, FALSE, TRUE, TRUE}` and when we run `vnum[myFiler]` and this will return all the numbers that are greater than 2. It produces the result:

```code
c d 
3 4 
```

## Getting Help in R

To get help in R with variable names, functions, etc., you can easily do this in two ways.

1. Using `help()`, in the parentheses you would add what you need help one. For example, if you wanted help with vectors you would type `help("vector")` in the Console tab in RStudio.

2. Getting help by typing `?` in front of what you want help with. Like in the last example, this is what you would use if you want to find more information on vectors `?.vectors`

## Matrices in R

When building matrices we will use the built-in function `matrix()`

## Syntax for `matrix()`

```r
matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE, dimnames = NULL)
```

`data` is what we are going to store in the matrix. By default it will store each entry as a row item. We can specify the number of rows by using `nrow` when typing the matrix function. We can specify the number of columns we want by using `ncol` when filling in the parameters of `matrix()`. `byrom` is a parameter in `matrix` that is false by default, and this organizes the data fed into the matrix by row rather than column.

### Naming the Rows and the Columns

Let's work by example. We want to create a calendar with the days we are "On," and the days we are "Off."

```r
week1 <- c("On", "On", "Off", "On", "On")
week2 <- c("Off", "Off", "Off", "On", "On")
week3 <- c("On", "On", "On", "On", "On")
week4 <- c("Off", "Off", "On", "Off", "Off")
#Creating a 1D array of variables#
month <- c(week1, week2, week3, week4)
#Naming the matrix workMonth from the 1D array "month"#
workMonth <- matrix(month, ncol = 5, nrow = 4, byrow = TRUE)
#Naming columns from the days array.#
colnames(workMonth) <- days
workweek <- c('1','2','3','4')
#Naming the rows from the workweek vector.#
rownames(workMonth) <- workweek
workMonth
```

## Adding Rows and Columns to a Matrix

There is two new ideas here: `cbind` and `rbind`. For the syntax for each of these: `rbind(matrixName, rowName)`

```r
#Exam scores#
eecs183 <- c(93,100,39,55,29,42)
eecs203 <- c(100,23,37,56,98,78)
eecs280 <- c(100,23,94,25,82,45)
eecs281 <- c(81,54,73,65,55,47)
#Putting the class name into a vector#
grades <- c (eecs183, eecs203, eecs280, eecs281)
#Making a matrix to display grades as a table or 2D array#
gradesMatrix <- matrix(grades, nrow = 4, byrow = TRUE)
#Naming rows-start#
classes <- c("EECS183","EECS203","EECS280","EECS281")
rownames(gradesMatrix) <- classes
#Naming rows-end#
#Naming cols-start#
examNum <- c(1,2,3,4,5,6)
colnames(gradesMatrix) <- examNum
#Naming cols-end#

#Displays the sum for EACH column#
colSums(gradesMatrix)
rowSums(gradesMatrix)
rowMeans(gradesMatrix)

#Adding columns and rows to a matrix#
eecs370 <- c(78,88,21,55,91)
#Matrix name is to the right and name of row is the left.#
updatedGrades <- rbind(gradesMatrix, eecs370)
#Adding a column named average#
average <- rowMeans(updatedGrades)
updatedGrades <- cbind(updatedGrades, average)
updatedGrades

```

## Factor and Categorical Matrices

Using R you can give a vector a certain order. When it comes to the idea of, say, that hot is better than cold, how can we show this? Using a function called `factor` we can assign this kind of order. Let's look at this example:

```r
animal <- c("dog", "cat", "dog", "cat", "cat")
#ID Number for dog and cat.#
idNum <- c(1,2,3,4,5)
factor(animal)
#Levels cat and dog are now assigned to animal vector.#
factorAnimal <- factor(animal)
#Nominal means there is no order. Ordinal means there is an order.

#Here is an order: cold < med < hot.
temps <- c("cold", "med", "hot", "hot", "hot", "cold", "med")
#We give factor the vector list, tell that this is ordered#
#, and now give the factor function the order that we want.#
factTemps <- factor(temps, ordered = TRUE, levels = c("cold", "med", "hot"))
factTemps
#Prints the number of times each word is present in the vector.#
summary(factTemps)
```

## Data Frames

A big difference between matrix and data frames is that data frames can be of different data types. Also, the rows and columns of a data frame can be labeled. Think of data frames as an Excel spreadsheet. Below are some data frames that are built into R-Studio.

```r
state.x77
USPersonalExpenditure
women
#To see all the datasets in R#
data()
```

Some of these data frames can be quite large. We can look at the first seven entries using `head(nameOfDataSet)`, or the last seven entries using `tail(nameOfDataSet)`.

Now, let's build some of our own data frames. We will first start out by creating some vectors. We have to make sure that all the vectors have the same length.

```r
#Making vectors.
days <- c("Mon", "Tues", "Weds", "Thurs", "Fri")
temp <- c(78,72,60,55,77)
#T means didn't rain, F means rain.
rain <- c(T,T,T,F,F)
```

Let's put these vectors in a data frame:

```r
data.frame(days, temp, rain)
#Saving this data frame into a variable named df.
df <- data.frame(days, temp, rain)
#Display of the data frame.
#   days temp  rain
# 1   Mon   78  TRUE
# 2  Tues   72  TRUE
# 3  Weds   60  TRUE
# 4 Thurs   55 FALSE
# 5   Fri   77 FALSE
```

We can run `str()` which will give us the structure of the data frame.

```r
str(df)
#Output.
# 'data.frame':5 obs. of  3 variables:
#  $ days: chr  "Mon" "Tues" "Weds" "Thurs" ...
#  $ temp: num  78 72 60 55 77
#  $ rain: logi  TRUE TRUE TRUE FALSE FALSE
```

## Data frames: Selection and Indexing

With data frames we can organize data by values. We also can return data frames by index. Here is the data frame syntax: `dataFrameName[rows,cols]`. Below are two examples how to return rows and columns.

```r
#Returns all contents in first row.
df[1, ]
#Returns all items in the first column
df[ ,1] 
```

We can return column names.

```r
#Returns everything in the rain column.
df[ ,"rain"]
#Returns all five rows and all column values in temp and days.
df[1:5, c('days','temp')]
# > df[1:5, c('days','temp')]
#    days temp
# 1   Mon   78
# 2  Tues   72
# 3  Weds   60
# 4 Thurs   55
5   Fri   77

#Returns all the items in the days' column. Returns as a vector.
df$days
# > df$days
# [1] "Mon"   "Tues"  "Weds"  "Thurs" "Fri"  

#Returns the same as above BUT returns in a data frame format.
df['days']
# > df['days']
#    days
# 1   Mon
# 2  Tues
# 3  Weds
# 4 Thurs
# 5   Fri
```

Using subsets is a great way to return data that is passing a certain parameter:

```r
#Grab the days where it doesn't rain (TRUE). We'll get a data frame where the values of rain are true.
subset(df, subset = rain == TRUE)
#syntax subset(dataframe, column arg value)
subset(df, subset = temp >= 78)

#Output:
# > subset(df, subset = rain == TRUE)
#   days temp rain
# 1  Mon   78 TRUE
# 2 Tues   72 TRUE
# 3 Weds   60 TRUE
# > subset(df, subset = temp >= 78)
#   days temp rain
# 1  Mon   78 TRUE
```

Using order will order the data frame by a certain value. Here is an example of using the `df` data frame and ordering it by the values in `temp`.

```r
sortedTemp <- order(df['temp'])
#Return sorted temps by index.
sortedTemp
# > sortedTemp
# [1] 4 3 2 5 1

#Using the sorted temp to have least temp to greatest temp by row number.
df[sortedTemp, ]
# > df[sortedTemp, ]
#    days temp  rain
# 4 Thurs   55 FALSE
# 3  Weds   60  TRUE
# 2  Tues   72  TRUE
# 5   Fri   77 FALSE
# 1   Mon   78  TRUE
```

## Overview of Data Frame Operations

In R, data frames are one of the most attractive features. We can make enormous data frames and perform all kinds of calculations, data manipulations, etc. Let's first make a empty data frame. We will name it `emptydf`, and to make a data frame we need the `data.frame()` built in R function.

```r
#Making an empty dataframe.
emptydf <- data.frame()
```

Now, let's make a data frame. We will make a vector that is 1-10, and another that are the first ten letters of the English alphabet. `letters` is a library in R that contain the twenty-six English letters of the alphabet.

```r
#Making a vector 1-10.
c1 <- 1:10
#Assigning c2 letters from the alphabet. The first ten.
c2 <- letters[1:10]
```

Naming columns are R can be done by the `data.frame()` function that is built into R. You can rename the columns by doing the following:

```r
#c1 is named col.name.1, and c2 is named col.name..2
df1 <- data.frame(col.name.1 = c1, col.name.2 = c2)
# Output:
# > df1
#    col.name.1 col.name.2
# 1           1          a
# 2           2          b
# 3           3          c
# 4           4          d
# 5           5          e
# 6           6          f
# 7           7          g
# 8           8          h
# 9           9          i
# 10         10          j
```

From the example above, we see that `c1` is associated with the first column name. So in the parameters of creating the data frame we assign `c1` to the name of `col.name.1`.

A very important feature with R is that we can read `.csv` files. We can import these `.csv` files to a data frame. For this example we are going to import the famous Iris data frame into a data frame called `df2`. We have `iris.csv` in the same working directory as our R project. Below is the syntax used to assign the `.csv` file to `df2`.

```r
#Importing .csv files
df2 <- read.csv("iris.csv")
```

We can save our data frames as a `.csv`. Let's say we want to save our first data frame `df1`. We would perform the following:

```r
#Write to a .csv! syntax is dataframe, then what the name of the file is.
write.csv(df1, file = "save_df.csv")
```

As said in the comments, the first parameter is the data frame you want to be saved as a `.csv` and the second is the name we want the file to be named. When looking at the data frame after we save it we see there is an extra `X` column. This is just the indices that were saved. We can remove these with `subset()`.

```r
#Removes the X column.
df1_copy <- subset(df1_copy, select = -X)
```

Below are simple examples on how to get the umber of rows, columns, and names of the columns.

```r
#Getting the number of rows in a data frame.
nrow(df1)
#Getting number of cols.
ncol(df1)
#Getting names of cols.
colnames(df1)
#Structure of the data frame.
str(df1)
```

With data frames we can reference a single cell and make changes to it. Let's say we want to get `e` in `df1` we would perform either `df[[5,2]]` or `df[[5,col.name.2]]`. These both say the same thing; go to row five and then column two. Now let's say we want to change `e` to a number like `616`. We can do this by the assignment operator `<-` onto the cell of the data frame.

```r
#Reference a single cell in a df.
df1[[5,2]]
#Changing value of a single cell.
df1[[5,"col.name.2"]] <- 9999
```

Large data frames we want to search for a certain column name. We can do this with the `$` operator. This acts as a `.` operator in a language like C++. For this example we will use the data frame of `mtcars`. Using the operators below will return a vector of the column.

```r
#We are going inside df and using $ to access the column name.
mtcars$mpg
#Same thing as above.
mtcars[, "mpg"]
mtcars[, 1]
```

We have options when it comes to returning a certain column as a data frame or a vector. Below will show the two methods of one set of braces versus two sets of braces.

```r
#Returns vector
mtcars[["mpg"]]
#Returns a df.
mtcars["mpg"]
#Returning multiple cols.
mtcars[c('mpg', 'cyl')]
```

Let's move into the area where we want to add two new rows to a data frame. The first column we are adding the value `2000` and in the second we are adding the string `new`.

```r
#Adding rows to a df
df2 <- data.frame(col.name.1 = 2000, col.name.2 = "new")
```

Now we are going to use the function `rbind`. This will bind this new row to an already existing data frame. We will store this new data frame in a data frame called `dfnew`.

```r
#Will use rbind
dfnew <- rbind(df1,df2)
```

Adding more columns to a data frame is done by accessing a data frame with `$`, but accessing it with the new name. Look at this example:

```r
#Doubles the first column of dfnew and saves this into
#a new column stored into dfnew.
dfnew$NEW_COL <- 2*dfnew$col.name.1
   col.name.1 col.name.2 NEW_COL
   #Output:
# 1           1          a       2
# 2           2          b       4
# 3           3          c       6
# 4           4          d       8
# 5           5       9999      10
# 6           6          f      12
# 7           7          g      14
# 8           8          h      16
# 9           9          i      18
# 10         10          j      20
# 11       2000        new    4000
```

Renaming columns can be done in two ways: many columns or one column. Let's look at both! If we are addressing multiple columns we are going to make a vector of them and use the function `colnames()` and pass into the parameters the data frame we want to rename. Renaming a single column is almost the same, but we must specify which column number it is within brackets.

```r
#Renaming columns.
colnames(dfnew) <- c("first", "second", "third")
#Selecting a certain column,
colnames(dfnew)[3] <- "NEWCOLNAME"
#Output:
#    first second NEWCOLNAME
# 1      1      a          2
# 2      2      b          4
# 3      3      c          6
# 4      4      d          8
# 5      5   9999         10
# 6      6      f         12
# 7      7      g         14
# 8      8      h         16
# 9      9      i         18
# 10    10      j         20
# 11  2000    new       4000
```

In R, there is this neat command where we can select every row or column by simply putting a `-` in-front of the row or column number.

```r
#Selecting every row but 2 thru 7.
df[-c(2:7),]
#OUTPUT:
#    first second NewColName
# 1      1      a          2
# 8      8      h         16
# 9      9      i         18
# 10    10      j         20
```

Conditional selection in data frames can be done by selecting the data frame you want and then in brackets selecting the data frame then accessing its columns with `$` then apply a logical operator. The example is showing all cars that have a mpg over 30.

```r
#Conditional Selection in dfs.
#Returns all cars that have mpg > 30.
mtcars[mtcars$mpg > 30, ]
#Returns the row where both of these conditions are true.
mtcars[mtcars$mpg > 20 & mtcars$cyl == 6, ]
#Returns specific columns.
mtcars[mtcars$mpg > 20 & mtcars$cyl == 6, c("mpg", "cyl", "hp")]
```

Now with some data frames that are imported from raw .csv files might be missing some data in certain cells. We can check if that is true by using `is.na()` function, and inside the parameters we pass in the data frame we want to use. It will return `FALSE` when the data cell is not empty and `TRUE` if it is. We can also use `any()` function; this function will return `FALSE` if all data cells are populated with data. Below are some examples with comment explanations.

```r
#Dealing with missing data.
#Detecting N/A or NULL data points. All false because no missing data point.
is.na(mtcars)
#any checks if any of these are true. If it returns false than that means
#that we have all good data points (i.e., none of them were NULL).
any(is.na(df))
#Checking columns of mtcars.
any(is.na(mtcars$mpg))
```

Let's say that we are missing some data in some data frame called `df`. Well, we can replace it by setting it to `0` or by replacing it with the mean of that column. Below are two examples. First is replacing any data point that comes back `TRUE` and replacing the `NULL` value with `0`. The other is replacing the missing data with the mean of the column of `mpg` .

```r
#Replace missing data with 0.
df[is.na(df)] <- 0
#Replace a certain column of missing data.
#If there is any missing data it is replaced by the mean of the mpg column.
mtcars$mpg[is.na(mtcars$mpg)] <- mean(mtcars$mpg)
```

## Section 10: List

List in R let us store many data frames into one variable. First we will create a vector, a matrix, and then a data frame:

```R
# Creating a vector.
v <- c(1,2,3)
# Creating a matrix with data = 1 thru 10
# and number of rows to be 2
m <- matrix(1:10, nrow = 2)

df <- mtcars
# This displays what kind of data is inside of these variables.
class(v)
class(m)
class(df)
```

Now that we have created these data structures we can put them in what we are going to call a list. As we recall from earlier, a list is a way to store multiple data structures into one variable. The main purpose of doing this is for organization.

```R
# Displays information with double brackets. [[1]] is v, [[2]] is m, [[3]] is df
my_list <- list(v, m, df)
```

As the comment says, `[[1]]` is the name for the first data structure `v`. We can name the data structures in the list by doing the following:

```R
my_named_list <- list(my_vec = v, my_matrix = m, my_dataframe = df)
```

We see now that the first item is called `my_vec`. To access this first element in this list we will use the `$` operator. With this we can store the data structure from the list. Below, we are storing the vector `my_vec`.

```R
copy_of_vec <- my_named_list$my_vec
```

We can make a lists of lists by combining the two lists. Let's look at the following:

```R
double_list <- c(my_named_list, my_named_list)
```
