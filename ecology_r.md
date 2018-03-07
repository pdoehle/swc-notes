## Introduction
### R
- R and RStudion are two different pieces of software.
	- Describe the differences
- In R there is no pointing and clicking, and that a good thing ...
- R allows you to reproduce your work faster.
- R has over 10,000 packages.
- R was made for data, and it scales.
- R produces high resolution graphics that are ready for publication.
- R is free, open-source, cross platform, and community supported.

### R Studio
- R studio is an Integrated Development Environment (IDE).
- RStudio can: write code, navigate files, inspect variables, visualize plots, integrate with Git for version control, develop packages, and write shiny apps.
- It is separate from R, but gives us a nicer way to interact with R.
- Show the "Help," "Plots," and "Files" tabs.
- Show the "Console" window.
	- Console commands are forgotten after your session.
- Show the "Script" window.
	- You can save script commands for later.
	- `Ctrl`+`Enter` shortcut for testing a line of code in the console.

### Project directory
- Good practice to contain the whole project in a single "working directory."
- Relative links are portable.
- It's a good idea to have separate folders for raw and clean data, and for scripts.
- It's a good idea to determine a system you like and keep it consistent across projects.

## Creating Objects in R
- Let's start a new project: *File* -> *New Project* -> *New Directory* -> *New Project* -> [ *Give your project a descriptive name* ] -> *Create Project*
- Under the "Files" tab, create a new folder called `data`.
- To begin with, R will do most math operations by tying it into the window.
- `>` is the prompt. It means R is ready to accept input.
- `+` means that R is expecting more input and that your command is not complete yet (this allows for multi-line commands).

```r
3 + 5
12/7
```

- To do useful things, we need to assign values to objects
- The assignment operator in R is `<-`

> In many programming languages, data is stored in *variables.* R uses the term *object* due to differences in the way memory is managed in R.

> !!!REVIEW AT BEGINNING OF SECOND WORKSHOP!!!

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

> !!!REVIEW AT BEGINNING OF SECOND WORKSHOP!!!

```r
weight_lb <- 2.2 * weight_kg
weight_lb
```

- If I now change the value of `weight_kg`, it *will not* change the value saved in `weight_lb`

```r
weight_kg <- 100
weight_lb
```

- Anything after `#` is treated as a comment in R
- The computer will ignore that line and it is for your reference
- Making comments is a good practice as it provides reference for others (or yourself in six months) as to what is happening in your code

- RStudio has the shortcut `Ctrl`+`Shift`+`C` for commenting multiple lines at once whle scripting.

> Demonstrate `Ctrl` + `Shift` + `C` on the following lines of code and the script window:

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
- We *call* a function when we want to execute the code in it

- Functions are not limited to working with numbers
- Functions can take as arguments and return almost any object we can create withing the R environment
- Functions can also take multiple arguments, like `round()`

```r
round(3.14159)
```

- `round()` defaults to rounding to the nearest whole number
- We can change this by changing the number of arguments we give `round()`

- Let's find out more about `round()`'s arguments

```r
args(round)
```

- We can look up the manual entry for a specific function.

```r
?round
```

- We can make general help searches if we don't know the function name.

```r
??ceiling
```

- RStudion has great built-in help, but sometimes a quick Google or Stack Exchange search is the best place to get help with examples.

- We have a list of arguments and we see that `digits=0` defaults to 0
- We can look up more details about the function by using a `?`

```r
?round
```

- Now we can round to two digits

```r
round(3.14159, 2)
```

- I can also label the arguments that I pass to a function.

