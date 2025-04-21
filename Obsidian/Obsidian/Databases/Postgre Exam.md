```sql
CREATE TABLE accounts(
	id SERIAL PRIMARY KEY,
	username VARCHAR(30) NOT NULL UNIQUE,
	password VARCHAR(30) NOT NULL,
	email VARCHAR(50) NOT NULL,
	gender CHAR(1) NOT NULL CHECK (gender IN ('M', 'F')),
	age INT NOT NULL,
	job_title VARCHAR(40) NOT NULL,
	ip VARCHAR(30) NOT NULL
);

CREATE TABLE addresses(
	id SERIAL PRIMARY KEY,
	street VARCHAR(30) NOT NULL,
	town VARCHAR(30) NOT NULL,
	country VARCHAR(30) NOT NULL,
	account_id INT NOT NULL REFERENCES accounts(id)
		ON UPDATE CASCADE
		ON DELETE CASCADE
);

CREATE TABLE photos(
	id SERIAL PRIMARY KEY,
	description TEXT,
	capture_date TIMESTAMP NOT NULL,
	views INT NOT NULL DEFAULT 0 CHECK (views >= 0)
);

CREATE TABLE comments(
	id SERIAL PRIMARY KEY,
	content VARCHAR(255) NOT NULL,
	published_on TIMESTAMP NOT NULL,
	photo_id INT NOT NULL REFERENCES photos(id)
		ON UPDATE CASCADE 
		ON DELETE CASCADE
);

CREATE TABLE accounts_photos(
	account_id INT NOT NULL REFERENCES accounts(id)
		ON UPDATE CASCADE 
		ON DELETE CASCADE,
	photo_id INT NOT NULL REFERENCES photos(id)
		ON UPDATE CASCADE 
		ON DELETE CASCADE,
		PRIMARY KEY (account_id, photo_id)
);

CREATE TABLE likes(
	id SERIAL PRIMARY KEY,
	photo_id INT NOT NULL REFERENCES photos(id)
		ON UPDATE CASCADE 
		ON DELETE CASCADE,
	account_id INT NOT NULL REFERENCES accounts(id)
		ON UPDATE CASCADE 
		ON DELETE CASCADE
);
```

```sql
INSERT INTO addresses(street, town, country, account_id)
SELECT
	username,
	password,
	ip,
	age
FROM accounts
WHERE gender = 'F';
```


```sql
UPDATE addresses
SET country = 'Blocked'
WHERE addresses.country like 'B%';

UPDATE addresses
SET country = 'Test'
WHERE addresses.country like 'T%';

UPDATE addresses
SET country = 'In Progress'
WHERE addresses.country like 'P%';
```

```sql
DELETE FROM addresses
WHERE 
	id % 2 = 0 AND
	street like '%r%';
```

```sql
SELECT 
	username,
	gender,
	age
FROM accounts
WHERE 
	age >= 18 AND 
	length(username) > 9
ORDER BY
	age DESC, username ASC;
```

```sql
SELECT 
	photos.id as photo_id,
	capture_date,
	description,
	COUNT(comments.id) AS comments_count
FROM photos
JOIN comments ON
	photos.id = comments.photo_id
WHERE
	description IS NOT NULL
GROUP BY (photos.id)
ORDER BY 
	COUNT(comments.id) DESC,
	photos.id ASC
LIMIT 3;
```

```sql
SELECT  
	CONCAT_WS(' ', accounts.id, username) as id_username,
	email
FROM accounts
JOIN accounts_photos ON
	accounts.id = accounts_photos.account_id
WHERE
	accounts_photos.account_id = accounts_photos.photo_id;
```

```sql
SELECT 
	id as photo_id,
	(
		SELECT 
			COUNT(*) 
		FROM likes
		WHERE 
			likes.photo_id = photos.id
	) as likes_count,
	(
		SELECT 
			COUNT(*) 
		FROM comments
		WHERE 
			comments.photo_id = photos.id
	) as comments_count	
FROM photos
ORDER BY 
	likes_count DESC,
	comments_count DESC,
	photos.id ASC;
```

```sql
SELECT 
	CONCAT_WS('', SUBSTRING(description, 1, 10), '...'),
	TO_CHAR(capture_date, 'DD.MM HH24:MI') AS date
FROM photos
WHERE 
	DATE_PART('day', capture_date) = 10
ORDER BY capture_date DESC;
```

```sql
CREATE OR REPLACE FUNCTION udf_accounts_photos_count(account_username VARCHAR(30))
RETURNS INT AS 
$$
	DECLARE
		acc_id INT;
		photos_count INT;
	BEGIN 
		SELECT 
			id INTO acc_id
		FROM accounts
		WHERE 
			username = account_username;
		
		SELECT 
			COUNT(*) INTO photos_count
		FROM accounts_photos
		WHERE account_id = acc_id;
		
		RETURN photos_count;
	END;
$$
LANGUAGE PLPGSQL;
```

```sql
CREATE OR REPLACE PROCEDURE udp_modify_account(address_street VARCHAR(30), address_town VARCHAR(30))
AS
$$
	BEGIN
		UPDATE accounts
		SET job_title = CONCAT_WS(' ', '(Remote)', job_title)
		WHERE accounts.id IN (
			SELECT 
				accounts.id 
			FROM addresses
			JOIN accounts on
				addresses.account_id = accounts.id
			WHERE 
				street = address_street AND 
				town = address_town
			);
	END;
$$
LANGUAGE PLPGSQL;
```

wrong imp
```sql
SELECT 
	photos.id as photo_id,
	COUNT(likes.id) as likes_count,
	COUNT(comments.id) as comments_count
FROM photos
JOIN likes ON
	photos.id = likes.photo_id
JOIN comments ON
	photos.id = comments.photo_id
GROUP BY 
	photos.id
ORDER BY 
	COUNT(likes.id) DESC,
	COUNT(comments.id) DESC,
	photos.id ASC;
```