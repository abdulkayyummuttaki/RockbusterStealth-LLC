#  Project Brief
# Rockbuster Stealth Data Analysis Project


Rockbuster Stealth LLC is a movie rental company that used to have stores around the
world. Facing stiff competition from streaming services such as Netflix and Amazon Prime,
the Rockbuster Stealth management team is planning to use its existing movie licenses to
launch an online video rental service in order to stay competitive.


Data Analyst has been hired by Rockbuster Stealth’s business intelligence (BI)
department to help with the launch strategy for the new online video service. The BI
department helps other departments, from inventory to customer insights, with data-related
queries. Data Analyst first task is to load all of Rockbuster’s data into a relational database
management system (RDBMS). Then, you’ll use SQL to analyze the data and answer any
ad-hoc business questions that other departments may have.

Before Data Analyst can begin analysis and answer more complex business questions, he
needs to acquire a good understanding of the various data points. And eventually, compile
the results of your research into an easily digestible format, which will be presented to the
Rockbuster Stealth Management Board.



## Key Questions and Objectives

The Rockbuster Stealth Management Board has asked a series of business questions, and
they expect data-driven answers that they can use for their 2020 company strategy. Here are
the main questions they’d like to answer:

● Which movies contributed the most/least to revenue gain?
● What was the average rental duration for all videos?
● Which countries are Rockbuster customers based in?
● Where are customers with a high lifetime value based?
● Do sales figures vary between geographic regions?


## Context

To answer the questions posed by the different departments, Data analysts query the data using SQL
(short for structured query language). Throughout this Achievement, Data analysts learn SQL
incrementally and will use it to answer increasingly complex business questions for each
task. The analysis results will be presented to Rockbuster management, so it requires
to visualize the data in an easy-to-consume manner. 

This project aims to develop skills that could be applied to any industry and highlight 
the ability to solve a variety of clients. The aim is to give hands-on experience with SQL.


## Data Set
In this Achievement, you'll be using a data set that contains information about Rockbuster's
film inventory, customers, and payments, among other things. The first thing you'll need to
do is load the data set into the PostgreSQL database. Keep in mind the following points
regarding the data set:
● It's around 3MB and contains several files.
● A relationship exists between two tables if a column name is present in both tables.


## Final Analysis Criteria for the Project
The final project will be evaluated on your ability to:

● Write moderately complex SQL queries to answer business questions.
● Present your SQL results to business managers by creating visualizations and telling
a compelling story.
● Present SQL results to technical colleagues using Excel and creating a
data dictionary.
● Create a professional project that can add to the portfolio and show to
employers.



## Tools/Skills/Procedure

• Relational database: PostgreSQL
• SQL
• Database querying using ERD
• Filtering
• Cleaning and summarising the data
• Joining tables
• Subqueries
• Common table expression(CTE)



## Uploading data dump to the PostgreSQL(version 2023) Database
• Install PostgreSQL and configure the settings.
• Create a database with the name Rockbuster.
• Once the "Rockbuster" database has been created, it will appear in the list of databases 
on the left-hand side of the screen. Right-click "Rockbuster" and select Restore:
• A Restore dialogue will appear. Select the three small dots to the right of the Filename
field to get the "Rockbuster.tar" file from "02 Data/Original Data".
• Before you can find your "Rockbuster.tar" file, however, you'll need to click the Format 
button in the bottom-right corner of the Select file dialogue and select All Files from the 
dropdown menu to make all the files visible:
• Select the “Rockbuster.tar” file, press OK, then click Restore.
• Once you’ve found the “Rockbuster.tar” file and restored everything correctly, 
you’ll see a window in the bottom-right corner of the screen confirming that the 
data dump has loaded successfully.

## Approach to the problem

#### 1. Understanding the data

• Extracting entity relationship diagram and creating a first draft of a data dictionary.
• Answer some basic business questions using SQL.
• Creating EDA using SQL ordering, limiting, and grouping data
• Extracting entity relationship diagram and creating a first draft of a data dictionary.
• Answer some basic business questions using SQL.
• Creating EDA using SQL ordering, limiting, and grouping data

#### 2. Data Cleaning deriving more columns
   
   • Identifying and namely duplicate, non-uniform, incorrect, and missing data and cleaning the data using a view
   • Preparing data for analysis by profiling and cleaning the data.
   • Deriving more additional columns using SQL group clause.

#### 3.Preparing for the business queries
• Creating a flat file for analysis using join queries.
• Writing subqueries to answer complex business questions
• Using CTE to create complex queries. 
• Evaluate the query performance and understand the DPA model.

#### 4. Storytelling
• Using flat data sheets from analytics queries upload to the tableau.
• Preparing visualisations using Tableau.
• Creating presentations of findings using Excel.


## Queries


### Actor			
Total Number of Actors	SELECT COUNT(1) FROM ACTOR	
200	

### Countries			
Total number of countries where Rockbuster customers are available	


