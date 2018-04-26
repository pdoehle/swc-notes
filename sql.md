# SQL

## Selecting Data
> * Explain the difference between a table, a record, and a field.
> * Explain the difference between a database and a database manager.
> * Write a query to select all values for specific fields from a single table.


### Relational Database
A *relational database* is a way to store and manipulate information.

* A relational database stores dat in *relations* made up of *records* with *fields*.
  * Relations are usually represntented as *tables*.
  * Each record is usually shown as a *row*.
  * Each field is usually represented as a *column*.
  * Records usually have a unique identifier, called a *key*, which is stored as one of its fields.
---

### Databases and Spreadsheets
* Spreadsheet
  * Directly edit cells
  * Use formulas based on other cells
* Databases
  * Send commands (called *queries*) to a database manager
  * The database manager manipulates data for us
  * *manager does lookups, calculations, and queries*
  * Returns results in a tabular form
---

### Slides SQL

* Queries are written in SQL
* **S**tructured **Q**uery **L**anguage
* *We will only look at a few ways SQL queries*
* *Should cover the majority of what most researchers do with their data.*
---

### Arctic Data

* *Handouts of data to help people visualize*
* **Person:** the people who took readings
* **Site:** the locations where readings were taken
* **Visited:** the date when readings were taken at a specific site
* **Survey:** the actual readings
---

### Person

|id | personal | family |
| --- | --- | --- |
| dyer | William | Dyer |
| pb | Frank | Pabodie |
| lake | Anderson | Lake |
| roe | Valentina | Roerich |
| danforth | Frank | Danforth |
---

### Site

|name | lat | long |
| --- | --- | --- |
| DR-1 | -49.85 | -128.57 |
| DR-3 | -47.15 | -126.72 |
| MSK-4 | -48.87 | -123.4 |
---

### Visited
    
| id | site | dated |
| --- | --- | --- |
| 619 | DR-1 | 1927-02-08 |
| 622 | DR-1 | 1927-02-10 |
| 734 | DR-3 | 1930-01-07 |
| 735 | DR-3 | 1930-01-12 |
| 751 | DR-3 | 1930-02-26 |
| 752 | DR-3 | -null- |
| 837 | MSK-4 | 1932-01-14 |
| 844 | DR-1 | 1932-03-22 |
---

### Survey

|taken | person | quant | reading |
| --- | --- | --- | --- |
|619 | dyer | rad | 9.82 |
|619 | dyer | sal | 0.13 |
| $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ |
|837 | lake | sal | 0.21 |
| 837 | roe | sal | 22.5 |
| 844 | roe | rad | 11.25 |
---

Begin by moving to the directory where the database file is saved.

```bash
cd ~/Desktop
```

To open the database in SQL type

```bash
sqlite3 survey.db
```

You should have a prompt that looks like the following:

```bash
SQLite version 3.13.0 2016-05-18 10:57:30
Enter ".help" for usage hints.
sqlite> 
```

SQL is now ready for us to make queries of the database. Note, some helpful commands:

```bash
.exit # Quit SQL
.quit # Same
.help # Get help with ``Dot'' commands
```

All SQLite commands are prefixed with a dot to distinguish them form SQL commands

Let's begin by getting a list of tables ...

```sql
.tables
```

```sql
Person   Site     Survey   Visited
```

We're going to change some of the settings to make the output easier to read.

```sql
sqlite> .mode column
sqlite> .header on
```

This will make columns display left-aligned and make sure that column headers are printed.

Let's begin by creating a SQL query that displays scientists' names. It will help to reference the piece of paper with the database printed on it.

```sql
sqlite> SELECT family, personal FROM Person;	
family      personal  
----------  ----------
Dyer        William   
Pabodie     Frank     
Lake        Anderson  
Roerich     Valentina 
Danforth    Frank 
```

SQL is not case-sensitive. I can write the same command as follows:

```sql
sqlite> select family, personal from Person;
family      personal  
----------  ----------
Dyer        William   
Pabodie     Frank     
Lake        Anderson  
Roerich     Valentina 
Danforth    Frank 
```

