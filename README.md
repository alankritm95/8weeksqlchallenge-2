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
