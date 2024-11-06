Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT DISTINCT(country), SUM(total_transaction_revenue) AS revenue FROM all_sessions
	WHERE total_transaction_revenue IS NOT NULL
	GROUP BY country
	ORDER BY revenue DESC

SELECT DISTINCT(city), SUM(total_transaction_revenue) AS revenue FROM all_sessions
	WHERE total_transaction_revenue IS NOT NULL
	AND city NOT LIKE 'not available in demo dataset'
	GROUP BY city
	ORDER BY revenue DESC

Answer: The USA has the highest total transaction revenue at ~$13b, followed by Israel at ~$600m and Australia at $350m.

San Francisco is the city with the highest total transaction revenue at ~$1.5b, followed by Sunnyvale at ~$992m and Atlanta at ~$854m.


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT DISTINCT(city), AVG(product_quantity), COUNT(transactions) FROM all_sessions
	WHERE city NOT LIKE 'not available in demo dataset'
	AND product_quantity IS NOT NULL
	GROUP BY city
	ORDER BY AVG(product_quantity) DESC

 SELECT DISTINCT(country), AVG(product_quantity), COUNT(transactions) FROM all_sessions
	WHERE product_quantity IS NOT NULL
	GROUP BY country
	ORDER BY AVG(product_quantity) DESC

Answer:
Spain is the country with the most orders, followed by the US, and Madrid is the city with the most orders, followwed be Salem.


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

CREATE TABLE ttabc AS
SELECT 
	country, 
	COUNT(v2_product_category)AS count_of_uses
FROM all_sessions
	WHERE country NOT LIKE '(not set)' 
	AND country NOT LIKE 'not available in demo dataset' 
	AND v2_product_category NOT LIKE '(not set)' 
	AND v2_product_category NOT LIKE '${escCatTitle}'
	GROUP BY country
	ORDER BY COUNT(v2_product_category) DESC, country
 
-----------------------------------------------------------------

WITH this AS(
SELECT 
	country, 
	v2_product_category,
	COUNT(v2_product_category) AS count_of_uses
FROM all_sessions
	WHERE v2_product_category NOT LIKE '(not set)' 
	AND v2_product_category NOT LIKE '${escCatTitle}'
	GROUP BY country, v2_product_category
)

SELECT 
	t.country, t.v2_product_category, t. count_of_uses, tt.count_of_uses AS tolal_uses
FROM this t
	JOIN ttabc tt ON t.country = tt.country
	ORDER BY tt.count_of_uses DESC, t.count_of_uses DESC
 
==================================================================

CREATE TABLE ttab AS
SELECT 
	city, 
	COUNT(v2_product_category)AS count_of_uses
FROM all_sessions
	WHERE city NOT LIKE '(not set)' 
	AND city NOT LIKE 'not available in demo dataset' 
	AND v2_product_category NOT LIKE '(not set)' 
	AND v2_product_category NOT LIKE '${escCatTitle}'
	GROUP BY city
	ORDER BY COUNT(v2_product_category) DESC, city
 
--------------------------------------------------------------------
WITH this AS(
SELECT 
	city, 
	v2_product_category,
	COUNT(v2_product_category) AS count_of_uses
FROM all_sessions
	WHERE city NOT LIKE '(not set)' 
	AND city NOT LIKE 'not available in demo dataset' 
	AND v2_product_category NOT LIKE '(not set)' 
	AND v2_product_category NOT LIKE '${escCatTitle}'
	GROUP BY city, v2_product_category
)

SELECT 
	t.city, t.v2_product_category, t. count_of_uses, tt.count_of_uses AS tolal_uses
FROM this t
	JOIN ttab tt ON t.city = tt.city
	ORDER BY tt.count_of_uses DESC, t.count_of_uses DESC

Answer:In the US, mens TShirts are the prevelent catagory, followed by youtube, but it's flipped in India.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
WITH everything AS(
	WITH that AS(
		WITH this AS(
			SELECT 
				country,
				v2_product_name AS name,
				COUNT(v2_product_name) AS total
			FROM all_sessions
				WHERE country NOT LIKE '(not set)'
				GROUP BY country, v2_product_name
				ORDER BY total DESC, country
		)
		SELECT *,
			RANK() OVER(PARTITION BY country ORDER BY total DESC) AS top 
		FROM this
	)
	SELECT 
		country, 
		name, 
		total 
	FROM that
		WHERE top = 1
		ORDER BY total DESC
)
SELECT * FROM everything

WITH everything AS(
	WITH that AS(
		WITH this AS(
			SELECT 
				city,
				v2_product_name AS name,
				COUNT(v2_product_name) AS total
			FROM all_sessions
				WHERE city NOT LIKE '(not set)'
				AND city NOT LIKE 'not available in demo dataset'
				GROUP BY city, v2_product_name
				ORDER BY total DESC, city
		)
		SELECT *,
			RANK() OVER(PARTITION BY city ORDER BY total DESC) AS top 
		FROM this
	)
	SELECT 
		city, 
		name, 
		total 
	FROM that
		WHERE top = 1
		ORDER BY total DESC
)
SELECT * FROM everything

Answer:
The most popular answer in all cities was "Google Men's 100% Cotton Short Sleeve Hero Tee White", but interestingly mountain view has the most skewed orders towards "Nest® Cam Indoor Security Camera - USA".
It's the same with countries, but with "22 oz YouTube Bottle Infuser" closer in second.





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







