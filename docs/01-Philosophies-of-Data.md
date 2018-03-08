# Philosophies of Data

## What are Data

Data are observations about the world. 

<!--

In order to make sense of and give
context to these observations we organize data into structures. In this chapter
we'll discuss three general types of data structres: data frames, matricies,
and hierarchies. When you're getting and cleaning data it's often useful to
have one of these data structures in mind as an end goal of how you want your
data to be organized. In the following chapters we'll show you how you can use
each of these data structures in order to achieve your goals.

## Data Frames and Tibbles

R is an object oriented programming language, and there are several varieties
of R objects you can use to represent data. In this book we will concentrate
mostly on how to use data frames as the central object for thinking about data.
Data frames are rectangular data objects, meaning that they are composed of
rows and columns, much like the layout of a piece of graph paper or a
spreadsheet. In the next few chapters we'll discuss how you can import data
into R from many different sources, but you can also create data frames with
code alone.

There's no better tool for creating your own data frames from scratch than the
`tibble` package. Install the package with `install.packages("tibble")`, and
then we can start creating our own data frames.

Let's start up the package and then create a simple data frame, where every
column is specified with a simple vector:


```r
library(tibble)

people <- data_frame(Name = c("Shannon", "Jeff", "Sean", "Roger"),
           Department = c("Genetics", "Biostatistics", "Cognitive Science", "Biostatistics"),
           School = c("JHU", "JHU", "UCSD", "Monash"))
people
```

```
## # A tibble: 4 x 3
##   Name    Department        School
##   <chr>   <chr>             <chr> 
## 1 Shannon Genetics          JHU   
## 2 Jeff    Biostatistics     JHU   
## 3 Sean    Cognitive Science UCSD  
## 4 Roger   Biostatistics     Monash
```

Let's take a look at what is printed when we enter our table of `people` in the
R console. The first line, `# A tibble: 4 x 3`, shows that the object being
printed *is* a tibble. A tibble is a data frame with some nice extra properties
which we'll discuss later. The second line, `Name   Department   School`, shows
the column names for the tibble. The third line, `<chr>   <chr>   <chr>`, shows
the type of data in each column. After the third line, each line shows one row
in the dataset, starting with a row number.

We can add rows and columns using the `add_row()` and `add_column()` functions
respectively. The `add_row()` function takes an argument for each column in the
tibble while the `add_column()` function takes an argument for each row in the
tibble.


```r
people <- add_row(people, Name = "Brian", Department = "Biostatistics", School = "JHU")
people
```

```
## # A tibble: 5 x 3
##   Name    Department        School
##   <chr>   <chr>             <chr> 
## 1 Shannon Genetics          JHU   
## 2 Jeff    Biostatistics     JHU   
## 3 Sean    Cognitive Science UCSD  
## 4 Roger   Biostatistics     Monash
## 5 Brian   Biostatistics     JHU
```

```r
people <- add_column(people, Puppet = c(FALSE, TRUE, FALSE, TRUE, TRUE))
people
```

```
## # A tibble: 5 x 4
##   Name    Department        School Puppet
##   <chr>   <chr>             <chr>  <lgl> 
## 1 Shannon Genetics          JHU    F     
## 2 Jeff    Biostatistics     JHU    T     
## 3 Sean    Cognitive Science UCSD   F     
## 4 Roger   Biostatistics     Monash T     
## 5 Brian   Biostatistics     JHU    T
```

Sometimes it's advantageous to view a tibble with the columns on the left, so
that all of the column names are printed to the console. You can use the
`glimpse()` function to flip a tibble along its diagonal axis from upper left
to lower right for viewing purposes, like a matrix transpose:


```r
glimpse(people)
```

```
## Observations: 5
## Variables: 4
## $ Name       <chr> "Shannon", "Jeff", "Sean", "Roger", "Brian"
## $ Department <chr> "Genetics", "Biostatistics", "Cognitive Science", "...
## $ School     <chr> "JHU", "JHU", "UCSD", "Monash", "JHU"
## $ Puppet     <lgl> FALSE, TRUE, FALSE, TRUE, TRUE
```

In additiion to creating tibbles from scratch with `data_frame()`, you can also
just use the `tibble()` function:


```r
rhode_island <- tibble(
  County = c("Bristol", "Kent", "Newport", "Providence", "Washington"),
  Population = c(49875, 166158, 82888, 626667, 126979)
)

rhode_island
```

```
## # A tibble: 5 x 2
##   County     Population
##   <chr>           <dbl>
## 1 Bristol         49875
## 2 Kent           166158
## 3 Newport         82888
## 4 Providence     626667
## 5 Washington     126979
```

The tibble package also provides the handy `tribble()` function, which allows
you to construct tibbles row-wise instead of column-wise:


