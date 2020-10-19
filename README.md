# SQueriosL

# This is a basic SQL queries page where I've put up queries practiced from various sources, starting from a a SQL book : JumpStartSQL :

CREATE TABLE employee (
 employee_id	INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
 last_name	VARCHAR(30) NOT NULL,
 first_name	VARCHAR(30) NOT NULL,
 email	        VARCHAR(100) NOT NULL,
 hire_date	DATE NOT NULL,
 notes	        MEDIUMTEXT,
 PRIMARY KEY (employee_id),
 INDEX (last_name),
 UNIQUE (email)
);

ENGINE=InnoDB;


-----------------

CREATE TABLE address (
 employee_id INTEGER UNSIGNED NOT NULL,
 address VARCHAR(50) NOT NULL,
 city VARCHAR(30) NOT NULL,
 state CHAR(2) NOT NULL,
 postcode CHAR(5) NOT NULL,
 FOREIGN KEY (employee_id)
 REFERENCES employee (employee_id)
)

ENGINE=InnoDB;


-----------------


CREATE TABLE charset_example (
    ->  id INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
    ->  ascii_string VARCHAR(255) CHARACTER SET ascii NOT NULL,
    ->  latin1_string VARCHAR(255) CHARACTER SET latin1 NOT NULL,
    ->  utf8_string VARCHAR(255) CHARACTER SET utf8 NOT NULL,
    ->  PRIMARY KEY (id)
    -> );


-----------------

 INSERT INTO employee(first_name, last_name, email, hire_date) 
 VALUES ('Nischal', 'Bhatia', 'nbhatia@example.com', '2014-12-15');

 INSERT INTO employee(first_name, last_name, email, hire_date)
 VALUES('Gurjot','Singh','singh.gurjot@example.com','2017-12-08');
 
 INSERT INTO employee(first_name, last_name, email, hire_date)
 VALUES('Jaskaran','Singh','sandhujk@example.com','2017-04-14');

 INSERT INTO employee(first_name, last_name, email, hire_date)
 VALUES ('Anjandeep', 'Singh', 'ginni@example.com', '2017-11-30');

 SELECT * FROM employee;

-----------------


 INSERT INTO address(employee_id,address,city,state,postcode)
 VALUES(1, '123 Main Street', 'Anytowne', 'XE', '97052');

-----------------
	
	GROUP BY CLAUSE : : 
	- Suppose we want to find the top 10 customers who’ve spent the most money renting
	  movies.

	SELECT customer_id, SUM(amount) AS Amt
	FROM payment
    ->  GROUP BY customer_id
    ->  ORDER BY Amt DESC
    ->  LIMIT 10;

------------------

	UPDATE CLAUSE : :

	SELECT customer_id, first_name, last_name
        FROM customer
        WHERE first_name = 'Courtney' AND last_name = 'Day';



--	UPDATE customer
        SET last_name = 'DAY-WEBB'
        WHERE customer_id = 245;

--	SELECT customer_id, first_name, last_name, last_update
        FROM customer
        WHERE first_name = 'Courtney';


------------------

	DELETE CLAUSE : :
		
	UPDATE customer
        SET active = 0
        WHERE customer_id = 245;


	DELETE FROM rental / payment
	WHERE customer_id = 245;

	Now, we are allowed to delete the original P.K rows

	DELETE FROM customer
	WHERE customer_id = 245;
	

-------------------

	Retrieve all of the actors with “SON” in their last name and sort them alphabetically.
	
	SELECT actor_id, last_name
        FROM actor
        WHERE last_name LIKE('%SON')
        ORDER BY last_name ASC;


	Calculate how many films there are for each rating category—G, PG, PG-13, R, and NC-17.


	SELECT rating, COUNT(film_id) AS NumFilm
        FROM film
        GROUP BY rating;

	
	What’s the ID of the customer who’s made the most visits to the video store?

	
	SELECT customer_id, COUNT(payment_id) AS NumVisits, amount
        FROM payment
        GROUP BY customer_id
        ORDER BY NumVisits DESC;

---------------------


	Let’s identify the top five actors who made the most film appearances and the
	number of films they’ve each starred in.

	SELECT actor_id, COUNT(actor_id) AS Appearances
        FROM film_actor
        GROUP BY actor_id
        ORDER BY Appearances DESC
        LIMIT 5;

	Now, since actor_id is also present in the actor table 
	we use another SELECT statement to retrieve corresponding rows
	
	SELECT actor_id, first_name, last_name
     -> FROM actor
     -> WHERE actor_id IN(107,102,198,181,23);
	
	We have all the information we wanted, but unfortunately we still need to match
	up the actors’ names and appearance counts manually
	
	JOIN lets us do exactly that by connecting two or more tables based on a relationship that we specify.
	
	
	SELECT a.first_name, a.last_name, COUNT(fa.actor_id) AS Appearance_Count
     -> FROM film_actor fa JOIN actor a
     -> ON a.actor_id = fa.actor_id
     -> GROUP BY fa.actor_id
     -> ORDER BY Appearance_Count DESC, a.first_name ASC, a.last_name ASC
     -> LIMIT 5;


