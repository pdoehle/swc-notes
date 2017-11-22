# Data Manipulation and Visualization using R

* Based of the [Data Carpentry](http://www.datacarpentry.org/ "Data Carpentry Website") lesson ["R for Data Analysis and Visualization of Ecological Data"](http://www.datacarpentry.org/R-ecology-lesson/ "R Ecology Lesson")

## Some Review of Bracket Notation

## Manipulating Data Frames

* One of the things that makes R so powerful is how many packages are available.

* A package is an "extension" to R that gives you more functions that you wouldn't otherwise have access to.

* We will be using the packages `dplyr` and `tidyr` to manipulate data.

* `dplyr` provides tools for manipulating data that tend to be more straight-forward than using the default bracket notation in base R.

* `tidyr` has functions for reshaping messy data into "clean" data (more on this later).

* The two packages work well together, and we will use them both.

* To begin with, we need to install the `tidyverse` package which is an "umbrella-package" that contains several packages, including `dplyr` and `tidyr`.

## Installing `tidyverse` and Downloading Data

* If you were not with us last time, or if you have not created a `data` folder, make sure you have a project with a folder called `data`.

* Download the data.

```r
download.file("https://ndownloader.figshare.com/files/2292169", "data/portal_data_joined.csv")
```

* Install and load the `tidyverse` package.

```r
install.packages("tidyverse")
library("tidyverse")
```

## Manipulating Data using `dplyr`
* Begin by importing the data using `tidyverse`'s `read_csv()` function.

* We can start to get a sense of the data by using the `str()`ucture function.

```r
str(surveys)
```

* `tbl_df` indicates that `surveys` is a "tibble," the newest incarnation of the data frame.

* We can see each of the column headings, and what type of data each column contains.

* We can select columns using `select()`.

```r
select(surveys, plot_id, species_id, weight)
```

* The tibble is the first argument, followed by the names of the columns we want to select.

* We can select rows using `filter()`.

```r
filter(surveys, year == 1995)
```

## Pipes
