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



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