---------------------

		JOINS : : 3 Types : : 

	CREATE TABLE foo(foo_id INTEGER, foo_value CHAR(3));
	
	CREATE TABLE bar(bar_id INTEGER, bar_value CHAR(3), foo_id INTEGER);

	INSERT INTO foo(foo_id, foo_value)
     -> VALUES(1, 'foo');

	INSERT INTO foo(foo_id, foo_value)
     -> VALUES(2, 'bar');

	INSERT INTO bar(bar_id, bar_value, foo_id)
     -> VALUES(1, 'baz', 2);

	INSERT INTO bar(bar_id, bar_value, foo_id)
     -> VALUES(2, 'qux', 3);

	 SHOW TABLES;
	+---------------+
	| Tables_in_foo |
	+---------------+
	| bar           |
	| foo           |
	+---------------+

	SELECT *
     -> FROM foo f INNER JOIN bar b
     -> ON b.foo_id = f.foo_id;
	
	+--------+-----------+--------+-----------+--------+
	| foo_id | foo_value | bar_id | bar_value | foo_id |
	+--------+-----------+--------+-----------+--------+
	|      2 | bar       |      1 | baz       |      2 |
	+--------+-----------+--------+-----------+--------+


mysql> SELECT *
    -> FROM foo f LEFT OUTER JOIN bar b
    -> ON b.foo_id = f.foo_id;

	+--------+-----------+--------+-----------+--------+
	| foo_id | foo_value | bar_id | bar_value | foo_id |
	+--------+-----------+--------+-----------+--------+
	|      1 | foo       |   NULL | NULL      |   NULL |
	|      2 | bar       |      1 | baz       |      2 |
	+--------+-----------+--------+-----------+--------+

mysql> SELECT *
    -> FROM foo f RIGHT OUTER JOIN bar b
    -> ON b.foo_id = f.foo_id;

	+--------+-----------+--------+-----------+--------+
	| foo_id | foo_value | bar_id | bar_value | foo_id |
	+--------+-----------+--------+-----------+--------+
	|      2 | bar       |      1 | baz       |      2 |
	|   NULL | NULL      |      2 | qux       |      3 |
	+--------+-----------+--------+-----------+--------+

mysql> SELECT *
    -> FROM foo JOIN bar;
	
	+--------+-----------+--------+-----------+--------+
	| foo_id | foo_value | bar_id | bar_value | foo_id |
	+--------+-----------+--------+-----------+--------+
	|      1 | foo       |      1 | baz       |      2 |
	|      2 | bar       |      1 | baz       |      2 |
	|      1 | foo       |      2 | qux       |      3 |
	|      2 | bar       |      2 | qux       |      3 |
	+--------+-----------+--------+-----------+--------+
	
----------------------

	ABSTRACTING WITH VIEWS : :	
	
mysql> CREATE VIEW Actor_Appearance AS
    -> SELECT a.first_name, a.last_name, COUNT(fa.film_id) AS Appearance_Count
    -> FROM film_actor fa JOIN actor a
    -> ON a.actor_id = fa.actor_id
    -> GROUP BY fa.actor_id;
	
mysql> SELECT *
    -> FROM Actor_Appearance
    -> ORDER BY Appearance_Count DESC
    -> LIMIT 5;
	
	+------------+-----------+------------------+
	| first_name | last_name | Appearance_Count |
	+------------+-----------+------------------+
	| GINA       | DEGENERES |               42 |
	| WALTER     | TORN      |               41 |
	| MARY       | KEITEL    |               40 |
	| MATTHEW    | CARREY    |               39 |
	| SANDRA     | KILMER    |               37 |
	+------------+-----------+------------------+
	

	### Duplication may arise, if ::
	
	SELECT c.first_name, c.last_name, c.last_update, a.address, a.last_update
	FROM customer AS c JOIN address AS a 
	ON a.address_id = c.address_id;
	
	### Possible Solution is to provide alias or : :

mysql> CREATE VIEW cust_address(first_name, last_name, cust_last_update, address, add_last_update) AS
    -> SELECT c.first_name, c.last_name, c.last_update, a.address, a.last_update
    -> FROM customer c JOIN address a
    -> ON a.address_id = c.address_id;


mysql> CREATE VIEW active_customer AS
    -> SELECT customer_id, first_name, last_name, email
    -> FROM customer
    -> WHERE active = 1;
	


------------------------------

	NORMALIZING FORMS : :

	THIRD NF :

	SELECT  a.actor_id, a.first_name, a.last_name, f.film_id, f.title
	FROM film_actor fa
 	JOIN actor a ON fa.actor_id = a.actor_id
 	JOIN film f ON fa.film_id = f.film_id;

	
------------------------------


	ALTERING TABLES : :

	mysql> ALTER TABLE actor
            -> ADD COLUMN bio VARCHAR(255) AFTER last_name;	
	
	mysql> ALTER TABLE actor
            -> DROP COLUMN bio;

	mysql> ALTER TABLE actor
            -> ADD INDEX idx_last_update(last_update);

	mysql> ALTER TABLE actor
            -> DROP INDEX idx_last_update;	
	

-------------------------------


	EXERCISE : :

 1.	Find the customers that rented the most movies
		
mysql> SELECT COUNT(rental_id) AS Num_Rented, customer_id
    -> FROM rental
    -> GROUP BY customer_id
    -> ORDER BY Num_Rented DESC
    -> LIMIT 100;
	
 2. 	Are there customers whose rental habits show they have a favorite actor? 
	Look through the other tables defined in the sakila database and observe how they follow 3NF.


	
	SELECT  a.actor_id, a.first_name, a.last_name, f.film_id, f.title
	FROM film_actor fa
 	JOIN actor a ON fa.actor_id = a.actor_id
 	JOIN film f ON fa.film_id = f.film_id;
	
