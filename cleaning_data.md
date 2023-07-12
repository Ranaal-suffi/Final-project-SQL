What issues will you address by cleaning the data?

1) By removing duplicate will help in creating a primary key for the table(unique column)
2) changing the missing value to null instead of different missing format and make it consistent.
3) change the date and time format into proper date format
4) Help fill in missing information.
5) dividing big table to smaller, easy to manage tables and that helped with reducing the null values in columns.



Queries:
Below, provide the SQL queries you used to clean your data.

1) working on all_sessions table:
	Convert the unknown or missing data as following:

	a)	Query:			update all_sessions
					set city = null
					WHERE city ='(not set)' or city = 'not available in demo dataset'

	b)	Query:			update all_sessions
					set country = null
					WHERE country ='(not set)' or country = 'not available in demo dataset'


	c)	Query:			set "productVariant" = null
					WHERE "productVariant" ='(not set)' or "productVariant" = 'not available in demo dataset'
	
	d) 	for v2productcategory, changed so it matches up in all product with same product SKU. By doing that I got rid of all (not set) category value 
		and make the category section uniform which can help in future clean up
		
		Query:			update all_sessions
					set v2productcategory = (category name to change it to)
					where productsku = (productnumber)

	e) 	Change productvariant that is associate to similar product- This setup helped fill some of the null values. Did the same to the "currencycode" column
		
		Query:			update all_sessions1
					set productvariant = 'Single Option Only'
					where productsku = (productnumber)
		-----------------------------------------------------------------------------------------------------------------

	Convert the date into YYYY-MM-DD format as it was without (-)
				
		Query:			ALTER TABLE all_sessions
					alter COLUMN date type date using to_date(date,'YYYYMMDD');
                -------------------------------------------------------------------

	Divided productprice and product revenue by 1,000,000 

		Query:			update all_sessions
					set (column name) =  (column name)/1000000  		

		-------------------------------------------------------------------

	For the time on the table, I converted to a time without timezone:

		Query:			alter TABLE all_sessions
					alter COLUMN "time" type time USING (interval '01:00' * "time")::time
		------------------------------------------------------------------

	Multiple Columns are empty without any data need to be removed for example 
		1-itemQuantity is null
		2-productRefundAmount is null
		3-itemRevenue is null
		4-searchKeyword is null

	Using the following:
		Query:			select * from all_sessions
					where [column name] is not null;

	By removing these columns, it will help make the table more readable and easy to determine if we need to divide the information into more 
	easy to manage table.
	
	For this table I decided to create two tables from it. 
	Table #1
		Query:			create TABLE all_sessions1 (fullVisitorId numeric,
						   		channelGrouping VARCHAR,
						   		time TIME, country VARCHAR,
						   		city VARCHAR, totalTransactions INTEGER,
						   		transactions integer, timeOnSite INTEGER,
						   		pageviews INTEGER, sessionQualityDim INTEGER,
						   		date date,  visitId NUMERIC, type varchar,
						   		productRefundAmount NUMERIC, productQuantity INTEGER,
						   		productPrice NUMERIC, productRevenue numeric,
						   		productSKU VARCHAR, v2ProductName VARCHAR,
						   		v2ProductCategory VARCHAR, productVariant varchar, currencyCode VARCHAR,
						   		itemQuantity INTEGER, itemRevenue NUMERIC, transactionRevenue NUMERIC,
						   		transactionId VARCHAR, pageTitle VARCHAR, searchKeyword VARCHAR,
						  		pagePathLevel1 varchar, eCommerceAction_type INTEGER,
						   		eCommerceAction_step INTEGER, eCommerceAction_option varchar);

	
	I copied the table to a new one called all_sessions1 

		Query:			INSERT INTO all_sessions1
					SELECT * from all_sessions;
	
	Table #2
		Query:			create TABLE all_sessions_visitors (fullVisitorId numeric,country VARCHAR,
							city VARCHAR, visitId NUMERIC)
	
	I copied the table to a new one called all_sessions1 
		Query:
					INSERT INTO all_sessions_visitors
					SELECT fullVisitorId ,country ,
						city , visitId
					from all_sessions;

	From there:
	1- Deleted all the empty columns itemQuantity, productRefundAmount, itemRevenue, searchKeyword from all_sessions1.
	2- I deleted all the duplicate in the visitid using the following query:
		(To help this step, I created a new column called ID that contains incremented value, this column was 
		deleted after the process) 
			
		Query:		DELETE from all_sessions_visitors al1
				using all_sessions_visitors al2
				where al1.id < al2.id
				and al1.visitid = al2.visitid;

	By doing that I end up with a table containing information about the visitor and it has a unique id which can be used as Primary key.
	
	I tried to create a third table and I would call it as all_sessions_orders to capture all the order transactions however this try was 
	not a successful due to too much missing information. I tried to find a common ground and fill the missing information but it was 
	not successful.

	To connect this table with others I needed a primary key for the table which there isn't one. I decided to create a column called ID with incrmented value.
	Doing so helped to make foreign keys that can be used to connect with other tables.

	For the all_sessions1 table (I just created) I started to fill some of the null values when possible. I made a aggregated search using
	productcategory:
		Query:		SELECT "v2ProductCategory", count(*)
				from all_sessions
				group by "v2ProductCategory"
				ORDER by "v2ProductCategory"
	
	So I started investigating under these two categories first.
								"${escCatTitle}"	19
								"(not set)"		757
	 I didn't want to convert them all into null instead I did it by searching by the name as follow:
		Query:		SELECT *
				from all_sessions1
				WHERE "v2ProductCategory"= '(not set)'
				ORDER by "productSKU"

	So I can see the productSKU and see if there are multiples with similar productSKU and category (not set)
	Then I refine my search by searching for product with same productsku from this category to see if there are 
	other product with same productsku but different category:

		Query:		SELECT *
				from all_sessions1
				WHERE "productSKU"= 'GGOEGAAX0104' 

	I was able to set some of them into their original category and fill some of the null values along the way in currencycode and productVariant columns.
