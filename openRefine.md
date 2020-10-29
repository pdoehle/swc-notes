# Data Cleaning with OpenRefine

- Notes from lesson [Data Cleaning with OpenRefine for Ecologists](https://datacarpentry.org/OpenRefine-ecology-lesson/)

## Useful Links

- [Clustering https://github.com/OpenRefine/OpenRefine/wiki/Clustering-In-Depth](https://github.com/OpenRefine/OpenRefine/wiki/Clustering-In-Depth)

## Setup

1. Download the [data](https://ndownloader.figshare.com/files/2252083) and save it to your desktop
2. Download the latest [stable release](https://openrefine.org/download.html) of OpenRefine and unzip it to a convenient location on your computer

> [Full teaching database](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459)
>
> - These data are observations from a small mammal community in southern Arizona.
> - The data come from a project studying the effects of rodents and ants on the plant community that has been running for almost forty years.
> - The rodents are sampled on a series of 24 plots, with different experimental manipulations controlling which rodents are allowed to access which plots.
> - This is a real data set that has been used in over 100 publications.
> - It has been simplified for this workshop.

### Reminders

- OpenRefine is self-contained within its own directory
- Navigate to the executable and double click to launch it
- It will launch in your web browser
- If it does launch, copy the URL in the command-line window into a browser (`http://127.0.0.1:3333/` or `http://localhost:3333`)
- OpenRefine is a Java program that runs locally like a web page; it's not connecting to the internet
- OpenRefine will not run correctly in Internet Explorer

## Introduction

**Questions:**

- What is OpenRefine useful for?

**Objectives:**

- Describe OpenRefine's uses and applications.
- Differentiate data cleaning from data organization.
- Experiment with OpenRefine's user interface.
- Locate helpful resources to learn more about OpenRefine.

### What is OpenRefine

- [OpenRefine](http://openrefine.org/) is a powerful, free, and open source tool that helps you with data wrangling
- OpenRefine started as Google Refine before it was released to the open source community
- It combines the GUI interface of Excel with the reproducibility of scripting languages like R and Python

### Benefits of OpenRefine

- OpenRefine automatically keeps a log of every change you make
  - These can be added to a publication as supplemental material
- OpenRefine does not allow you to modify your original file
  - You must export to a new file to save your work
  - OpenRefine keeps your original data pristine
- Any operation can be undone
- OpenRefine can repeat your steps for more than one data set
- OpenRefine provides a user-friendly interface for complex clustering algorithms

### Uses of OpenRefine

- Overview a data set
- Resolve inconsistencies
- Help you split data into granular parts
- Match local data with other data sets
- Save a set of data cleaning steps for replay on multiple files
- Limited to smaller data sets (100,000 rows)
  - Still great for prototyping large data sets to determine steps

## Working with OpenRefine

**Questions:**

- How can we import data into OpenRefine?
- How can we sort and summarize data with OpenRefine?
- How can we find and correct errors with OpenRefine?

**Objectives:**

- Create a new OpenRefine project from a `.csv` file.
- Look at facets and how they sort and summarize data.
- Look at clustering and how to apply it to edit groups of typos.
- Undo/redo steps.
- Split values into multiple columns.
- Remove white spaces from cells.

### Creating a Project

- OpenRefine can import many different file types
  - Tab-separated (`.tsv`)
  - Comma-separated (`.csv`)
  - Excel (`.xls`, `.xlsx`)
  - JSON (`.json`)
  - XML
  - Google Spreadsheets
- See the [Importers](https://github.com/OpenRefine/OpenRefine/wiki/Importers) documentation for more information

1. Launch OpenRefine
2. Click *Create Project* and select *This Computer*
3. Click *Browse* and select `Portal_rodents_19772002_scinameUUIDs.csv`. Click *Open*
4. Click *Next >>*
5. Look at the preview and ensure all the settings are correct
   - You can adjust how OpenRefine interprets the file to ensure it is reading it correctly
6. Rename your project to `portal_rodents`
7. click *Create Project >>*

- Point out key items in the user interface
  - Navigating through pages
  - How many rows to display

### Faceting

- Faceting is a core OpenRefine operation
- Faceting allows you to group rows and see big-picture trends
- Faceting allows you to change rows in bulk
- Faceting groups values according to a criterion

- Use faceting to look for errors in the `scientificName` column

1. Scroll to the `scientificName` column
2. Click the down arrow and choose *Facet* > *Text facet*
   1. The facet displays every unique value along with a count of how many times it appears
   2. Sort by *name* or *count*
3. Hover over one of the names. Notice the *edit* option
   1. You could use this option to fix all instances of an misspelled name. 
   2. Find `Crotalus viridis` and click *edit*
   3. Add a space in front of the name and click *Apply*
   4. Point out the `(blank)` category

### Other available facets

- Numeric (displays graphs)
- Time line (for date)
- Custom
- Scatter plot (displays graphs)

### Clustering

- Clustering allows you to find things that are similar to each other
- e.g. `New York` and `new york` are likely the same item, but the computer will not necessarily equate them
- OpenRefine comes will built-in clustering algorithms to identify things that are like each other

1. In the `scientificName` text facet, lick the *Cluster* button
   1. You can change the *Method* and *Keying Function*
2. Try different combinations and see what clusters emerge
3. Select the *key collision* method and the *metaphone3* keying function.
4. Click the *Merge?* box beside each, then click *Merge Selected and Recluster* to apply corrections
5. Try selecting methods again and see if you can find further improvements, but don't re-cluster again
6. Click *Close*

- An in-depth introduction to [clustering](https://github.com/OpenRefine/OpenRefine/wiki/Clustering-In-Depth)

### Split

- You can split columns based on a separator

1. Split `scientificName` into separate columns for genus and species
2. Click the down arrow at the top of `scientificName`. Choose *Edit Column* > *Split into several columns...*
3. Replace the comma with a space in the *Separator* box
4. Un-check the box that says *Remove this column*
5. Click *OK*
6. Notice you can use any separator you want

## Undo/Redo

- OpenRefine lets us undo and redo steps for the entire duration of the session

1. Click where it says *Undo / Redo*. You can see all the steps you have made.
2. Click on the steps you want to go back to (`1. Mass edit 739 cells in column scientificName` in this case)
3. Notice that you can go forwards and backwards
4. Go back before the split
5. Notice you must go to the step before the change you want to undo

### Trim leading and trailing white space

- OpenRefine provides tools for removing hidden white space characters at the beginning or end of words

1. In `scientificName`, choose *Edit cells* > *Tranform* > *Trim leading and trailing whitespace*
2. Perform the same split operation on `scientificName that you did earlier.
3. This time you only get two new columns, why?

## Filtering and Sorting with OpenRefine

**Questions:**

- How can we select only a subset of our data to work with?
- How can we sort our data?

**Objectives:**

- Employ *text filter* or *include/exclude* to filter a subset of rows.
- Sort tables by a column.
- Sort tables by multiple columns.

### Filtering

- We can filter entries to work on a subset of data in OpenRefine

1. *scientificName* > *Text filter*
2. Close the text facet
3. Type *bai* and press <kbd>Return</kdb>
4. Change the view to show fifty rows
5. You can now see all entries with a scientific name that starts with "bai"

### Excluding Entries

1. You have two different species in this subset
2. *scientificName* > *Facet* > *Text facet*
3. You can combine filters and facets
4. Filters give you a subset of your data by filtering out other data
5. Facets give you a general overview of the currently selected data
6. Hover over the two species and *include*/*exclude* them

### Sort

- You can sort columns according to all kind of criteria by selecting `Sort...` from the columns drop down arrow
- You can also specify what order `Blanks` and `Errors` are sorted in the results

> Exercise: Sort by month. How can you ensure that months are in order?
>
> Ensure you are sorting by number and not alphabetically.

- If you already have a sort in place, the menu changes to remind you that you have already set a sort
- You get additional options...
  - *Sort* > *Sort...* - Modify the original sort
  - *Sort* > *Reverse* - Reverse the order
  - *Sort* > *Remove sort* - Undo your sort

### Sorting by multiple columns

- You can sort by multiple columns by repeating this process for each column
- The order of the sort will depend on the order you set the sorts
- To restart the sorting process, check the *sort by this column alone* box in the *Sort* pop-up menu

1. *mo* > *Sort* > *Remove sort*

> Exercise: Sort by `year`, `month`, and `day` in that order. Try putting some in reverse order. Use *mo* > *Sort* > *Remove sort*. Notice how it changes the order. Remove all your sorts once your are finished.

## Examining numbers in OpenRefine

**Questions:**

- How can we convert a column from one data type to another?
- How can we visualize relationships among columns?


**Objectives:**

- Transform a text column into a number column.
- Identify and modify non-numeric values in a column using facets.
- Use the scatter plot facet to examine relationships among columns.

### Numbers

- OpenRefine treats all imported values as text values by default
- We can change a column's type by using *Edit cells* > *Common transforms*

1. Remove any facets and filters
2. *recordID* > *Edit cells* > *Common transforms...* > *To number*
3. Try transforming `scientificName` to numerical values. Did it work?

### Numeric facet

- Now that we have numeric values, we can work with numeric facets
- Numeric facets can help us uncover non-number values or blanks

1. Change a few values in `recordID` to letters
2. Apply a numeric facet to the column
3. Experiment with the slider and check boxes in this facet
   1. The slider is a histogram of the values in the column
4. Remove the facet and undo the non-numeric changes

### Scatter plot facet

- The scatter plot facet allows us to examine the relationship between different numeric columns

1. 

## Numbers
* By default, all values are imported into OpenRefine as text. However, we have numbers too.
1. Remove all filters and facets so we can look at all the data.
2. *recordID* > *Edit cells* > *Common transforms...* > *To number*
3. Transform three more columns, including *period*.

## Numeric Facet
* Numeric facets have some neat features that we don't get with text facets.
* One feature is that we can see how columns relate to each other using scatter plots.
1. *recordID* > *Facet* > *Scatterplot facet*
* We can click on one of the scatter plots to select that particular facet.
1. Click and draw a box on the scatter plot. This will select matching entries in the data.

## Scripts from OpenRefine
* One nice aspect of OpenRefine is that I can reproduce the same steps I just did on another version of similar data.
1. *Undo/Redo* > *Extract...*
2. Copy the JSON code into a text editor.
3. Start a new project and import the uncleaned version of the data set that we started with.
4. *Undo/Redo* > *Apply*
5. Paste the contents of the `.txt` file.
6. *Perform operations*.

## Saving and Exporting
* OpenRefine continually saves by default (similar to other Google software).
1. Click *Open*.
3. Select *Project* instead of importing a data file.
* You can also export projects to be shared with colleagues.
* `.tar.gz` is a special type of compressed file (like `.zip`) and you may need a special `.zip` program to open it. OpenRefine will automatically handle this when you import the project.
1. Start a new project.
2. *Import Project*
* Using these methods, the entire OpenRefine project and its accompanying files are available to you.
* You can also export the cleaned data.
1. *Export* > *Comma-separated value*