`SELECT COUNT(name) FROM category \
WITH aggregate_customer_amount_cte (customer_id, first_name, last_name, city, country, amount ) 
AS
( 
  SELECT customer.customer_id, 
	 first_name, 
	 last_name, 
	 City,
         country, 
         SUM(amount) as amount

  FROM Customer	
  INNER JOIN payment USING (customer_id)
  INNER JOIN address USING (address_id )
  INNER JOIN city USING (city_id)
  INNER JOIN country USING (country_id)
  GROUP BY customer.customer_id,
           first_name,
	   last_name, 
           city,
           country   
)
SELECT  
COUNT( DISTINCT country ) 
FROM aggregate_customer_amount_cte;`
	
`RESULT:
108`	


Total number of active customer countries	

"WITH aggregate_customer_amount_cte (customer_id, first_name, last_name, city, country, amount )
AS
( 
	SELECT customer.customer_id, 
 
	       first_name,
	
	       last_name, 
	
	       city, 
	
	       country, 
	
	       SUM(amount) as amount,
	
	       activebool as status
	
	FROM Customer	
 
	INNER JOIN payment USING (customer_id)
 
	INNER JOIN address USING (address_id )
 
	INNER JOIN city USING (city_id)
 
	INNER JOIN country USING (country_id)
 
	GROUP BY customer.customer_id,
                 first_name,
		 last_name, 
   
	          city,
	          country
	   
)


SELECT  COUNT( DISTINCT country ) FROM aggregate_customer_amount_cte WHERE status IS true"	

RESULT: 108	


Total number of non active customer countries	"WITH aggregate_customer_amount_cte (customer_id, first_name, last_name, city, country, amount ) AS
( 
	SELECT customer.customer_id, 
		   first_name, 
		   last_name, 
	       city, 
	       country, 
	       SUM(amount) as amount,
		   activebool as status
	FROM Customer										   
	INNER JOIN payment USING (customer_id)
	INNER JOIN address USING (address_id )
	INNER JOIN city USING (city_id)
	INNER JOIN country USING (country_id)
	GROUP BY customer.customer_id,first_name,last_name, 
	       city,country
)

SELECT  COUNT( DISTINCT country ) FROM aggregate_customer_amount_cte
WHERE status IS false"	0	
Total Number of cities where Rockbuster customer presence exist	"WITH aggregate_customer_amount_cte (customer_id, first_name, last_name, city, country, amount ) AS
( 
	SELECT customer.customer_id, 
		   first_name, 
		   last_name, 
	       city, 
	       country, 
	       SUM(amount) as amount,
		   activebool as status
	FROM Customer										   
	INNER JOIN payment USING (customer_id)
	INNER JOIN address USING (address_id )
	INNER JOIN city USING (city_id)
	INNER JOIN country USING (country_id)
	GROUP BY customer.customer_id,first_name,last_name, 
	       city,country
)

SELECT  COUNT( DISTINCT city ) FROM aggregate_customer_amount_cte"	

RESULT:
597	


### Customer			
Total number of customers	

"WITH aggregate_customer_amount_cte (customer_id, first_name, last_name, city, country, amount ) AS
( 
	SELECT customer.customer_id, 
		   first_name, 
		   last_name, 
	       city, 
	       country, 
	       SUM(amount) as amount,
		   activebool as status
	FROM Customer										   
	INNER JOIN payment USING (customer_id)
	INNER JOIN address USING (address_id )
	INNER JOIN city USING (city_id)
	INNER JOIN country USING (country_id)
	GROUP BY customer.customer_id,first_name,last_name, 
	       city,country
)

SELECT  COUNT( customer_id ) FROM aggregate_customer_amount_cte

RESULT:
599	

### Films			
Total number of films	

SELECT count(film_id) FROM film	

RESULT:1000	
Average rental duration	SELECT Round(AVG(rental_duration),2) FROM film	
RESULT: 4.99	
Max rental duration	SELECT Round(MAX(rental_duration),2) FROM film	
RESULT: 7	
Min rental duration	SELECT Round(MIN(rental_duration),2) FROM film	
RESULT: 3	
MIN rental rate	SELECT Round(MIN(rental_rate),2) FROM film	
RESULT: 0.99	
MAX rental rate	SELECT Round(MAX(rental_rate),2) FROM film	
RESULT: 4.99	
Average  rental rate	SELECT Round(AVG(rental_rate),2) FROM film	
RESULT: 2.98	
Minimum replacement cost	SELECT Round(MIN(replacement_cost),2) FROM film	
RESULT: 9.99	
Maximum replacement cost	SELECT Round(MAX(replacement_cost),2) FROM film	
RESULT: 29.99	
Average replacement cost	SELECT Round(AVG(replacement_cost),2) FROM film	
RESULT: 19.98	
			
SELECT rating, COUNT(rating) 
FROM film GROUP BY rating 
ORDER BY CASE 
		WHEN rating = 'G' THEN 'A' 
		WHEN rating = 'PG' THEN 'B' 
		WHEN rating = 'PG-13' THEN 'C' 
		WHEN rating = 'R' THEN 'D' 
		ELSE 'E' 
	END"	

 RESULT:
 rating	 count
 
		G	178
  
		PG	194
  
		PG-13	223

		R	195
  
		NC-17	210
  
			
Total number of film genre	
"SELECT Count(distinct name) 
FROM film_category
INNER JOIN category USING (category_id)"

RESULT: 17	

Total number of Gernre	
SELECT COUNT(name) FROM category

RESULT: 	20	


