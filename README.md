# Customer-Behavior-Analysis---Cinema
This project and the data used was a part of business case in '[Data Got Talent 2023](https://datagottalent.vn/)' Competition. It focuses on examining patterns, trends, and factors influencing customer spending in order to gain insights into their preferences, purchasing habits when customers go to the cinema. Suggesting solutions to increase cinema revenue.
## Background
The film distribution business has provided a detailed dataset on operations, revenue, and customer attendance for each movie screening in May 2019. This dataset includes three tables providing information such as customer ID, screening date and time, movie title, number of tickets sold, etc. From this data, the business needs to understand issues and market demands to develop an appropriate operational plan, thereby enhancing overall efficiency.
- Raw dataset: The business has provided a dataset consisting of 3 tables
- Data Quality:
In the Film table, the majority of movie data is not present in the Ticket table, and vice versa for movies in the Ticket table. (The Film Table only includes movies produced in the United States - US). The films are not consistent in terms of rating and language (English - Film Table and Vietnamese - Ticket Table)
  In the Ticket table, orders placed through the website consistently return null values for orderid, total, saledate, and popcorn.
  In the Ticket table, there are 3 occurrences of customerID with unusually high total orders and ticket quantities within a one-month period.
  Explanation: Customers paying at the counter do not provide ID information, leading to difficulty in accurately identifying information and consumer behavior. The system has automatically assigned 3 IDs for these transactions; however, analyzing based on inaccurate data may result in skewed outcomes.
  


## Data Cleansing
### Film Table 
- Updating movie information in the Film table by referencing external data (IMDb) to provide comprehensive movie details and enhance synchronization and linkage with the Ticket table.
- Updating age ratings in the Film table by referencing IMDb and the Ticket table to ensure compliance with Vietnamese Cinema regulations.
- Crawling data for movies screened in 2019 from IMDb as the source and utilizing The Movie Database API to retrieve additional supplementary information.
 

### Ticket Table 
- Format the data type of the columns
- Adding columns film_id, film_en, and rating to display information and establish correct links with the movies in the Film table.
 

## Data Modeling 
![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/3ef50f34-2208-407d-91a5-8d18e3fc4fd4)
## Data Analysis
### 1. Customers
![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/e11e3026-c02b-40b8-904f-906b0d3acc62)
### 2. Domestic and Foreign Films
![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/b7b6619d-70a7-4902-8a82-061f79eef1b5)
![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/363193a3-8b7f-47bc-8c7e-fe7adba26b22)

```SQL
WITH joined_table AS (
    SELECT
        customer_id,
        DATEPART(day, date) daygaps,
        order_id,
        Eng_film, 
        country
    FROM Ticket2 AS t1
    JOIN film AS t2 ON t1.film_id = t2.film_id
)
,table2 AS (
    SELECT
        customer_id,
        COUNT(DISTINCT daygaps) AS ReturnFrequency,
        COUNT(DISTINCT order_id) AS PurchaseTransactions,
        COUNT(DISTINCT eng_film) AS total_films
    FROM joined_table
    GROUP BY customer_id
    HAVING COUNT(DISTINCT daygaps) > 1
	)
, table3 AS (
	SELECT table2.customer_id, 
	ReturnFrequency,
	PurchaseTransactions,
	COUNT (distinct order_id) AS ForeignFilmPurchaseTransactions,
	PurchaseTransactions - COUNT (distinct order_id) AS DomesticFilmPurchaseTransactions
	FROM joined_table
	RIGHT JOIN table2
	ON joined_table.customer_id = table2.customer_id
	WHERE country <> 'vietnam'
GROUP BY table2.customer_id, table2.ReturnFrequency, table2.PurchaseTransactions
) 
SELECT DISTINCT ReturnFrequency,
COUNT ( Customer_id) OVER ( Partition BY ReturnFrequency) AS CustomerQuantity,
SUM ( PurchaseTransactions) OVER ( Partition BY ReturnFrequency) total_orders,
SUM ( ForeignFilmPurchaseTransactions) OVER ( Partition BY ReturnFrequency) ForeignFilmPurchaseTransactions,
SUM ( DomesticFilmPurchaseTransactions) OVER ( Partition BY ReturnFrequency) DomesticFilmPurchaseTransactions,
FORMAT (SUM ( ForeignFilmPurchaseTransactions) OVER ( Partition BY ReturnFrequency)*1.0/SUM ( PurchaseTransactions) OVER ( Partition BY ReturnFrequency),'p' ) AS PercentofForeignFilmPurchases,
FORMAT (SUM ( DomesticFilmPurchaseTransactions) OVER ( Partition BY ReturnFrequency)*1.0/SUM ( PurchaseTransactions) OVER ( Partition BY ReturnFrequency),'p' ) AS PercentofFilmPurchases
FROM table3
```
### 3. Cinema Hall
![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/90aa0504-afd7-469a-b36f-4424b2336de5)





