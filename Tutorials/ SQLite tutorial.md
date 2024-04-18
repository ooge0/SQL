-- CREATE TABLE()
-- Creates a new table.
CREATE TABLE food (id INTEGER PRIMARY KEY, name TEXT UNIQUE, price INTEGER CHECK (price <= 30), food_category TEXT , exp_date DATE);
CREATE TABLE IF NOT EXISTS products (id INTEGER PRIMARY KEY, name TEXT, product_id INTEGER, price INT, customer_id INT);
CREATE TABLE IF NOT EXISTS customer (id INTEGER PRIMARY KEY, name TEXT, customer_id INTEGER);

-- ALTER TABLE()
-- Modifies an existing table.
ALTER TABLE products ADD COLUMN food_id INT;

-- DROP TABLE()
-- Deletes an existing table.
DROP TABLE IF EXISTS food;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS customer;

-- INSERT INTO()
-- Inserts a new row into a table.
-- Insert 12 records into the 'products' table
INSERT INTO products (id, name, product_id, food_id, price, customer_id)
VALUES
    (1, 'Flour', 101, 1, 3.99, 1),
    (2, 'Tomato Sauce', 102, 1, 2.99, 1),
    (3, 'Cheese', 103, 1, 4.99, 1),
    (4, 'Pepperoni', 104, 1, 2.49, 1),
    (5, 'Bun', 105, 2, 1.99, 2),
    (6, 'Beef Patty', 106, 2, 3.49, 2),
    (7, 'Lettuce', 107, 2, 0.99, 2),
    (8, 'Tomato', 108, 2, 1.49, 2),
    (9, 'Croutons', 109, 3, 0.49, 3),
    (10, 'Chicken', 110, 3, 3.99, 3),
    (11, 'Pasta Noodles', 111, 4, 2.99, 4),
    (12, 'Alfredo Sauce', 112, 4, 4.99, 4);

-- Insert 12 records into the 'food' table with categories and prices
INSERT INTO food (id, name, food_category, price, exp_date)
VALUES
    (1, 'Pizza', 'Expensive', 15.99, '2001-01-01'),
    (2, 'Burger', 'Moderate', 8.99, '2002-02-02'),
    (3, 'Salad', 'Moderate', 7.49, '2003-03-03'),
    (4, 'Pasta', 'Expensive', 18.99, '2004-04-04'),
    (5, 'Sandwich', 'Moderate', 6.99, '2005-05-05'),
    (6, 'Steak', 'Expensive', 25.99, '2006-06-06'),
    (7, 'Sushi', 'Expensive', 22.99, '2007-07-07'),
    (8, 'Soup', 'Cheap', 4.99, '2008-08-08'),
    (9, 'Taco', 'Moderate', 9.99, '2009-09-09'),
    (10, 'Cake', 'Moderate', 12.99, '2010-10-10'),
    (11, 'Ice Cream', 'Cheap', 3.99, '2011-11-11'),
    (12, 'Coffee', 'Cheap', 1.99, '2012-12-12');


-- Insert 12 records into the 'customer' table
INSERT INTO customer (id, name, customer_id)
VALUES
    (1, 'John Doe', 1),
    (2, 'Alice Smith', 2),
    (3, 'Bob Johnson', 3),
    (4, 'Carol Williams', 4),
    (5, 'David Brown', 5),
    (6, 'Emma Jones', 6),
    (7, 'Frank Miller', 7),
    (8, 'Grace Davis', 8),
    (9, 'Henry Martinez', 9),
    (10, 'Ivy Wilson', 10),
    (11, 'Jack Taylor', 11),
    (12, 'Karen Anderson', 12);

-- UPDATE()
-- Updates the values of one or more rows in a table.
UPDATE food SET name = 'Burger' WHERE id = 1;
UPDATE products SET name = 'Butter' WHERE id = 1;

-- DELETE FROM()
-- Deletes one or more rows from a table.
DELETE FROM food WHERE id = 1;

-- SELECT()
-- Selects rows from a table.
SELECT * FROM food;
SELECT * FROM products;
SELECT * FROM customer;
SELECT id, name, food_id  FROM food;
SELECT id, name, product_id , food_id FROM products;

