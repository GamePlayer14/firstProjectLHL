What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.


I definitely screwed up by removing a lot of data, without realizing that I shouldn't. I have a lot of missing data now, probably more than before, but the thought proccess behind it was that (for example) I'm not going to use the data from cities that don't exist, so why should they be there. In retrospect, I can probably create a view without them there. I still have the files anyways, so I can always recreate the database, but I think I want to keep it this way, because I'm not always gonna have the chance to just redo it.

moving on to what queries I used to see my answers better, I tried to find out what a 10 digit number was in terms of date and time, so I used:

  TO_TIMESTAMP(visit_start_time) AT TIME ZONE 'UTC'

That helped me turn nonsensical data into something I could read. I later changed it to be a timestamp itself, because no one is gonna look at that and see a date and time. I also quite frequently removed cities that were set as "(not set)" or "not available in demo dataset" with simple where queries:

  
	WHERE city NOT LIKE '(not set)' 
	AND city NOT LIKE 'not available in demo dataset' 

Also v2_product_category had a similar problem, so I solved it with a similar query:

   
	AND v2_product_category NOT LIKE '(not set)' 
	AND v2_product_category NOT LIKE '${escCatTitle}'

time_on_site is a number that corrolates to the amount of seconds i believe, so i made it show better:

  SELECT time_on_site, CONCAT((time_on_site / 60),':',SUBSTRING(CAST(CAST(MOD(time_on_site,60)AS decimal)/100 AS name),3,2)) FROM analytics

I looked to see if my math was correct, and it turned out great.

most of the things I changed about the database changed my in the analytics table, giving way different answers, and the things I changed in the all_sessions table I was not looking as much at anyways, so there is not much surface level damage. In the future I will think twice before changing the database.
