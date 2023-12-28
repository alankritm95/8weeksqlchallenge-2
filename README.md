# 8weeksqlchallenge-2

## Case Study #2 Pizza Runner
View the case study [here](https://8weeksqlchallenge.com/case-study-2/)

## Introduction

While perusing his Instagram feed, Danny stumbled upon something truly captivating: "80s Retro Styling and Pizza Is The Future!" Intrigued, he became enamored with the concept, realizing that pizza alone wouldn't secure the seed funding needed to expand his budding Pizza Empire. Determined to add a unique twist, Danny conceived the brilliant idea to "Uberize" it, giving birth to Pizza Runner.

Taking the first step, Danny recruited a team of "runners" to deliver fresh pizzas directly from Pizza Runner Headquarters, aka his own house. Undeterred by financial constraints, he maxed out his credit card to hire freelance developers tasked with creating a mobile app capable of seamlessly handling customer orders.

## Dataset

Datasets crucial for this case study include:

1. **Runners Table:**
   - Provides information on the registration_date for each new runner.

2. **Customer_Orders Table:**
   - Captures customer pizza orders, with each row representing an individual pizza within an order. 
   - The pizza_id corresponds to the ordered pizza type, exclusions denote ingredient_id values to be removed, and extras indicate ingredient_id values to be added.

3. **Runner_Orders Table:**
   - Tracks orders assigned to runners, with the possibility of cancellations by either the restaurant or the customer.
   - pickup_time signifies the timestamp when the runner arrives at Pizza Runner headquarters to collect freshly cooked pizzas.
   - Distance and duration fields detail the runner's travel metrics for delivering orders.

4. **Pizza_Names Table:**
   - Highlights the two available pizzas at Pizza Runner: Meat Lovers and Vegetarian.
  
5. **Pizza_Recipes Table:**
   - Specifies the standard set of toppings associated with each pizza_id.
  
6. **Pizza_Toppings Table:**
   - Enlists all topping_name values along with their corresponding topping_id values.

  

## Entity Relationship Diagram

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/645f746a-b24c-4312-bbc4-b7d6ff84b1ff)
  

## Data Cleaning & Transformation

### customer_orders table

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/2afdbb7c-f4d9-485d-9ee2-b7a79a5a12a3)

Cleaning the table:

DROP TABLE IF EXISTS cust_orders_temp;
CREATE TEMPORARY TABLE cust_orders_temp AS
SELECT order_id, customer_id, pizza_id,
case when exclusions = ''  then NULL 
when exclusions = 'null' then NULL 
else exclusions end as exclusions,
case when extras = '' then NULL
when extras = 'null' then NULL
else extras end as extras, order_time

FROM customer_orders;

SELECT * FROM cust_orders_temp;

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/de0327c8-f6c9-41f8-a1d7-c3241375cdb7)

### runner_orders table 

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/d36f5313-7351-43e8-b44d-e52046ac1b2b)


Cleaning the table:

drop TABLE IF EXISTS runr_orders_temp;
CREATE TEMPORARY TABLE runr_orders_temp AS
select order_id, runner_id, pickup_time,
case when distance like '%km%' then replace(distance, 'km', '')
when distance like '% km%' then replace(distance, ' km', '')
when distance = 'null' then NULL
else distance end as 'distance',

case when duration like '% minutes%' then replace(duration, ' minutes', '')
when duration like '%minutes%' then replace(duration, 'minutes', '')
when duration like '%minute%' then replace(duration, 'minute', '')
when duration like '% mins%' then replace(duration, ' mins', '')
when duration like '%mins%' then replace(duration, 'mins', '')
when duration = 'null' then NULL
else duration
end as 'duration',

case when cancellation = '' then NULL
when cancellation = 'null' then NULL
else cancellation end as 'cancellation'
from runner_orders;

select * from runr_orders_temp;

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/99b2c9c9-0bf3-49a6-b6c8-95ef8c0a3577)

## Case Study Questions

### How many pizzas were ordered?

SELECT count(pizza_id) AS "Total_Pizzas_Ordered"
FROM pizza_runner.cust_orders_temp;

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/d62264b0-c8e0-42ad-b4c6-ffa28ae5b865)


### How many unique customer orders were made?

select count(distinct order_id) as 'unique_orders'
 from cust_orders_temp;

 ![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/9b0f6c00-daea-43f9-82be-1e1a42710e9f)


### How many successful orders were delivered by each runner?

select runner_id, count(order_id) as 'delivered_orders'
 from runr_orders_temp
 where cancellation is NULL
 group by runner_id;

 ![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/36c80e62-12b7-4ca7-a2e2-c86be6c572d9)


### How many of each type of pizza was delivered?


select c.pizza_id, pizza_name, count(c.pizza_id) as 'tot_pizzas'
 from runr_orders_temp r join cust_orders_temp c
 on r.order_id = c.order_id join pizza_names p
 on c.pizza_id = p.pizza_id
 where cancellation is NULL
 group by c.pizza_id, pizza_name;

![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/6ad2676b-1699-4140-a13c-6d459940c202)

 
### How many Vegetarian and Meatlovers were ordered by each customer?

 select customer_id, pizza_name,count(pizza_name) as count from cust_orders_temp c
 join pizza_names p
 on c.pizza_id = p.pizza_id
 group by c.customer_id, pizza_name; 

 ![image](https://github.com/alankritm95/8weeksqlchallenge-2/assets/129503746/12c4fa70-9584-41cf-942e-fb467fa137e2)


### What was the maximum number of pizzas delivered in a single order?

WITH cte AS
(
  SELECT 
    c.order_id, 
    COUNT(c.pizza_id) AS pizza_per_order
  FROM cust_orders_temp c
  JOIN runr_orders_temp r
    ON c.order_id = r.order_id
  WHERE cancellation is null
  GROUP BY c.order_id
)

SELECT
  MAX(pizza_per_order) AS pizza_count
FROM cte;


### For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
### How many pizzas were delivered that had both exclusions and extras?
### What was the total volume of pizzas ordered for each hour of the day?
### What was the volume of orders for each day of the week?




