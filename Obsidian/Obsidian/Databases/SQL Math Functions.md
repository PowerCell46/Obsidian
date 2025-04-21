
```sql
CREATE OR REPLACE FUNCTION fn_calculate_sum(first_number int, second_number int)
RETURNS INT AS 
$$
	BEGIN
		RETURN first_number + second_number;
	END
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE FUNCTION fn_calculate_extraction(first_number int, second_number int)
RETURNS INT AS 
$$
	BEGIN
		RETURN first_number - second_number;
	END;
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE FUNCTION fn_calculate_multiplication(first_number int, second_number int)
RETURNS INT AS
$$
	BEGIN
		RETURN first_number * second_number;
	END
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE FUNCTION fn_calculate_division(first_number int, second_number int)
RETURNS INT AS
$$
	DECLARE
		result_var int;
	BEGIN
		IF first_number = 0 THEN result_var := 0;
		ELSIF second_number = 0 THEN result_var := null;
		ELSE result_var := first_number / second_number;
		END IF;
		
		RETURN result_var;
	END;
$$
LANGUAGE PLPGSQL;
```


