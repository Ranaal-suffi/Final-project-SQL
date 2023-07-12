Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
	
	1) For countries only: (the result was five countries only)
		select country, sum("totalTransactionRevenue") as total
		from all_sessions
		where country is not NULL
		group by country
		having sum("totalTransactionRevenue")> 0
		order by total DESC

	2) For cities only: (The result was 19 cities)
		select city, sum("totalTransactionRevenue") as total
		from all_sessions
		Where city is not NULL
		group by city
		having sum("totalTransactionRevenue")> 0
		order by total DESC

	3) For both countrys and cities: (It was 21)
		select country, city, sum("totalTransactionRevenue") as total
		from all_sessions
		WHERE country is not Null or city is not null
		group by country, city
		having sum("totalTransactionRevenue")> 0
		order by total DESC

Answer:
The answer for the first is United States, Israel, Australia, Canad, and Swizerland.
The seconde formala, it contains 19
The third one it contains 21 teh first five are all from united states

______________________________________________________________________________________________________
**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
	1)For countrys only:
		select  country, avg ("productQuantity")
		from all_sessions
		where country is not null 
		group by  country
		having avg("productQuantity") > 0
		order by avg("productQuantity") desc

	2)for cities only:

		select  city, avg ("productQuantity")
		from all_sessions
		where city is not null 
		group by  city
		having avg("productQuantity") > 0
		order by avg("productQuantity") desc

	3)For both country and city:
	
		select country,  city, avg ("productQuantity")
		from all_sessions
		where city is not null OR country IS NOT NULL
		group by  country, city
		having avg("productQuantity") > 0
		order by avg("productQuantity") desc


Answer:
  SQL Query 1) "Spain"		10.00
		"United States"	4.024
		"Colombia"	1.00
		"France"	1.00
		"Canada"	1.00
		"Argentina"	1.00
		"India"		1.00
		"Ireland"	1.00
		"Mexico"	1.00
		"Finland"	1.00

  SQL Query 2) "Madrid"		10.00
		"Salem"		8.00
		"Atlanta"	4.00
		"Houston"	2.00
		"New York"	1.17
		"Chicago"	1.00
		"San Jose"	1.00
		"Columbus"	1.00
		"Los Angeles"	1.00
		"Ann Arbor"	1.00
		"Mountain View"	1.00
		"Detroit"	1.00
		"Bengaluru"	1.00
		"Sunnyvale"	1.00
		"Seattle"	1.00
		"San Francisco"	1.00
		"Dallas"	1.00
		"Dublin"	1.00
		"Palo Alto"	1.00

  SQL Query 3) There were 26 but below top 5

		"Spain"	"Madrid"		10.00
		"United States"			9.85
		"United States"	"Salem"		8.00
		"United States"	"Atlanta"	4.00
		"United States"	"Houston"	2.00

__________________________________________________________________________________________________________________________________
**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
		select "v2ProductCategory",count(*), country, city
		from all_sessions
		where city is not null and "productQuantity" >0
		group by "v2ProductCategory", country, city
		order by count(*) DESC, "v2ProductCategory"
	

Answer:
There were 25 category the top two were:

The most product category 
	"Home/Nest/Nest-USA/" from United States
	"Home/Apparel/Men/Mens-T-Shirt/"  from United States

The pattern that can be noticed is that:
	1- United States is the most consumer from this website
	2- The most popular item is electronics/ cameras




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
		1) when did not exclude cities with null value:

			SELECT "productSKU",  sum("productQuantity"),"v2ProductName", country, city
			from all_sessions
			group by "productSKU", "v2ProductName", country, city
			having sum("productQuantity")>0
			order by sum("productQuantity") DESC

		2) when excluded cities with null value

			SELECT "productSKU",  sum("productQuantity"),"v2ProductName", country, city
			from all_sessions
			where city is not null 
			group by "productSKU", "v2ProductName", country, city
			having sum("productQuantity")>0
			order by sum("productQuantity") DESC



Answer:
 SQL Query 1)It is:
		1- leatherette Jorunal from united states- 65
 		2- reusable shopping bags from united states- 50 
		3- Waze Dress Socks from Spain Madrid-10

SQL Query 2)It is:
		1- Waze dress socks from Spain Madrid -10
		2- Red Spiral Google Notebook from United States Salem - 8
		3- Reusable shopping bags United States Atlanta - 4


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
		select country, city, sum(productrevenue)
		from all_sessions1
		group by country, city
		having sum(productrevenue)> 0
		order by sum(productrevenue)



Answer:

The only country that generated revenue for this web site was United States. However, these numbers are inaccurate, there are
number of product revenue rows empty even though product quantity >0

		SELECT "productQuantity", "productPrice", "productRevenue"
		from all_sessions
		where "productQuantity" > 0 and "productRevenue" is null
		**the result was 49