-- JOIN()
-- Combines the rows from two or more tables.
SELECT f.id, f.name, p.product_id FROM food AS f JOIN products AS p ON f.id = p.food_id;

-- LEFT JOIN: Returns all rows from the left table (food) and the matching rows from the right table (products)
SELECT f.id AS food_id, f.name AS food_name, p.name AS product_name, c.name AS customer_name
FROM food f
LEFT JOIN products p ON f.id = p.food_id
LEFT JOIN customer c ON f.customer_id = c.id;

-- RIGHT JOIN: Returns all rows from the right table (products) and the matching rows from the left table (food)
SELECT f.id AS food_id, f.name AS food_name, p.name AS product_name, c.name AS customer_name
FROM products p
RIGHT JOIN food f ON f.id = p.food_id
LEFT JOIN customer c ON f.customer_id = c.id;

-- INNER JOIN: Returns only the rows where there is a match in both tables
SELECT f.id AS food_id, f.name AS food_name, p.name AS product_name, c.name AS customer_name
FROM food f
INNER JOIN products p ON f.id = p.food_id
INNER JOIN customer c ON f.customer_id = c.id;

-- GROUP BY()
-- Groups rows together based on the values in a column.
SELECT name, COUNT(*) AS total_orders FROM food GROUP BY name;

-- ORDER BY()
-- Orders the rows in a result set by the values in a column.
SELECT name, COUNT(*) AS total_orders FROM food ORDER BY total_orders DESC;

-- DISTINCT()
-- Removes duplicate rows from a result set.
SELECT DISTINCT name FROM food;

-- LIMIT()
-- Limits the number of rows returned by a query.
SELECT id, name FROM food LIMIT 10;

-- OFFSET()
-- Skips the first n rows returned by a query.
SELECT id, name FROM food OFFSET 10; -- MySQL
SELECT id, name FROM food LIMIT -1 OFFSET 10; -- SQLite

-- LIKE()
-- Performs a pattern match on a string.
SELECT name, id FROM food WHERE name LIKE '%a%';

-- IN()
-- Checks if a value is in a list of values.
SELECT name, id FROM food WHERE id IN (1, 2, 3);

-- EXISTS()
-- Checks if a row exists in a table.
SELECT name, id FROM food WHERE EXISTS (SELECT 1 FROM customer WHERE customer.id = food.customer_id); 

-- CASE()
-- Performs a multi-way conditional operation.
SELECT id, name, CASE WHEN price > 100 THEN 'Expensive' WHEN price > 50 THEN 'Moderate' ELSE 'Cheap' END FROM food; 

SELECT
    f.id,
    f.name,
    f.price,
    f.food_category,
    f.exp_date,
    CASE
        WHEN JULIANDAY(f.exp_date) - JULIANDAY('now') < 0 THEN 'Expired'
        ELSE 'Valid'
    END AS expiration_status
FROM
    food f;

-- COUNT()
-- Returns the number of rows in a table or the number of rows that satisfy a condition.
SELECT COUNT(*) FROM food;

-- SUM()
-- Returns the sum of the values in a column.
SELECT SUM(price) FROM food;

-- AVG()
-- Returns the average of the values in a column.
SELECT AVG(price) FROM food;

-- MIN()
-- Returns the minimum value in a column.
SELECT MIN(price) FROM food;

-- MAX()
-- Returns the maximum value in a column.
SELECT MAX(price) FROM food;

-- UPPER()
-- Converts a string to uppercase.
SELECT UPPER(name) FROM food;

-- LOWER()
-- Converts a string to lowercase.
SELECT LOWER(name) FROM food;

-- LENGTH()
-- Returns the length of a string.
SELECT name, LENGTH(name) FROM food;

-- TRIM()
-- Removes leading and trailing spaces from a string.
SELECT name ,TRIM(name) FROM food;

-- CONCAT()
-- Concatenates two strings.
SELECT name || ' with Cheese' FROM food;

-- SUBSTR()
-- Extracts a substring from a string.
SELECT SUBSTR(name, 1, 3) FROM food;

-- INSTR()
-- Returns the position of a substring in a string.
SELECT name, INSTR(name, 'a') FROM food;

-- IFNULL()
-- Returns a substitute value if a value is NULL.
SELECT name, IFNULL(price, 0) FROM food;