This can be challenging for complex SQL queries. A common convention is to use upper-case letters for SQL statements in order to distinguish them from table and column names. That is the convention we will use today.

Also, notice that all the commands end with `;`. This lets SQL know that the command is complete. If we forget, we can get ...

```sql
sqlite> SELECT family, personal FROM Person
   ...> 
   ...> 
   ...> 
   ...> 
```

If this happens, type `;` and press enter.

```sql
   ...> ;
family      personal  
----------  ----------
Dyer        William   
Pabodie     Frank     
Lake        Anderson  
Roerich     Valentina 
Danforth    Frank     
sqlite> 
```

Unlike working in a spreadsheet, we do not have to think of columns and rows as being stored in a particular order. We can display in whatever order we want.

```sql
sqlite> SELECT personal, family FROM Person;
personal    family    
----------  ----------
William     Dyer      
Frank       Pabodie   
Anderson    Lake      
Valentina   Roerich   
Frank       Danforth 
```

We can even repeat columns.

```sql
sqlite> SELECT id, id, id FROM Person;
id          id          id        
----------  ----------  ----------
dyer        dyer        dyer      
pb          pb          pb        
lake        lake        lake      
roe         roe         roe       
danforth    danforth    danforth 
```

We can use wild cards like the bash shell.

```sql
sqlite> SELECT * FROM Person;
id          personal    family    
----------  ----------  ----------
dyer        William     Dyer      
pb          Frank       Pabodie   
lake        Anderson    Lake      
roe         Valentina   Roerich   
danforth    Frank       Danforth
```

## Sorting and Removing Duplicates
> * Write queries that display results in a particular order.
> * Write queries that eliminate duplicate values from data.


As we begin an exploration of our Antarctic data, we want to consider the following:

* What kind of quantity measurements were taken at each site (*see the Survey table*)?
* Which scientists took measurements on the expedition?
* Where did the scientists take their measurements?
---

Let's begin by looking at the type of quantity measurements taken during the survey.

```sql
sqlite> SELECT quant FROM Survey;
quant     
----------
rad       
sal       
rad       
sal       
rad       
sal       
temp      
rad       
sal       
temp      
rad       
temp      
sal       
rad       
sal       
temp      
sal       
rad       
sal       
sal       
rad 
```

Notice there is a lot of redundant data, making it hard to tell how many unique categories there are. To eliminate the redundant output, we can add the `DISTINCT` key word.

```sql
sqlite> SELECT DISTINCT quant from Survey;
quant     
----------
rad       
sal       
temp 
```

We can now easily see that there are three types of quantity measurements that were taken.

We can see which sites have which `quant` measurements by using `DISTINCT` and querying multiple columns.

```sql
sqlite> SELECT DISTINCT taken, quant FROM Survey;
taken       quant     
----------  ----------
619         rad       
619         sal       
622         rad       
622         sal       
734         rad       
734         sal       
734         temp      
735         rad       
735         sal       
735         temp      
751         rad       
751         temp      
751         sal       
752         rad       
752         sal       
752         temp      
837         rad       
837         sal       
844         rad
```

Notice that distinct *pairs* are selected. Notice that 752 in the `taken` column had four entries initially, but we now only have the three unique pairs.

> How can we get a list of the scientists on the expedition?
>
> *Answer:* `SELECT * FROM Person`

Remember that database entries are not bound to any specific ordering. This gives us the flexibility to define how we want results arranged. Let's order our scientists by `id`.

```sql
sqlite> SELECT * FROM Person ORDER BY id;
id          personal    family    
----------  ----------  ----------
danforth    Frank       Danforth  
dyer        William     Dyer      
lake        Anderson    Lake      
pb          Frank       Pabodie   
roe         Valentina   Roerich 
```

By default, results are displayed in ascending order.

```sql
sqlite> SELECT * FROM Person ORDER BY id ASC;
id          personal    family    
----------  ----------  ----------
danforth    Frank       Danforth  
dyer        William     Dyer      
lake        Anderson    Lake      
pb          Frank       Pabodie   
roe         Valentina   Roerich 
```

However, we can also display results in descending order.

