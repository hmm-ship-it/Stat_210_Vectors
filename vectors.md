Stat 210 Vectors
================

There are two types of vectors, Atomic and Lists

Atomic consist of subtypes: logical, integer, double, character,
complex, and raw

Lists Can contain other lists. Lists can hold multiple types within

NULL - absence

type() will return the type of a vector typeof() length() will return
the length/size When using floats in R use dplyr::near() to evaluate
instead of ==

Additional meta-data can be within a vector. 3 important types of
augmented vectors Factors, Dates, Data Frames & Tibbles

Place an L after a number to make it an integer

Special values NA, NaN, Inf and -Inf These also should not use == but
use instead is.finite() is.infinite() is.na() is.nan()

Strings simply use pointers to the same object to cut down on memory
usage Rare missing value data types NA logical NA\_integer\_ NA\_real\_
double NA\_character\_
    Char

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
#logical
c(TRUE, TRUE, FALSE, NA)
```

    ## [1]  TRUE  TRUE FALSE    NA

``` r
#Colon notation generates from 1 to 10. Performs modulus on each item in the list
# If the modulus result equals 0 .... Implicit statement?
1:10 %% 2 == 0
```

    ##  [1] FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE

Exercises

``` r
x <- NaN
is.finite(x)
```

    ## [1] FALSE

``` r
!is.infinite(x)
```

    ## [1] TRUE

These two things aren’t evaluating the same thing otherwise you would
expect the \! to make them return the same values. Missing values are
not considered finite, while is.infinite considers just Inf and -Inf to
be infinite

``` r
dplyr::near
```

    ## function (x, y, tol = .Machine$double.eps^0.5) 
    ## {
    ##     abs(x - y) < tol
    ## }
    ## <bytecode: 0x564970394f70>
    ## <environment: namespace:dplyr>

The function works by checking if the difference in absolute value is
less than a number

3.  A integer vector can take 11 possible int values 0-9 & NA upto 2^32
    A double can represent upto 2^64 with -Inf, Inf,NA, and Nan being
    part of those values.
4.  floor (round down), ceiling (round up), round (round up at .5 down
    at .4), and…. trunc() which rounds towards zero
5.  parse\_logical (replace second part with integer and number)

<!-- end list -->

``` r
x <- sample(20, 100, replace = TRUE)
y <- x > 10 
```

Useful functions to check data type is\_logical() is\_integer()
is\_double() is\_numeric() is\_character() is\_atomic() is\_list()
is\_vector()

scalar version checks that the length is one is\_scalar\_logical()

Tidyverse doesn’t like to automatically ‘recycle’ smaller size vectors
to match larger ones for operations. if you want to do it anyways use
tibble(x = 1:4, y = rep(1:2, 2))

you can name vectors eg c(x = 1, y = 2, z = 3) you can name them after
the fact too set\_names(1:3, c(“a”,“b”,“c”)) subsetting within a vector
is like an index in an array x\[a\] and will return the value at a
x\[c(3,2,1)\] yields ‘three’, ‘two’, ‘one’ Negative values counter
intuitively drop values out of the output. don’t mix + and -
x\[c(-1,-3,-5)\] x\[\] blank means all In R you can use names to access
vector values e.g. x\[“xyz”, “def”\] 5 2

\[\[ extract a single item, not meta like attached
    names

1.  
<!-- end list -->

``` r
is.na(x) #returns true is NA false if not
```

    ##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [13] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [25] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [37] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [49] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [61] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [73] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [85] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ##  [97] FALSE FALSE FALSE FALSE

``` r
mean(is.na(x))#adds up the 0's and 1s and divides by the total, tells you how much is na
```

    ## [1] 0

2.  is vector checks for a vector, and names, but if it has anything
    else it’s not considered a vector. is atomic checks for one of the
    basic types

3.  purr simply has more ways it allows you to set names

4.  
<!-- end list -->

``` r
singled_out <- function(x){
  x[[length(x)]]
}
 singled_out(c(1,2,3,4,5,6,7,8,9)) 
```

    ## [1] 9

``` r
No0ddsAllowed <- function(x){
  x[x %% 2 == 0]
}
 No0ddsAllowed(c(1,2,3,4,5,6,7,8,9)) 
```

    ## [1] 2 4 6 8

``` r
bully_last_pick <- function(x){
  x[-length(x)]
}
bully_last_pick(c(1,2,3,4,5,6,7,8,9)) 
```

    ## [1] 1 2 3 4 5 6 7 8

``` r
getting_even <- function(x){
  x[is.na(x) %% 2 == 0]
}
getting_even(c(1,2,3,4,5,6,7,8,9,NA)) 
```

    ## [1] 1 2 3 4 5 6 7 8 9

5.  One includes Na the other does not
6.  it returns Na

You can create a list with list() str() shows what STRucture is in a
list Lists can have multiple object types within it list(list(1,2),TRUE,
“a”)

a \<- list(a = 1:3, b= " astring", c=pi d = list(-1,-5))

to access elements within a list \[a\] - returns list object with
selected elements within it (pepper shaker with packets within it)
\[\[a\]\] - returns just the element within the list \[\[a\]\]\[\[x\]\]
- returns the x element within the element a of the list $ is used to
access and single element a$a is equivalent to a\[\["a\]\]

2.  The major difference between a tibble and list is that tibbles must
    have the same number of rows, while a list can vary. A list can
    contain a data frame or tibble.

attributes Vectors can contain additional meta data called attributes.
set attributes with attr() see them with attributes() important for
adding names, Dimensions, or Class

x \<- 1:10 attr(x, “greeting”) \#\> NULL attr(x, “greeting”) \<- “Hi\!”
attr(x, “farewell”) \<- “Bye\!” attributes(x) \#\> $greeting \#\> \[1\]
“Hi\!” \#\> \#\> $farewell \#\> \[1\] “Bye\!”

R can be used with object oriented programming. You have generic methods
such as as.date() which depending on the arguement type that is passed
to it will be handled by a specific methoded contained within the (I am
guessing) class. methods(“as.date”) will display all the specific
methods. getS3method() shows the implementation

1.  
<!-- end list -->

``` r
hms::hms(3600)
```

    ## 01:00:00

``` r
j <-hms::hms(3600)

class(j)
```

    ## [1] "hms"      "difftime"

``` r
typeof(j)
```

    ## [1] "double"

It returns a time, that is an object of difftime, which holds a double

2.  It generates an error if different size vectors are used.
3.  So long as the list is the correct length it is okay…even if there
    are different types involved.
