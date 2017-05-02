# SQL

## Selecting Data

### Relational Database

A *relational database* is a way to store and manipulate information.

* Databases are arranged as tables.
* Each table has columns that describe the data (variables)
* Each table has rows that contain the data (entries/observations)
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

We can use wildcards like the bash shell.

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

We can make queries based on partial matches as well. In SQL `%` functions as a wildcard just like `*` does for the shell. Let's query all the sites that begin with 'DR'.

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

Just as we would with a comlex bash pipe and filter setup, we start with simple SQL queries and add pieces until we have built up a complex SQL query that we are certain works correctly. If we have a large database, queries can take time and this can prove to be too time-consuming. One strategy is to create a database that is a subset and then construct the appropriate queries. Once you know it works, then you can use it with the full data set.

## Calculating New Values

After carefully re-reading the expedition logs, we realize that the readiation measurements they reposrt may need to be corrected upward by 5%. Instead of modifying the stored data, we can do this calculation as part of our query.

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

We can use all of the typical arimetic operators with column headings. Let's convert all the temperature readings in `Survey` from Fahrenheit to Celsius. Let's build our query in two steps. First, pull out all the temperature readings and where they were taken. Then modify the query to produce the same results in Celsius.

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


