# SQL for Ecology
##Preparation Note
Have a tab open with the table of data types for people to select during the first section.

## Databases Using SQL
### Objectives
- Describe why relational database are useful.
- Create and populate a database from a text file.
- Define SQLite data types.

### Goals
- Questions: How has the hindfoot length and weight of Dipodomys species changed over time? What is the average weight of each species, per year?
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
- There are many different datbase management systems for working with relational data.
- We are going to use SQLite today.
- SQL is the language used across most database management systems.

### Relational Database
- Let's take a look at the pre-existing database `portal_mammals.sqlite`.
- Click on the "Open Database" button, navigate to and select `portal_mammals.sqlite`
- A *relational database* is a way to store and manipulate information.
- A relational database stores dat in *relations* made up of *records* with *fields*.
  - Relations are usually represntented as *tables*.
    - "Database Structure"
  - Each record is usually shown as a *row*.
    - "Browse Data"
  - Each field is usually represented as a *column*.
    - "Browse Data"
  - Records usually have a unique identifier, called a *key*, which is stored as one of its fields.
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

!!!MAKE A SLIDE ABOUT THE ANATOMY OF A SQL QUERY!!!  

```sql
SELECT year
FROM surveys;
```
- SQl is not case sensitive. Capitalizing key words helps with readability.

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
ELECT DISTINCT year, species_id
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
-- Get post 2000 dat on Dipodomys species
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
- Apply filters to find missing values in SQL.

### `COUNT` and `GROUP BY`
- SQL has several aggregate functions (functions that compile all the data together and give a single result).

- We can count the number of records.

```sql
SELECT COUNT(*)
FROM surveys;
```

- We can find out how much all these individuals weigh by using `SUM()`.

```sql
SELECT COUNT(*), SUM(WEIGHT)
FROM surveys;
```

- We these functions accept mathematical arguments. We can output the results in kilograms.

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

- Modify your query so it outputs only the weights between 5 and 10.

```sql
SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight)
FROM surveys
WHERE (weight > 5) AND (weight < 10);
```

- We can use `GROUP BY` to count how many individuals were counted in each species.

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
