## sql-retail_sales-project1

```sql   
create table retail_sales
(
	transactions_id	int primary key, 
	sale_date date,
	sale_time time,	
	customer_id int,
	gender varchar(15), 
	age int,
	category varchar(15),
	quantiy	int,
	price_per_unit	float,
	cogs float,
	total_sale float
);
```

**Data Cleaning**
```sql
delete from retail_sales
where 
transactions_id is null
or
sale_date is null 
or 
sale_time is null
or 
customer_id is null
or 
gender is null
or
category is null
or
quantiy is null
or
cogs is null
or
total_sale is null;
```

**Data exploration**

**how many sales we have?**

```sql
select count(*) as total_sales from retail_sales;
```

**how many unique customers we have?**

```sql
select count(distinct(customer_id)) from retail_sales;
```

**how many unique category we have?**

```sql
select distinct(category) from retail_sales;
```

## Business key problems & answers**

**My Analysis & Findings**

**Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05**

```sql
select * from retail_sales
where sale_date = '2022-11-05';
```

**Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022**

```sql
select * from retail_sales
where category = 'Clothing' 
and to_char(sale_date,'yyyy-mm') = '2022-11' **--The TO_CHAR function converts DATETIME or DATE value to character str value**
and quantiy>=4;
```

**Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.**

```sql
select category, sum(total_sale) as total_sales_value,
count(*) as total_orders
from retail_sales
group by category;
```

**Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**

```sql
select category,round(avg(age),0) as age_avg 
from retail_sales
where category = 'Beauty'
group by 1;
```

**Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.**

```sql
select *
from retail_sales
where total_sale > 1000;
```

**Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**

```sql
select gender,category,
count(transactions_id) as total_transaction_num
from retail_sales
group by gender,category;
```

**Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year?**

```sql
select * from
(
	select 
		extract(year from sale_date) as year,
		extract(month from sale_date) as month,
		avg(total_sale) as avg_sale,
		rank()over(partition by extract(year from sale_date) order by avg(total_sale) desc) as rank
	from retail_sales
	group by year,month
) as t1
where rank = 1;
```

**Q.8 Write a SQL query to find the top 5 customers based on the highest total sales?**

```sql
select customer_id, sum(total_sale) as total_sales
from retail_sales
group by 1
order by 2 desc
limit 5;
```

**Q.9 Write a SQL query to find the number of unique customers who purchased items from each category?**

```sql
select category,
		count(distinct(customer_id)) as unique_customer
from retail_sales
group by category;
```

**Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)?**

```sql
with hourly_sales
as
(
select *,
		case
			when extract (hour from sale_time)<12 then 'morning'
			when extract (hour from sale_time) between 12 and 17 then 'afternoon'
			else 'evening'
            end as shift
from retail_sales
)
select shift,
		count(*) as total_orders
from hourly_sales
group by shift;
```
**End**

## Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## Stay Updated and Join the Community
LinkedIn: www.linkedin.com/in/tusarkant-jena-6762b8208

Email-id: jtusarkant@gmail.com

**Thank you for your support, and I look forward to connecting with you!**