```sql
sqlite> SELECT * FROM Person ORDER BY id DESC;
id          personal    family    
----------  ----------  ----------
roe         Valentina   Roerich   
pb          Frank       Pabodie   
lake        Anderson    Lake      
dyer        William     Dyer      
danforth    Frank       Danforth
```

We can also sort results within each column.

```sql
sqlite> SELECT taken, person, quant FROM Survey ORDER BY taken ASC, person DESC;
taken       person      quant     
----------  ----------  ----------
619         dyer        rad       
619         dyer        sal       
622         dyer        rad       
622         dyer        sal       
734         pb          rad       
734         pb          temp      
734         lake        sal       
735         pb          rad       
735                     sal       
735                     temp      
751         pb          rad       
751         pb          temp      
751         lake        sal       
752         roe         sal       
752         lake        rad       
752         lake        sal       
752         lake        temp      
837         roe         sal       
837         lake        rad       
837         lake        sal       
844         roe         rad 
```

> Check out `752`.

This gives us a good idea about which scientists were at which site.

> Can you come up with a query that allows us to examine which scientists performed which measurements by selecting the right columns and removing duplicates?
>
> *Answer:* `SELECT DISTINCT quant, person FROM Survey ORDER BY quant ASC;`

## Filtering
> * Write queries that select records that satisfy user-specified conditions.
> * Explain the order in which the clauses in a query are executed.


One of the places databases shine is their ability to filter data based on certain criteria. One example of this in SQL is the `WHERE` keyword. Let's query all the `DR-1` sites.

```sql
sqlite> SELECT * FROM Visited WHERE site='DR-1';
id          site        dated     
----------  ----------  ----------
619         DR-1        1927-02-08
622         DR-1        1927-02-10
844         DR-1        1932-03-22
```

The database manager does a two-step search when executing this command. First, it checks each row for rows that contain `DR-1`. Next, it selects columns based on the `SELECT` part of the command. This allows us to make queries that filter based on records that will not actually appear in the final results.

```sql
sqlite> SELECT id FROM Visited WHERE site='DR-1';
id        
----------
619       
622       
844
```

We can use the keywords `AND` and `OR` to make our `WHERE` searches more complex.

```sql
sqlite> SELECT * FROM Visited WHERE site='DR-1' AND dated<'1930-01-01';
id          site        dated     
----------  ----------  ----------
619         DR-1        1927-02-08
622         DR-1        1927-02-10
```

Here we have filtered results based on `DR-1` sites and sites that were visited before January 1, 1930.

> How would we find out what measurements were taken by either Lake or Roerich?
>
> *Answer:* `SELECT * FROM Survey WHERE person='lake' OR person='roe';`

Another way we could do the query from the question above is to use `IN`.

```sql
sqlite> SELECT * FROM Survey WHERE person IN ('lake', 'roe');
taken       person      quant       reading   
----------  ----------  ----------  ----------
734         lake        sal         0.05      
751         lake        sal         0.1       
752         lake        rad         2.19      
752         lake        sal         0.09      
752         lake        temp        -16.0     
752         roe         sal         41.6      
837         lake        rad         1.46      
837         lake        sal         0.21      
837         roe         sal         22.5      
844         roe         rad         11.25 
```

Notice that this notation is nice if we have more than two names that we are looking for.

We can combine `AND` and `OR`.

```sql
sqlite> SELECT * FROM Survey WHERE quant='sal' AND (person='lake' OR person='roe');
taken       person      quant       reading   
----------  ----------  ----------  ----------
734         lake        sal         0.05      
751         lake        sal         0.1       
752         lake        sal         0.09      
752         roe         sal         41.6      
837         lake        sal         0.21      
837         roe         sal         22.5
```

Notice that we have to be careful with parenthesis. Look what happens when we don't use the parenthesis.

```sql
sqlite> SELECT * FROM Survey WHERE quant='sal' AND person='lake' OR person='roe';
taken       person      quant       reading   
----------  ----------  ----------  ----------
734         lake        sal         0.05      
751         lake        sal         0.1       
752         lake        sal         0.09      
752         roe         sal         41.6      
837         lake        sal         0.21      
837         roe         sal         22.5      
844         roe         rad         11.25
```

