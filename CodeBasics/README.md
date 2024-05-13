# PROVIDE INSIGHTS TO MANAGEMENT IN CONSUMER GOODS

<img src="https://github.com/rajathratnakaran/SQL-projects/assets/92428713/e8cab7bc-96e1-485a-9805-fff3f5b289c3" width="600" height="400">

## INTRODUCTION

Atliq Hardwares (imaginary company) is one of the leading computer hardware producers in India and well expanded in other countries too.

However, the management noticed that they do not get enough insights to make quick and smart data-informed decisions. They want to expand their data analytics team by adding several junior data analysts. Tony Sharma, their data analytics director wanted to hire someone who is good at both tech and soft skills. Hence, he decided to conduct a SQL challenge which will help him understand both the skills.

## Task:  

Imagine yourself as the applicant for this role and perform the following task

1.    There are 10 ad hoc requests for which the business needs insights.
2.    You need to run a SQL query to answer these requests. 
3.    The target audience of this dashboard is top-level management - hence you need to create a presentation to show the insights.
4.    Be creative with your presentation.

## Adhoc request #1:
Provide the list of markets in which customer "Atliq Exclusive" operates its business in the APAC region.

SQL Query:
```
select distinct market from dim_customer
where customer = "Atliq Exclusive" and region ="APAC";
```

Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/57911fe4-bf92-47b0-a666-1f93f31d7285)

## Adhoc request #2:
What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields, unique_products_2020
unique_products_2021, percentage_chg

```
SELECT X.A as unique_products_2020, Y.B as unique_products_2021, ROUND(((B-A)/A)*100,2) AS percentage_chg from 
(SELECT distinct count(product_code) as A FROM fact_gross_price
where fiscal_year ="2020") as X,
(SELECT distinct count(product_code) as B FROM fact_gross_price
where fiscal_year ="2021") as Y;
```

Result:


![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/367d0612-8064-4923-ae23-364c2973cd2a)

## Adhoc request #3:
Provide a report with all the unique product counts for each segment and sort them in descending order of product counts. The final output contains 2 fields, segment and product_count

````
SELECT segment,count(product_code) as product_count FROM dim_product
group by segment
order by product_count desc;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/25767221-6035-4386-8149-83607e007170)

## Adhoc request #4:
Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields,
segment, product_count_2020, product_count_2021, difference

````
with X as (select d.segment AS Segment, count(distinct(d.product_code)) as A from dim_product d
			left join fact_sales_monthly f on d.product_code = f.product_code
			where f.fiscal_year = '2021'
			group by d.segment),
	Y as (select d.segment as Segment, count(distinct(d.product_code)) as B from dim_product d
			left join fact_sales_monthly f on d.product_code = f.product_code
			where f.fiscal_year = '2020'
			group by d.segment)
select X.Segment,X.A as product_count_2021,Y.B as product_count_2020, (X.A - Y.B) AS difference from X,Y
where X.Segment = Y.Segment;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/8753dcfb-ff7b-4df1-a17b-9c894217018e)

# Adhoc request #5:
Get the products that have the highest and lowest manufacturing costs. The final output should contain these fields, product_code
product, manufacturing_cost

````
Select * from 
	(Select product_code, max(manufacturing_cost) as manufacturing_cost from fact_manufacturing_cost
	group by product_code
	order by manufacturing_cost desc
	limit 1) as X
	union 
	(Select product_code, min(manufacturing_cost) as min from fact_manufacturing_cost
	group by product_code
	limit 1);
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/612ad228-9b59-4787-ab0c-e4a3a64a873c)

# Adhoc request #6:
Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct for the fiscal year 2021 
and in the Indian market. The final output contains these fields, customer_code, customer, average_discount_percentage

````
select f.customer_code, d.customer, f.pre_invoice_discount_pct as average_discount_percentage
	from dim_customer d left join fact_pre_invoice_deductions f
	on d.customer_code = f.customer_code
    where market ="India" and f.fiscal_year = "2021"
    order by average_discount_percentage desc
    Limit 5;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/09c72098-0a5c-4f31-b640-b2fd10606083)


# Adhoc request #10:
Get the Top 3 products in each division that have a high total_sold_quantity in the fiscal_year 2021? The final output contains these
fields, division, product_code, product, total_sold_quantity, rank_order

````
WITH CTE1 AS (select d.product_code, d.division, d.product, sum(f.sold_quantity) as total_sold_quantity
	from dim_product d left join fact_sales_monthly as f
	on d.product_code = f.product_code
	where f.fiscal_year = "2021"
	group by d.product_code, d.division, d.product),
    CTE2 AS (Select *, rank()over(partition by division order by total_sold_quantity desc)as rank_order
	from CTE1)
    SELECT * FROM CTE2
    where rank_order in (1,2,3);
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/6522a4e7-3ec2-4a82-8089-ae77e4dc337e)








