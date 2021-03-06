# Basic SQL Guide

## Contents
[Basic SQL](#basic-sql)
- [Anatomy of a Basic SQL Query](#anatomy-of-a-basic-sql-query)
- [At the Bare Minimum: SELECT and FROM](#at-the-bare-minimum-select-and-from)
- [Column Names](#column-names)
- [An Important Tip: Explore the Dataset](#an-important-tip-explore-the-dataset)
- [Slicing Your Data: WHERE](#slicing-your-data-where)
- [Controlling the Output: LIMIT](#controlling-the-output-limit)
- [Super Powering the WHERE Clause](#super-powering-the-where-clause)
- [Comparison Operations on Numerical Data](#comparison-operations-on-numerical-data)
- [Comparison Operators on Non-Numerical Data](#comparison-operators-on-non-numerical-data)
- [More Operators to Super Power the WHERE Clause](#more-operators-to-super-power-the-where-clause)
- [LIKE](#like)
- [Wildcards and ILIKE](#wildcards-and-ilike)
- [IN](#in)
- [BETWEEN](#between)
- [IS NULL](#is-null)
- [AND](#and)
- [OR](#or)
- [NOT](#not)
- [Sorting Data: ORDER BY](#sorting-data-order-by)
- [DISTINCT](#distinct)
- [Aggregations in the SELECT Clause](#aggregations-in-the-select-clause)
- [Counting all rows](#Counting-all-rows)
- [Counting individual columns](#counting-individual-columns)
- [SUM](#sum)
- [MIN and MAX](#min-and-max)
- [AVG](#avg)
- [Simple Arithmetic in SQL](#simple-arithmetic-in-sql)
- [GROUP BY](#group-by)
- [GROUP BY with ORDER BY](#group-by-with-order-by)
- [HAVING](#having)


This SQL guide is meant to help you get started with SQL. It's helpful for absolute beginners but better for beginners that need a reference when coding. This guide is adapted from Mode Analytics Intro to SQL which is a great introduction to SQL, however, this guide with the accompanying datasets provide a more hands-on experience that allows you to code live with tools used in industry. All tables found in the Mode Analytics guide are loaded in our databases but we added dozens more to get you better acquainted with SQL and analytics. 

# Basic SQL
### Anatomy of a Basic SQL Query

The anatomy of a SQL query is the same every single time. The clauses (e.g., SELECT, FROM, WHERE) are always in the same order, however, many of the clauses are optional. In this section, I’ll explain the SQL clauses that are always required to pull data as well as a few operators and math operations that help convert raw data into something useful. 

Note: The SQL code examples use the Strata Scratch database and is executable on Strata Scratch through your web browser. I would recommend copying and pasting the code, executing it, and taking a look at the output. 

### At the Bare Minimum: SELECT and FROM

There are two required ingredients in any SQL query: SELECT and FROM — and they have to be in that order.  
Back to SELECT and FROM
SELECT indicates which columns of the table you’d like to view, and FROM identifies the table you want to pull data from.

```sql
SELECT 
    year, 
    month, 
    west
FROM datasets.us_housing_units
```

- In this example, we’re pulling data from a schema called `datasets` and a table called `us_housing_units`. Within the table, we’re interested in the data that are stored in the year, month, and west columns.
- a schema is a logical way to group objects like tables, procedures, views. Think of a schema as a container for your database tables.

Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should not include a comma after the last column name.

If you want to select every column in a table, you can use `*` instead of typing out the column names:

```sql
SELECT *
FROM datasets.us_housing_units
```

### Column Names

If you’d like your results to look a bit more presentable, you can rename columns to include spaces. For example, if you want the west column to appear as West Region in the results, you would have to type:

```sql
SELECT west AS "West Region"
FROM datasets.us_housing_units
```

This gives us the following output:

![strata scratch](assets/3b.png)

- Without the double quotes, that query would read ‘West’ and ‘Region’ as separate objects and would return an error. 

Note that the results will only be case sensitive if you put column names in double quotes. The following query, for example, will return results with lower-case column names.

```sql
SELECT west AS West_Region,
       south AS South_Region
FROM datasets.us_housing_units
```

Output:

![strata scratch](assets/4b.png)

- West_Region would be returned as west_region since double quotes are missing.
- In practice, you’d want to stick to one naming convention, either “West Region” or west_region. Having a consistent naming convention helps standardize coding practices and makes everyone more efficient when pulling data.

### An Important Tip: Explore the Dataset

Now that you understand the basics of query data from a table, the next step is to query, format, and aggregate data so that it’s useful. What’s difficult is that there are often no way to preview the data in the tables. Before diving too deep in any SQL query or analysis, I will always explore tables before starting to write complex queries. All you need to do is perform what I call a SELECT STAR or

```sql
SELECT 
    * 
FROM datasets.us_housing_units
```

This will allow you to see all the columns and some data in the table so that you better understand the data types and column names before writing any complicated query.

![strata scratch](assets/5b.png)

### Slicing Your Data: WHERE 

Now you know how to pull data from tables and even specify what columns you want in your output. But what if you’re interested only in housing units sold in January? The WHERE clause allows you to returns rows of data that meet your criteria. 

The WHERE clause, in this example, will go after the FROM clause. In the WHERE clause you need to write a logical operator. For example, if you’re interested in pulling data from month 1, simply write `month = 1` in the WHERE clause.

```sql
SELECT *
FROM datasets.us_housing_units
WHERE month = 1
```

Output:

![strata scratch](assets/6b.png)

- Note that `month` is a column in the table and the months are represented by numbers. Remember to do a `SELECT * ` to explore the table before writing your queries.

### Controlling the Output: LIMIT

The LIMIT clause is optional and is used to control the number of rows displayed in the output. The LIMIT clause goes at the very end of your SQL query. I find this clause useful when exploring data.  The following syntax limits the number of rows to only 100:

```sql
SELECT *
FROM datasets.us_housing_units
LIMIT 100
```


### Super Powering the WHERE Clause

The WHERE clause is powerful. You can leverage operators and mathematical operations to slice your data into different views. In addition, you can chain together all these operators to efficiently narrow in on the data.

### Comparison Operations on Numerical Data

The most basic way to filter data is to use comparison operators. The easiest way to understand them is to start by looking at a list of them:
- Equal to                        =
- Not equal to                    <>  or  !=
- Greater than                    >
- Less than                       < 
- Greater than or equal to        >=
- Less than or equal to           <=

These comparison operators make the most sense when applied to numerical columns. For example, let’s use > to return only the rows where the West Region produced more than 30 housing units

```sql
SELECT *
FROM datasets.us_housing_units
WHERE west > 30
```

- The SQL query is saying, select all the data located in the schema `datasets` and in the table `us_housing_units` where the `west` column (i.e., the west region) has values greater than 30. 
- The SQL query will then go through the `west` column and look for values greater than 30 then output all the rows in the table where west > 30. 

![strata scratch](assets/49b.png)

### Comparison Operators on Non-Numerical Data

Some of the above operators work on non-numerical data as well. `=` and `!=` make perfect sense — they allow you to select rows that match or don’t match any value, respectively. For example, run the following query and you’ll notice that none of the January rows show up:

```sql
SELECT *
FROM datasets.us_housing_units
WHERE month_name != 'January'
```

Output:

![strata scratch](assets/9b.png)

### More Operators to Super Power the WHERE Clause

Here’s a list of additional logical operators to use in the WHERE clause: 

```sql
LIKE allows you to match similar values, instead of exact values.
IN allows you to specify a list of values you’d like to include.
BETWEEN allows you to select only rows within a certain range.
IS NULL allows you to select rows that contain no data in a given column.
AND allows you to select only rows that satisfy two conditions.
OR allows you to select rows that satisfy either of two conditions.
NOT allows you to select rows that do not match a certain condition.
```

### LIKE

LIKE is a logical operator that allows you to match on similar values rather than exact ones.

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE "group" LIKE 'Snoop%'
```

- The LIKE operator will look for values that start with Snoop. The % symbol is a wildcard (explained below) and ignores any value coming after Snoop. 
- Note that the LIKE operator is case sensitive so `Snoop` and `snoop` are different when using LIKE. 

### Wildcards and ILIKE

The % used above represents any character or set of characters. In this case, % is referred to as a “wildcard.” LIKE is case-sensitive, meaning that the above query will only capture matches that start with a capital “S” and lower-case “noop.” To ignore case when you’re matching values, you can use the ILIKE command:

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE "group" ILIKE 'snoop%'
```

Output:

![strata scratch](assets/11b.png)

- In this case, using ILIKE allows you to be case insensitive. `Snoop` is the same as `snoop` according to ILIKE.

You can also use _ (a single underscore) to substitute for an individual character:

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE artist ILIKE 'dr_ke'
```

Output:

![strata scratch](assets/12b.png)

- In this case any alphanumeric value can take the place of the `_` symbol. We’re obviously looking for Drake but this query will catch any misspellings in the `a` portion of his name (e.g., drbke)

### IN

IN is a logical operator in SQL that allows you to specify a list of values that you’d like to include in the results. 

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE year_rank IN (1, 2, 3)
```

Output:

![strata scratch](assets/13b.png)

- Here I’m only interested in data where year_rank is 1 or 2 or 3. 

As with comparison operators, you can use non-numerical values, but they need to go inside single quotes. Regardless of the data type, the values in the list must be separated by commas. Here’s another example:

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE artist IN ('Taylor Swift', 'Usher', 'Ludacris')
```

The output here should only yield data corresponding to artists named Taylor Swift or Usher or Ludacris.

![strata scratch](assets/14b.png)

### BETWEEN

BETWEEN is a logical operator in SQL that allows you to select only rows that are within a specific range. It has to be paired with the AND operator, which you’ll learn about in a later.

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE year_rank BETWEEN 5 AND 10
```

- Here I’m only interested in data where year_rank is 5 to 10. 
- `Between` is inclusive so the year_rank can include 5 and 10 (i.e., not 6 to 9). 

### IS NULL

IS NULL is a logical operator in SQL that allows you to exclude rows with missing data from your results.

Some tables contain null values—cells with no data in them at all. This can be confusing for heavy Excel users, because the difference between a cell having no data and a cell containing a space isn’t meaningful in Excel. In SQL, the implications can be pretty serious. 

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE artist IS NULL
```

- IS NULL will only catch cells with no data. A space is considered data so IS NULL will not catch any cell with a space. Be mindful of that when exploring your dataset.

### AND

AND is a logical operator in SQL that allows you to select only rows that satisfy two conditions.

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE year = 2012 AND year_rank <= 10
```

You can use AND with any comparison operator, as many times as you want. If you run this query, you’ll notice that all of the requirements are satisfied.

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE year = 2012
   AND year_rank <= 10
   AND "group" ILIKE '%feat%'
```

- This query will return all data in the `billboard_top_100_year_end` table for the year 2012, year_rank is less or equal to 10, and where the group has the word `feat` (i.e., Top 10 song collaborations in 2012).

![strata scratch](assets/18b.png)

### OR

OR is a logical operator in SQL that allows you to select rows that satisfy either of two conditions. It works the same way as AND, which selects the rows that satisfy both of two conditions.

```sql
SELECT *
  FROM datasets.billboard_top_100_year_end
 WHERE year_rank = 5 OR artist = 'Gotye'
 ```

- The query will return all data where the year_rank is 5 or the artist is named Gotye regardless of his year_rank. 

### NOT

NOT is a logical operator in SQL that you can put before any conditional statement to select rows for which that statement is false.

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE year = 2013
   AND year_rank NOT BETWEEN 2 AND 3
```

Output:

![strata scratch](assets/20b.png)

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
WHERE year = 2013
   AND artist IS NOT NULL
```

Output:

![strata scratch](assets/21b.png)

### Sorting Data: ORDER BY

Once you’ve learned how to filter data, it’s time to learn how to sort data. The ORDER BY clause allows you to reorder your results based on the data in one or more columns. First, take a look at how the table is ordered by default:

```sql
SELECT * 
FROM datasets.billboard_top_100_year_end
```

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
ORDER BY artist
```

You’ll need to specify whether you want the data to be displayed in ascending order or descending order. 

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
ORDER BY artist ASC
```

Will output data alphabetically by artist

![strata scratch](assets/50b.png)

```sql
SELECT *
FROM datasets.billboard_top_100_year_end
ORDER BY artist DESC
```

Will output data reverse alphabetically by artist

![strata scratch](assets/51b.png)

Sometimes you’re not necessarily interested in an output of all the data. Your question that you’re trying to answer is simpler like `how many rows are in this table?` or `how many top 10 songs did Usher produce in 2012?`. In these cases, we don’t want a list of values but would rather have one value — the answer.  You can process data in the SELECT clause. 

### DISTINCT

You’ll occasionally want to look at only the unique values in a particular column. You can do this using SELECT DISTINCT syntax.

```sql
SELECT 
    DISTINCT month
FROM datasets.aapl_historical_stock_price
```

- Outputs all the distinct values in the month column of the table.

![strata scratch](assets/37b.png)

DISTINCT is handy when you want to count the number of unique values in a column (e.g., distinct months or distinct users). 

```sql
SELECT 
    COUNT(DISTINCT month) AS unique_months
FROM datasets.aapl_historical_stock_price
```

Output: ``` 12 ```

## AGGREGATIONS IN THE SELECT CLAUSE
### Counting all rows

COUNT is a SQL aggregate function for counting the number of rows in a particular column. COUNT is the easiest aggregate function to begin with because verifying your results is extremely simple. 

```sql
SELECT 
    COUNT(*)
FROM datasets.aapl_historical_stock_price
```

Output:``` 3527 ```

Important note: count(*) also counts the null values. If you want to exclude null values, refer below.

### Counting individual columns

Things start to get a little bit tricky when you want to count individual columns. The following code will provide a count of all of rows in which the high column does not contain a null.

```sql
SELECT 
    COUNT(high)
FROM datasets.aapl_historical_stock_price
```

Output:``` 3527 ```

- Note that by specifying the name of the column `high` in `count()`, the query will ignore any nulls in the `high` column and only count the rows containing values. 

### SUM 

SUM is a SQL aggregate function that totals the values in a given column. Unlike COUNT, you can only use SUM on columns containing numerical values.

```sql
SELECT 
    SUM(volume)
FROM datasets.aapl_historical_stock_price
```

Output:``` 73442072063 ```

### MIN and MAX

MIN and MAX are SQL aggregation functions that return the lowest and highest values in a particular column.

```sql
SELECT MIN(volume) AS min_volume,
       MAX(volume) AS max_volume
FROM datasets.aapl_historical_stock_price
```

Output:

![strata scratch](assets/29b.png)

### AVG

AVG is a SQL aggregate function that calculates the average of a selected group of values. It’s very useful, but has some limitations. First, it can only be used on numerical columns. Second, it ignores nulls completely.

```sql
SELECT 
    AVG(high)
FROM datasets.aapl_historical_stock_price
```

Running the code above will give an output of ```506.5```.

### Simple Arithmetic in SQL

You can perform arithmetic in SQL using the same operators you would in Excel: +, -, *, /. However, in SQL you can only perform arithmetic across columns on values in a given row. To clarify, you can only add values in multiple columns from the same row together using +.

```sql
SELECT year,
       month,
       west,
       south,
       west + south AS south_plus_west
  FROM datasets.us_housing_units
```

The output will contain as many rows as are in the table. Only west and south will be added together on a row level. 

![strata scratch](assets/31b.png)

```sql
SELECT year,
       month,
       west,
       south,
       west + south - 4 * year AS nonsense_column
  FROM datasets.us_housing_units
```

Output:

![strata scratch](assets/32b.png)

### GROUP BY

SQL aggregate functions like COUNT, AVG, and SUM have something in common: they all aggregate across the entire table. But what if you want to aggregate only part of a table? For example, you might want to count the number of entries for each year.

In situations like these, you’d need to use the GROUP BY clause. GROUP BY allows you to separate data into groups, which can be aggregated independently of one another. 

The GROUP BY clause always comes towards the end of the SQL query. It technically goes after the WHERE clause but if the WHERE clause is missing then the GROUP BY will come after the FROM clause (or JOIN clause, but we haven’t learned that yet). 

You’ll know which column name to include in the GROUP BY because it’s listed in the SELECT clause. You only want to include column names that are not being operated on in the GROUP BY clause. In the example below, you do not add COUNT(*) in the GROUP BY because COUNT is an operator. 

```sql
SELECT year,
       COUNT(*) AS count
FROM datasets.aapl_historical_stock_price
GROUP BY year
```

![strata scratch](assets/33b.png)

- The query outputs a count of all the rows by year
- You only add `year` in the GROUP BY because you want to split the COUNT by year.

```sql
SELECT year,
       month,
       COUNT(*) AS count
FROM datasets.aapl_historical_stock_price
GROUP BY year, month
```

Output:

![strata scratch](assets/34b.png)

- You add both year and month in the GROUP BY because you’re interested in the row count by year and month.

### GROUP BY with ORDER BY

The order of column names in your GROUP BY clause doesn’t matter—the results will be the same regardless. If you want to control how the aggregations are grouped together, use ORDER BY. Try running the query below, then reverse the column names in the ORDER BY statement and see how it looks:

```sql
SELECT year,
       month,
       COUNT(*) AS count
FROM datasets.aapl_historical_stock_price
GROUP BY year, month
ORDER BY month, year
```

Output:

![strata scratch](assets/35b.png)

### HAVING

However, you’ll often encounter datasets where GROUP BY isn’t enough to get what you’re looking for. Let’s say that it’s not enough just to know aggregated stats by month. After all, there are a lot of months in this dataset. Instead, you might want to find every month during which AAPL stock worked its way over $400/share. The WHERE clause won’t work for this because it doesn’t allow you to filter on aggregate columns—that’s where the HAVING clause comes in:

```sql
SELECT year,
       month,
       MAX(high) AS month_high
FROM datasets.aapl_historical_stock_price
GROUP BY year, month
HAVING MAX(high) > 400
ORDER BY year, month
```
Output:

![strata scratch](assets/36c.png)

- The HAVING clause always comes after the GROUP BY but before the ORDER BY clauses.
- It might be more intuitive to use a WHERE clause rather than HAVING clause in this query but you’re not allowed to process or aggregate data in the WHERE clause. This is due to the order of operations when the CPU performs the SQL query. The SQL query is processed by first processing the SELECT, FROM, and GROUP BY clauses. From that dataset, the HAVING clause will act on the data and remove any stock prices below 400. Lastly, the SQL query will order the data by year and month as indicated by the ORDER BY clause. 