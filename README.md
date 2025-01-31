# Pizza Sales Data Analysis Using SQl

![Pizza Logo](https://github.com/sampleshankar/Pizza_sales_sql_project/blob/main/food-menu-delicious-pizza-social-media-banner-template_106176-575.jpg)

##Objective

# Business Problems And Solutions

### Retrieve the total number of orders placed.

```
SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders;
```
    
### Calculate the total revenue generated from pizza sales.

```
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price)) AS Total_Revenue
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id
```
    
     
### Identify the most common pizza size ordered.

```
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC
LIMIT 1;
```

### List the top 5 most ordered pizza types along with their quantities.

```
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
```

### Join the necessary tables to find the total quantity of each pizza category ordered.

```
SELECT 
    SUM(order_details.quantity) AS quantity,
    pizza_types.category
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
order by quantity desc;
```

### Determine the distribution of orders by hour of the day.

```
SELECT 
    HOUR(order_time) as hour, COUNT(order_id) as order_count
FROM
    orders
GROUP BY HOUR(order_time);
```

### Join relevant tables to find the category-wise distribution of pizzas.
```
select category,count(name) from pizza_types
group by category
```

### Group the orders by date and calculate the average number of pizzas ordered per day.

```
SELECT 
    ROUND(AVG(quantity)) AS avg_pizza_ordered_per_day
FROM
    (SELECT 
        orders.date_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.date_date) AS order_quantity; 
```
### Analyze the cumulative revenue generated over time.

```
select date_date,
sum(revenue) over(order by date_date) as cum_revenue
from 
(select orders.date_date,
sum(order_details.quantity*pizzas.price) as revenue
from order_details join pizzas
on order_details.pizza_id = pizzas.pizza_id
join orders
on orders.order_id=order_details.order_id
group by orders.date_date) as sales;
```

 ### Calculate the percentage contribution of each pizza type to total revenue.
```
SELECT 
    pizza_types.category,
    (SUM(order_details.quantity * pizzas.price) / (SELECT 
            ROUND(SUM(order_details.quantity * pizzas.price)) AS Total_Revenue
        FROM
            order_details
                JOIN
            pizzas ON pizzas.pizza_id = order_details.pizza_id)) * 100 AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY revenue DESC;
```