Notice the last row of these results. We got results that were `sal` and `lake`, or any of the `roe` results (even those that were not `sal` measurements).

We can make queries based on partial matches as well. In SQL `%` functions as a wild card just like `*` does for the shell. Let's query all the sites that begin with 'DR'.

```sql
sqlite> SELECT * FROM Visited WHERE site LIKE 'DR%';
id          site        dated     
----------  ----------  ----------
619         DR-1        1927-02-08
622         DR-1        1927-02-10
734         DR-3        1930-01-07
735         DR-3        1930-01-12
751         DR-3        1930-02-26
752         DR-3                  
844         DR-1        1932-03-22
```

Notice that we used the keyword `LIKE` as a part of our `WHERE` search. Finally, we can see all the different types of measurements taken by either `lake` or `roe`, by once again using `DISTINCT`.

```sql
sqlite> SELECT DISTINCT person, quant FROM Survey WHERE person='lake' OR person='roe';
person      quant     
----------  ----------
lake        sal       
lake        rad       
lake        temp      
roe         sal       
roe         rad 
```

Just as we would with a complex bash pipe and filter setup, we start with simple SQL queries and add pieces until we have built up a complex SQL query that we are certain works correctly. If we have a large database, queries can take time and this can prove to be too time-consuming. One strategy is to create a database that is a subset and then construct the appropriate queries. Once you know it works, then you can use it with the full data set.

## Calculating New Values
> * Write queries that calculate new values for each selected record.


After carefully re-reading the expedition logs, we realize that the radiation measurements they report may need to be corrected upward by 5%. Instead of modifying the stored data, we can do this calculation as part of our query.

```sql
sqlite> SELECT 1.05 * reading FROM Survey WHERE quant='rad';
1.05 * reading
--------------
10.311        
8.19          
8.8305        
7.581         
4.5675        
2.2995        
1.533         
11.8125 
```

We can use all of the typical arithmetic operators with column headings. Let's convert all the temperature readings in `Survey` from Fahrenheit to Celsius. Let's build our query in two steps. First, pull out all the temperature readings and where they were taken. Then modify the query to produce the same results in Celsius.

```sql
sqlite> SELECT taken, reading FROM Survey WHERE quant='temp';
taken       reading   
----------  ----------
734         -21.5     
735         -26.0     
751         -18.5     
752         -16.0     
sqlite> SELECT taken, round(5*(reading-32)/9, 2) FROM Survey WHERE quant='temp';
taken       round(5*(reading-32)/9, 2)
----------  --------------------------
734         -29.72                    
735         -32.22                    
751         -28.06                    
752         -26.67
```

The names of columns in our output are looking pretty bad. Fortunately, SQL gives us a way to change this.

```sql
sqlite> SELECT taken, round(5*(reading-32)/9, 2) AS Celsius FROM Survey WHERE quant='temp';
taken       Celsius   
----------  ----------
734         -29.72    
735         -32.22    
751         -28.06    
752         -26.67
```

We can also combine values from different fields.

```sql
sqlite> SELECT personal || ' ' || family FROM Person;
personal || ' ' || family
-------------------------
William Dyer             
Frank Pabodie            
Anderson Lake            
Valentina Roerich        
Frank Danforth 
```

## Missing Data
> * Explain how databases represent missing information.
> * Write queries that handle missing information correctly.

Missing data is represented in a database by a special value called `null`. We have to be a little bit careful when dealing with `null`.

Let's look at the `Visited` table.

```sql
sqlite> SELECT * FROM Visited;
id          site        dated     
----------  ----------  ----------
619         DR-1        1927-02-08
622         DR-1        1927-02-10
734         DR-3        1930-01-07
735         DR-3        1930-01-12
751         DR-3        1930-02-26
752         DR-3                  
837         MSK-4       1932-01-14
844         DR-1        1932-03-22
```

Let's select the records that came before 1930.

```sql
sqlite> SELECT * FROM Visited WHERE dated<'1930-01-01';
id          site        dated     
----------  ----------  ----------
619         DR-1        1927-02-08
622         DR-1        1927-02-10
```

Now let's look at the records that came after 1930.

