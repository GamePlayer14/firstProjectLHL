What issues will you address by cleaning the data?

I will remove any columns where there is no data, redundant data, and any rows where the city is not set or not available in demo dataset. I will also make sure all columns are the right format, eg. visit_start_time is a timestamp.



Queries:
Below, provide the SQL queries you used to clean your data.

ALTER TABLE all_sessions
DROP COLUMN transaction_revenue;
DROP COLUMN product_refund_amount;
DROP COLUMN item_quantity;
DROP COLUMN page_path_level1;
DROP COLUMN currency_code;
RENAME COLUMN item_revenue TO payment_method;

DELETE FROM all_sessions
WHERE city LIKE '(not set)'
OR city LIKE 'not available in demo dataset'

ALTER TABLE analytics
DROP COLUMN social_engagement_type;

DELETE FROM analytics
WHERE visit_id IN (
  SELECT visit_id
  FROM (
    SELECT visit_id, COUNT(*) AS count
    FROM analytics
    GROUP BY visit_id
    HAVING COUNT(*) > 1
  ) AS duplicates
);
