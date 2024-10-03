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
SELECT distinct(MARKET) FROM dim_customer
WHERE REGION = "APAC"
AND CUSTOMER = "Atliq Exclusive";
```

Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/57911fe4-bf92-47b0-a666-1f93f31d7285)

## Adhoc request #2:
What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields, unique_products_2020
unique_products_2021, percentage_chg

```
WITH CTE1 AS (SELECT COUNT(DISTINCT(PRODUCT_CODE)) AS PC_2020 FROM fact_sales_monthly
			  WHERE FISCAL_YEAR = '2020'),
	 CTE2 AS (SELECT COUNT(DISTINCT(PRODUCT_CODE)) AS PC_2021 FROM fact_sales_monthly
			  WHERE FISCAL_YEAR = '2021')
	 SELECT *, round(((PC_2021-PC_2020)/PC_2020)*100,2) AS PP FROM CTE1,CTE2;
```

Result:


![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/367d0612-8064-4923-ae23-364c2973cd2a)

## Adhoc request #3:
Provide a report with all the unique product counts for each segment and sort them in descending order of product counts. The final output contains 2 fields, segment and product_count

````
select segment,(count(product)) as product_count from dim_product
group by segment
order by product_count desc;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/25767221-6035-4386-8149-83607e007170)

## Adhoc request #4:
Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields,
segment, product_count_2020, product_count_2021, difference

````
with cte1 as (select distinct(p.segment) as seg,count(distinct(p.product_code)) as PC_2020
		from dim_product p left join fact_sales_monthly f
                on p.product_code = f.product_code
		where f.fiscal_year = '2020'
		group by p.segment),
	 cte2 as (select distinct(p.segment) as seg,count(distinct(p.product_code)) as PC_2021
		from dim_product p left join fact_sales_monthly f
		on p.product_code = f.product_code
		where f.fiscal_year = '2021'
		group by p.segment)   
                select a.seg,a.pc_2020 as product_count_2020,b.pc_2021 as product_count_2021, (b.PC_2021 - a.PC_2020) as difference
		from cte1 a inner join cte2 b
                on a.seg = b.seg
                order by difference desc;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/8753dcfb-ff7b-4df1-a17b-9c894217018e)

# Adhoc request #5:
Get the products that have the highest and lowest manufacturing costs. The final output should contain these fields, product_code
product, manufacturing_cost

````
(select p.product_code,p.product,f.manufacturing_cost 
from dim_product p right join fact_manufacturing_cost f
on p.product_code = f.product_code
WHERE manufacturing_cost = (SELECT MAX(manufacturing_cost) FROM fact_manufacturing_cost))
UNION
(select p.product_code,p.product,f.manufacturing_cost 
from dim_product p right join fact_manufacturing_cost f
on p.product_code = f.product_code
WHERE manufacturing_cost = (SELECT MIN(manufacturing_cost) FROM fact_manufacturing_cost));
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/612ad228-9b59-4787-ab0c-e4a3a64a873c)

# Adhoc request #6:
Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct for the fiscal year 2021 
and in the Indian market. The final output contains these fields, customer_code, customer, average_discount_percentage

````
SELECT F.customer_code,C.customer, ROUND(AVG(F.pre_invoice_discount_pct),2) AS average_discount_percentage
FROM fact_pre_invoice_deductions f LEFT JOIN dim_customer c
ON F.customer_code = C.customer_code
WHERE fiscal_year = '2021'
GROUP BY C.CUSTOMER
ORDER BY pre_invoice_discount_pct DESC
LIMIT 5;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/09c72098-0a5c-4f31-b640-b2fd10606083)

# Adhoc request #7:
Get the complete report of the Gross sales amount for the customer “Atliq Exclusive” for each month. This analysis helps to get an idea of low and high-performing months and take strategic decisions.The final report contains these columns: Month,Year,Gross sales Amount
````
with cte1 as (select s.product_code,s.sold_quantity,s.date as dates
	      from dim_customer c left join fact_sales_monthly s
              on c.customer_code=s.customer_code
              where customer = 'Atliq Exclusive'),
     cte2 as (select CTE1.DATES AS DATES, monthname(cte1.dates) as Month, year(cte1.dates) as Year,(cte1.sold_quantity*f.gross_price)  
              as GS
              from cte1 left join fact_gross_price f
              on cte1.product_code = f.product_code
              )
     select Month,Year,concat(ROUND((sum(GS)/1000000),2),' M') as gs from cte2
     group by month,year
     order by year, month(cte2.dates);
````
Result:

![image](https://github.com/user-attachments/assets/82096e84-b5ce-4d53-a9a8-3530d0ed562b)

# Adhoc request #8:
In which quarter of 2020, got the maximum total_sold_quantity? The final output contains these fields sorted by the total_sold_quantity, 
Quarter, total_sold_quantity
````
with cte1 as (select monthname(date) as Monthname, sold_quantity
			  from fact_sales_monthly
			  where fiscal_year = '2020'),
     cte2 as (select case when cte1.Monthname in ('September','October','November') then 'Q1'
						  when cte1.Monthname in ('February','January','December') then 'Q2'
                          when cte1.Monthname in ('March','April','May') then 'Q3'
					  else "Q4"
                    end as Quarter,CTE1.SOLD_QUANTITY
              FROM CTE1
              )
SELECT Quarter, concat(ROUND((sum(SOLD_QUANTITY)/1000000),2),' M') as total_sold_quantity FROM CTE2
group by quarter
order by total_sold_quantity desc; 
````
Result:

![image](https://github.com/user-attachments/assets/cb0afb5e-75b5-4f22-80a6-29c4fc4996a0)

# Adhoc request #9:

Which channel helped to bring more gross sales in the fiscal year 2021 and the percentage of contribution? The final output contains these fields,channel, gross_sales_mln,percentage
````
with cte1 as (select f.product_code,f.customer_code,f.sold_quantity, c.channel from dim_customer c left join fact_sales_monthly f
              on c.customer_code = f.customer_code
 	      where f.fiscal_year ='2021'),
     cte2 as (select cte1.channel, sum(cte1.sold_quantity*g.gross_price) as total_sales from cte1 left join fact_gross_price g
  	      on cte1.product_code = g.product_code
              group by cte1.channel)
	      select channel,concat(round((sum(total_sales)/1000000),2),' M') as gross_sales_mln,
              concat(round(total_sales/(sum(total_sales) OVER())*100,2),' ','%') AS percentage 
              from cte2
              group by channel
              order by percentage desc;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/0eb45516-d7f0-4f75-b852-ab44bec97169)


# Adhoc request #10:
Get the Top 3 products in each division that have a high total_sold_quantity in the fiscal_year 2021? The final output contains these
fields, division, product_code, product, total_sold_quantity, rank_order
````
with cte1 as (select p.division, p.product_code, p.product, sum(s.sold_quantity) as sold_quantity
              from dim_product p left join fact_sales_monthly s
	      on p.product_code = s.product_code
              where s.fiscal_year = '2021'
	      group by p.product_code),
     cte2 as (select division,product_code,product, sold_quantity, 
              dense_rank()over(partition by division order by sold_quantity desc) as rank_order
	      from cte1)
              select * from cte2
              where rank_order < 4;
````
Result:

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/6522a4e7-3ec2-4a82-8089-ae77e4dc337e)