```sql
sqlite> SELECT * FROM Visited WHERE dated>='1930-01-01';
id          site        dated     
----------  ----------  ----------
734         DR-3        1930-01-07
735         DR-3        1930-01-12
751         DR-3        1930-02-26
837         MSK-4       1932-01-14
844         DR-1        1932-03-22
```

Notice that site id `752` didn't show up in the dates before or after 1930. This is because we don't know the date for this entry so we don't know if it is before or after 1930. Since databases represent "I don't know" with `null`, the answer to any operation that involves `null` is always `null`. Recall that `WHERE` is looking for all the values that are `TRUE` for a given condition. This is a problem when we want to find all the `NULL` entries.

> What will happen when we input the following expression?
> `sqlite> SELECT * FROM Visited WHERE dated=NULL;`

Notice that when we input the above query, we don't get any results. That's because any operation with `NULL` is `NULL`, not `TRUE`! We see the same when testing whether or not a value is not `NULL`.

```sql
sqlite> SELECT * FROM Visited WHERE dated!=NULL;
```

This is a problem. Sometimes we really want all the `NULL` or non-`NULL` values. There is a special search criteria we can use with `WHERE` just for this situation.

```sql
sqlite> SELECT * FROM Visited WHERE dated IS NULL;
id          site        dated     
----------  ----------  ----------
752         DR-3 
```

Similarly, we can query all of the non-`NULL` values.

```sql
sqlite> SELECT * FROM Visited WHERE dated IS NOT NULL;
id          site        dated     
----------  ----------  ----------
619         DR-1        1927-02-08
622         DR-1        1927-02-10
734         DR-3        1930-01-07
735         DR-3        1930-01-12
751         DR-3        1930-02-26
837         MSK-4       1932-01-14
844         DR-1        1932-03-22
```

`NULL` values can cause pitfalls when working with databases. Consider an example where we want to find all the salinity measurements in `Survey` that were not taken by Lake.

```sql
sqlite> SELECT * FROM Survey WHERE (quant='sal' AND person!='lake');
taken       person      quant       reading   
----------  ----------  ----------  ----------
619         dyer        sal         0.13      
622         dyer        sal         0.09      
752         roe         sal         41.6      
837         roe         sal         22.5 
```

Unfortunately, this query did not include records where we don't know who took the reading.

```sql
sqlite> SELECT * FROM Survey WHERE quant='sal' AND (person!='lake' OR person IS NULL);
taken       person      quant       reading   
----------  ----------  ----------  ----------
619         dyer        sal         0.13      
622         dyer        sal         0.09      
735                     sal         0.06      
752         roe         sal         41.6      
837         roe         sal         22.5 
```

Depending on our situation, we will have to make a judgement on whether or not we want to include the `NULL` value. Since we don't know who took the reading, there is always a chance that it was done by Lake.

## Aggregation
> * Define aggregation and give examples of its use.
> * Write queries that compute aggregated values.
> * Explain how missing data is handled during aggregation.

SQL has many different aggregate functions. By aggregate functions, we mean functions like `min` and `max` that take several entries and create a single aggregate result.

> How do we query all the `dated` entries from the `Visited` table?
> sqlite> `SELECT dated FROM Visited;`

Let's practice a few aggregate functions.

**min**

```sql
sqlite> SELECT min(dated) FROM Visited;
min(dated)
----------
1927-02-08
```

**max**

```sql
sqlite> SELECT avg(reading) FROM Survey WHERE quant='sal';
avg(reading)    
----------------
7.20333333333333
```

**avg**

```sql
sqlite> SELECT avg(reading) FROM Survey WHERE quant='sal';
avg(reading)    
----------------
7.20333333333333
```

**count**

```sql
sqlite> SELECT count(reading) FROM Survey WHERE quant='sal';
count(reading)
--------------
9 
```

**sum**

```sql
sqlite> SELECT sum(reading) FROM Survey WHERE quant='sal';
sum(reading)
------------
64.83 
```

We can even combine aggregate functions. For example, we can query both the min and max of all the salinity measurements that are below 1.0.