__________________________________________________________________________________________________________________________________________________________________________________________


2) working on products table:
	This table didn't required much cleaning as it was easy to determine the primary key for it. The table has a unique column and it 
	is used as primary key. Using the following code:
		
		Query:		select "productSKU", count (*)
				from products
				group by "productSKU"
				HAVING count(*)>1
	
	v2ProductCategory and productPrice that are in all_sessions need to be moved to this table so we can have all the product infomration in one table and can help 
	reduce the numbers of columns in all_sessions table
__________________________________________________________________________________________________________________________________________________________________________________________

3) Working on sales_reports table:
	productSKU is unique id and is used as pk, using the pk was connected with all_sessions

		Query:		select "productSKU", count (*)
				from Sales_report
				group by "productSKU"
				HAVING count(*)>1
___________________________________________________________________________________________________________________________________________________________________________________________

4) Working with sales_SKU table:
	No duplicate with productsSKU and it is used as a pk and connected with all_sessions table
	
		Query:		select "productSKU", count (*)
				from Sales_BY_SKU
				group by "productSKU"
				HAVING count(*)>1
____________________________________________________________________________________________________________________________________________________________________________________________

5) Working with analytics table:
	a) Convert the date column from integer to date format using:

		Query:		ALTER TABLE analytics
				alter COLUMN date type date using to_date(date,'YYYYMMDD');

	b) Divided the unit price and the revenue columns by 1,000,000:

		Query:		update analytics
				set (column name) =  (column name)/1000000  

	c) Convert visitStartTime to time stampe:

		Query:		ALTER TABLE analytics
				alter COLUMN "visitStartTime" TYPE timestamp using to_timestamp("visitStartTime")

	This table didn't have primary key that can be used to connect with other tables, so I created a new table called new_analytics
	to help connect analytics to all_sessions.
	The new table new_analytics contains the following columns:
				vistNum
				visitid
				visitstarttime
				date

	The code I used to create the table:
		
		Query:		create table new_analytics (visitNum varchar, 
					    		visitid integer,
					     		visitstartTime date,
					      		date date);
			
	To copy the data from teh oridginal table I used the following:
		
		Query:		INSERT INTO new_analytics
				SELECT visitNumber, 
					visitId, 
					visitStartTime,
					date
				from analytics;

	I create an extra column called ID which has incrmented value to help me delet all the duplicate visitid in this table (new_analytics).
	I used the following to remove the duplicate:
			
		Query:		DELETE from new_analytics na1
				using new_analytics na2
				where na1.id < na2.id
				and na1.visitid = na2.visitid;

	after this process, i deleted the ID column and assigned primary key to visitid in new_analytics table which help to connect this table to all_sessions.
	As well, I created a new column called ID with incrmented value to the analytics table and assigned it as a primary key. Having a primary key to the analytics 
	table was the only way for me to be able to create a forieng which is used to connect to new_analytic table.



