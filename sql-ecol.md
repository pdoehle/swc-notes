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