```sql
sqlite> SELECT min(reading), max(reading) FROM Survey WHERE quant='sal' AND reading<=1.0;
min(reading)  max(reading)
------------  ------------
0.05          0.21 
```

Unlike other functions, aggregate functions ignore `NULL` values instead of returning `NULL` when it encounters a `NULL` value.

```sql
sqlite> SELECT min(dated) FROM Visited;
min(dated)
----------
1927-02-08
```

Let's suppose we suspect bias in our data. We suspect that some scientists' radiation readings are higher than others.

```sql
sqlite> SELECT  person, count(reading), round(avg(reading), 2)
   ...> FROM Survey
   ...> WHERE quant='rad';
person      count(reading)  round(avg(reading), 2)
----------  --------------  ----------------------
roe         8               6.56  
```

This is not quite what we were looking for. We would like an average reading for each scientist. Unfortunately, SQL will pick a random entry (usually the first or last hit) when making a query that mixes data with aggregate data. In order to fix this problem, we need to use the `GROUP BY` command.

```sql
qlite> SELECT    person, count(reading), round(avg(reading),2)   
   ...> FROM	 Survey
   ...> WHERE	 quant='rad'
   ...> GROUP BY person;
person      count(reading)  round(avg(reading),2)
----------  --------------  ---------------------
dyer        2               8.81                 
lake        2               1.82                 
pb          3               6.66                 
roe         1               11.25 
```

## Combining Data
> * Explain the operation of a query that joins two tables.
> * Explain how to join data in meaningful ways.
> * Write tables that join queries on equal keys.
> * Explain what primary and foreign keys are, and why they are useful.

Suppose we need to submit our antarctic data to a website that requires data submissions to be submitted as latitude, longitude, date, quantity, and reading. However, this data is mixed up across different tables in our database. Fortunately, we can combine data from different tables.

```sql
sqlite> SELECT * FROM Site JOIN Visited;
name        lat         long        id          site        dated     
----------  ----------  ----------  ----------  ----------  ----------
DR-1        -49.85      -128.57     619         DR-1        1927-02-08
DR-1        -49.85      -128.57     622         DR-1        1927-02-10
DR-1        -49.85      -128.57     734         DR-3        1930-01-07
DR-1        -49.85      -128.57     735         DR-3        1930-01-12
DR-1        -49.85      -128.57     751         DR-3        1930-02-26
DR-1        -49.85      -128.57     752         DR-3                  
DR-1        -49.85      -128.57     837         MSK-4       1932-01-14
DR-1        -49.85      -128.57     844         DR-1        1932-03-22
DR-3        -47.15      -126.72     619         DR-1        1927-02-08
DR-3        -47.15      -126.72     622         DR-1        1927-02-10
DR-3        -47.15      -126.72     734         DR-3        1930-01-07
DR-3        -47.15      -126.72     735         DR-3        1930-01-12
DR-3        -47.15      -126.72     751         DR-3        1930-02-26
DR-3        -47.15      -126.72     752         DR-3                  
DR-3        -47.15      -126.72     837         MSK-4       1932-01-14
DR-3        -47.15      -126.72     844         DR-1        1932-03-22
MSK-4       -48.87      -123.4      619         DR-1        1927-02-08
MSK-4       -48.87      -123.4      622         DR-1        1927-02-10
MSK-4       -48.87      -123.4      734         DR-3        1930-01-07
MSK-4       -48.87      -123.4      735         DR-3        1930-01-12
MSK-4       -48.87      -123.4      751         DR-3        1930-02-26
MSK-4       -48.87      -123.4      752         DR-3                  
MSK-4       -48.87      -123.4      837         MSK-4       1932-01-14
MSK-4       -48.87      -123.4      844         DR-1        1932-03-22
```

Using the `JOIN` command creates a cross product of the two tables. That means each entry from Site is matched with each entry from Visited. We get (3 * 8 = 24) entries in the joined table. Since each table has 3 fields, we get (3 + 3 = 6) fields in the joined table.

Notice that the computer has no way of knowing that the field `name` in the `Site` table corresponds to the field `site` in the `Visited` table. We have to tell the computer that.

