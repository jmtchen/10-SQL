1a)

    SELECT first_name, last_name FROM actor;

1b)

    SELECT CONCAT()UPPER(first_name), ' ', UPPER(last_name)) AS Actor_Name FROM actor;

2a)

    SELECT actor_id, first_name, last_name FROM actor
    WHERE first_name = 'Joe';

2b)

    SELECT first_name, last_name FROM actor
    WHERE last_name LIKE'%GEN%';

2c)

    SELECT first_name, last_name FROM actor
    WHERE last_name LIKE'%LI%'
    ORDER BY last_name, first_name;

2d)

    SELECT country_id, country FROM country
    WHERE country IN ('Afghanistan', 'Bangladesh', 'China');

3a)

    ALTER TABLE actor 
    ADD COLUMN middle_name VARCHAR(45) after first_name;

3b)

    ALTER TABLE actor 
    MODIFY COLUMN middle_name BLOB;

3c)

    ALTER TABLE actor 
    DROP COLUMN middle_name;

4a)

    SELECT last_name, Count(last_name) AS count from actor 
    GROUP BY last_name;

4b)

    SELECT last_name, Count(last_name) AS count from actor 
    GROUP BY last_name
    HAVING count >= 2;

4c)

    UPDATE actor
    SET first_name = 'HARPO', last_name = 'WILLIAMS'
    WHERE first_name = 'GROUCHO';

4d)

    UPDATE actor
    SET first_name =
    CASE
    WHEN first_name = 'HARPO' AND last_name = 'WILLIAMS'
	    THEN 'GROUCHO'
    WHEN first_name = 'GROUCHO' AND last_name = 'WILLIAMS'
	    THEN 'MUCHO GROUCHO'
    ELSE first_name
    END;

5a)

    SHOW CREATE TABLE address;

6a)

    SELECT first_name, last_name, address
    FROM address
    JOIN staff
    USING(address_id);

6b)

    SELECT staff_id, first_name, last_name, sum(amount) as Total_Amount_Rung
    FROM payment  
    JOIN staff
    USING(staff_id)
    WHERE payment_date LIKE '2005-08%'
    GROUP BY staff_id;

6c)

    SELECT title, count(actor_id) as Actors_Count
    FROM film
    JOIN film_actor
    USING (film_id)
    GROUP BY title;

6d)

    SELECT title, count(film_id) as Inventory_Count
    FROM inventory 
    JOIN film
    USING (film_id)
    WHERE title = 'Hunchback Impossible';

6e)

    SELECT last_name, first_name, SUM(amount) AS Total_Amount_Paid
    FROM customer
    JOIN payment
    USING(customer_id)
    GROUP BY last_name;

7a)

    SELECT title
    FROM film
    WHERE film_id IN
    (
     SELECT film_id
     FROM film
     WHERE language_id IN
     (
     SELECT language_id
     FROM language
     WHERE name = "English"
     )
    )  
    AND title LIKE 'K%' OR title LIKE 'Q%';

7b)

    SELECT first_name, last_name
    FROM actor
    WHERE actor_id IN
    (
     SELECT actor_id
     FROM film_actor
     WHERE film_id IN
     (
     SELECT film_id
     FROM film
     WHERE title = "Alone Trip"
     )
    );

7c)

    SELECT last_name, first_name, email
    FROM customer
    JOIN address
    USING (address_id)
    JOIN city
    USING (city_id)
    JOIN country
    USING (country_id)
    WHERE country = 'Canada';

7d)

    SELECT title, name AS category
    FROM film
    JOIN film_category
    USING (film_id)
    JOIN category
    USING (category_id)
    WHERE name = "family";

7e)

    SELECT title, COUNT(inventory_id) AS Most_Frequently_Rented_Movie_Greater_Or_Equal_To_20
    FROM film
    JOIN inventory
    USING(film_ID)
    JOIN rental
    USING (inventory_id)
    GROUP BY title
    HAVING COUNT(inventory_id) >= 20
    ORDER BY COUNT(inventory_id) DESC;

7f)

    SELECT store_id, concat('$',format(sum(amount),2)) as Business_In_Dollars 
    FROM store
    JOIN customer
    USING(store_id)
    LEFT OUTER JOIN payment
    USING (customer_id)
    GROUP BY store_id;

7g)

    SELECT store_id, city, country
    FROM store
    JOIN address
    USING(address_id)
    JOIN city
    USING (city_id)
    JOIN country
    USING (country_id);

7h) 

    SELECT name, concat('$',format(sum(amount),2)) AS Gross_Revenue 
    FROM category
    JOIN film_category
    USING (category_id)
    JOIN inventory
    USING (film_id)
    JOIN rental
    USING (inventory_id)
    JOIN payment
    USING (rental_id)
    GROUP BY name
    ORDER BY Gross_Revenue DESC
    LIMIT 5;

8a)

    CREATE VIEW Top5_Genre_View AS
    SELECT name, concat('$',format(sum(amount),2)) AS Gross_Revenue 
    FROM category
    JOIN film_category
    USING (category_id)
    JOIN inventory
    USING (film_id)
    JOIN rental
    USING (inventory_id)
    JOIN payment
    USING (rental_id)
    GROUP BY name
    ORDER BY Gross_Revenue DESC
    LIMIT 5;
 
8b)

    Refresh SCHEMAS, View Menu, Select top5_genre_view and click on table icon
    SELECT * FROM Top5_Genre_View;

8c)

    DROP VIEW IF EXISTS Top5_Genre_View;