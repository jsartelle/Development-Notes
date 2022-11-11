> [!warning] Warnings
> - The order of keywords matters!
> - Table/column names that are also keywords (ex. `trigger`) must be quoted using `

# Examples

## Basic query

```sql
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

```sql
SELECT DISTINCT `name`, `phone_number` ...
```

### Count number of distinct values

```sql
SELECT COUNT(DISTINCT first_name) ...
```

### Group by distinct value (with count for each)

```sql
SELECT first_name, count(*)
FROM users
GROUP BY first_name
```

### Select values from JSON field (JSON_EXTRACT)

```sql
SELECT JSON_EXTRACT(column_name, '$.version') ...
```

## WHERE

### Pattern matching (LIKE)

```sql
WHERE name LIKE '%alice%'
```

- case sensitive:

```sql
WHERE name LIKE BINARY '%Alice%'
```

### Regular expressions (REGEXP_LIKE)

- backslashes must be escaped

```sql
WHERE REGEXP_LIKE(name, '\\w+_\\d{4}_\\d{2}_\\d{2}')
```

### List of values (WHERE IN)

```sql
WHERE name IN ('Alice', 'Bob')
```

### Range (BETWEEN)

- both sides are inclusive

```sql
WHERE id BETWEEN 100 AND 200
```

- dates must be explicitly cast

```sql
WHERE
	created_at BETWEEN CAST('2022-01-01' AS DATE)
	AND CAST('2022-01-31' AS DATE)
```

## UPDATE

```sql
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

```sql
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

```sql
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

```sql
UPDATE
    albums a
SET
    a.album_type = CASE
        WHEN a.track_count = 1 THEN 'Single'
        WHEN a.track_count <= 5 THEN 'EP'
        ELSE 'LP'
    END
```

## Transactions

- running scripts in a transaction lets you test & roll back changes

```sql
START TRANSACTION;

-- destructive code goes here - don't forget a semicolon at the end
;

-- add a SELECT here to verify that the changes look good
;

ROLLBACK;
-- or to commit the changes use COMMIT;
```

# SQL mode

```sql
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
