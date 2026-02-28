# Restaurant-Consumer-Data-analysis
Analyzed restaurant and customer behavior using SQL to generate business insights.

## Project Overview

**Project Title**: Restaurant cunsumer analysis<br>
**Database**: `food`

This project demonstrates SQL skills used by data analysts to explore, clean, and analyze restaurant and consumer behavior data. The project involves designing a relational database, performing exploratory data analysis (EDA), and answering business questions using SQL queries.

## Objectives

* Create and structure a restaurant database
* Analyze customer demographics & dining behavior
* Evaluate restaurant performance & ratings
* Identify cuisine popularity trends
* Generate business insights using SQL


## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `food`.
- **Table Creation**: Tables including restaurants, restaurant_cuisine, consumers, consumer_preferences, and ratings were created to store restaurant details, cuisine types, customer demographics, preferences, and rating information for analysis.

- **restaurants(table):-**
Stores restaurant details.
```sql
CREATE DATABASE food;

create table restaurants(
restaurant_id int primary key,
name varchar(100),
city varchar(100),
state varchar(100),
country varchar(50),
zip_code varchar(50),
latitude decimal(10,5),
longitude decimal(10,5),
alcohol_service varchar(100),
smoking_Allowed varchar(30),
price varchar(20),
franchise varchar(5),
seating_type varchar(50),
parking varchar(30)
);
```
- **restaurant_cuisine:-**
Maps restaurants to cuisines.
```sql
create table restaurant_cuisine(
restaurant_id int,
cuisine varchar(100),
foreign key (restaurant_id) references restaurants(restaurant_id)
);
```
- **consumers:-**
Stores customer demographics.
```sql
create table  consumers (
consumer_id varchar(20) primary key,
city varchar(50),
state varchar(100),
country varchar(100),
latitude DECIMAL(10,6),
longitude DECIMAL(10,6),
smoker VARCHAR(20),
drink_level VARCHAR(50),
transportation_method VARCHAR(50),
marital_status VARCHAR(50),
children VARCHAR(50),
age DECIMAL(10,4),
occupation VARCHAR(100),
budget VARCHAR(20)
);
```
- **consumer_preferences:-**
```sql
create table consumer_preferences(
consumer_id varchar(100),
preferred_cuisine varchar(50),
foreign key (consumer_id) references consumers(consumer_id) 
);
```
- **ratings:-** Stores customer feedback.
```sql
create table ratings(
consumer_id varchar(100),
restaurant_id int,
overall_rating int,
food_rating int,
serice_rating int,
foreign key (consumer_id) references consumers(consumer_id),
foreign key(restaurant_id) references restaurants(restaurant_id)
);
```

### 2. Data Exploration & Cleaning

### Total Records

```sql
SELECT COUNT(*) FROM consumers;
SELECT COUNT(*) FROM restaurants;
```
### Unique Cuisines

```sql
SELECT DISTINCT Cuisine FROM restaurant_cuisine;
```
### Null Value Check

```sql
SELECT *
FROM consumers
WHERE Age IS NULL OR Occupation IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Zero Analyst

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or wo
