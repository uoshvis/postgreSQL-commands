# SQL Basics 

`CREATE DATABASE test;`

Connect to db:

`sudo -u postgres psql -h localhost -p 5432 -U postgres test`

Or

`\c test`

### Create table

```
CREATE TABLE table_name (
    Column name + data type + constraints if any
)
```

[Postgres Data Types](https://www.postgresql.org/docs/9.5/datatype.html)

#### Describe database or table

`\d`

`\d + table_name`


### Data generator

[https://www.mockaroo.com/](https://www.mockaroo.com/)
 
Import from file:

`\i + file_location`

### Aggregate Functions

```
SELECT country_of_birth, COUNT(*)
FROM person
GROUP BY country_of_birth
HAVING COUNT(*) > 100
ORDER BY country_of_birth;
```

Min value of group:
```
SELECT make, MIN(price) FROM car GROUP BY make;
```
Group sum:
```
SELECT make, SUM(price) FROM car GROUP BY make;
```

[https://www.postgresql.org/docs/9.5/functions-aggregate.html](https://www.postgresql.org/docs/9.5/functions-aggregate.html)

### Arithmetic

```
SELECT id, make, model, price AS original_price,
ROUND(price *0.1, 2) AS ten_percent,
ROUND(price - (price*.1), 2) AS discount_after_ten_percent
FROM car;
```

### COALESCE

Default value:
```
SELECT COALESCE(email, 'email not provided')
FROM person;
```

### NULLIF

`SELECT NULLIF(10, 10);` --> `NULL`

`SELECT NULLIF(1, 10);` --> `1`

### Division by zero

`SELECT COALESCE(10 / NULLIF(0,0), 0);` --> `0`

### Timestamp and Date

```
SELECT NOW();
SELECT NOW()::DATE;
SELECT NOW()::TIME;
```
