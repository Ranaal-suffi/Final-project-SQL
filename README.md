# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
Create clean and easy to analyze database
create relations between tables
Finding a way to fill the null values in the database without compromising the data

___________________________________________________________________
## Process
1- Cleaned the data which includes removing any inconsistencies in the data.
2- Find a primary key if possible and find a way around if there isn't any key
3- try to fill as many empty data with valid information

## Results
1) some of the information is not the same in all tables, for example fullvisitorId in the analytic table different than what is 
in the all_sessions table.
2) category names in the category section were not unified 
3) So much missing data
4) Some tables don't have a primary key
5) analytics table was hard to connected to any table. For that reason I created a new table called new_analytics.
6) cleaned the new_analytics table from any duplicates and that led to finding a primary key.
7) created a new column in the analytics table called ID which the value will be incrmented and used as a primary key
8) After initiating the primary key to analytics table, I was able to create a foreign key to connect this table to the new_analytics
   table.
9) I was able to connect the new_analytics table to all_sessions table and with that, all my tables are now connected.

## Challenges 
Learning how to download a data from a csv file was challenging but it helped me learning something new.
chosing the right data type for each column was very frustrating.

## Future Goals
I would like to scan each column especially in all_sessions table and make sure that I executed all the possibilities of filling
all the null values from existing data.
divide all_sessions into 2-3 tables for better data management
make use of the userid column.

