# Databases Using SQL

## Setup (If not Already Done)
* Download and install [Firefox](https://www.mozilla.org/en-US/firefox/new/).
* Install the [SQLite Manager](https://addons.mozilla.org/en-us/firefox/addon/sqlite-manager/) Add-on for Firefox.
* Download the [dataset](https://ndownloader.figshare.com/articles/1314459/versions/6) and associated files and unzip it in a convenient location.

## Motivation
Previously, we used Excel and OpenRefine to organize our data into computer-readable data. Now we will move to the next part of the data workflow, which involves reading in our data and using it for analysis and visualization.

## Dataset
The data we will be using is a time-series for a small mammal community in southern Arizona. This is part of a project studying the effects of rodents and ants on the plant community that has been running for almost 40 years. The rodents are sampled on a series of 24 plots, with different experimental manipulations controlling which rodents are allowed to access which plots.

This is a real dataset that has been used in over 100 publications. It's been simplified just a little bit for the workshop, but you can download the [full dataset](http://esapubs.org/archive/ecol/E090/118/) and work with it using exactly the same tools we’ll learn about today.

## Why Use a Database
* Databases are similar to spreadsheets, but have key differences.
* Spreadsheet
  * Directly edit cells
  * Use formulas based on other cells
* Databases
  * Send commands (called *queries*) to a database manager (we will use sqlite3).
  * The database manager manipulates data for us
  * *manager does lookups, calculations, and queries*
  * Returns results in a tabular form
* So why go through the trouble of using a database?
  * Keep data separate from analysis.
    * Easy to reproduce results.
    * No risk of accidentally changing data while running analysis.
  * Fast (even for large amounts of data)
  * Improves quality control of data entry by forcing data types

## Looking at the Data
Let's begin by looking at the data. Open the following files in a spreadsheet program: `surveys.csv`, `species.csv`, and `plots.csv`.

> **Question:**
> What files and information would I need for the following questions:
> * How has the hindfoot length and weight of the animals belonging to the *Dipodomys* genus changed over time?
> * What is the average weight of each species per year?
> What operations would I need to perform if I wre doing these analyses by hand?

> **Answer:**
> Here are some of the basic data operations we will need:
> * select subsets of the data (rows and columns)
> * group subsets of data
> * do math and other caculations
> * combine data across spreadsheets

The strength of a database is that it will automate these tasks for us in a way that we can replicate in case our data changes later.

## Looking at SQLite Manager
Let's begin by looking at a pre-made database from these `.csv` files.
* Open `portal_mamals.sqlite` in the SQLite 3 manager.
* Databases contain tables (see left-hand menu).
* To see the contents of a table, click on the table name and click the "Browse & Search" tab.
* On the face of it, a database simply feels like a collection of tables.
* A database also allows us to create relationship between different entries in different tables.
* Rows are refered to as *records*.
* Each record has a *field* (columns).
* Each record has a unique identifier, called a *primary key*.
* Records may have keys that refer to data in other tables.
* This allows us to create relationship between different keys in different tables.
* The "Structure" tab tells us information about the structure of the table itself.
* We see the **S**tructured **Q**uery **L**anguage statement that would create the original table under "Create statement."
* We have information about the columns of the table under the "Columns" section.
* The "Execute SQL" tab is where we can input SQL commands and queries.
* We can change general database settings in the "DB Settings" tab.

## Principles of Database Design
* Every record's fields should have atomic values (can't be broken down any further)
  * *See month, day, and year in the surveys table*
* In general it is easier to put fields together than to split them apart.
* There should be one field per type of information.
* Split data into separate tables with each table corresponding to a different class of information.
* In order to relate infomation between tables, there should be a shared column.
* Recall that each record has a *primary key* which serves as its "address."
* Fields that refer to primary keys in other tables are called *foreign keys*.

## Creating Our Own Database
Let's see how to recreate this database from scratch using the original `.csv` files.

1. **Database -> New Database**
2. **Database -> Import**
3. Select the `surveys.csv` file to import.
4. Check "First row contains column names".
5. Make sure that "Ignore Trailing Separator/Delimiter" is unchecked.
6. When asked if you want to modify the table click **OK**.
7. Change the data types for each column according the table in the handout.
8. Repeat this process to create tables for `plots.csv` and `surveys.csv`.

> Notice that I can add new records by going to the "Browse & Search" tab and using the "Add" button at the upper-right-hand corner.

# Writing SQL Queries
Let's begin by using the **surveys** table. Click on the "Execute SQL" tab.

```sql
SELECT year FROM surveys;
```

The result is all the entries from the *year* field in the *surveys* table.

Every SQL command must end with `;`. SQL will not consider the command completed until it finds `;`. That means we can write the same command as follows:

```sql
SELECT year
FROM surveys;
```

This means that as our queries become more complex, we can spread them accross several lines to make them more readable.

It is also good practice to capitalize the key words in a SQL command so that it is easier to read.

We can get more information about the date by adding more columns to our query.

```sql
SELECT year, month, day
FROM surveys;
```

Finally, I can use the wildcard `*` to select all the columns from a table.

```sql
SELECT *
FROM surveys;
```

# Unique Values
We can specify only unique values by using the `UNIQUE` keyword.

```sql
SELECT species_id
FROM surveys;
```

```sql
SELECT DISTINCT species_id
FROM surveys;
```

If we select more than one column, then the distinct pairs are returned.

```sql
SELECT DISTINCT year, species_id
FROM surveys;
```

> **Challenge:** Find the distinct years in which measurements were taken for the the surveys.

> **Answer:** `SELECT DISTINCT year FROM surveys;`

## Calculated Values
We can also do calculations with the values in a query.

```sql
SELECT year, month, day, weight/1000
FROM surveys;
```

We now have wieght in kilograms instead of grams.

## Filtering
We can also select data based on certain criteria. For example, if we only want data for the species *Dipodomys merriami*, we can use the `WHERE` key word.

```sql
SELECT *
FROM surveys
WHERE species_id='DM';
```

We can also set conditions based on numeric data that meets a certain condition.

```sql
SELECT * FROM surveys
WHERE year >= 2000;
```

Notice, we have all the columns for the years both including and after 2000.

SQL also allows us to filter criteria using the key words `AND` and `OR`.

```sql
SELECT *
FROM surveys
WHERE (year >= 2000) AND (species_id = 'DM');
```

This query gives us all the columns from the `surveys` table where the year is 2000 or later and the species is *Dipodomys merriami.*

If we want a query that returns information for any of the *Dipodomys* genus, we can use all three species codes in the following query.

```sql
SELECT *
FROM surveys
WHERE (species_id = 'DM') OR (species_id = 'DO') OR (species_id = 'DS');
```

> **Question:** Write a query that returns the day, month, and species_id for individuals caught on Plot 1 that weight more than 75 grams.

> **Answer:** `SELECT day, month, species_id FROM surveys WHERE (plot_id=1) AND (weight>75);`

## Building More Complex Queries
We simplify the above query if we have a list of criteria by using the `IN` statement.

Before:
```sql
SELECT *
FROM surveys
WHERE (species_id = 'DM') OR (species_id = 'DO') OR (species_id = 'DS');
```

After:
```sql
SELECT *
FROM surveys
WHERE (year >= 2000) AND (species_id IN ('DM', 'DO', 'DS'));
```

When writing more complex queries, it is helpful to start simple and slowly add more elements to the query making sure that each part works.

It can also helpful to build your queries using a smaller subset of the data, making sure everything works before using your code on the full database.

Finally, making human-readable comments is also helpful.

```sql
-- Get post 2000 data on Dipodomys' species
-- These are in the surveys table, and we are interested in all columns
SELECT * FROM surveys
-- Sampling year is in the column `year`, and we want to include 2000
WHERE (year >= 2000)
-- Dipodomys' species have the `species_id` DM, DO, and DS
AND (species_id IN ('DM', 'DO', 'DS'));
```

## Sorting
We can sort our results by using the `ORDER BY` key words. Lets look at our **species** table.

```sql
SELECT *
FROM species;
```

We can order our results by *taxa*.

```sql
SELECT *
FROM species
ORDER BY taxa ASC;
```

Notice the keyword `ASC` which is short for ascending. We can also display results in descending order.

```sql
SELECT *
FROM species
ORDER BY taxa DESC;
```

We could also order by genus and then species.

```sql
SELECT *
FROM species
ORDER BY genus ASC, species ASC;
```

> **Question:** Write a query that returns *year*, *species_id*, and *weight* in kilograms from the surveys table, sorted with the largest weights at the top.

> **Answer:** `SELECT year, species_id, weight/1000 FROM surveys ORDER BY weight DESC;`

We can even sort by a column that does not appear in the final results.

```sql
SELECT genus, species
FROM species
WHERE taxa = 'Bird'
ORDER BY species_id ASC;
```

The `WHERE` and `ORDER BY` statements are executed before the `SELECT` statement, allowing us to sort by fields that don't even appear in the final results.

## SQL Aggregation

Aggregate functions combine results from multiple multiple values to produce a single piece of data.

Some of the aggregate functions SQL has available:
* `COUNT` - counts a set of records
* `SUM` - sum
* `MAX` - maximum
* `MIN` - minimum
* `AVG` - average

Let's count the number of records in *surveys*

```sql
SELECT COUNT(*)
FROM surveys;
```

By using `*`, we are telling the `COUNT()` function to count the total number of records.

Let's find out how much all of those individuals weight.

```sql
SELECT COUNT(*), SUM(weight)
FROM surveys;
```

> **Question:** Write a query that returns: toatl weight, average weight, and the min and max weights for all animals caught over the duration of the survey.

> **Answer:** `SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight) FROM surveys;`

> **Question:** Can you modify it so that it outputs these values only for weights etween 5 and 10?

> **Answer:** `SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight)`
> `FROM surveys WHERE (weight>=5) AND (weight<=10);`

We can group our data by a specific category before executing an aggregate command. For example, if we want to count the number of individuals captured by the *species_id*, we could use the `GROUP BY` command.

```sql
SELECT species_id, COUNT(*)
FROM surveys
GROUP BY species_id;
```

## The HAVING Keyword
We can also sort results by using the keyword `HAVING` in conjunction with an aggregate command.

```sql
SELECT species_id, COUNT(species_id)
FROM surveys
GROUP BY species_id
HAVING COUNT(species_id) > 10;
```

`HAVING` works exactly the same as `WHERE`, but instead of using fields, we use aggregate functions.

> **Question:** Write a query that returns the number of *genus* in each *taxa* from the *species* table. Return only the *taxa* with more than 10 *genus*.

> **Answer:** `SELECT taxa, COUNT(genus) FROM species GROUP BY taxa HAVING COUNT(genus) > 10;`

## Saving Queries for Future Use

We can save a query for future use by using the `CREATE VIEW` command. If we wanted to look at data from May to September during the year 2000, we could use the following query.

```sql
SELECT *
FROM surveys
WHERE year = 2000 AND (month > 4 AND month < 10);
```

We can save this query for later as a *view* called *summer_2000*.

```sql
CREATE VIEW summer_2000 AS
SELECT *
FROM surveys
WHERE year = 2000 AND (month > 4 AND month < 10);
```

We can now directly reference the view as if it were its own table.

```sql
SELECT *
FROM summer_2000;
```

## Handling NULL Values
Values that are `NULL` are SQL's way of representing an unknown value. The default in the SQLite database manager is to highlight these cells in pink. Let's examine how the database manager deals with `NULL` values by counting the number of male and female individuals.

```sql
SELECT COUNT(*)
FROM summer_2000
WHERE sex != 'F';
```

We see there are 375 male individuals.

```sql
SELECT COUNT(*)
FROM summer_2000
WHERE sex != 'M';
```

This query tells us there are 360 female individuals. Counting the total, we should have 735 individuals total.

Let's write a query to check ourselves.

```sql
SELECT COUNT(*)
FROM summer_2000;
```

Why do we end up with a count of 781 individuals total. This is because there were 51 individuals who's sex was not recorded and so these values were `NULL` in the database. Since `NULL` is the database's version of "I don't know," it could not be determined whether or not these records matched the search criteria and so they were not included in the results.

> **Things to keep in mind about `NULL` values:**
> * Any basic math operation in which one of the records is `NULL` will always return an answer of `NULL`.
> * Aggregate functions ignore `NULL` values.

## Joins

The `JOIN` command allows us to combine information from two different tables. By default the `JOIN` command produces a cross product.

```sql
SELECT *
FROM plots
JOIN species;
```

This is typically not very useful, and so we need to specify how we want the information joined using `ON`.

```sql
SELECT *
FROM surveys
JOIN species
ON surveys.species_id = species.species_id;
```

Now we have joined information from the *surveys* tables and the *species* table so that records that have the same *species_id* are matched together. Notice that we can use "dot" notation to specify a field within a specific table.

All the fields from the first table are listed, and then all the fields from the second table are listed. This means that *species_id* gets listed twice. To avoid this, we can use the `USING` command.

```sql
SELECT *
FROM surveys
JOIN species
USING (species_id);
```

We might not want all the fields from both backgrounds. For example, we might want information on when an individual was captured, but instead of the *species_id*, we want the actual *genus* and *species*.

```sql
SELECT surveys.year, surveys.month, surveys.day, species.genus, species.species
FROM surveys
JOIN species
ON surveys.species_id = species.species_id;
```

## Putting It All Together

Let's put all this together. Suppose we want the average mass for individuals on each different kind of plot.

```sql
SELECT plots.plot_type, AVG(surveys.weight)
FROM surveys
JOIN plots
ON surveys.plot_id = plots.plot_id
GROUP BY plots.plot_type;
```

## Functions

SQL has many functions that allow us to manipulate data that we query. We're already seen several aggregate functions like `SUM` and `COUNT`. Another useful function is `IFNULL`.

```sql
SELECT species_id, sex, IFNULL(sex, 'U')
FROM surveys;
```

> **Question:** Write a query that returns 30 instead of `NULL` for values in the `hindfoot_length` column.
> **Answer:** `SELECT IFNULL(hindfoot_length, 30) FROM surveys;`

The opposite of `IFNULL` is `NULLIF`. This function returns `NULL` if the first argument is equal to the second argument.

We can "null out" plot 7:

```sql
SELECT species_id, plot_id, NULLIF(plot_id, 7)
FROM surveys;
```

## Aliases
Aliases are a nice tool for making the labels of our columns easier to read. To change the label of a column, we use the `AS` keyword.

Recall our earlier query where we changed all the unknown sexes to 'U'.

```sql
SELECT species_id, sex, IFNULL(sex, 'U')
FROM surveys;
```

Notice the third column is difficult to read. We can change our query as follows to relabel this column.

```sql
SELECT species_id, sex, IFNULL(sex, 'U') AS non_null_sex
FROM surveys;
```

## Some Final Thoughts
> **Question:** The purpose of SQL is to be able to ask specific questions about our data. With a partner, translate the following questions into a SQL query.
> * How many plots from each type are there?
> * What is the average weight of each taxa?
> * How many specimens of each sex are there for each year?
> * 

> **How many plots from each type are there?**
> `SELECT plot_type, COUNT(*) AS num_plots FROM plots GROUP BY plot_type ORDER BY num_plots DESC;`

> **What is the average weight of each taxa?**
> `SELECT species.taxa AS taxa, AVG(surveys.weight) AS average_weight FROM surveys JOIN species ON species.species_id = surveys.species_id GROUP BY taxa;`

> **How many specimens of each sex are there for each year?**
> `SELECT year, IFNULL(sex, "unknown") AS sex, COUNT(*) AS num_animals FROM surveys GROUP BY year, sex;`
