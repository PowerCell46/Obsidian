```SQL
SELECT DISTINCT ON (first_name)
	concat_ws(' ', first_name, middle_name, last_name) AS "Full Name",
	age,
	add_date
FROM PERSON;
```
- Eliminate duplicate results based on the column **first_name**

```SQL
CREATE VIEW PERSON_VIEW AS 
SELECT 
	id,
	concat_ws(' ', first_name, middle_name, last_name) AS "Full Name",
	age AS "Age",
	mood AS "Current Mood",
	add_date AS "Date Added"
FROM 
	PERSON 
ORDER BY
	ID ASC;

SELECT * FROM PERSON_VIEW;
```
- Create and call a view

```sql
SELECT 
	concat_ws(' ', first_name, middle_name, last_name) as "Full Name",
	CASE
		WHEN age < 19 THEN 'School Student'
		WHEN age >= 19 AND age < 24 THEN 'University Student'
		ELSE 'Working Person' 
	END AS "Current Society Position"
FROM 
	PERSON
ORDER BY
	"Current Society Position";
```
- Switch case syntax in **Postgre SQL**


# SQL JOINs

## Inner join 
### Normal join - Selects records that have matching values in both tables.

```sql
SELECT 
	ProductId,
	ProductName,
	CategoryName
From PRODUCTS
INNER JOIN CATEGORIES ON
	PRODUCTS.CategoryID = CATEGORIES.CategoryId;
```

## Left join
### Returns all records from the left table, and the matching records from the right table. The result is 0 records from the right side, if there is no match.

```sql
SELECT
	NAME
FROM PERSON
LEFT JOIN CITY ON
	PERSON.CityId = CITY.id;
```

## Right join 
### Returns all records from the right table, and matching records from the left table. The result is 0 records from the left side, if there is no match.


```sql
SELECT 
	NAME
FROM CITY
RIGHT JOIN PERSON ON
	PERSON.CityId = CITY.id;
```

## Full Outer join 

### Inner + Left + Right JOINs - Joins two tables based on a common column. It selects records that have matching values in these columns and the remaining rows from both of the tables.

```sql
SELECT
	CUSTOMERS.id,
	CUSTOMERS.first_name,
	ORDERS.item
FROM CUSTOMERS
FULL OUTER JOIN ORDERS ON
	CUSTOMERS.id = ORDERS.id;
```

## Cross Join
### Returns the Cartesian product of the two tables - всеки със всеки

```sql
SELECT 
	COL1,
	COL2,
	COL3
FROM TABLE_1
CROSS JOIN TABLE_2;
```

```sql
%% Other way to get the same result %%

SELECT 
	last_name,
	name as department_name
FROM 
	employees,
	departments;
```
```
```
## Inner Join

```sql
SELECT
	T1.first_name,
	T1.last_name,
	T2.first_name,
	T2.last_name
FROM Person T1
JOIN Person T2 ON
	T1.boss_id = T2.id;
```

## Natural Join
### Just like a normal join that automatically selects the columns that have the same name and data type in both tables.

```sql
SELECT
	*
FROM table1
NATURAL JOIN table2;
```


# Create Table Index

```sql
CREATE INDEX people_first_name_index
ON
	people(first_name);
```


# SQL Functions
- declaring the variables in the function head
```sql
CREATE OR REPLACE FUNCTION fn_full_name(first_name VARCHAR, last_name VARCHAR)
RETURNS VARCHAR AS
$$
	BEGIN
		RETURN CONCAT_WS(' ', first_name, last_name);
	END
$$
LANGUAGE PLPGSQL;
```

- Using the input variables as indexes
```sql
CREATE OR REPLACE FUNCTION fn_full_name(VARCHAR, VARCHAR)
RETURNS VARCHAR AS
$$
	BEGIN
		RETURN CONCAT_WS(' ', $1, $2);
	END
$$
LANGUAGE PLPGSQL;
```

- Using declared variables for the input values
```sql
CREATE OR REPLACE FUNCTION fn_full_name(VARCHAR, VARCHAR)
RETURNS VARCHAR AS
$$
	DECLARE 
		first_name ALIAS FOR $1
		last_name ALIAS FOR $2
	BEGIN
		RETURN CONCAT_WS(' ', first_name, last_name);
	END
$$
LANGUAGE PLPGSQL;
```

#### Function Returning an id of a city
```sql
CREATE OR REPLACE FUNCTION fn_get_city_id(city_name VARCHAR)
RETURNS INT AS 
$$
	DECLARE 
		fn_result INT;
	BEGIN
		SELECT 
			id INTO fn_result 
		FROM city 
		WHERE name = city_name;
		
		RETURN fn_result;
	END
$$
LANGUAGE PLPGSQL;
```

- More Complex task
```sql
CREATE OR REPLACE FUNCTION fn_get_city_id(
	IN city_name VARCHAR, OUT city_id INT, OUT STATUS INT
) AS
$$
	DECLARE
		temp_id INT;
	BEGIN
		SELECT 
			id INTO temp_id 
		FROM city 
		WHERE 
			NAME = city_name;

		IF temp_id IS NULL THEN 
			SELECT 100 INTO status;
		ELSE 
			SELECT temp_id, 0 INTO city_id, status;
		END IF;
	END
$$
LANGUAGE PLPGSQL;
```

- Function returning a table
```sql
CREATE OR REPLACE FUNCTION fn_is_game_over(is_game_over BOOLEAN)
RETURNS TABLE(name VARCHAR(50), game_type_id INT, is_finished BOOLEAN) AS
$$
	BEGIN
		RETURN QUERY (SELECT 
				games.name, 
				games.game_type_id,
				games.is_finished
			FROM games
			WHERE games.is_finished = is_game_over);
	END;
$$
LANGUAGE PLPGSQL;
```

# SQL Conditionals
- Declaring a full name variable and based on the inputs conditionally return a result
```sql
CREATE OR REPLACE FUNCTION fn_full_name(first_name VARCHAR, last_name VARCHAR)
RETURNS VARCHAR AS
$$
	DECLARE
		full_name VARCHAR;
	BEGIN
		IF first_name IS NULL AND last_name IS NULL 
			THEN full_name := NULL;
			
		ELSIF first_name IS NULL 
			THEN full_name := last_name;
			
		ELSIF last_name IS NULL 
			THEN full_name := first_name;
			
		ELSE 
			full_name = CONCAT_WS(' ', first_name, last_name);
			
		END IF;
		
		RETURN full_name;
	END
$$
LANGUAGE PLPGSQL;
```

### [[SQL Math Functions]]

# SQL Procedures
- They do not return a value unlike functions
```sql
CREATE PROCEDURE sp_add_person(first_name varchar, last_name varchar, age int, add_date date, middle_name varchar, mood varchar, city_name varchar)
AS
$$
	BEGIN
		INSERT INTO PERSON
		VALUES
			(fn_get_new_person_id(), first_name, last_name, age, add_date, middle_name, mood, fn_get_city_id(city_name));
	END;
$$
LANGUAGE PLPGSQL;


CALL sp_add_person('Kristian', 'Undefined', 30, '2024-05-17', null, 'happy', 'Sofia');
```

### [[SQL Transactions]]


### [[SQL Triggers]]


# [[SQL To-Do learn list]]
