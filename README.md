# Data-analytics-project-3-niyoenterprise-


I will be presenting my final project from the niyo enterprise bootcamp, using excel, tableau and my sql workbench focusing mainly on analytics.

The data consisted of two separate retail data sets which needed to be joined within tableau and SQL using the 'customer id' values.

TABLEAU
...
To work out what would be valuable analysis for this data set...I first started with tableau to get the first picture of what the data was telling me;
for example the most popular and most sold product category is "Books" which i used a bubble chart answer.

I also wanted to know " what was the most popular city for buying books?" so i used bar chart to compare the city & quantities sold which resulted in city code 3 followed closely by 4 and 5 spending the most on books.

However, the line chart provided the most interesting twist as i would of assumed "books" would of been on the increase but clothing was steadly increasing year on year, while books were still popular but decreasing in sales.

EXCEL
...




SQL
...
select* from final.retail_transactions;
select* from final.retail_customers;

-- INNER JOIN tables base on customer id
select * FROM final.retail_customers CUS
Inner join final.retail_transactions TRANS
ON CUS.customer_id = TRANS.cust_id;

-- what are the number of transactions across genders?
-- (M) = 11811.   -- (F) = 11233. -- (MISCELLANEOUS)= 9
select gender, count(transaction_id)
FROM final.retail_customers CUS
Inner join final.retail_transactions TRANS
ON CUS.customer_id = TRANS.cust_id
group by gender;

-- total number of product catergories
-- 6
select count(distinct prod_category) AS 'NUMBER OF PRODUCT CATEGORY' from final.retail_transactions;


-- what are the total number of items in personal appliances/tax/qty/sales?
-- RESULT ( ELECTRONICS 1) TAX (243551.9099999 ETC) QTY (2427) (2110196.40000000 ETC)
select prod_category, count(distinct prod_subcategory) AS 'TOTAL APPLIANCES', sum(Tax) AS 'TOTAL TAX', 
sum(Qty) AS 'TOTAL QUANTITY', sum(total_amt) AS 'TOTAL SALES'
from final.retail_transactions
where prod_subcategory = "Personal Appliances"
group by prod_category;

-- what is the average amount of tax by city code?
-- 0 (176). 1 (250). 2 (252) 3 (247). 4 (244) 5 (244) etc
select city_code, avg(Tax) AS 'AVERAGE TAX'
FROM final.retail_customers CUS
Inner join final.retail_transactions TRANS
ON CUS.customer_id = TRANS.cust_id
group by city_code
order by 1 asc;


-- what are the products with the most returns?
-- QUANTITY-- BOOKS (1471) ELECTRONICS (1250) HOME & KITCHEN (1245) ETC...
select prod_category, sum(total_amt) AS 'TOTAL SALES', sum(Qty) AS 'TOTAL QUANTITY'
from  final.retail_transactions
where total_amt < '-1'
group by prod_category
order by 2 asc;

-- total number of STORES
-- 4
select count(distinct store_type) AS 'NUMBER OF STORE TYPES' from final.retail_transactions;

-- WHICH STORE HAS THE HIGHEST RETURNS
-- e-shop
SELECT store_type, COUNT(total_amt) AS 'TOTAL AMOUNT OF RETURNS'
from final.retail_transactions
where total_amt < '-1'
group by store_type
order by 1 asc;

-- INNER JOIN tables base on customer id
select * FROM final.retail_customers CUS
Inner join final.retail_transactions TRANS
ON CUS.customer_id = TRANS.cust_id;

-- compare transaction days to possible targets?
select tran_date,total_amt, store_type,
case
when total_amt > 8000 then 'smashing day'
when total_amt between 5000 and 7999 then 'moderate day'
when total_amt between 1 and 4999 then 'bad day'
else 'returns'
end 'day evaluation'
from final.retail_transactions;


-- NOT QUITE WHAT I WAS GOING FOR....
select gender, city_code, prod_category, total_amt
FROM final.retail_transactions TRANS
Inner join final.retail_customers CUS
ON CUS.customer_id = TRANS.cust_id
where city_code = 5
order by prod_category,gender;



