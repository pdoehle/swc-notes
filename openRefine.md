# What is OpenRefine
* It's a piece of software that runs in your browser and helps with the cleaning of messy data.
> If OpenRefine is running but does not automatically open in your browser, go to either `http://127.0.0.1:3333/` or `http://localhost:3333` to launch the program.
* It keeps track of each step you took with while working with the data.
	* These steps can be reported to journals, granting agencies, and can be published as supplemental material.
* All actions in OpenRefine can be undone.
* You are forced to save your work in a new file. OpenRefine does not modify your original data set.
* OpenRefine saves a lot of time in cleaning up data.
* You can repeat the steps you took with one data set on a new data set.
* Provides an intuitive interface for complex clustering algorithms that make data cleaning convenient.
* OpenRefine is open source.

## Creating a Project
* Start up OpenRefine
* Talk about different methods for importing data and starting a new project
* Browse to `Portal_rodents` and select the `.csv` file. Click *Next*.
* Talk about the features in the preview pane.
* Once all looks good, click *Create Project*.

## Faceting
* One of the primary ways that OpenRefine handles data is through faceting. Let's see an example of what that looks like.
1. Scroll over to the `scientificName` column.
2. Click the down arrow and choose `Facet` > `Text facet`.
* In the left panel, you'll now see a box containing every unique value in the `scientificName` column long with a number representing how man time that value occurs in the column.
* Notice you can sort by `name` and `count`.
> Do you notice any problems with the data?
* The facet gets right the heart of the problem. We will talk more in-depth about fixing data mistakes in a little bit.

### Question
> **Using faceting, find out how many years are represented in the survey.**
> A text facet on the `yr` column shows 26 years from 1977 to 2002 inclusive.

## Clustering
* Clustering means finding things that are similar to each other. It's great for finding entries that are different, but possibly represent the same thing.
* Clustering is a great way to tease out the kinds of spelling errors we saw with the first facet.
1. Clear our the `yr` facet and reset the `scientificName` facet.
2. Click on *Cluster*.
* We see many different kinds of algorithms that are available for clustering.
> If you want to see more on the technical details of clustering, you can check out this [website](https://github.com/OpenRefine/OpenRefine/wiki/Clustering-In-Depth "Clustering").
1. Select the `key collision` method and `metaphone3` keying function. It should identify three clusters.
2. Click the *Merge* box beside each one.
3. Click *Merge Selected and Recluster*.
* Feel free to select different methods and see what clusters you get.
* **IMPORTANT:** Don't merge anymore clusters. I know there are still errors, but we still need problems for later features.

## Split
* We can split a column into atomic data if need be.
1. Click the arrow for the `scientificName` column.
2. Choose *Edit Column* > *Split into several solumns*
3. In the pop-up, replace the comma with a space in the *separator* box.
4. Uncheck the box that says *Remove this column*.
> **Notice some of the entries are blank. What happened?**
> Leading spaces!
* We'll look at leading spaces in a few.
* Let's rename the scientificName2 column to species.
1. Click the arrow on the `scientificName2` column.
2. Click *Edit column* > *Rename this column*.
3. Rename the column to `species`.
* Notice OpenRefine does not allow two columns to have the same name. Each column must have a unique name.
* Let's change the column to `speciesName`.

## Undo/Redo
* Next to the *Facet/Filter* tab at the top, we have *Undo/Redo*.
* If we click on this, we can see every step we have done so far and revert our state back to each one.
1. Go back to the state where the scientific names were clustered, but not yet split.
* Let's go back and fix those cumbersome white spaces in the `scientificName` column.

## Trim Leading and Trailing White Space
1. *scientificName* > *Edit cells* > *Common transforms* > *Trim leading and trailing whitespace*
* Notice the *Split* step from before has now disappeared in the *Undo/Redo* history.
> **Repeat the same split operation as before (Don't forget to uncheck *Delete this Column*). Are the results different?**
> It works this time!

## Filtering
* If your facet is still on, remove it.
1. *scientificName* > *Text filter*
2. Type `bai`.
* There are 48 results. We can click on the *Show: 50* link at the top to see them all.
1. *scientificName* > *Facet* > *Text facet*.
* Notice we can combine filters and facets.
* Filters give us a subset of the data based on the criteria we set while facets give an overview description of the data currently selected.
* We can click on the names and use the *Inclue/Exclude* links to display one or the other.

## Sorting
* You can sort by different criteria.
1. *mo* > *Sort...*
2. Sort by text.
> **What's the problem with this method?**
> 10 comes after 1 when sorting by text.
1. Sort *mo* by number.
* You can sort by multiple columns.
1. Put another sort on plot by number.
> **What plots have entries for January?**
> 3, 5, and 19.
* If I want to sort by just plot, I check the box *sort by this column alone* when creating a sort.
1. Sort by only *plot*.
2. Remove all sorts by clicking on the *Sort* link at the top and selecting *Remove sort*
> **Sort by *year*, *month*, and *day*, in that order.**
* I can remove any of the sorts in any order by going back up to the *Sort* menu.

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
1. Drag a box on the scatter plot. This will filter out these particular data points from the scatter plot.

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
1. Exit.
2. Start a new project.
3. Select *Project* instead of importing a data file.
* You can also export projects to be shared with colleagues.
* `.tar.gz` is a special type of compressed file (like `.zip`) and you may need a special `.zip` program to open it. OpenRefine will automatically handle this when you import the project.
1. Start a new project.
2. *Import Project*
* Using these methods, the entire OpenRefine project and its accompanying files are available to you.
* You can also export just the cleaned data.
1. *Export* > *Comma-separated value*
