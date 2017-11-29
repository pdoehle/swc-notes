# Data Manipulation and Visualization using R

* Based of the [Data Carpentry](http://www.datacarpentry.org/ "Data Carpentry Website") lesson ["R for Data Analysis and Visualization of Ecological Data"](http://www.datacarpentry.org/R-ecology-lesson/ "R Ecology Lesson")

## Some Review of Bracket Notation

## Manipulating Data Frames

* One of the things that makes R so powerful is how many packages are available.

* A package is an "extension" to R that gives you more functions.

* We will be using the packages `dplyr` and `tidyr` to manipulate data.

* `dplyr` provides tools for manipulating data that tend to be more straight-forward than using the default bracket notation in base R.

* `tidyr` has functions for reshaping messy data into "clean" data.

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

```r
surveys <- read_csv("data/portal_data_joined.csv")
```

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

* What if you want to select and filter at the same time?

* We can use an intermediate variable ...

```r
surveys2 <- filter(surveys, weight < 5)
surveys_sml <- select(surveys, species_id, sex, weight)
head(surveys_sml)
```

* We could nest functions instead ...

```r
surveys_sml <- select(filter(surveys, weight< 5), species_id, sex, weight)
head(surveys_sml)
```

* `dplyr` provides pipes!

```r
surveys %>% 
  filter(weight < 5) %>% 
  select(species_id, sex, weight)
```

* Shortcut: `Ctrl`+`Shift`+`M`

* With a pipe, the results from the previous step are placed in the first argument of the next function.

* We can take the entire expression and assign it to a variable.

```r
surveys_sml <- surveys %>% 
  filter(weight < 5) %>% 
  select(species_id, sex, weight)

surveys_sml
```

> Challenge: Using pipes, subset the surveys data to include individuals collected before 1995 and retain only the columns `year`, `sex`, and `weight`.

```r
surveys %>% 
  filter(year < 1995) %>% 
  select(year, sex, weight)
```

## Mutate
* `mutate()` allows us to create new columns based on the values of other columns.
	* An example of this is unit conversions.

* Let's create a new column with weight in kilograms instead of grams.

```r
surveys %>% 
  mutate(weight_kg = weight / 1000)
```

* There are too many columns. The column headers are listed but we can't see the new column. Let's use `select()` to grab just a few of the columns.

```r
surveys %>% 
  mutate(weight_kg = weight / 1000) %>% 
  select(species_id, weight, weight_kg)
```

* We can even create new columns from the newly created column all in the same command.

```r
surveys %>% 
  mutate(weight_kg = weight / 100, weight_kg2 = weight_kg * 2) %>% 
  select(species_id, weight, weight_kg, weight_kg2)
```

* Let's get rid of the `NA` values and just look at observations where a measurement was taken.

```r
> surveys %>% 
  filter(!is.na(weight)) %>% 
  mutate(weight_kg = weight / 1000) %>% 
  select(species_id, weight, weight_kg) 
```

* `!` negates whatever comes after it.

* `is.na()` returns true if a value is `NA`.

* If I want to just look at the first few lines, I can use the `head()` function.
	* Normally `head()` takes the object as it's argument, but in this case we are piping so we can just use `head` without an argument.

```r
surveys %>% 
  filter(!is.na(weight)) %>% 
  mutate(weight_kg = weight / 1000) %>% 
  select(species_id, weight, weight_kg) %>% 
  head()
```

## The `summarize()` Function
* Working with data often requires a "split-apply-combine" workflow. `dplyr` gives us tools for this.
	* Split the data into groups.
	* Apply some analysis to each group.
	* Combine the results.

* `group_by()` takes the name of a column containing *categorical* variables.
	* ex: male, female

* `summarize()` collapses each group into a single-row summary of that group.

```r
surveys %>% 
  group_by(sex) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE))
```

* Notice that when we used the `mean()` function, we got rid of the `NA`'s before taking the mean.

* We can group by more than one column.

```r
surveys %>% 
  group_by(sex, species_id) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE))
```

* Let's look at a few more rows of that tibble ...

```r
surveys %>% 
  group_by(sex, species_id) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE)) %>% 
  print(n=92)
``` 

* NaN stands for "not a number," and happens when there was no value for calculating the summary statistic.

* We can fix this by removing missing values first. We get the added benefit of not needing to remove `NA`'s when we use the `mean()` function.

```r
surveys %>% 
  filter(!is.na(weight)) %>% 
  group_by(sex, species_id) %>% 
  summarize(mean_weight = mean(weight)) %>% 
  print(n=92)
```

* We can also have multiple summaries.

```r
surveys %>% 
  filter(!is.na(weight)) %>% 
  group_by(sex, species_id) %>% 
  summarize(mean_weight = mean(weight),
            min_weight = min(weight))
```

* Sometimes we want to count the number of objects in a group. We can do this using `tally()`.

```r
surveys %>% 
  group_by(sex) %>% 
  tally()
```

## Exporting Data
* Once you have finished your analysis or cleaned up your data, you may want to export it to a `.csv` file for storage or collaboration. Let's practice by creating a newly cleaned data set that we export for use in our next section.

* Begin by creating a new folder in your project directory called `data_output`.

* Remove missing values.

```r
surveys_complete <- surveys %>% 
  filter(species_id != "", 
         !is.na(weight),
         !is.na(hindfoot_length),
         sex != "")
```

* In the next section, we will plot how the abundance of different species has changed over time, so we want to remove all observations for rare species.
	* The criteria we will use for "rare" is species that have been observed less than fifty times.

* First, create a pipeline that counts and filters out the rare species.

```r
species_counts <- surveys_complete %>% 
  group_by(species_id) %>% 
  tally %>% 
  filter( n >= 50)
species_counts
```

* We now have a list of species with over fifty observations and their associated counts.

* Now we filter our survey based on this list.

```r
surveys_complete <- surveys_complete %>% 
  filter(species_id %in% species_counts$species_id)
```

* Let's make sure we all have the same data set.

```r
dim(surveys_complete)
[1] 30463    13
```

* Once we all have the same data set, we are ready to export the data!

```r
write_csv(surveys_complete, path = "data_output/surveys_complete.csv")
```

## Visualizing Data
* If you not are not still in the same work space, you need to load `tidyverse` and the data.

```r
library("tidyverse")
surveys_complete <- read_csv("data_output/surveys_complete.csv")
```

* To plot our data, we will use `ggplot2`. This is a library that allows us to create highly customized, publication quality graphs.

* Graphs are built from layers. This approach requires us to do some extra book keeping, but it allows us fine-grained control over every aspect of the plot.

* Begin by binding the data to a specific data frame used by `ggplot`.

```r
ggplot(data = surveys_complete)
```

* Notice RStudio gives us a blank, grey square. We now have a plot object, but we haven't built any of the layers yet.

* We need to add aesthetics, which define what plot objects our data's variables will be mapped to.

```r
ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length))
```

* `ggplot` now knows that `weight` will map to the x-axis and `hindfoot_length` will map to the y-axis.

* We now need to add a geometry layer which will describe what shape `ggplot` uses to map the variables to their associated axis.

```r
ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length)) + geom_point()
``` 

* `ggplot` offers many different geometries (even maps). Today we will look at some of the more common ones.

* Notice the syntax, `+ geom_point()`, lends to the idea of adding layers. We can leverage this syntax to save the base plot in a variable and experiment with what different layers look like.

* Each layer has it's own properties that we can change. For example, we can change the transparency of the points to avoid overplotting.

```r
surveys_plot + geom_point(alpha = 0.1)
```

* We can set color.

```r
surveys_plot + geom_point(alpha = 0.1, color = "blue")
```

* Layers can have their own aesthetics that are separate from the base graphic aesthetics. For example, we can see the distinction in species by mapping color to the `species_id` variable.

```r
surveys_plot + geom_point(alpha = 0.1, aes(color = species_id))
```

* We can make boxplots using the boxplot geometry.

```r
surveys_plot <- ggplot(data = surveys_complete, aes(x = species_id, y = weight))
surveys_plot + geom_boxplot()
```

* We can add points to our boxplot by adding another layer using the `geom_jitter` geometry. This gives us an idea of the number of points and their distributuion.

```r
surveys_plot + geom_boxplot(alpha = 0) + geom_jitter(alpha = 0.3, color = "tomato")
```

* Now our boxplots have been obscured. Let's switch the order of the layers.

```r
surveys_plot + geom_jitter(alpha = 0.3, color = "tomato") + geom_boxplot(alpha = 0)
```

## Plotting Time Series Data
* Let's plot the number of captures per year for each species.

```r
yearly_counts <- surveys_complete %>% 
  group_by(year, species_id) %>% 
  tally()
timelapse <- ggplot(data = yearly_counts, aes(x = year, y = n))
timelapse + geom_line()
```

* This isn't helpful, we plotted all numbers by year, but all the species were lumped together for each year.

* We need to plot a separate line for each species.

* Even though we sorted by `year` and `species_id` in the data, the plot aesthetic that is responsible for mapping variables to objects in the plot doesn't know that we need to separate the data into groups by `species_id`.

```r
timelapse <- ggplot(data = yearly_counts, aes(x = year, y = n, group = species_id))
timelapse + geom_line()
```

* The plot becomes even more clear if we tell the aesthetic to group by color instead of just simply grouping.

```r
timelapse <- ggplot(data = yearly_counts, aes(x = year, y = n, color = species_id))
timelapse + geom_line()
```

## Faceting
* Sometimes it is helpful to compare each line side-by-side on its own plot. `ggplot` provides the faceting feature for this.

```r
timelapse <- ggplot(data = yearly_counts, aes(x = year, y = n))
timelapse + geom_line() + facet_wrap(~ species_id)
```

* `facet_wrap()` takes arguments of the form `rows ~ columns`.

* In this case, we left the "rows" part blank and specified that we wanted to organize the columns of the facet based on the `species_id`.

* Each column has a `species_id`, and since we didn't specify anything for rows, `facet_wrap()` automatically filled out rows by wrapping once there was no space left.

* Let's add more detail to this plot by designated the difference between male and female counts.

```r
yearly_sex_counts <- surveys_complete %>% 
                       group_by(year, species_id, sex) %>% 
                       tally()
timelapse <- ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = sex))
timelapse + geom_line() + facet_wrap(~ species_id)
```

## Themes
* `ggplot` offers many different themes for the final product. These can also be added as layers.

```r
timelapse + geom_line() + facet_wrap(~ species_id) + theme_bw()
```

* Themes themselves have many options. For example, I can take away the grid lines.

```r
timelapse + geom_line() + facet_wrap(~ species_id) + theme_bw() + theme(panel.grid = element_blank())
```

## Polishing the Final Product
* Let's change the axes to have more informative names.

```r
final_plot <- timelapse + geom_line() + facet_wrap(~ species_id) + theme_bw()
final_plot + labs(title = "Observed species in time", x = "Year of observation", y = "Number of species")
```

* We can increase the readability by changing the text properties using the theme.

```r
final_plot + 
  labs(title = "Observed species in time", x = "Year of observation", y = "Number of species") +
  theme(text=element_text(size = 16))
```

* We also have many options for changing the numbers on the axes to make them more readable.

```r
final_plot + 
  labs(title = "Observed species in time", x = "Year of observation", y = "Number of species") +
  theme(text=element_text(size = 16), axis.text.x = element_text(color = "grey20", size = 12, 
                                                           angle = 90, hjust = 0.5, vjust = 0.5))
```

# Arranging and Exporting Plots
* Faceting allows me to split a plot into many different plots, while `gridExtra` allows me to do the opposite, that is, take different plots and put them together in the same figure.

```r
library(gridExtra)
```

* Let's combine two plots into one and export it.

* First, we'll create a box plot with a log scale on the y-axis.

```r
weight_boxplot <- ggplot(data = surveys_complete, aes(x = species_id, y = weight)) +
                    geom_boxplot() +
                    xlab("Species") +
                    ylab("Weight (g)") +
                    scale_y_log10()
weight_boxplot
```

* Now, lets create a time series plot that's grouped by `species_id`.

```r
count_plot <- ggplot(data = yearly_counts, aes(x = year, y = n, color = species_id)) +
                      geom_line() +
                      xlab("Year") +
                      ylab("Abundance")
count_plot
```

* Combine the plots using the `grid.arrange()` function from the `gridExtra` library.

```r
combo_plot <- grid.arrange(weight_boxplot, count_plot, ncol = 2, widths = c(4,6))
```

* Finally, it's time to save our plot.

```r
ggsave("combo_plot.png", combo_plot, width = 10, dpi = 300)
```

## Aditional Resources
[Data Carpentry](http://www.datacarpentry.org/ "Data Carpentry")
* Today's lesson (plus more) can be found here.

[Software Carpentry](https://software-carpentry.org/ "Software Carpentry")
* A similar organization to Data Carpentry with R lessons.

[R For Data Science](http://r4ds.had.co.nz/ "R for Data Science")
* A good book that builds on the concepts discussed today.

**ggplot2** by Hadley Wickham
* Very detailed explanation of ggplot2's capabilities.
* Contains explanations on how the software works and what decisions the developers made.

**Introductory Statistics with R** by Peter Dalgaard
* Statistics textbook using R.
* Great for learning how to do different statistical tests in R

Note: Both **ggplot2** and **Introductory Statistics with R** are available for free to download in PDF form through the OSU library if you are affiliated with OSU.
