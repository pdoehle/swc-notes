# SQL for Ecology
## Preparation Note
- Have a tab open with the [table](datacarpentry.org/sql-ecology-lesson/00-sql-introduction/index.html) of data types for people to select during the first section.
- Items for Etherpad:
	- [Dataset Page]( https://figshare.com/articles/Portal_Project_Teaching_Database/1314459 ):  https://figshare.com/articles/Portal_Project_Teaching_Database/1314459 
	- [Dataset Download](https://ndownloader.figshare.com/files/2292171): https://ndownloader.figshare.com/files/2292171
	- [Carpentries Setup Page]( https://datacarpentry.org/sql-ecology-lesson/setup.html ):  https://datacarpentry.org/sql-ecology-lesson/setup.html 

## Dataset
- Time-series for a small mammal community in southern Arizona.
- Study of the effect of rodents and ants on a plant community.
- Been running almost 40 years.
- Rodents sampled on a series of 24 plots with different experimental manipulations controlling which rodents are allowed access to which plots.
- Dataset has been used in over 100 publications.
- Simplified a bit for the purposes of this workshop.

## Databases Using SQL
### Objectives
- Describe why relational database are useful.
- Create and populate a database from a text file.
- Define SQLite data types.

### Qustions
- Begin by openning the three CSV files in Excel: `surveys.csv`, `species.csv`, and `plots.csv`.
- Questions: How has the hind-foot length and weight of Dipodomys species changed over time? What is the average weight of each species, per year?
- To answer these kinds of questions, we often need to do the following types of data operations:
  - Select subsets of the data (rows and columns).
  - Group subsets of data.
  - Do math and other calculations.
  - Combine data across spreadsheets (tables).
- Ideally we want the computer to do this work for us!

### Why Use Relational Databases
- Databases keep your data separate from your analysis.
  - There is no risk of accidentally changing data when you analyze it.
  - We can rerun a query if we get new data.
- Relational databases are fast, even for large amounts of data.
- Databases improve quality control of data entry.

### Database Management Systems
- There are many different database management systems for working with relational data.
- We are going to use SQLite today.
- SQL is the language used across most database management systems.
	- **S**tructured **Q**uery **L**anguage.

### Relational Database
- Let's take a look at the preexisting database `portal_mammals.sqlite`.
- Click on the "Open Database" button, navigate to and select `portal_mammals.sqlite`
- A *relational database* is a way to store and manipulate information.
- A relational database stores data in *relations* made up of *records* with *fields*.
  - Relations are usually representented as *tables*.
    - "Database Structure"
  - Each record is usually shown as a *row*.
    - "Browse Data"
  - Each field is usually represented as a *column*.
    - "Browse Data"
  - Records usually have a unique identifier, called a *primary key*, which is stored as one of its fields.
- The "Execute SQL" tab is where we type in the queries we will be making.
- Data in tables have types. All values in a field must have the same type.
  - "Database Structure"

### Database Design
- A well-designed database adheres to the following principles:
  - Every row-column combination contains a single *atomic* value.
    - The value does not contain parts we might want to work with separately.
  - There is one field per type of information.
  - There is not redundant information.
    - Data should be split into different tables with one table per "class" of information.
    - There needs to be a shared identifier between tables, known as a *foreign key*.
    - *Foreign keys* reference a primary key in another table.
      - e.g.: Table of hospital patient info. and table with patient names and ID numbers.

### Creating a New Database
- Let's create a new database.
- "File" -> "New Database"
- Give it a name and click "Save".
- Click "Cancel" on the "Edit Table" window.
- "File" -> "Import" -> "Data from CSV file..."
- Select `surveys.csv`.
- Ensure "Column names in the first line" is checked.
- Click "OK" twice.

### Challenge
- Import the `plots` and `species` tables.
- Change the field data types according to the table displayed on the screen by clicking on the table and then clicking "Modify Table."
- We can add and remove single entries in the "Browse Data" tab.

## Basic Queries
### Objectives
- Write and build queries.
- Filter data given various criteria.
- Sort the results of a query.

### Writing my First Query
- Select only the year column from the surveys tables.

```sql
SELECT year
FROM surveys;
```
- SQL is not case sensitive. Capitalizing key words helps with readability.

- SQL statements don't end until the `;` character. Putting each part of the query on its own line also helps with readability.

- You can select more than just one field.

```sql
SELECT year, month, day
FROM surveys;
```

- We can select all the columns in a table using the wildcard character `*`.

```sql
SELECT *
FROM surveys;
```

- Instead of getting all the results back, we can look at a subset of the results to see if our query is working correctly.

```sql
SELECT *
FROM surveys
LIMIT 10;
```

### Unique Values
- If we want only unique values, we can use the key word `DISTINCT`.

```sql
SELECT DISTINCT species_id
FROM surveys;
```

- If we select more than one column, the distinct pairs are returned.

```sql
SELECT DISTINCT year, species_id
FROM surveys;
```

### Calculated Values
- We can calculate values within a query.

- If we want weight in kilograms instead of grams, we could use the following query:

```sql
SELECT year, month, day, weight/1000
FROM surveys;
```

- If the data type is integer, the database will use integer division. To force floating point division, divide by `1000.0`.

- SQL also has functions that can be used to calculate values.

- The following SQL statement rounds the results of the weight calculation to two decimal places.

```sql
SELECT plot_id, species_id, sex, weight, ROUND(weight / 1000, 2)
FROM surveys;
```

### Filtering
- We can use `WHERE` to filter results based on specific criteria.

```sql
SELECT *
FROM surveys
WHERE species_id='DM';
```

- Select all the data from `surveys` that has been collected since 2000.

```sql
SELECT *
FROM surveys
WHERE year >= 2000;
```

- We can use `AND` and `OR` to create more sophisticated conditional tests.

```sql
SELECT *
FROM surveys
WHERE (year >= 2000) AND (species_id = 'DM');
```

### Challenge
- Use `OR` to query for all the entries in the `surveys` table from the dipodomys species (species codes `DM`, `DO`, and `DS`).

```sql
SELECT *
FROM surveys
WHERE (species_id = 'DM') OR (species_id = 'DO') OR (species_id = 'DS');
```

### Building More Complex Queries
- Get data for the three dipodomys species from the year 2000 on.

- `IN` saves us from typing a complex `OR` statement.

```sql
SELECT *
FROM surveys
WHERE (year >= 2000) AND (species_id IN ('DM', 'DO', 'DS'));
```

- Notice how we added layers of complexity as we went. This is a good strategy for complex queries.

- If a query becomes very complex, it can be useful to add comments by using `--`.

```sql
-- Get post 2000 data on Dipodomys species
-- These are in the surveys table and we are interseted in all columns
SELECT * 
FROM surveys
-- Sampling year is in the column `year`, and we want to include 2000
WHERE (year >= 2000)
-- Dipodomys species have the `species_id` DM, DO, and DS
AND (species_id IN ('DM', 'DO', 'DS'));
```

### Sorting
- We can sort the results of our queries.

- Let's see how that works with the `species` table.

```sql
SELECT *
FROM species;
```

- Notice how this categorical data gets it's own table which makes managing the measured data more straightforward.

- Let's order by taxa.

```sql
SELECT *
FROM species
ORDER BY taxa ASC;
```

- We can also use descending order.

```sql
SELECT *
FROM species
ORDER BY taxa DESC;
```

- We can sort on several fields in the same query.

```sql
SELECT *
FROM species
ORDER BY genus ASC, species ASC;
```

### Order of Execution
- SQL has the following order of operations:
  1. Filter
  2. Sort
  3. Display

- We can take advantage of this, filtering and sorting our result based on a field that will not be displayed in the final output.

```sql
SELECT genus, species
FROM species
WHERE taxa = 'Bird'
ORDER BY species_id ASC;
```

## SQL Aggregation and Aliases
### Objectives
- Apply aggregation to group records in SQL.
- Filter and order results of a query based on aggregate functions.
- Employ aliases to assign new names to items in a query.
- Save a query to make a new table.

### `COUNT` and `GROUP BY`
- SQL has several aggregate functions (functions that compile all the data together and give a single result).

- We can count the number of records.

```sql
SELECT COUNT(*)
FROM surveys;
```

- We can find out how much all these individuals weigh by using `SUM()`.

```sql
SELECT COUNT(*), SUM(weight)
FROM surveys;
```

- These functions accept mathematical arguments. We can output the results in kilograms.

```sql
SELECT ROUND(SUM(weight)/1000.00, 3)
FROM surveys;
```

- Other aggregate functions include `MAX`, `MIN`, and `AVG`.

### Challenge
- Write a query that returns the total weight, average weight, minimum weight, and maximum weight for all the animals caught over the duration of the survey.

```sql
SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight)
FROM surveys;
```

- Modify your query so it  only includes the weights between 5 and 10 in its calculations.

```sql
SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight)
FROM surveys
WHERE (weight > 5) AND (weight < 10);
```

- We can use `GROUP BY` to count how many individuals are in each group.

```sql
SELECT species_id, COUNT(*)
FROM surveys
GROUP BY species_id;
```

- `GROUP BY` tells SQL what field or fields we want to use to aggregate the data.

- Don't forget we can order the results based on specific criteria.

```sql
SELECT species_id, COUNT(*)
FROM surveys
GROUP BY species_id
ORDER BY COUNT(species_id);
```

### Aliases
- We can use aliases to make results more clear.

```sql
SELECT MAX(year) AS last_surveyed_year
FROM surveys;
```

### `HAVING`
- When using aggregate functions, we use the key word `HAVING` instead of `WHERE` to conditionally select results.

```sql
SELECT species_id, COUNT(species_id)
FROM surveys
GROUP BY species_id
HAVING COUNT(species_id) > 10;
```

- Don't forget, we can use `AS` to make column headings more readable.

```sql
SELECT species_id, COUNT(species_id) AS occurrences
FROM surveys
GROUP BY species_id
HAVING occurrences > 10;
```

### Saving Queries for Future Use
- You can save queries for later by using views.

- You can think of a view as its own table that saves a query in the database for later use.

- Suppose we have a project that only deals with the data collected during the summer (from May to September) of 2000.

```sql
SELECT *
FROM surveys
WHERE year = 2000 AND (month > 4 AND month < 10);
```

- Now we save this query as a `VIEW`.

```sql
CREATE VIEW summer_2000 AS
SELECT *
FROM surveys
WHERE year = 2000 AND (month > 4 AND month < 10);
```

- The view shows up in *Database Structure* like the tables do.

- Now we can query and work with a subset of the data that we need for our project.

```sql
SELECT *
FROM summer_2000
WHERE species_id == 'PE';
```

### NULL Values
- Be very careful with NULL values. Remember that the boolean value of an expression with a NULL value is NULL!

- To get around this, we can use `IS NULL` and `IS NOT NULL`.

## Joins
### Objectives
- Employ joins to combine data from two tables.

### Joins
- We use `JOIN` (comes after `FROM`) to join information between two tables.

- `JOIN` creates a cross-product of the two tables by default which is usually not what we want. `ON` lets us specify which field we want to match between the two tables.

```sql
SELECT *
FROM surveys
JOIN species
ON surveys.species_id = species.species_id;
```

- The `.` notation tells SQL which table to select a given field name from.

- If the column you are using to join the two tables has the same name in both tables, you can use the shorthand `USING`.

```sql
SELECT *
FROM surveys
JOIN species
USING(species_id);
```

- We can use the `.` notation in the `SELECT` part of the statement as well.

```sql
SELECT surveys.year, surveys.month, surveys.day, species.genus, species.species
FROM surveys
JOIN species
ON surveys.species_id = species.species_id;
```

## SQL Database and R
### Objectives
- Access a database from R.
- Run SQL queries in R using `RSQLite` and `dplyr`.

### Connecting to Databases
- By default R loads the entire dataset you are working with into your computer's memory. This does not work well if your data set is too big.

- One of the ways to get around this is to connect R to an external database.

- You can make queries and import the subset of the data that you want to work with.

- We will use the packages `dbplyr` and `RSQLite` to connect R to a database.

- Start a new RStudio project and create a new R script.

```r
install.packages(c("dbplyr", "RSQLite"))
library(dplyr)
library(dbplyr)
```

- If students don't have the file they can re-download it using the following:

```r
dir.create("data", showWarnings = FALSE)
download.file(url = "https://ndownloader.figshare.com/files/2292171",
              destfile = "data/portal_mammals.sqlite", mode = "wb")
```

- We begin by connecting to the database.

- This command will not import the database. It just connects to the database file.

```r
mammals <- DBI::dbConnect(RSQLite::SQLite(), "~/Path/to/database")
```

- DBI is a package that allows R to connect to any database no matter what the database management system is.

- RSQLite allows R to interface with SQLite databases.

- `src_dbi` gives us information about the database.

```r
src_dbi(mammals)
```

- The `tbl` function sends queries to the database.

```r
tbl(mammals, sql("SELECT year, species_id, plot_id FROM surveys"))
```

- One of the strength of bring a database into R is that we can use the `dplyr` syntax and are no longer bound to explicit SQL statements.

```r
surveys <- tbl(mammals, "surveys")
surveys %>% 
select(year, species_id, plot_id)
```

- `surveys` is behaving like a data frame, but it's not quite a true data frame.

- Notice the output is telling us the results are from a "lazy query" and that it doesn't know how many rows are in the data frame.

- Behind the scenes, `dplyr` does the following:
  - translates your R code into SQL
  - submits it to the database
  - translates the database's response into an R data frame.

- "Lazy execution" refers to the fact that R won't do anything in the execution until it has to. In this case it pulled in enough data to give us output and then gives us the message `... with more rows`. It won't pull in the rest of the rows until it actually has to do some computation with them.
