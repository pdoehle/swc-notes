## Creating Objects in R
- To begin with, R will do most math operations by tying it into the window.

```r
3 + 5
12/7
```

- To do useful things, we need to assign values to objects
- The assignment operator in R is `<-`

> In many programming languages, data is stored in *variables.* R uses the term *object* due to differences in the way memory is managed in R.

```r
weight_kg <- 55
```

- Shortcut for `<-` is `Alt` + `-`
- Object names are case-sensitive
- It's best to avoid `.` in object names

- When assigning a value to an object, R doesn't print anything
- You can force it to by using parenthesis

```r
weight_kg <- 55
(weight_kg <- 55)
```

- You can also check the value stored in an object by retyping its name

```r
weight_kg
```

- We can use objects to do arithmetic

```r
2.2 * weight_kg
weight_kg
```

- We can change the value of a variable by assigning it a new value

```r
weight_kg <- 57.5
2.2 * weight_kg
```

- We can store the results in a new variable

```r
weight_lb <- 2.2 * weight_kg
weight_lb
```

- If I now change the value of `weight_kg`, it will not change the value saved in `weight_lb`

```r
weight_kg <- 100
weight_lb
```

- Anything after `#` is treated as a comment in R
- The computer will ignore that line and it is for your reference
- Making comments is a good practice as it provides reference for others (or yourself in six months) as to what is happening in your code

- RStudio has the shortcut `Ctrl`+`Shift`+`C` for highlighting multiple lines at once

> Demonstrate `Ctrl` + `Shift` + `C` on the following lines of code:

```r
mass <- 47.5
age <- 122
mass <- mass * 2.0
age <- age - 20
mass_index <- mass/age
```

## Functions and their Arguments
- Functions are independent modules of code that can be called from your script/console
- They save us time so that we don't have to rewrite a piece of code if we need to use it multiple times
- `sqrt()` is an example of a function in R

```r
sqrt(2)
```

- The inputs of a function are called *arguments*
- If the function returns something, it returns a *value*
- We *call* a function when we want to excute the code in it

- Functions are not limited to working with numbers
- Functions can take as arguemtns and return almost any object we can create withing the R environment
- Functions can also take multiple arguemtns, like `round()`

```r
round(3.14159)
```

- `round()` defaults to rounding to the nearst whole number
- We can change this by changing the number of arguments we give `round()`

- Let's find out more about `round()`'s arguments

```r
args(round)
```

- We have a list of arguments and we see that `digits=0` defaults to 0
- We can look up more details about the function by using a `?`

```r
?round
```

- Now we can round to two digits

```r
round(3.14159, 2)
```

- If I explicitly name all the arguments, then can list them in any order that I want

```r
round(digits = 2, x = 3.14159)
```

- It's good practice to name at least the optional arguments so that somebody else can read your code without referencing the help manual

## Vectors and Data Types
- In R (or any programming language), we organize our data into abstract structures called data structures
- The most common data structure in R is the *vector*
- A vector is a series of values that can be either characters or numbers
- We stitch a series of values together by using the `c()` function

```r
weight_g <- c(50, 60, 65, 82)
weight_g
```

- Vectors can also contain characters

```r
animals <- c("mouse", "rat", "dog")
animals
```

- The quotes tell R that this is a string and not a variable

- The are many functions that help us work with vectors
- `length()` tells us how long a vector is

```r
length(weight_g)
length(animals)
```

- `class()` tells us what type of objects are in the vector

```r
class(weight_g)
class(animals)
```

- We can use `c()` to add another element to a vector

```r
weight_g <- c(weight_g, 90)   # add to the end of the vector
weight_g <- c(30, weight_g)   # add to the beginning of the vector
weight_g
```

- We reference values within a vector using `[ ]` to indicate index

> Notice the 1 - indexing.

```r
animals <- c("mouse", "rat", "dog", "cat")
animals[2]
animals[c(3, 2)]
```

- We can pull the same element out multiple times

```r
more_animals <- animals[c(1, 2, 3, 2, 1, 4)]
more_animals
```

- We can also index elements by using logical values
	- A logical value is a value that is either true or false

```r
weight_g <- c(21, 34, 39, 54, 55)
weight_g[c(TRUE, FALSE, TRUE, TRUE, FALSE)]
```

- Usually you don't type logical values out by hand, they are usually the output from another statement
- For example, suppose you want all the weights in `weight_g` that are over 50

```r
weight_g > 50
weight_g[weight_g > 50]
```

- We can use the AND (`&`) or the OR (`|`) tests to combine multiple statements
	- AND: both statements must be true
	- OR: one of the statements must be true

- Suppose we want all the weights that are less than 30 grams or greater than 50 grams

```r
weight_g[weight_g < 30 | weight_g > 50]
```

- Suppose we want all the weights greater than or equal to 30, or equal to 21

```r
weight_g[weight_g >= 30 & weight_g == 21]
```

- `==` test for equlity, it does not mean "equal to"
- We can use these kinds of statements to search for whether of not a vector has certain elements

```r
animals <- c("mouse", "rat", "dog", "cat")
animals[animals == "cat" | animals == "rat"]
```

- To make this less tedious for multiple values, we can use `%in%`

```r
animals %in% c("rat", "cat", "dog", "duck", "goat")]
animals[animals %in% c("rat", "cat", "dog", "duck", "goat")]
```

## Missing Data
- R represents missing data with `NA`
- Many functions will return `NA` if your data is missing values

```r
heights <- c(2, 4, 4, NA, 6)
mean(heights)
max(heights)
```

- To fix this, we tell `mean()` and `max()` to ignore the missing data

```r
mean(heights, na.rm = TRUE
max(heights, na.rm = TRUE
```
