
```sql
CREATE TABLE ITEMS(
	id SERIAL PRIMARY KEY,
	STATUS INT,
	CREATE_DATE DATE
);


CREATE TABLE ITEMS_LOGS(
	id SERIAL PRIMARY KEY,
	STATUS INT,
	CREATE_DATE DATE
);


CREATE OR REPLACE FUNCTION log_items()
RETURNS TRIGGER AS
$$
	BEGIN
		INSERT INTO items_logs(status, create_date)
		VALUES (new.status, new.create_date);
		
		RETURN new;
	END;
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE TRIGGER log_items_trigger
AFTER INSERT ON items
FOR EACH ROW 
EXECUTE PROCEDURE log_items();


INSERT INTO ITEMS(status, create_date)
VALUES
	(1, '2024-05-17'),
	(0, '2024-05-17');
	
	
SELECT * FROM items_logs;
```


```sql
CREATE OR REPLACE FUNCTION delete_last_item_log()
RETURNS TRIGGER AS
$$
	BEGIN
		DELETE 
		FROM items_logs
		WHERE 
			id <= (
				(SELECT id from items_logs order by id desc limit 1) - 10
			);
		
		RETURN new;
	END;
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE TRIGGER delete_old_logs
AFTER INSERT ON items_logs
FOR EACH ROW 
EXECUTE PROCEDURE delete_last_item_log();
```

```sql
CREATE OR REPLACE FUNCTION delete_last_item_log()
RETURNS TRIGGER AS 
$$
	BEGIN
		WHILE (SELECT count(*) from items_log) > 10 LOOP
			DELETE 
			FROM items_log 
			WHERE 
				id = (
					SELECT 
						id 
					FROM items_log 
					ORDER BY id
					LIMIT 1
				);
		END LOOP;
	END;	
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE TRIGGER clear_item_log
AFTER INSERT ON items_logs
FOR EACH EXECUTE delete_last_item_log();
```


```sql
CREATE TABLE deleted_employees(
	employee_id SERIAL PRIMARY KEY,
	first_name VARCHAR(20),
	middle_name VARCHAR(20),
	last_name VARCHAR(20),
	job_title VARCHAR(20),
	department_id INT,
	salary Numeric(19, 4)
);


CREATE OR REPLACE FUNCTION backup_fired_employees()
RETURNS TRIGGER AS 
$$
	BEGIN
		INSERT INTO deleted_employees(first_name, middle_name, last_name, job_title, department_id, salary)
		VALUES
			(old.first_name, old.middle_name, old.last_name, old.department_id, old.salary);
		
		RETURN NEW;
	END;
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE TRIGGER backup_employees
AFTER DELETE ON employees
FOR EACH ROW 
EXECUTE PROCEDURE backup_fired_employees();
```