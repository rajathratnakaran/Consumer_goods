## Online retail Orders Analysis

## This document tries to solve some of the scenarios for an online retail store's order management functionality database.

### This project is based on the order management functionality of an online retail store in which you are provided with the “orders” database and you are asked some queries related to it. Answers to these queries will help the company in making data-driven decisions that will impact the overall growth of the online retail store.
### Skills and Tools: Joins, Sub Queries, SQL-clauses-statements-conditions, MySQL Workbench.


*1. Write a query to Display the product details (product_class_code, product_id, product_desc,product_price,) as per the following criteria and sort them in descending order of category:*

*a. If the category is 2050, increase the price by 2000*

*b. If the category is 2051, increase the price by 500*

*c. If the category is 2052, increase the price by 600.*

*Hint: Use case statement. no permanent change in table required.*

```
select product_class_code, product_id, product_desc,product_price,
case when product_class_code = '2050' then product_price+2000
when product_class_code = '2051' then product_price+500
when product_class_code = '2052' then product_price+600
else product_price
end as new_product_price from product
order by PRODUCT_CLASS_CODE desc;
```

*2) Write a query to display (product_class_desc, product_id, product_desc,product_quantity_avail ) and Show inventory status of products* *as below as per their available quantity:*
*a. For Electronics and Computer categories, if available quantity is <= 10, show 'Low stock', 11 <= qty <= 30, show 'In stock', >= 31,* *show 'Enough stock'*
*b. For Stationery and Clothes categories, if qty <= 20, show 'Low stock', 21 <= qty <= 80, show 'In stock', >= 81, show 'Enough stock'*
*c. Rest of the categories, if qty <= 15 – 'Low Stock', 16 <= qty <= 50 – 'In Stock', >= 51 'Enough stock'*
*For all categories, if available quantity is 0, show 'Out of stock'.*
*Hint: Use case statement.*

```
select b.product_class_desc, a.product_id, a.product_desc,
a.product_quantity_avail, 
case 
when b.product_class_desc in ('Electronics','Computer') and a.product_quantity_avail <=10 then "Low stock"
when b.product_class_desc in ('Electronics','Computer') and a.product_quantity_avail between 11 and 30 then "In stock"
when b.product_class_desc in ('Electronics','Computer') and a.product_quantity_avail >=10 then "Enough stock"
when b.product_class_desc in ('Stationery','Clothes') and a.product_quantity_avail <=20 then "Low stock"
when b.product_class_desc in ('Stationery','Clothes') and a.product_quantity_avail between 21 and 80 then "In stock"
when b.product_class_desc in ('Stationery','Clothes') and a.product_quantity_avail >=81 then "Enough stock"
when b.product_class_desc not in ('Stationery','Clothes','Electronics','Computer') and a.product_quantity_avail <=15 then "Low stock"
when b.product_class_desc not in ('Stationery','Clothes','Electronics','Computer')  and a.product_quantity_avail between 16 and 50 then "In stock"
when b.product_class_desc not in ('Stationery','Clothes','Electronics','Computer')  and a.product_quantity_avail >=51 then "Enough stock"
else "Out of Stock"
end as inventory_status
from product a left join product_class b
on a.PRODUCT_CLASS_CODE = b.PRODUCT_CLASS_CODE;
```

*3. Write a query to Show the count of cities in all countries other than USA & MALAYSIA, with more than 1 city, in the descending order* *of CITIES.*
*[NOTE: ADDRESS TABLE, Do not use Distinct]*

```
select count(city) as Number_of_cities,country from (SELECT * FROM address
			   where country NOT IN ("Malaysia" ,"USA")) A
		group by country
        having count(city)>1
```



