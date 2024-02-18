# Customer-Behavior-Analysis---Cinema
This project and the data used was a part of business case in '[Data Got Talent 2023](https://datagottalent.vn/)' Competition. It focuses on examining patterns, trends, and factors influencing customer spending in order to gain insights into their preferences, purchasing habits when customers go to the cinema. Suggesting solutions to increase cinema revenue.
## Background
The film distribution business has provided a detailed dataset on operations, revenue, and customer attendance for each movie screening in May 2019. This dataset includes three tables providing information such as customer ID, screening date and time, movie title, number of tickets sold, etc. From this data, the business needs to understand issues and market demands to develop an appropriate operational plan, thereby enhancing overall efficiency.
- Raw dataset: The business has provided a dataset consisting of 3 tables
  ![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/0724aaa0-2249-4bff-9e78-e324bff9253c)
- Data Quality:
In the Film table, the majority of movie data is not present in the Ticket table, and vice versa for movies in the Ticket table. (The Film Table only includes movies produced in the United States - US). The films are not consistent in terms of rating and language (English - Film Table and Vietnamese - Ticket Table)
  ![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/b7a917ca-2385-4036-893b-526833c4afd2)
  In the Ticket table, orders placed through the website consistently return null values for orderid, total, saledate, and popcorn.
  ![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/85481fbe-c4a9-40a3-8f83-8deb073fd95e)
  In the Ticket table, there are 3 occurrences of customerID with unusually high total orders and ticket quantities within a one-month period.
  Explanation: Customers paying at the counter do not provide ID information, leading to difficulty in accurately identifying information and consumer behavior. The system has automatically assigned 3 IDs for these transactions; however, analyzing based on inaccurate data may result in skewed outcomes.
  
  ![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/ac26da20-934a-4246-8574-44782cd9ad2a)

## Data Cleansing
### Film Table 
- Updating movie information in the Film table by referencing external data (IMDb) to provide comprehensive movie details and enhance synchronization and linkage with the Ticket table.
- Updating age ratings in the Film table by referencing IMDb and the Ticket table to ensure compliance with Vietnamese Cinema regulations.
- Crawling data for movies screened in 2019 from IMDb as the source and utilizing The Movie Database API to retrieve additional supplementary information.
  ![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/92d977de-5f5a-4c88-a536-c52a672dda13)

### Ticket Table 
- Format the data type of the columns
- Adding columns film_id, film_en, and rating to display information and establish correct links with the movies in the Film table.
  ![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/95226bca-f09a-42ef-8fc0-e86117e65c36)

## Data Modeling 
![image](https://github.com/acnibh/Customer-Behavior-Analysis---Cinema/assets/146699917/3ef50f34-2208-407d-91a5-8d18e3fc4fd4)
## Data Analysis




