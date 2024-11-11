Question 1: What is the average semtiment score for each country?

SQL Queries:

	WITH this AS(
	SELECT 
		DISTINCT a.country, 
		AVG(sentiment_score)OVER(PARTITION BY a.country) AS avg_s_score
	FROM sales_report sr
		JOIN all_sessions a ON sr.product_sku = a.product_sku
	)
	SELECT *, DENSE_RANK()OVER(ORDER BY avg_s_score DESC) FROM this
		ORDER BY avg_s_score DESC

Answer: Sint Maarten has the highest avg sentiment score of 0.9, and Gibraltar with the least at -0.5.



Question 2: what time do the most units get sold?

SQL Queries:

	SELECT 
		SUBSTRING(CONCAT(CAST(TO_TIMESTAMP(visit_start_time) AT TIME ZONE 'UTC' AS TIME)),0,3)AS time,
		SUM(units_sold) 
	FROM analytics
		GROUP BY time
		ORDER BY time

Answer: Sales peak at around 7:00pm, and at their lowest at around 9:00am.



Question 3: What are the most popular products, and how much do we have stocked?

SQL Queries:

	SELECT name, ordered_quantity, stock_level FROM products
	ORDER BY ordered_quantity DESC

Answer: Kickballs are the most popular product with 15,170 ordered, and we have a remaining stock of 723.



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
