# PROVIDE INSIGHTS TO MANAGEMENT IN CONSUMER GOODS

![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/e8cab7bc-96e1-485a-9805-fff3f5b289c3)

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