```sql
sqlite> SELECT * FROM Site JOIN Visited ON Site.name=Visited.site;
name        lat         long        id          site        dated     
----------  ----------  ----------  ----------  ----------  ----------
DR-1        -49.85      -128.57     619         DR-1        1927-02-08
DR-1        -49.85      -128.57     622         DR-1        1927-02-10
DR-1        -49.85      -128.57     844         DR-1        1932-03-22
DR-3        -47.15      -126.72     734         DR-3        1930-01-07
DR-3        -47.15      -126.72     735         DR-3        1930-01-12
DR-3        -47.15      -126.72     751         DR-3        1930-02-26
DR-3        -47.15      -126.72     752         DR-3                  
MSK-4       -48.87      -123.4      837         MSK-4       1932-01-14
```

Note that the use of the dot notation allows us to make it clear to the database manager which field, on which table we are talking about. Let's now get the format the website is asking for.

```sql
sqlite> SELECT Site.lat, Site.long, Visited.dated, Survey.quant, Survey.reading
   ...> FROM Site JOIN Visited JOIN Survey
   ...> ON Site.name=Visited.site
   ...> AND Visited.id=Survey.taken
   ...> AND Visited.dated IS NOT NULL;
lat         long        dated       quant       reading   
----------  ----------  ----------  ----------  ----------
-49.85      -128.57     1927-02-08  rad         9.82      
-49.85      -128.57     1927-02-08  sal         0.13      
-49.85      -128.57     1927-02-10  rad         7.8       
-49.85      -128.57     1927-02-10  sal         0.09      
-47.15      -126.72     1930-01-07  rad         8.41      
-47.15      -126.72     1930-01-07  sal         0.05      
-47.15      -126.72     1930-01-07  temp        -21.5     
-47.15      -126.72     1930-01-12  rad         7.22      
-47.15      -126.72     1930-01-12  sal         0.06      
-47.15      -126.72     1930-01-12  temp        -26.0     
-47.15      -126.72     1930-02-26  rad         4.35      
-47.15      -126.72     1930-02-26  sal         0.1       
-47.15      -126.72     1930-02-26  temp        -18.5     
-48.87      -123.4      1932-01-14  rad         1.46      
-48.87      -123.4      1932-01-14  sal         0.21      
-48.87      -123.4      1932-01-14  sal         22.5      
-49.85      -128.57     1932-03-22  rad         11.25 
```

There is some special terminology when talking about how different table relate to each other is a database. A *primary key* is a value that uniquely identifies each record in a table. For example, `Person.id` is a primary key in the `Person` table. A *foreign key*, on the other hand, is a value that uniquely identifies a record in another table. Therefore, `Survey.person` is a foreign key in `Survey` as it relates entries in `Survey` back to unique entries in `Person`.

Most database designers feel that every table should have a well-defined primary key that is separate from the data itself. For example, schools and universities often use student numbers, and hospitals often have patient numbers. For this reason, SQLite automatically adds row numbers.

```sql
sqlite> SELECT rowid, * FROM Person;
rowid       id          personal    family    
----------  ----------  ----------  ----------
1           dyer        William     Dyer      
2           pb          Frank       Pabodie   
3           lake        Anderson    Lake      
4           roe         Valentina   Roerich   
5           danforth    Frank       Danforth
```

## Data Hygiene
> * Explain what an atomic value is.
> * Explain why every value in a database should be atomic.
> * Explain what a primary key is and why every record should have one.

Now that we some of the ins-and-outs of working with databases, there are some tips we can follow to make sure that we have "clean" data that is easy to work with in a database.

