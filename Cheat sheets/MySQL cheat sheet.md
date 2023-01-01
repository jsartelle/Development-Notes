> [!warning] Warnings
> - The order of keywords matters!
> - Table/column names that are also keywords (ex. `trigger`) must be quoted using `

# Examples

## Basic query

```mysql
SELECT
	u.id,
	u.name,
	p.name AS petName,
	p.species
FROM
	database.users u
	JOIN database.pets p ON p.owner_id = u.id
WHERE
    p.species = 'cat'
    AND p.owner_id IS NOT NULL
ORDER BY
	u.id DESC
LIMIT 50
OFFSET 0
```

## SELECT

### Select distinct values

```mysql
SELECT DISTINCT `name`, `phone_number` ...
```

### Count number of distinct values

```mysql
SELECT COUNT(DISTINCT first_name) ...
```

### Group by distinct value (with count for each)

```mysql
SELECT first_name, COUNT(*)
FROM users
GROUP BY first_name
```

### Select values from JSON field (JSON_EXTRACT)

```mysql
SELECT JSON_EXTRACT(column_name, '$.version') ...
```

## WHERE

### Pattern matching (LIKE)

```mysql
WHERE name LIKE '%alice%'
```

- case sensitive:

```mysql
WHERE name LIKE BINARY '%Alice%'
```

### Regular expressions (REGEXP_LIKE)

- backslashes must be escaped

```mysql
WHERE REGEXP_LIKE(name, '\\w+_\\d{4}_\\d{2}_\\d{2}')
```

### List of values (IN)

```mysql
WHERE name IN ('Alice', 'Bob')
```

### Range (BETWEEN)

- both sides are inclusive

```mysql
WHERE id BETWEEN 100 AND 200
```

### Dates

```mysql
WHERE date(created_at) = '2022-12-25'
```

- if the date column has an index

```mysql
WHERE
    created_at >= '2022-12-25'
AND
    created_at < '2022-12-26'
```

## UPDATE

```mysql
UPDATE
    albums a
SET
    a.recommended = 1
WHERE
    a.stars = 5
```

## WITH

- lets you store temporary results that you can refer to later
- MySQL does not let you combine UPDATE and LIMIT, this can be used to get around that

```mysql
WITH ids AS (
    SELECT
        a.id
    FROM
        albums a
    WHERE
        a.stars = 5
    LIMIT 10
)

UPDATE
    albums a
SET
    a.recommended = 1
WHERE
    a.id IN (SELECT * FROM ids)
```

## CASE

- like switch statements
    - no fallthrough

```mysql
SELECT
    a.name,
    CASE
        WHEN a.rating <= 2 THEN 'Bad'
        WHEN a.rating = 3 THEN 'Average'
        WHEN a.rating >= 4 THEN 'Good'
    END AS quality
FROM
    albums a
```

```mysql
UPDATE
    albums a
SET
    a.album_type = CASE
        WHEN a.track_count = 1 THEN 'Single'
        WHEN a.track_count <= 5 THEN 'EP'
        ELSE 'LP'
    END
```

## DESCRIBE

- prints a table's schema

```mysql
DESCRIBE database.users
```

## Transactions

- running scripts in a transaction lets you test & roll back changes

```mysql
START TRANSACTION;

-- destructive code goes here - don't forget a semicolon at the end
;

-- add a SELECT here to verify that the changes look good
;

ROLLBACK;
-- or to commit the changes use COMMIT;
```

# SQL mode

```mysql
-- get
SELECT @@GLOBAL.sql_mode;

-- set
SET GLOBAL sql_mode = 'NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';
```

# Renaming columns

Renaming existing columns can cause deployed code to break. One approach to safely renaming columns:

1. update the code to handle both the old and new names and deploy
2. rename the column
3. update the code to remove the old name

Another approach:

1. add a new column with the new name and copy the data over
2. update the code to use the new column and deploy
3. drop the old column
