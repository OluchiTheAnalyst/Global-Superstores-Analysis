# Global-Superstores-Analysis

## Table of content
- [Project Overview](#project-overview)
- [Data source](#data-source)
- [Tools](#tools)
- [Data cleaning](#data-cleaning)
- [Exploratory data analysis](#exploratory-data-analysis)
- [Data analysis](#data-analysis)
- [Data Modelling](#data-modelling)
- [Data visualizations](#data-visualizations)
- [Results](#results)
- [Recommendations](#recommendations)

## Project Overview
This data analysis project is aimed to analyze the performance and profitability of Global stores during the past years. I seek to identify trends pertaining the profitability and performance of this store and draw out meaningful insight that will aid the management make informed decisions and gain deeper understanding in the companies performance.

## Data Source
Sales data: this data was provided by vephla university and it is "global_superstores.CSV" FILE containing detailed information about each sales made by global_superstores.

## Tools
1. Microsoft Excel : this tool was used for

 - Data preparation
 - Data cleaning.

 3. SQL (POSTGRESQL): this tool was used for
    
 - The entire data analysis (Exploratory Data Analysis)
 - Writing of queries.

 5. Microsoft POWER BI: this tool was used for
    
 - Data visualization
 - Data modelling
 - Creating of report.

## Data Cleaning
I performed the following task in the data preparation phase;

 - Data inspection
 - Checking and handling duplicate values
 - Data cleaning and formatting
 - Removing special characters
 - Converting the data to a Standard excel table

## Exploratory Data Analysis
EDA involved exploring the sales data to answer key questions, such as;

- What are the three countries that generated the highest total profit for Global
Superstore in 2014?
- For each of these three countries, find the three products with the highest total profit.
Specifically, what are the products’ names and the total profit for each product

- Identify the 3 subcategories with the highest average shipping cost in the United States.

- Assess Nigeria’s profitability (i.e., total profit) for 2014. How does it compare to other
African countries?
- What factors might be responsible for Nigeria’s poor performance? You might want to
investigate shipping costs and the average discount as potential root causes.

- Identify the product subcategory that is the least profitable in Southeast Asia.
Note: For this question, assume that Southeast Asia comprises Cambodia,
Indonesia, Malaysia, Myanmar (Burma), the Philippines, Singapore, Thailand, and
Vietnam.
- Is there a specific country in Southeast Asia where Global Superstore should stop
offering the subcategory identified in 4a?

- Which city is the least profitable (in terms of average profit) in the United States? For
this analysis, discard the cities with less than 10 Orders.
- Why is this city’s average profit so low?

- Which product subcategory has the highest average profit in Australia?

- Which customer returned items and what segment do they belong
- Who are the most valuable customers and what do they purchase?

## Data Analysis

```SQL
CREATE TABLE Global_superstore_file (row_id Int,
									order_id text,
									order_date text,
									ship_date text,
									ship_mode varchar,
									customer_id text,
									customer_name varchar,
									segment varchar,
									city varchar,
									state varchar,
									country varchar,
									postal_code int,
									market varchar,
									region varchar,
									product_id text,
									return varchar,
									category varchar,
									sub_category varchar,
									product_name varchar,
									sales float,
									quantity int,
									discount float,
									profit float,
									shipping_cost float,
									order_priority varchar);

select * from Global_superstore_file


            --Question 1.
/* What are the three countries that generated the highest total profit for Global
Superstore in 2014?*/

--first thing to do is to group the countries according to their total profits, then sort it from highest to lowest, and then find the top 3
select * from global_superstore_file


SELECT sum(profit) AS profit, max(order_date) AS order_date, country
fROM
   global_superstore_file
WHERE
   order_date LIKE '%14'
GROUP BY 
   country
ORDER BY 
   profit DESC
limit 3;

                --ANSWER
/* the 3 countries that generated the highest total profit for global superstore in 2014 are 
--united states with 93507.513
--china with 48807.675
--india with 46793.993 */

        --QUESTION B
/*For each of these three countries, find the three products with the highest total profit.
Specifically, what are the products’ names and the total profit for each product? */

-- For each of these countries, I am going to find the top 3 products with the highest profit

--For United states
SELECT sum(profit) AS profit, product_name, country
From 
   global_superstore_file
WHERE
   country = 'United States'
GROUP BY 
   product_name, country
ORDER BY 
   profit DESC
limit 3;

       --ANSWER
/*The 3 products with the highest total profits in United states are
-- Canon image Class copier with total profit of 25199.928
-- fellowes PB500 electric punch plastic somb binding machine with total profit of 7753.039
-- hewlett packard Lasar jet 3310 copier with total profit 6983.885 */

-- For China
SELECT sum(profit) AS profit, product_name, country
From 
   global_superstore_file
WHERE
   country = 'China'
GROUP BY 
   product_name, country
ORDER BY 
   profit DESC
limit 3;

/* the 3 products with the highest total profit in china are
--sharp wireless fax digital with total profit of 2894.100
--HP copy machine color with total profit of 2855.13
--Samsung smart phone VolP with total profit of 2672.099 */

-- For India
SELECT sum(profit) AS profit, product_name, country
From 
   global_superstore_file
WHERE
   country = 'India'
GROUP BY 
   product_name, country
ORDER BY 
   profit DESC
limit 3;

/* The 3 products with the highest total profit in india are
--Sauder Classic Bookcase traditional with total profit of 2903.58
--Apple smartphone with callerId with total profit of 2817.99
--Cisco smart phone with callerID with total profit of 1609.38 */

                 --Question 2.
--Identify the 3 subcategories with the highest average shipping cost in the United States

Select * from Global_superstore_file

Select avg(shipping_cost) AS average_shipping_cost, sub_category, country
from
  Global_superstore_file
WHERE
  country = 'United States'
GROUP BY
  Sub_category, country
ORDER BY 
  average_shipping_cost DESC
LIMIT 3;

               --ANSWER
/* The 3 Subcategories with the highest average shipping cost for the united states are	
--Copiers
--Machines
--Tables */



              --Question 3.
/* a) Assess Nigeria’s profitability (i.e., total profit) for 2014. How does it compare to other
African countries? */

-- TO access nigeria's profitability, I am going to query out the total profit for the coumtry nigria
-- In 2014 from the highest to the lowest


Select * from global_superstore_file

SELECT SUM(profit) AS total_profit, order_date, country 
FROM
   global_superstore_file
WHERE
   country = 'Nigeria' and order_date LIKE '%14'
GROUP BY 
   country, order_date
ORDER BY
  total_profit DESC;
  
-- for other african countries 
--for SENEGAL

SELECT SUM(profit) AS total_profit, order_date, country 
FROM
   global_superstore_file
WHERE
   country = 'Senegal' and order_date LIKE '%14'
GROUP BY 
   country, order_date
ORDER BY
  total_profit DESC;
  
--For Mozambique

SELECT SUM(profit) AS total_profit, order_date, country 
FROM
   global_superstore_file
WHERE
   country = 'Mozambique' and order_date LIKE '%14'
GROUP BY 
   country, order_date
ORDER BY
  total_profit DESC;
  
-- For CONGO DR

SELECT SUM(profit) AS total_profit, order_date, country 
FROM
   global_superstore_file
WHERE
   country = 'Democratic Republic of the Congo' and order_date LIKE '%14'
GROUP BY 
   country, order_date
ORDER BY
  total_profit DESC;
  
-- For Morocco

SELECT SUM(profit) AS total_profit, order_date, country 
FROM
   global_superstore_file
WHERE
   country = 'Morocco' and order_date LIKE '%14'
GROUP BY 
   country, order_date
ORDER BY
  total_profit DESC;
  
-- For Somalia
SELECT SUM(profit) AS total_profit, order_date, country 
FROM
   global_superstore_file
WHERE
   country = 'Somalia' and order_date LIKE '%14'
GROUP BY 
   country, order_date
ORDER BY
  total_profit DESC;
  

/*                       QUESTION 3b) 
What factors might be responsible for Nigeria’s poor performance? You might want to
investigate shipping costs and the average discount as potential root causes */

SELECT AVG(discount) AS average_discount, shipping_cost, country
FROM
  global_superstore_file
WHERE 
  country = 'Nigeria'
GROUP BY 
   shipping_cost, country
ORDER BY
   shipping_cost DESC;


/*                                Question 4.
a) Identify the product subcategory that is the least profitable in Southeast Asia.
Note: For this question, assume that Southeast Asia comprises Cambodia,
Indonesia, Malaysia, Myanmar (Burma), the Philippines, Singapore, Thailand, and
Vietnam.*/

Select * from global_superstore_file

--I am going to be looking for the least profitable product sub_categories in each of this countries mentioned above

-- For Cambodia
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Cambodia'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;
 
-- The least profitable products sub_category in Cambodia is MACHINE with $0 profit

--For Indonesia
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Indonesia'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;

-- The least profitable products sub_category in Indonesia is TABLES with $-1214.6 loss

--For Malaysia
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Malaysia'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;

-- The least profitable sub_category in Malaysia is COPIERS with $0 profit

--For Myanmar(Burma)
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Myanmar (Burma)'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;
	
--The least profitable sub_category in Myanmar(Burma) is TABLES with $-592.146 loss

--For the philippines
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Philippines'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;
	
-- the least Profitable Sub_category in philippines is TABLES with $-976.5 loss

--For singapore
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Singapore'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;
	
--The least profitable sub_category in singapore is PAPER with Total profit of $0

-- for thailand
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Thailand'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;
	
--the least profitable sub_category in thailand is TABLES with loss of $-1294.35

--For Vietnam
SELECT sum(profit) AS total_profit, product_name, sub_category, country
FROM
    global_superstore_file
WHERE
    country = 'Vietnam'
GROUP BY 
    product_name, sub_category, country
ORDER BY
    total_profit ASC;

--the least profitable sub_category in Vietnam is ACCESSORIES with loss of $-692.93


      --QUESTION b) 
/*Is there a specific country in Southeast Asia where Global Superstore should stop
offering the subcategory identified in 4a? */

--YES, some countries in 4a are making NO profit, while some are making losses,
--so i suggest Global_superstores should stop offering the sub_category to countries that are making LOSSES, then give sometime to the countries 
--that are making NO PROFIT yet , they might Bounce back.
-- the following countries below are making huge losses
-- INDONIESIA, MYANMAR(BURMA), PHILIPPINES, THAILAND, VIETNAM


               --Question 5.
--a) Which city is the least profitable (in terms of average profit) in the United States? For
--this analysis, discard the cities with less than 10 Orders.

 select * from global_superstore_file 
 
SELECT AVG(profit) AS average_profit, country, city, quantity
FROM
   Global_superstore_file
WHERE
   country = 'United States' AND quantity >= 10
GROUP BY
   country, city, quantity
ORDER BY
   average_profit ASC;

       --ANSWER
-- from my deductions CONCORD is the least profitable city in the United states


--b) Why is this city’s average profit so low?
  --ANSWER
--It is because the order_priority is critical

              --Question 6.
--a) Which product subcategory has the highest average profit in Australia

SELECT AVG(profit) AS average_profit, country, sub_category   
FROM
   Global_superstore_file
WHERE
   country = 'Australia'
GROUP BY
   country, sub_category
ORDER BY
   average_profit DESC;

          --ANSWER
-- the product subcategory with the highest average profit is APPLIANCES

/*               Question 7.
a) Which customer returned items and what segment do they belong */

select * from global_superstore_file

SELECT return, segment, customer_name
FROM
  global_superstore_file;

/*          QUESTION b) 
Who are the most valuable customers and what do they purchase? */

Select sum(profit) as total_profit, customer_name, product_name
FROM
   global_superstore_file
GROUP BY 
   customer_name, product_name
ORDER BY
   total_profit DESC
LIMIT 5;

     --ANSWER
/* i picked this according to how much profits each customers made, 
our  TOP 5 most valuable customers are

--TAMARA CHAND
--RAYMOND BUCH
--HUNTER LOPEZ
--ADRIAN BARTON
--SANJIT CHAND */ 


```

## Data Modelling
[Data modelling]
![DATA MODELLING (CAPSTONE PROJECT)](https://github.com/OluchiTheAnalyst/Global-Superstores-Analysis/assets/152318870/d05e0df6-1795-40ba-92df-d36727d0ef84)


## Data Visualizations
[Dashboard]

![PowerBI PAGE 1 (CAPSTONE PROJECT)](https://github.com/OluchiTheAnalyst/Global-Superstores-Analysis/assets/152318870/a6c6ead3-0ad0-4a01-aa42-dec04ab98215)

![PowerBI PAGE 2 (CAPSTONE PROJECT)](https://github.com/OluchiTheAnalyst/Global-Superstores-Analysis/assets/152318870/539d54b7-c3fe-4cac-aae2-22f76040ca1c)
 
![Power BI PAGE 3 (CAPSTONE PROJECT)](https://github.com/OluchiTheAnalyst/Global-Superstores-Analysis/assets/152318870/ad5d7d85-d56f-459b-af89-8b516d7978cf)

## Results
The analysis results are summarized as follows;

              QUESTION 1 ANSWER
              
- the 3 countries that generated the highest total profit for global superstore in 2014 are
  
--united states with 93507.513

--china with 48807.675

--india with 46793.993 

                                         ANSWER B
- The 3 products with the highest total profits in United states are

-- Canon image Class copier with total profit of 25199.928

-- fellowes PB500 electric punch plastic somb binding machine with total profit of 7753.039

-- hewlett packard Lasar jet 3310 copier with total profit 6983.885 

- the 3 products with the highest total profit in china are
--sharp wireless fax digital with total profit of 2894.100
  
--HP copy machine color with total profit of 2855.13

--Samsung smart phone VolP with total profit of 2672.099

- The 3 products with the highest total profit in india are
--Sauder Classic Bookcase traditional with total profit of 2903.58
  
--Apple smartphone with callerId with total profit of 2817.99

--Cisco smart phone with callerID with total profit of 1609.38
  

                                         QUESTION 2 (ANSWER)
The 3 Subcategories with the highest average shipping cost for the united states are	
--Copiers
--Machines
--Tables


                                        QUESTION 3 (ANSWER)
Comparing the Total profits of Nigeria to other 5 african countries shown here, i can say that the total profit 
or the profitability of Nigeria to other african countries is drastically LOW. Because the highest profit Nigeria made in
2014 is negative which means that Nigeria didn't make any profit instead Nigeria made LOSSES.


                                       QUESTION 3b (ANSWER)
Judging from this investigation, Nigeria had a very poor performance because of the discount.
imagine that when shipping cost was lowest at about $0.02 a discount of 0.7% was given, now Imagine
when shipping cost is at its highest at about $234.85 and they still gave discount of 0.7% when other african countries 
did not give any discount at all, how won't nigeria fold? 


                                       QUESTION 4 (ANSWER)
- For Cambodia
  
--The least profitable products sub_category in Cambodia is MACHINE with $0 profit.

- For Indonesia
  
-- The least profitable products sub_category in Indonesia is TABLES with $-1214.6 loss

- For Malaysia
  
-- The least profitable sub_category in Malaysia is COPIERS with $0 profit

- For Myanmar(Burma)
  
--The least profitable sub_category in Myanmar(Burma) is TABLES with $-592.146 loss

- For the philippines
  
--the least Profitable Sub_category in philippines is TABLES with $-976.5 loss

- For singapore
  
--The least profitable sub_category in singapore is PAPER with Total profit of $0

- for thailand
  
--the least profitable sub_category in thailand is TABLES with loss of $-1294.35

- For Vietnam
  
--the least profitable sub_category in Vietnam is ACCESSORIES with loss of $-692.93


                                     QUESTION 4B
--YES, some countries in 4a are making NO profit, while some are making losses,
so i suggest Global_superstores should stop offering the sub_category to countries that are making LOSSES, then give sometime to the countries 
that are making NO PROFIT yet , they might Bounce back.

-- the following countries below are making huge losses

-- INDONIESIA, MYANMAR(BURMA), PHILIPPINES, THAILAND, VIETNAM


                                   --Question 6.
                                   
-- the product subcategory with the highest average profit is APPLIANCES



               Question 7.
- TAMARA CHAND
  
- RAYMOND BUCH
  
- HUNTER LOPEZ
  
- ADRIAN BARTON
  
- SANJIT CHAND 

## Recommendations
Based on the analysis and results, I recommend the following actions;

- For those countries with little or no profits, can global stores do something like a 'black friday' on those products where the prices of
those products are slashed by a certain percentage, so that customers can patronize that product and you sell off.

- Invest in marketing and promotions like advertisements, especially in those countries with poor profitability.

- Nigeria should invest more in their country and stop importing every single thing they use, because from the analysis, shipping_cost was a major factor 
that contributed to nigeria's poor performance.

- Focus on expanding and promoting your product in the southeastern asian region through adverts e.t.c because that is the commercial hub of the world.
  