```r
uc_schools <- tribble(
  ~Name,         ~Founded, ~Enrolled,
  "Los Angeles",     1919,     42239,
  "Berkeley",        1868,     37581,
  "Davis",           1908,     35415,
  "San Diego",       1960,     31502,
  "Irvine",          1965,     30757,
  "Santa Barbara",   1909,     23051,
  "Riverside",       1954,     21680,
  "Santa Cruz",      1965,     17866,
  "Merced",          2005,      6268,
  "San Fransisco",   1873,      4904
)

uc_schools
```

```
## # A tibble: 10 x 3
##    Name          Founded Enrolled
##    <chr>           <dbl>    <dbl>
##  1 Los Angeles      1919    42239
##  2 Berkeley         1868    37581
##  3 Davis            1908    35415
##  4 San Diego        1960    31502
##  5 Irvine           1965    30757
##  6 Santa Barbara    1909    23051
##  7 Riverside        1954    21680
##  8 Santa Cruz       1965    17866
##  9 Merced           2005     6268
## 10 San Fransisco    1873     4904
```

### Summary

- Data frames are rectangular data sctructures like spreadsheets. 
- Tibbles are data frames with extra helpful features.
- You can create a tibble with `tibble()` and add rows and columns with
`add_row()` and `add_column()`.
- View a transposed tibble with `glimpse()`.
- Creat row-wise tibbles with `tribble()`.

### Exercises

1. Create a tibble like `uc_schools`, but with information about the four
University Centers that are part of the State University of New York system.
2. Construct the tibble you just created with `tribble()`.

## Tidy Data

We're living through an exciting moment in history where data science is
emerging as its own academic discipline. One of data science's central ideas
is the notion of tidy data, which Hadley Wickham introduced in his
[Tidy Data](http://vita.had.co.nz/papers/tidy-data.html) paper in the Jouranl
of Statistical Software. I strongly recommend that you read the paper. The
concept of tidy data boils down to three principles:

1. Groups of observations should be stored in rectangular tables.
2. Each observation should be represented by a row in the table.
3. Each variable observed should be represented by a column in the table.

Practicing tidy data principles can help you ask important questions about your
data. Let's take a look at an example of an untidy dataset of people in a drug
trial:


Drug           < 18   18-35   36-45   46-64   65+
------------  -----  ------  ------  ------  ----
Jeffazapram       5      16      11      32    10
Rogerdone        14       9       1      18     4
Brianophil       66      72      81      30    21

Looking at this table we can see that first column contains the names of each
drug. Every other column corresponds to an age group of study participants, with
the number of participants in each column. When evaluating if a dataset is tidy
first ask: what are the variables in the dataset? Here we can identify three
variables: the name of the drug, the age group of the participants, and the
number of people in each drug/age group. After you've identified the variables
ask: is each variable represented with a column? Here we can see that the three
variables we identified are not represented by columns. Let's take a look at the
tidy version of this dataset:


Drug          Age      Participants
------------  ------  -------------
Jeffazapram   < 18                5
Rogerdone     < 18               14
Brianophil    < 18               66
Jeffazapram   18-35              16
Rogerdone     18-35               9
Brianophil    18-35              72
Jeffazapram   36-45              11
Rogerdone     36-45               1
Brianophil    36-45              81
Jeffazapram   46-64              32
Rogerdone     46-64              18
Brianophil    46-64              30
Jeffazapram   65+                10
Rogerdone     65+                 4
Brianophil    65+                21

Both tables show the same information, and the tidy version of the table is
larger than the untidy version. In the following chapters we will discuss how
they tidy data allows us to ask questions about data more easily.

## Matrix Data

Besides data frames and tibbles the other rectagular shaped data sctructure you
should be aware of in R is the matrix. Matricies encode at least three different
pieces of information about each value in the matrix: the X position of a value,
the Y position of a value, and the value itself. We can create matricies using
the `matrix()` function:


```r
matrix(c("a", "b", "c", "d"), nrow = 2, ncol = 2)
```

```
##      [,1] [,2]
## [1,] "a"  "c" 
## [2,] "b"  "d"
```

When constructing a matrix from a vector the values are filled in column-wise
by default. Matricies are often useful for storing images and performing
operations with linear algebra.

The same information in the matrix above would like this in a tidy dataset:


```r
tibble(X = c(1, 1, 2, 2), 
       Y = c(1, 2, 1, 2), 
       Value = c("a", "b", "c", "d"))
```

```
## # A tibble: 4 x 3
##       X     Y Value
##   <dbl> <dbl> <chr>
## 1  1.00  1.00 a    
## 2  1.00  2.00 b    
## 3  2.00  1.00 c    
## 4  2.00  2.00 d
```


## Hierarchical Data

One form of data that doesn't fit neatly into a rectangle is heirarchical data.
The most common hierarchical data sctructure in R is a list. Any other data
structure can be the element of a list, and lists that contain other lists are
often use to represent hierarchical data in R. We'll encounter a few types of
heirarchical data structures that can be converted to lists of lists when we
discuss getting data from the web in a later chapter.

-->
