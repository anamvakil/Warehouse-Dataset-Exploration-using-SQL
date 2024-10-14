# Using SQL to explore the Warehouse Dataset

Uploading a warehouse dataset to the account to explore the COUNT and COUNT Distinct function

![image](https://github.com/user-attachments/assets/53f22bc9-357d-4375-b122-4924776426e4)

![image](https://github.com/user-attachments/assets/16bee2c4-8988-4ff4-92de-1ec16252685f)

Running some basic SQL queries to explore the datasets

![image](https://github.com/user-attachments/assets/47f15fe2-959f-4ad0-af13-df815d4d162a)

To help us eliminate the duplicate records, we will use DISTINCT() on the warehouse_id column. This will ensure that only distinct, non-repeated values are returned from the order table.

```
SELECT DISTINCT warehouse_id 
FROM `absolute-accord-429300-g1.warehouse_orders.orders`;
```

![image](https://github.com/user-attachments/assets/7a1f5535-450a-4721-ab0c-9281335af70b)

Now let us assume that we want to know the total number of the distinct warehouse_id records.

```
SELECT COUNT(DISTINCT warehouse_id)
FROM `absolute-accord-429300-g1.warehouse_orders.orders`;
```

![image](https://github.com/user-attachments/assets/2f0b9651-d0b1-4779-8c9e-f63ed64f0f3b)

In the below example, we will try extracting the records from the warehouse table where the maximum capacity of any warehouse is greater than 250

```
SELECT *
FROM `absolute-accord-429300-g1.warehouse_orders.warehouse` 
WHERE maximum_capacity>250;
```

![image](https://github.com/user-attachments/assets/844d091f-38c0-4956-9efd-2bf368cb6cd7)

We can see that warehouse_alias has two categories of warehouse names. Let us consider we want to extract the records where warehouse_alias ends with "Centre"

![image](https://github.com/user-attachments/assets/13a4b959-4a7e-4fe6-95cd-c586afc852ad)

```
SELECT warehouse_alias 
FROM `absolute-accord-429300-g1.warehouse_orders.warehouse`
WHERE warehouse_alias LIKE "%Center";
```

![image](https://github.com/user-attachments/assets/516d0903-44ce-424d-a6ff-8e841f020b80)


Now, we will try to create Aliases and use JOIN

```
SELECT  
*
FROM `absolute-accord-429300-g1.warehouse_orders.orders` AS orders
```

![image](https://github.com/user-attachments/assets/4860f60f-8e2e-4636-b961-e788b617ca51)

```
SELECT 
* 
FROM `absolute-accord-429300-g1.warehouse_orders.warehouse` AS warehouse
```

![image](https://github.com/user-attachments/assets/3be0512c-d1a1-499e-b7c2-14463bbda28d)

The above method can also be done in a much shorter way as shown in the below given example.

```
SELECT  
*
FROM `absolute-accord-429300-g1.warehouse_orders.orders` AS orders
JOIN `absolute-accord-429300-g1.warehouse_orders.warehouse` AS warehouse
ON orders.warehouse_id = warehouse.warehouse_id
```

To select the specific columns we need to work with, we can use the following query to return just those columns instead of the entire table.

```
SELECT  
  orders.*,
  warehouse.warehouse_alias,
  warehouse.state
FROM `absolute-accord-429300-g1.warehouse_orders.orders` AS orders
JOIN `absolute-accord-429300-g1.warehouse_orders.warehouse` AS warehouse
ON orders.warehouse_id = warehouse.warehouse_id
```

![image](https://github.com/user-attachments/assets/d5b6a830-5063-49fa-9d11-1fcdf83ebe35)

As shown above we could retrieve ***warehouse_alias*** and ***state*** column from the warehouse table and join it with the order table.

Now we will try to find the number of states with warehouses that have shipped orders.

```
SELECT  
  COUNT(warehouse.state) as num_states
FROM `absolute-accord-429300-g1.warehouse_orders.orders` AS orders
JOIN `absolute-accord-4293Toouse_orders.warehouse` AS warehouse
ON orders.war,ehouse_id = warehouse.warehouse_to return juste](https://github.com/userthettachments/assets/4d98ca7b-4849-45fb-8cad-29143a439a8f)

An important thing to note is how the query returned more than 9,000 results. There are only 50 states, so this is not the answer we are looking for! This is because the query counted every record (row) that included a state, regardless of duplicates or null values.

```
SELECT  
  COUNT (DISTINCT(warehouse.state)) as num_states
FROM `absolute-accord-429300-g1.warehouse_orders.orders` AS orders
JOIN `absolute-accord-429300-g1.warehouse_orders.warehouse` AS warehouse
ON orders.warehouse_id = warehouse.warehouse_id
```

![image](https://github.com/user-attachments/assets/ef5f79ea-27bc-4c95-a168-65bde5555cf4)

In order to group the number of orders by state, we will use GROUP BY

```
SELECT  
  state,
  COUNT(DISTINCT order_id) as num_orders
FROM `absolute-accord-429300-g1.warehouse_orders.orders` AS orders
JOIN `absolute-accord-429300-g1.warehouse_orders.warehouse` AS warehouse
ON orders.warehouse_id = warehouse.warehouse_id
GROUP BY
  warehouse.state
```

![image](https://github.com/user-attachments/assets/21e17a6f-f0d0-458b-8fdd-5476d617f8c5)
