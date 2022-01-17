# SQL and Postgres: CRUD Cheatsheet

## Objective
The purpose of this cheat sheet is to show the reader how to perform basic CRUD operations using Postgres and SQL. CRUD is an acronym that refers to the following essential operations of any database:
|       |        |
| ----- | ------ |
| **C** | Create |
| **R** | Read   |
| **U** | Update |
| **D** | Delete |

I will use the following table to demonstrate that concept:
| Name      | Country | Population | Area |
| --------- | ------- | ---------- | ---- |
| Tokyo     | Japan   | 37400068   | 8233 |
| Delhi     | India   | 2814000    | 1484 |
| Shanghai  | China   | 2558200    | 6341 |
| Sao Paulo | Brazil  | 21650000   | 1521 |

## Create

### Creating a table:

```sql
CREATE TABLE cities (
     name VARCHAR(50),
     country VARCHAR(50),
     population INTEGER,
     area INTEGER
);
```



#### Cities table data types:

| Data type     | description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `VARCHAR(50)` | variable length character, a string containing less than _x_ characters.            |
| `INTEGER`     | a whole number in between roughly -2.14 x 10<sup>9</sup> and 2.14 x 10<sup>9</sup>. |

### Inserting data into a table:

```sql
INSERT INTO
  cities (name, country, population, area)
VALUES
  ('Tokyo', 'Japan', 37400068, 8233),
  ('Delhi', 'India', 28514000, 1484),
  ('Shanghai', 'China', 25582000, 6341),
  ('Sao Paulo', 'Brazil', 21650000, 1521);
```



| Name      | Country | Population | Area |
| --------- | ------- | ---------- | ---- |
| Tokyo     | Japan   | 37400068   | 8233 |
| Delhi     | India   | 2814000    | 1484 |
| Shanghai  | China   | 2558200    | 6341 |
| Sao Paulo | Brazil  | 21650000   | 1521 |


## Read
### Selecting data from a table

To select the entire table:
```sql 
SELECT * FROM cities;
```

To get (a) particular column(s): 
```sql
SELECT 
     name, 
     area 
FROM 
     cities;
```

## Calculated columns

Since we have both population and area in the cities table, suppose we would want to get the population density (`population / area`).
 * To do this, we can use SQL to calculate a column based off of existing columns!


```SQL
SELECT 
     name, population / area AS population_density 
FROM 
     cities;
```

| name      | population_density |
| --------- | ------------------ |
| Tokyo     | 4542               |
| Delhi     | 19214              |
| Shanghai  | 4032               |
| Sao Paulo | 14232              |


* For string operations you can:

| operator   | description                                |
| ---------- | ------------------------------------------ |
| `\|\|`     | join two strings together                  |
| `CONCAT()` | join two strings together                  |
| `LOWER()`  | to lower case                              |
| `UPPER()`  | to upper case                              |
| `LENGTH()` | get the number of characters in the string |

Using these string operators, suppose we would like to get "city, country" as full_name in our table:

```sql
SELECT 
     name || ', ' || country AS full_name
FROM 
     cities;
```
or:
```sql
SELECT 
     CONCAT(name, ', ', country ) as full_name
FROM
     cities;
```

### Filtering Data: 
 
Suppose we want to get the name and the area of each city with an area less than 4000.
* To do this, we can do the following:
  
```sql
SELECT 
     name,
     area
FROM 
     cities
WHERE
     area > 4000;
```

We will get:
| name     | area |
| -------- | ---- |
| Tokyo    | 8233 |
| Shanghai | 6341 |

## Update
Suppose we want to update the population of Tokyo:

```sql
UPDATE 
     cities
SET
     population = 1241242141
WHERE
     name = 'Tokyo';
```

If we were to query for a city with `name = 'Tokyo'`, we will get the following response.
```sql
SELECT 
     * 
FROM 
     cities 
WHERE
     name = 'Tokyo';
```

| name  | country | population | area |
| ----- | ------- | ---------- | ---- |
| Tokyo | Japan   | 1          | 8233 |

## Delete

To delete the city of Tokyo, you can perform the following query:

```SQL
DELETE FROM
     cities
WHERE
     name = 'Tokyo';
```

After sending a query for all cities, we will get the following response:
```SQL
SELECT 
     *
FROM 
     cities
```


| name      | country | population | area |
| --------- | ------- | ---------- | ---- |
| Delhi     | India   | 28514000   | 1484 |
| Shanghai  | China   | 25582000   | 6341 |
| Sao Paulo | Brazil  | 21650000   | 1521 |
