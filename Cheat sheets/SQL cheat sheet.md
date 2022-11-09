# Examples

All of the below are using MySQL

## Basic query

> [!warning] Warnings
> - The order of keywords matters!
> - Table/column names that are also keywords (ex. `trigger`) must be quoted using `

```sql
SELECT
	u.id,
	u.name,
	p.name,
	p.pricing
FROM
	users u
	JOIN enrollments e ON u.id = e.user_id
	JOIN plans p ON e.plan_id = p.id
WHERE
    u.type = 'customer'
    AND u.plan IS NOT NULL
ORDER BY
	u.id DESC
LIMIT 50
OFFSET 0
```

## Pattern matching

```sql
WHERE first_name LIKE '%james%'
```

## Regular Expressions

- escape backslashes

```sql
SELECT DISTINCT
	name
FROM
	documents
WHERE NOT
	REGEXP_LIKE(name, '\\w+_\\d{4}_\\d{2}_\\d{2}')
```

## Where in (multiple values)

```sql
SELECT * FROM users WHERE first_name IN ('Alice', 'Bob')
```

## Find values in a range

Both sides are inclusive

```sql
WHERE id BETWEEN 100 AND 200
```

Dates must be explicitly cast

```sql
WHERE
	created_at BETWEEN CAST('2022-01-01' AS DATE)
	AND CAST('2022-01-31' AS DATE)
```

## Select distinct values

```sql
SELECT DISTINCT `name`, `phone_number` ...
```

## Count number of distinct values

```sql
SELECT COUNT(DISTINCT first_name) ...
```

## Group by distinct value (with count for each)

```sql
SELECT first_name, count(*)
FROM users
GROUP BY first_name
```

## Select values from JSON field

```sql
SELECT JSON_EXTRACT(column_name, '$.version') ...
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
