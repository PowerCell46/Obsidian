# Sequences
```sql
CREATE SEQUENCE seq_Customers_CustomerID
	AS INT
	START WITH 1
	INCREMENT BY 1;


SELECT NEXT VALUE FOR seq_Customers_CustomerID;
```

# Normal Forms

## First Normal Form (1NF)
- Table should only have single (atomic) values/columns7
- Values stored in a column should be of the same type
- All the columns in a table should have unique names
- The order in which data is stored should not matter

## Second Normal Form (2NF)
- The table should be in the First Normal form
- It shouldn't have Partial Dependency (dependency on part of the primary key)

## Third Normal Form (3NF)
- The table is in the Second Normal Form
- It doesn't have Transitive Dependency

# Common Table Expressions (Named queries)

can be used to improved readability and code reuse
```sql
WITH Employees_CTE (FirstName, LastName, DepartmentName)
AS 
(
	SELECT 
		e.firstName, 
		e.lastName,
		d.Name
	FROM Employees as e
	LEFT JOIN Departments as d ON
		d.DepartmentId = e.DepartmentId
);
```

# Clustered Indexes
( вграден в таблицата, защото е част от данните)
- **Is actually the data itself** - самите данни  - използват се данните като индекс
- Very useful for fast execution of WHERE, ORDER BY, and GROUP BY clauses
- **Maximum 1 clustered index per table**
- If a table has no clustered index its data rows are stored in an unordered structure (heap).

# Non-Clustered Indexes
(външен обект)
- препоръчително/задължително е нуждата на клъстъред индекс
- useful for fast retrieving of a range of records
- maintained in a separate structure in the DB
- Tend to be much narrower than the base table
- can locate the exact record(s) with less I/O
- Has at least one more intermediate level than the clustered index
- Much less valuable if a table doesn't have a clustered index
- A non-clustered index has pointers to the actual data rows (pointers to the clustered index if there is one).
```sql
Create NONCLUSTERED INDEX 
IX_Employees_FirstName_LastName
ON Employees(FirstName, LastName);
```

# Functions

```sql
CREATE OR ALTER FUNCTION udf_getSalaryLevel(@Salary Money)
RETURNS VARCHAR(10) AS
BEGIN 
	DECLARE @result VARCHAR(10)
	
	IF (@Salary < 30000) SET @Result = 'Low'
	ELSE IF (@Salary <= 50000) SET @Result = 'Average'
	ELSE SET @Result = 'High';
	
	RETURN @Result;
END;
```


# Procedures

```sql
CREATE PROCEDURE USP_ADDNUMBERS_2
	(@FIRSTNUMBER INT, @SECONDNUMBER INT)
AS
	SELECT @FIRSTNUMBER + @SECONDNUMBER;


EXEC usp_addNumbers_2 10, 5;
```

- Using an output parameter in the procedure's body
```sql
CREATE PROCEDURE USP_ADDNUMBERS
	(@FIRSTNUMBER INT, @SECONDNUMBER INT, @RESULT INT OUTPUT)
AS
	SET @RESULT = @FIRSTNUMBER + @SECONDNUMBER;

-- Declaring a variable
DECLARE @Res INT;

EXEC USP_ADDNUMBERS 5, 10, @RES OUTPUT;

SELECT @Res;
```

- Procedure returning more than one variable
```sql
CREATE PROCEDURE usp_MultipleResult
	(@ID INT = 1)
AS 
	SELECT 
		* 
	FROM PERSON
	WHERE PERSON.ID = @ID;
	
	SELECT 
		* 
	FROM ITEM
	WHERE OWNER_ID = @ID;

EXEC usp_MultipleResult 1;
```

- Transaction with a **rollback**
```sql
Use movies;

BEGIN TRANSACTION
	UPDATE MOVIEEXEC
	SET NETWORTH = NETWORTH * 1.1;
	SELECT * FROM MOVIEEXEC;
ROLLBACK;
```

```sql
CREATE OR ALTER FUNCTION udf_get_city_id(
@cityName VARCHAR(50)) 
RETURNS INT AS 
BEGIN 
	DECLARE @cityID INT; 
	SELECT @cityID = ID 
		FROM CITY 
		WHERE name = @cityName; 
	RETURN @cityID; 
END;

INSERT INTO PERSON(id, FIRST_NAME, LAST_NAME, age, homeTown)
VALUES
	(3, 'Stiliyan', 'Nikolov', 20, dbo.udf_get_city_id('Sofia'));
```

- Creating a trigger
```sql
CREATE TRIGGER tr_AddLogsToPersonLogsOnUpdate
ON Person FOR UPDATE 
AS 
	INSERT INTO PersonLogs(personId, oldAge, newAge, DateTimeOfChange)
	SELECT 
		i.id,
		d.AGE,
		i.AGE,
		GETDATE()
	FROM inserted i
	JOIN deleted d
		on d.ID = i.ID;


UPDATE PERSON
SET AGE = AGE + 1
WHERE ID = 3;


SELECT * FROM PERSONLOGS;
```

- Date Functions
```sql
SELECT DATEDIFF(day, '2000-05-05', '2000-06-06');

SELECT DATEPART(WEEKDAY, GETDATE());

SELECT DATENAME(WEEKDAY, GETDATE());

SELECT DATEADD(year, 1, '2023-05-29');
```