```r
round(x = 3.14159, digits = 2)
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

> !!!REVIEW AT BEGINNING OF SECOND WORKSHOP!!!

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

> !!!REVIEW AT BEGINNING OF SECOND WORKSHOP!!!

- We reference values within a vector using `[ ]` to indicate index

> Notice the 1 - indexing.

```r
animals <- c(animals, "cat")
animals
animals[2]
animals[c(3, 2)]
```

- We can pull the same element out multiple times

```r
more_animals <- animals[c(1, 2, 3, 2, 1, 4)]
more_animals
```

- We can also index elements by using *logical values*

> In R, a logical value is a value that is either true or false

```r
weight_g <- c(21, 34, 39, 54, 55)
weight_g[c(TRUE, FALSE, TRUE, TRUE, FALSE)]
```

- Usually you don't type logical values out by hand, they are usually the output from another statement
- For example, suppose you want all the weights in `weight_g` that are over 50

> !!!REVIEW AT BEGINNING OF SECOND WORKSHOP!!!

```r
weight_g > 50
weight_g[weight_g > 50]
```

- We can use the AND (`&`) or the OR (`|`) tests to combine multiple statements
	- AND: both statements must be true
	- OR: one of the statements must be true

- Suppose we want all the weights that are less than 30 grams **or** greater than 50 grams

```r
weight_g[weight_g < 30 | weight_g > 50]
```

- Suppose we want all the weights greater than or equal to 30, **or** equal to 21

```r
weight_g[weight_g >= 30 | weight_g == 21]
```

- `==` test for equality, it does not mean "equal to"

> Challenge: Use `&` to return all the values in `weight_g` that are greater than 30 and less than 50.

```r
weight_g[weight > 30 & weight_g < 50]
```

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

- To return the index of where an item is found, we can use the `match()` function.

```r
match("dog", animals)
```

- To insert an object into the middle of a vector, use the `append()` function.

```r
append(animals, c("giraffe", "zebra"), after = 3)
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
```

> Challenge: Ignore the missing values and find the maximum value of `heights` using `max()`.

```r
max(heights, na.rm = TRUE
```

# Starting with Data
- Begin by downloading the data into your `/data` file

```r
download.file("https://ndownloader.figshare.com/files/2292169", "data/portal_data_joined.csv")
```

- Now load the data into your workspace

```r
surveys <- read.csv('data/portal_dat_joined.csv')
surveys
```

- To just output the first 6 lines, use `head()`

```r
head(surveys)
```

> Note: we can examine the original `.csv` file from within RStudio. Have a look around and get a sense of the data.

## Data Frames

- When data is brought in from a spreadsheet, it is stored in R as a *data frame*.
- Data frames are used for statistics and plotting
- Each column is a vector
	- This forces each column to only have one data type

- To inspect the **st**ructure of a data frame, use `str()`

```r
str(surveys)
```

- There are several functions for examining the size of a data frame

```r
dim(surveys)
nrow(surveys)
ncol(surveys)
```

- Functions for examining data frame content

```r
head(surveys)
tail(surveys)
```

- Functions for examining row and column names

```r
names(surveys)   # Column names
rownames(surveys)
```

- `summary()` prints out summary statistics for each column

```r
summary(surveys)
```

## Indexing and Subsetting Data Frames
- Since data frames have two dimensions, we must account for rows and columns when we reference elements

```r
head(surveys)
surveys[1,1]      # 1st element in the first column
surveys[1,6]      # 1st element in the 6th column
surveys[,1]       # 1st column in the data frame
surveys[1:3, 7]   # 1st 3 elements of the 7th column
surveys[3,]       # 3rd element for all columns
```

- `:` is a special function that creates numeric vectors of integers in increasing or decreasing order.

```r
1:10
10:1
```

- You can use the `-` sign to exclude certain parts of the data frame

```r
surveys[,-1]  # Include whole data frame *except* last column
```

- You can also use the title of a column

```r
surveys$species_id
```

> !!!END OF FIRST WORKSHOP. REMAINING MATERIAL OPTIONAL BASED ON REMAINING TIME!!!

## Factors
- Notice how some of the vectors in the `surveys` data frame are "factors"

```r
str(surveys)
```

- Factors are like vectors of string, but they are actually vectors of integers
- Correspond to the "categorical variable" concept in statistics.
- R list all the unique strings and assigns each one an integer code (after sorting in alphabetical order)
- Once created, the factor can only contain this predefined list. We can change these entries, but we can't add to them.
- This list of possible values is called the factor's "levels"

```r
sex <- factor(c("male","female","female","male"))
levels(sex)
nlevels(sex)
```

- In this case, R will assign `1` to the level "female" since alphabetically it comes before "male"
	- Even thought the first string in the list was "male"

- Sometimes we need to reorder the factors for our specific analysis

```r
sex
sex <- factor(sex, levels = c("male", "female"))
sex
```

- One nice feature about factors is that you can quickly plot the number of observations represented by each factor level
- Let's look at the number of male and females captured during the experiment

```r
plot(surveys$sex)
```

- We can change the entries in a factor
- For example, consider if we wanted to label all the observations that were not assigned a gender with the word "missing,"

```r
sex <- surveys$sex
head(sex)
levels(sex)
levels(sex)[1] <- "missing"
levels(sex)
head(sex)
```

- While factors are useful for plotting, they can also be cumbersome at times
- You can import new data and specify that you don't want any factors
- Let's re-import our data and we'll set only the `plot_type` column to be a factor

```r
surveys <- read.csv("data/portal_dat_joined.csv", stringsAsFactors = FALSE)
str(surveys)
surveys$plot_type <- factor(surveys$plot_type)
```

## Formatting Dates
- One big challenge when wrangling data is to format dates correctly
- To do this we will use an R library called `lubridate`, which is found inside of the `tidyverse` package
- A library is a collection of functions and code (or other libraries) that extends the base functionalities of R
- `tidyverse` is an "umbrella package" that contains several sublibraries. These libraries all abide by the tidy data principles layed out in the paper "Tidy Data" by Hadley Wickham.

```r
install.packages("tidyverse")
```

> Many errors can be addressed by using `install.packages("tidyverse", dependencies = TRUE)

- the `ymd()` function changes the data type to dates

```r
library(lubridate)
paste(surveys$year, surveys$month, surveys$day, sep='-')
surveys$date <- ymd(paste(surveys$year, surveys$month, surveys$day, sep='-'))
str(surveys)
```

- Notice the error: `Warning: 129 failed to parse.`
	- Some of the dates didn't convert
	- Let's check it out

```r
surveys_dates <- paste(surveys$year, surveys$month, surveys$day, sep = "-")
head(surveys_dates)
surveys_dates[is.na(surveys$date)]
```

# Manipulating and Analyzing Data with dplyr
- `dplyr` is a set of tools that makes many tasks in data manipulation easier
- To use `dplyr` we need to load the right library

```r
library("tidyverse")
```

- `dplyr` uses `read_csv()` instead of `read.csv()`

```r
surveys <- read_csv("data/portal_dat_joined.csv")
str(surveys)
```

- `surveys` is now a "tibble"
- tibbles are the most recent version of data frames
	- The only difference we need to worry about today is the fact that tibbles never have factors, but always use vectors of characters

## Selecting Columns and Filtering Rows
- One of the big advantages of `dplyr` is that referencing pieces of the tibble is easier than the traditional index notation of data frames

```r
select(surveys, plot_id, species_id, weight)
```

- Notice the object is the first argument
- The columns that we want then follow as the rest of the arguments

- We can filter rows based on specific criteria

```r
filter(surveys, year == 1995)
```

## Pipes
- Suppose that I want to select and filter at the same time, now what?
- Pipes allow us to take the output of one function and input it into another function

```r
surveys %>% 
  filter(weight < 5) %>% 
  select(species_id, sex, weight)
```

- Each function call is missing the first argument since it was supplied through the pipe
- You can use the shortcut `Ctrl` + `Shift` + `M` to type a pipe in RStudio

- To save this as an object, do the following

```r
surveys_sml <- surveys %>% 
  filter(weight < 5) %>% 
  select(species_id, sex, weight)

surveys_sml
```

# Mutate
- Often we want to create new columns from existing columns
- We could create a new column from the weight column (which is currently in grams)

```r
surveys %>% 
  mutate(weight_kg = weight / 1000) %>% 
  head()
```

- `dplyr` allows us to group our data into categories and create statistical summaries of the data as well

```rsurveys %>% 
  group_by(sex) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE))
```

- We are not limited to just one category
	- We can summarize with subgroups as well

```r
surveys %>% 
  group_by(sex, species_id) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE))
```

## Exporting Data
- R has function for exporting `.csv` files just as it has functions for reading them
- Begin by creating a new folder in your project directory called `/data_output`
	- Raw data should be kept separate from processed data
	- The original data should never be changed

- Let's clean up our data and write out the results

```r
surveys_complete <- surveys %>%
  filter(species_id != "",
         !is.na(weight),
         !is.na(hindfoot_length),
         sex != "")
```

- In the next section, we will look at plotting how species abundances have changed through time, so we will remove observations for rare species

- We will begin by counting how many of each species was observed and filtering out those observations which were less than 50

```r
species_counts <- surveys_complete %>% 
  group_by(species_id) %>% 
  tally() %>% 
  filter(n >= 50)
```

- Now we will only keep the most common species

```r
surveys_complete <- surveys_complete %>% 
  filter(species_id %in% species_counts$species_id)
```

- Check that we all have the same clean data

```r
dim(surveys_complete)
```

- We should all have a tibble that is 30,463 rows and 13 columns

- We're now ready to write out to a `.csv`
- If you check your folder with RStudio, it should be there

# Plotting Data
> If not still in the workspace, then the students will need `tidyverse` and the data that we wrote out from the last workshop

```r
library(tidyverse)
surveys_complete <- read_csv("data_output/surveys_complete.csv")
```

- The package `ggplot2` can help us create publication quality graphs with minimal amounts of adjustments and tweaking
- `ggplot` graphics are built step by step by adding new elements

- Begin by binding the plot to a specific data frame

```r
ggplot(data = surveys_complete)
```

- We can change the presentation of things like size, shape, color, variables to be plotted, etc. by using the aesthetics argument

```r
ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length))
```

- We can specify the graphical representation of our data points with `geom_point()`

```r
ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length)) + geom_point()
```

- This is just the surface of what `ggplot2` has to offer
- Happy data wrangling!





