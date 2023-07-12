What are your risk areas? Identify and describe them.

1) There was so much information that was missing from the tables, especially the all_sessions table.

2) The fullVisitorId values are different between all_sessions and analytics tables, both the format and the distinct number

3) There is a column in analytics table which is called Userid, it was completely null, so was multiple other columns in all_sessions table

4) All_sessions table has too many columns and make the repetitive information high

5) Although there was a sale, the revenue column was empty or had null values for both analytics and all_sessions table


QA Process:
Describe your QA process and include the SQL queries used to execute it.

to calculate fullvisitorId in analytics:	SELECT count(DISTINCT "fullvisitorId")		
						FROM analytics
					**the result was (120018)

to calculate fullvisitorId in all_sessions:	SELECT count(DISTINCT "fullvisitorId")		
						FROM all_sessions
					**the result was (14147)

It is not a match
_______________________________________________________________________________________

Counting the visitId in analytics		SELECT count(DISTINCT "visitId")
						FROM analytics
					**the result was (148642)

Counting the visitId in analytics		SELECT count(DISTINCT "visitId")
						FROM all_sessions
					**the result was (14556)

It is not a match
______________________________________________________________________________________

Calculating productSKU

						select count(DISTINCT so."productSKU") 
						FROM sales_report so JOIN sales_by_sku sb ON so."productSKU"= sb."productSKU"
					**the result was 454



compare to sales_report table:			select count(DISTINCT "productSKU") 
						FROM sales_report
					**the result was 454
It is a match


And products table: 				select count(DISTINCT so."productSKU") 
						FROM sales_report so JOIN products pr ON so."productSKU"= pr."productSKU"
					**the result was 454
It is a match
_____________________________________________________________________________________

AS checking analytic table it looks like that the revenue was not calculated and left null:

					SELECT "units_sold", "unit_price", "revenue"
					from analytics
					where "units_sold" > 0 and "revenue" is null
				**The result was 79791

The same with all_sessions table:
 
					SELECT "productQuantity", "productPrice", "productRevenue"
					from all_sessions
					where "productQuantity" > 0 and "productRevenue" is null
				**The result was 49

That defiantly will affect the total revenue in one of the questions.
______________________________________________________________________________________






