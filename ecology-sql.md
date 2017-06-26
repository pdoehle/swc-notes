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

