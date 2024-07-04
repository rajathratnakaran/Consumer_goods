# Online retail Orders Analysis

<img src="https://www.42signals.com/wp-content/uploads/2023/10/analytics2-01.png" width="800" height="400">

## INTRODUCTION

This project is based on the order management functionality of an online retail store in which you are provided with the “orders” database and you are asked some queries related to it. Answers to these queries will help the company in making data-driven decisions that will impact the overall growth of the online retail store. 

## Task:  

Imagine yourself as the applicant for this role and perform the following task

1.    There are 8 ad hoc requests for which the business needs insights.
2.    You need to run a SQL query to answer these requests. 
3.    The target audience of this dashboard is top-level management - hence you need to create a presentation to show the insights.
4.    Be creative with your presentation.

## Adhoc request #1:
Write a query to Display the product details (product_class_code, product_id, product_desc,
product_price,) as per the following criteria and sort them in descending order of category:
a. If the category is 2050, increase the price by 2000
b. If the category is 2051, increase the price by 500
c. If the category is 2052, increase the price by 600.

SQL Query:
```
select product_class_code, product_id, product_desc,
  case when PRODUCT_CLASS_CODE = 2050 then (PRODUCT_PRICE + 2000)
	   when PRODUCT_CLASS_CODE = 2051 then (PRODUCT_PRICE + 500)
       when PRODUCT_CLASS_CODE = 2052 then (PRODUCT_PRICE + 600)
       else product_price
       end as product_price
from product
order by product_class_code desc;
```

Result:
For demostartion purpose the data is limited to category 2050,2051,2052. 

Before: 
![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/52c16c44-96bd-40e0-a10a-de5c2493de3a)

After:
![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/3c166e64-0d33-4ee6-9d30-151864df2742)









