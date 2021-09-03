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
[Date/Time Types](https://www.postgresql.org/docs/9.1/datatype-datetime.html)

```
SELECT NOW();
SELECT NOW()::DATE;
SELECT NOW()::TIME;
```

#### Date arithmetic

`SELECT NOW() - INTERVAL '1 YEAR';`

`MONTHS`, `DAYS`

`SELECT (NOW() + INTERVAL '9 MONTHS')::DATE;`

#### Extracting

`SELECT EXTRACT(YEAR FROM NOW());`

`MONTH`, `DAY`, `DOW`, `CENTURY`


#### Age function

`SELECT first_name, last_name, gender, date_of_birth, AGE(NOW(), date_of_birth) AS age FROM person;`


### Drop primary key (constraint)
`\d person`

Use index name.

`ALTER TABLE person DROP CONSTRAINT person_pkey;`

### Add primary key

`ALTER TABLE person ADD PRIMARY KEY (id);`

### Unique constraints

```
SELECT email, count(*) FROM person
GROUP BY email
HAVING COUNT(*) > 1;
```
Add with name:
```
ALTER TABLE person
ADD CONSTRAINT unique_email_address UNIQUE (email);
```
Add with auto name:

```
ALTER TABLE person
ADD UNIQUE (email);
```

Drop:
```
ALTER TABLE person DROP CONSTRAINT unique_email_address;
```

### Check constraints

```
ALTER TABLE person
ADD CONSTRAINT gender_constraint
CHECK (gender = 'Female' OR gender = 'Male');
```

### Delete records

```
DELETE FROM person
WHERE id = 1;
```
### Update records

```
UPDATE person
SET first_name = 'Omar', email = 'ommar@gmail.com'
WHERE id = 11;
```
### On conflict do nothing

```
...
ON CONFLICT (id) DO NOTHING;
```

### Upsert

Override existing data
```
...
ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email,
last_name = EXCLUDED.last_name, first_name = EXCLUDED.first_name;
```

### Adding Relationship Between Table

Create car table first

```
CREATE TABLE person (
...,
car_id BIGINT REFERENCES car (id),
UNIQUE(car_id)
);
```
Uses unique constraint for car_id.

### Updating FK columns

```
UPDATE person SET car_id = 2 WHERE id = 1;
```

### Inner Joins

Common records

```
SELECT person.first_name, car.make, car.model, car.price
FROM person
JOIN car ON person.id == car.id;
```

Toggle Expanded display

`\x` 

### Left Joins

```
SELECT * FROM person
LEFT JOIN car ON car.id = person.card_id
WHERE car.* IS NULL;
```

### Deleting records with FK

Cascade is a bad practise.

### Exporting to CSV

```
\copy (SELECT * FROM person LEFT JOIN car ON ...) TO '/path/to/dir/filename' DELIMETER ',' CSV HEADER;
```

### Serial and sequences

```
\d
SELECT * FROM person_id_seq;
SELECT nextval('person_id_seq'::regclass);
```

Restart value

`ALTER SEQUENCE person_id_seq RESTART WITH 10;`

### Extensions

Functions that can add extra functionality to database

`SELECT * FROM pg_available_extensions;`

### Universally unique identifier

[Universally unique identifier](https://en.wikipedia.org/wiki/Universally_unique_identifier)

```
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
SELECT * FROM pg_available_extensions;
```
List available functions

`\df`

v4 - random generator

unique 
```
SELECT uuid_generate_v4();
```

#### UUID as primary keys

```
CREATE TABLE person (
    person_uid UUID NOT NULL PRIMARY KEY,
...);

-- INSERT INTO PERSON

insert into person (person_uid, ...)
values(uuid_generate_v4(), ...);

```


```
SELECT * FROM person
JOIN car USING (car_uid);
```
