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
c. If the category is 2052, increase the price by 600

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


## Adhoc request #2:
Write a query to display (product_class_desc, product_id, product_desc, product_quantity_avail ) and Show inventory status of products as below as per their available quantity:

a. For Electronics and Computer categories, if available quantity is <= 10, show 'Low stock', 11 <= qty <= 30, show 'In stock', >= 31, show 'Enough stock'

b. For Stationery and Clothes categories, if qty <= 20, show 'Low stock', 21 <= qty <= 80, show 'In stock', >= 81, show 'Enough stock'

c. Rest of the categories, if qty <= 15 – 'Low Stock', 16 <= qty <= 50 – 'In Stock', >= 51 – 'Enough stock'
For all categories, if available quantity is 0, show 'Out of stock'.
Hint: Use case statement.

SQL Query:
```
select q.product_class_desc, p.product_id, p.product_desc, p.product_quantity_avail,
case 
	when q.product_class_desc  in ('Electronics', 'Computer')  
    then
		case
			when p.product_quantity_avail <= 20 then "Low Stock"
              		when p.product_quantity_avail between 11 and 30 then "In Stock"
			when p.product_quantity_avail >= 31 then "Enough stock"
            		else "No info"
		end
	else
		case	
			when q.product_class_desc  in ('Stationery', 'Clothes')
			then
				case
					when p.product_quantity_avail <= 10 then "Low Stock"
					when p.product_quantity_avail between 21 and 80 then "In Stock"
					when p.product_quantity_avail >= 81 then "Enough stock"
					else "No info"
		end
	else 
		case
			when p.product_quantity_avail <= 15 then "Low Stock"
			when p.product_quantity_avail between 16 and 50 then "In Stock"
			when p.product_quantity_avail >= 51 then "Enough stock" 
			else "No info"
		end
	end
end as "Inventory status"
from product p left join product_class q
on p.PRODUCT_CLASS_CODE = q.PRODUCT_CLASS_CODE;	
```

Result:
For demostartion purpose the data is limited 23 rows. 
![image](https://github.com/rajathratnakaran/SQL-projects/assets/92428713/92c942fc-708a-4067-bd2f-a0e10170ab57)