1. Every value should be atomic since it's easier to combine values than to separate them. (ex: `personal` and `family` in `Person` table)
2. Every record should have a unique primary key. This can be a serial number with no intrinsic meaning or it can be a value in the table.
3. There should be no redundant information. This is balancing act (see `Site` and `Visited`). We could have all the records in one giant table like a spreadsheet, but then we run the risk of needing to change multiple values needlessly when we update information.
4. The units for every value should be stored explicitly. We don't have this in our `Survey` table and it's a problem (see Roerich's values).

## Creating and Modifying Data
> * Write statements that create tables.
> * Write statements to insert, modify, and delete records.

So far we have discussed how to get data from databases, but not how to put data in databases. Let's create a new database.

```sql
.quit
```

```bash
sqslite3 new-data.db
sqlite> .mode column
sqlite> .header on
```

Notice, we have a new, empty database.

```sql
sqlite> .tables
sqlite> 
```

Let's use our handout to recreate a new version of the database we were just working with.

```sql
sqlite> CREATE TABLE Person(id text, personal text, family text);
```

Notice that after each column header, we put the type of data that will be entered under that column. Most databases support at least the following data types:
* integer - signed integer
* real - floating point number
* text - character string
* blob (binary large object) - allows objects like images
* boolean - true or false (represented by 0 and 1 in SQL)
* date/time values

> Many databases use *hybrid storage* instead of putting everything directly in the database. In a *hybrid storage* model, names of large picture files are stored in the database in order to reference the actual picture file stored elsewhere on the disk.

```sql
sqlite> CREATE TABLE Site(name text, lat real, long real);
sqlite> CREATE TABLE Visited(id integer, site text, dated text);
sqlite> CREATE TABLE Survey(taken integer, person text, quant real, reading real);
sqlite> .tables
Person   Site     Survey   Visited
```

We now have four new, empty tables in our database. We can delete tables by using the `DROP TABLE` command.

```sql
sqlite> .tables
Person   Site     Visited
```

SQL also gives us many tools for carefully defining what kind of data can go into a table. For example, we could recreate `Survey` with more restrictions.

```sql
sqlite> CREATE TABLE Survey(
   ...> taken integer not null, -- where reading taken
   ...> person text,            -- may not know who took it
   ...> quant real not null,    -- the quantity measured
   ...> reading real not null,  -- the actual reading
   ...> foreign key(taken) references Visited(id),
   ...> foreign key(person) references Person(id)
   ...> );
sqlite> .tables
Person   Site     Survey   Visited
```

Don't let the multiple lines throw you off. The command follows the same pattern as when it was only on one line, but we've added line breaks to make it a little more readable. Notice the following:

* We can require that data entries have a value by using the `not null` designation.
* We can designate which keys correspond to primary keys in other tables by using `foreign key()` and `references()`.
* Anything following `--` is a comment

We can insert data into a blank table using the `INSERT INTO` command.

```sql
sqlite> INSERT INTO site values('DR-1', -49.85, -128.57);
sqlite> SELECT * FROM Site;
name        lat         long      
----------  ----------  ----------
DR-1        -49.85      -128.57   
sqlite> INSERT INTO site values('DR-3', -47.15, -126.72);
sqlite> SELECT * FROM Site;
name        lat         long      
----------  ----------  ----------
DR-1        -49.85      -128.57   
DR-3        -47.15      -126.72
```

We can also transfer values from one table into another.

```sql
sqlite> CREATE TABLE JustLatLong(lat text, long text);
sqlite> INSERT INTO JustLatLong SELECT lat, long FROM Site;
sqlite> SELECT * FROM JustLatLong;
lat         long      
----------  ----------
-49.85      -128.57   
-47.15      -126.72 
```

We can modify an existing entry by using `UPDATE`.

```sql
sqlite> UPDATE JustLatLong SET lat=-50.0 WHERE lat=-49.85;
sqlite> SELECT * FROM JustLatLong;
lat         long      
----------  ----------
-50.0       -128.57   
-47.15      -126.72 
```

Be careful to remember the `WHERE` clause or else the command will update all the entries in the table.

As long as we are careful to keep the database internally consistent, we can use the `DELETE` command to remove an entry.

```sql
qlite> DELETE FROM JustLatLong WHERE lat=-50.0;
sqlite> SELECT * FROM JustLatLong;
lat         long      
----------  ----------
-47.15      -126.72 
```

## Programming with Databases in R
> * Write a short program that executes SQL queries in R.
> * Explain why most database applications are written in a general-purpose language rather than in SQL.

* Demonstrate how R scripts can query databases.
* Show R script running in Rstudio.
* R has many helper functions for interacting with databases.
* R is better at manipulating data.
* Point them to further SWC material and more R resources there.
