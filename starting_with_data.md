Question 1: find all duplicate records

SQL Queries:
		select "visitId", count(*)as duplicate, country 
		from all_sessions
		group by "visitId", country
		having count(*) > 1
		

Answer: 
	552	

         ________________________________________________________________________________

Question 2: find the total number of unique visitors (`fullVisitorID`)

SQL Queries:

		select count(distinct "fullVisitorId")
		from all_sessions

Answer:
	14147

          __________________________________________________________________________________

Question 3: find the total number of unique visitors by referring sites

	I assumend referring site is channel grouping

SQL Queries:
		select count(distinct "visitId"), "channelGrouping"
		from all_sessions
		group by "channelGrouping"
		order by count(distinct "visitId") desc

Answer:
	The number of unique visitors by referring site is 2488

	8348	"Organic Search"
	2862	"Direct"
	2488	"Referral"
	488	"Paid Search"
	247	"Affiliates"
	121	"Display"
	5	"(Other)"

	  ___________________________________________________________________________________

Question 4: find each unique product viewed by each visitor

SQL Queries:
		select DISTINCT("v2ProductName"), "visitId"
		from all_sessions
		group by "visitId", "v2ProductName"
		order by "visitId"

Answer:
	15129

	  ____________________________________________________________________________________

Question 5: compute the percentage of visitors to the site that actually makes a purchase

SQL Queries:
I created two temperary table and I joined them together, that was the only solution I was successful with

Table#1)	create TEMP TABLE total_visitor AS
		select count(*) as total, 'new' as idcol FROM all_sessions

Table#2)	create TEMP TABLE total_purch AS
		select count(*) as totalpurchas, 'new' as idcol FROM all_sessions
		where "productQuantity">0

Join the two table together:
		select  sum((tp.totalpurchas* 100/ tv.total))
		from total_visitor tv join total_purch tp on tp.idcol=tv.idcol


Answer: 0.35%
