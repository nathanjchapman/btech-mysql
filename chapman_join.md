# JOIN Assignment

For each of the questions in this assignment include the SQL query and the results of the query. All solutions will require a join.

Use the northwind.sqlPreview the document file to complete this assignment.

Load the northwind database and data then complete the following queries.

### 1. Using the customers and orders table find all the orders that were created by an owner of a company.

SQL:
```sql
SELECT orders.id,
    customers.first_name,
    customers.last_name,
    customers.job_title,
    orders.order_date
FROM orders
INNER JOIN customers
ON orders.customer_id=customers.id
WHERE customers.job_title = "Owner";
```

Output:
```
+----+------------+-----------+-----------+---------------------+
| id | first_name | last_name | job_title | order_date          |
+----+------------+-----------+-----------+---------------------+
| 41 | Ming-Yang  | Xie       | Owner     | 2006-03-24 00:00:00 |
| 44 | Anna       | Bedecs    | Owner     | 2006-03-24 00:00:00 |
| 68 | Ming-Yang  | Xie       | Owner     | 2006-05-24 00:00:00 |
| 71 | Anna       | Bedecs    | Owner     | 2006-05-24 00:00:00 |
+----+------------+-----------+-----------+---------------------+
```


### 2. Display the first name, last name and privilege name for the only entry in the employee_privileges table.

SQL:
```sql
SELECT employees.first_name, employees.last_name, privileges.privilege_name
FROM employees, employee_privileges, privileges
WHERE employees.id = employee_privileges.employee_id
AND privileges.id = employee_privileges.privilege_id;
```

Output:
```
+------------+-----------+--------------------+
| first_name | last_name | privilege_name     |
+------------+-----------+--------------------+
| Andrew     | Cencini   | Purchase Approvals |
+------------+-----------+--------------------+

```

### 3. Display the product code, product name, reorder level, and supplier for all products that have a reorder level of at least 25.

SQL:
```sql
SELECT products.product_code,
    products.product_name,
    products.reorder_level,
    suppliers.company
FROM products
INNER JOIN suppliers_products
ON products.id = suppliers_products.product_id
INNER JOIN suppliers
ON suppliers.id = suppliers_products.supplier_id
WHERE products.reorder_level >= 25;
```

Output:
```
+--------------+--------------------------------------+---------------+------------+
| product_code | product_name                         | reorder_level | company    |
+--------------+--------------------------------------+---------------+------------+
| NWTG-52      | Northwind Traders Long Grain Rice    |            25 | Supplier A |
| NWTP-56      | Northwind Traders Gnocchi            |            30 | Supplier A |
| NWTC-82      | Northwind Traders Hot Cereal         |            50 | Supplier A |
| NWTJP-6      | Northwind Traders Boysenberry Spread |            25 | Supplier B |
| NWTDFN-80    | Northwind Traders Dried Plums        |            50 | Supplier B |
| NWTB-43      | Northwind Traders Coffee             |            25 | Supplier C |
| NWTB-81      | Northwind Traders Green Tea          |           100 | Supplier C |
| NWTB-43      | Northwind Traders Coffee             |            25 | Supplier D |
| NWTJP-6      | Northwind Traders Boysenberry Spread |            25 | Supplier F |
| NWTSO-98     | Northwind Traders Vegetable Soup     |           100 | Supplier F |
| NWTSO-99     | Northwind Traders Chicken Soup       |           100 | Supplier F |
| NWTCM-40     | Northwind Traders Crab Meat          |            30 | Supplier G |
| NWTCM-95     | Northwind Traders Tuna Fish          |            30 | Supplier G |
| NWTCM-96     | Northwind Traders Smoked Salmon      |            30 | Supplier G |
| NWTCS-83     | Northwind Traders Potato Chips       |            30 | Supplier I |
| NWTCO-3      | Northwind Traders Syrup              |            25 | Supplier J |
| NWTCA-48     | Northwind Traders Chocolate          |            25 | Supplier J |
+--------------+--------------------------------------+---------------+------------+
```

### 4. Find all the inventory transactions that are on hold and the products for those transactions. In the output display the transaction id, the date the transaction was last modified, the product name, the quantity ordered, and the transaction type.

SQL:
```sql
SELECT inventory_transactions.id,
    inventory_transactions.transaction_modified_date,
    products.product_name,
    inventory_transactions.quantity,
    inventory_transactions.transaction_type,
    inventory_transaction_types.type_name AS `status`
FROM inventory_transactions
INNER JOIN products ON inventory_transactions.product_id = products.id
INNER JOIN inventory_transaction_types
ON inventory_transactions.transaction_type = inventory_transaction_types.id
WHERE inventory_transactions.transaction_type = 3;
```

Output:
```
+-----+---------------------------+-------------------------------+----------+------------------+---------+
| id  | transaction_modified_date | product_name                  | quantity | transaction_type | status  |
+-----+---------------------------+-------------------------------+----------+------------------+---------+
|  86 | 2006-03-24 14:49:16       | Northwind Traders Dried Plums |       20 |                3 | On Hold |
|  87 | 2006-03-24 14:49:20       | Northwind Traders Green Tea   |       50 |                3 | On Hold |
|  88 | 2006-03-24 14:50:09       | Northwind Traders Chai        |       25 |                3 | On Hold |
|  89 | 2006-03-24 14:50:14       | Northwind Traders Coffee      |       25 |                3 | On Hold |
|  90 | 2006-03-24 14:50:18       | Northwind Traders Green Tea   |       25 |                3 | On Hold |
|  96 | 2006-03-30 16:46:34       | Northwind Traders Beer        |       12 |                3 | On Hold |
|  97 | 2006-03-30 17:23:27       | Northwind Traders Beer        |       10 |                3 | On Hold |
|  98 | 2006-03-30 17:24:33       | Northwind Traders Beer        |        1 |                3 | On Hold |
| 104 | 2006-04-04 11:01:37       | Northwind Traders Coffee      |      300 |                3 | On Hold |
| 136 | 2006-04-25 17:04:57       | Northwind Traders Gnocchi     |      110 |                3 | On Hold |
+-----+---------------------------+-------------------------------+----------+------------------+---------+
```

### 5. Find all the orders that have not been shipped and display the order id, the order date, and the employee first and last name associated with the order.

SQL:
```sql
SELECT orders.id,
    orders.order_date,
    employees.first_name,
    employees.last_name,
    orders_status.status_name AS 'status'
FROM orders
INNER JOIN employees ON employees.id = orders.employee_id
INNER JOIN orders_status ON orders.status_id = orders_status.id
WHERE orders.shipped_date IS NULL;
```

Output:
```
+----+---------------------+------------+-----------+--------+
| id | order_date          | first_name | last_name | status |
+----+---------------------+------------+-----------+--------+
| 43 | 2006-03-24 00:00:00 | Nancy      | Freehafer | New    |
| 44 | 2006-03-24 00:00:00 | Nancy      | Freehafer | New    |
| 68 | 2006-05-24 00:00:00 | Nancy      | Freehafer | New    |
| 69 | 2006-05-24 00:00:00 | Nancy      | Freehafer | New    |
| 70 | 2006-05-24 00:00:00 | Nancy      | Freehafer | New    |
| 71 | 2006-05-24 00:00:00 | Nancy      | Freehafer | New    |
| 80 | 2006-04-25 17:03:55 | Andrew     | Cencini   | New    |
| 81 | 2006-04-25 17:26:53 | Andrew     | Cencini   | New    |
+----+---------------------+------------+-----------+--------+
```

### 6. Find all the orders and the order details for all non-invoiced orders going to California. Display all available information for these orders.

SQL:
```sql
SELECT * FROM orders
INNER JOIN order_details ON orders.id = order_details.order_id
INNER JOIN order_details_status ON order_details.status_id = order_details_status.id
AND orders.ship_state_province = 'CA'
AND order_details_status.status_name != "Invoiced";
```

Output:
```
+----+-------------+-------------+---------------------+--------------+------------+-------------+----------------+-------------+---------------------+----------------------+---------------------+--------------+--------+--------------+-----------+-------+----------+---------------+-----------+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+----+-------------+
| id | employee_id | customer_id | order_date          | shipped_date | shipper_id | ship_name   | ship_address   | ship_city   | ship_state_province | ship_zip_postal_code | ship_country_region | shipping_fee | taxes  | payment_type | paid_date | notes | tax_rate | tax_status_id | status_id | id | order_id | product_id | quantity | unit_price | discount | status_id | date_allocated | purchase_order_id | inventory_id | id | status_name |
+----+-------------+-------------+---------------------+--------------+------------+-------------+----------------+-------------+---------------------+----------------------+---------------------+--------------+--------+--------------+-----------+-------+----------+---------------+-----------+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+----+-------------+
| 81 |           2 |           3 | 2006-04-25 17:26:53 | NULL         |       NULL | Thomas Axen | 123 3rd Street | Los Angelas | CA                  | 99999                | USA                 |       0.0000 | 0.0000 | NULL         | NULL      | NULL  |        0 |          NULL |         0 | 91 |       81 |         56 |   0.0000 |    38.0000 |        0 |         0 | NULL           |              NULL |         NULL |  0 | None        |
| 81 |           2 |           3 | 2006-04-25 17:26:53 | NULL         |       NULL | Thomas Axen | 123 3rd Street | Los Angelas | CA                  | 99999                | USA                 |       0.0000 | 0.0000 | NULL         | NULL      | NULL  |        0 |          NULL |         0 | 90 |       81 |         81 |   0.0000 |     2.9900 |        0 |         5 | NULL           |              NULL |         NULL |  5 | No Stock    |
+----+-------------+-------------+---------------------+--------------+------------+-------------+----------------+-------------+---------------------+----------------------+---------------------+--------------+--------+--------------+-----------+-------+----------+---------------+-----------+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+----+-------------+
```

### 7. Using the purchase orders and purchase order status tables, find all the submitted purchase orders. Display all available information.

SQL:
```sql
SELECT * FROM purchase_orders
INNER JOIN purchase_order_status
WHERE purchase_orders.status_id = purchase_order_status.id
AND purchase_order_status.status = 'Submitted';
```

Output:
```
+-----+-------------+------------+---------------------+---------------------+-----------+---------------+--------------+--------+--------------+----------------+----------------+-------+-------------+---------------+--------------+----+-----------+
| id  | supplier_id | created_by | submitted_date      | creation_date       | status_id | expected_date | shipping_fee | taxes  | payment_date | payment_amount | payment_method | notes | approved_by | approved_date | submitted_by | id | status    |
+-----+-------------+------------+---------------------+---------------------+-----------+---------------+--------------+--------+--------------+----------------+----------------+-------+-------------+---------------+--------------+----+-----------+
| 146 |           2 |          2 | 2006-04-26 18:26:37 | 2006-04-26 18:26:37 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL  |           2 | NULL          |            2 |  1 | Submitted |
| 147 |           7 |          2 | 2006-04-26 18:33:28 | 2006-04-26 18:33:28 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL  |        NULL | NULL          |            2 |  1 | Submitted |
| 148 |           5 |          2 | 2006-04-26 18:33:52 | 2006-04-26 18:33:52 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL  |        NULL | NULL          |            2 |  1 | Submitted |
+-----+-------------+------------+---------------------+---------------------+-----------+---------------+--------------+--------+--------------+----------------+----------------+-------+-------------+---------------+--------------+----+-----------+
```

### 8. Find all the submitted purchase orders, who submitted them, and to which supplier they belong to. Display the purchase order id, the employee first and last name, and supplier name. Rename the purchase order id column to "po_id".

SQL:
```sql
SELECT purchase_orders.id AS `po_id`,
    employees.first_name,
    employees.last_name,
    suppliers.company,
    purchase_order_status.status
FROM purchase_orders
INNER JOIN purchase_order_status ON purchase_order_status.id = purchase_orders.status_id
INNER JOIN employees ON employees.id = purchase_orders.created_by
INNER JOIN suppliers ON suppliers.id = purchase_orders.supplier_id
WHERE purchase_order_status.status = 'Submitted';
```

Output:
```
+-------+------------+-----------+------------+-----------+
| po_id | first_name | last_name | company    | status    |
+-------+------------+-----------+------------+-----------+
|   146 | Andrew     | Cencini   | Supplier B | Submitted |
|   147 | Andrew     | Cencini   | Supplier G | Submitted |
|   148 | Andrew     | Cencini   | Supplier E | Submitted |
+-------+------------+-----------+------------+-----------+
```

### 9. Using the purchase orders and purchase order details tables, find all the purchase orders that have a total cost over $1000. Display all information.

SQL:
```sql
SELECT * FROM purchase_orders
INNER JOIN purchase_order_details ON purchase_orders.id = purchase_order_details.purchase_order_id
WHERE purchase_order_details.quantity * purchase_order_details.unit_cost > 1000;
```

Output:
```
+-----+-------------+------------+---------------------+---------------------+-----------+---------------+--------------+--------+--------------+----------------+----------------+---------------------------------------+-------------+---------------------+--------------+-----+-------------------+------------+----------+-----------+---------------------+---------------------+--------------+
| id  | supplier_id | created_by | submitted_date      | creation_date       | status_id | expected_date | shipping_fee | taxes  | payment_date | payment_amount | payment_method | notes                                 | approved_by | approved_date       | submitted_by | id  | purchase_order_id | product_id | quantity | unit_cost | date_received       | posted_to_inventory | inventory_id |
+-----+-------------+------------+---------------------+---------------------+-----------+---------------+--------------+--------+--------------+----------------+----------------+---------------------------------------+-------------+---------------------+--------------+-----+-------------------+------------+----------+-----------+---------------------+---------------------+--------------+
|  90 |           1 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 253 |                90 |         43 | 100.0000 |   34.0000 | 2006-01-22 00:00:00 |                   1 |           61 |
|  91 |           3 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 260 |                91 |         66 |  80.0000 |   13.0000 | 2006-01-22 00:00:00 |                   1 |           58 |
|  92 |           2 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 242 |                92 |          6 | 100.0000 |   19.0000 | 2006-01-22 00:00:00 |                   1 |           40 |
|  92 |           2 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 244 |                92 |          8 |  40.0000 |   30.0000 | 2006-01-22 00:00:00 |                   1 |           42 |
|  92 |           2 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 246 |                92 |         17 |  40.0000 |   29.0000 | 2006-01-22 00:00:00 |                   1 |           44 |
|  92 |           2 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 248 |                92 |         20 |  40.0000 |   61.0000 | 2006-01-22 00:00:00 |                   1 |           46 |
|  92 |           2 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 251 |                92 |         40 | 120.0000 |   14.0000 | 2006-01-22 00:00:00 |                   1 |           48 |
|  92 |           2 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 255 |                92 |         51 |  40.0000 |   40.0000 | 2006-01-22 00:00:00 |                   1 |           51 |
|  93 |           5 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 257 |                93 |         56 | 120.0000 |   28.0000 | 2006-01-22 00:00:00 |                   1 |           38 |
|  93 |           5 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 258 |                93 |         57 |  80.0000 |   15.0000 | 2006-01-22 00:00:00 |                   1 |           39 |
|  94 |           6 |          2 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | 2006-01-22 00:00:00 |            2 | 261 |                94 |         72 |  40.0000 |   26.0000 | 2006-01-22 00:00:00 |                   1 |           36 |
|  98 |           2 |          4 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | Purchase generated based on Order #36 |           2 | 2006-01-22 00:00:00 |            4 | 268 |                98 |         41 | 200.0000 |    7.0000 | 2006-01-22 00:00:00 |                   1 |           78 |
|  99 |           1 |          3 | 2006-01-14 00:00:00 | 2006-01-22 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | Purchase generated based on Order #38 |           2 | 2006-01-22 00:00:00 |            3 | 269 |                99 |         43 | 300.0000 |   34.0000 | 2006-01-22 00:00:00 |                   1 |           76 |
| 102 |           1 |          1 | 2006-03-24 00:00:00 | 2006-03-24 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | Purchase generated based on Order #41 |           2 | 2006-04-04 00:00:00 |            1 | 272 |               102 |         43 | 300.0000 |   34.0000 | NULL                |                   0 |         NULL |
| 105 |           5 |          7 | 2006-03-24 00:00:00 | 2006-03-24 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | Check          | Purchase generated based on Order #46 |           2 | 2006-04-04 00:00:00 |            7 | 275 |               105 |         57 | 100.0000 |   15.0000 | 2006-04-05 00:00:00 |                   1 |          100 |
| 106 |           6 |          7 | 2006-03-24 00:00:00 | 2006-03-24 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | Purchase generated based on Order #46 |           2 | 2006-04-04 00:00:00 |            7 | 276 |               106 |         72 |  50.0000 |   26.0000 | 2006-04-05 00:00:00 |                   1 |          113 |
| 107 |           1 |          6 | 2006-03-24 00:00:00 | 2006-03-24 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | Purchase generated based on Order #47 |           2 | 2006-04-04 00:00:00 |            6 | 277 |               107 |         34 | 300.0000 |   10.0000 | 2006-04-05 00:00:00 |                   1 |          107 |
| 110 |           1 |          3 | 2006-03-24 00:00:00 | 2006-03-24 00:00:00 |         2 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | Purchase generated based on Order #49 |           2 | 2006-04-04 00:00:00 |            3 | 280 |               110 |         43 | 250.0000 |   34.0000 | 2006-04-10 00:00:00 |                   1 |          103 |
| 146 |           2 |          2 | 2006-04-26 18:26:37 | 2006-04-26 18:26:37 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | NULL                |            2 | 292 |               146 |         20 |  40.0000 |   60.0000 | NULL                |                   0 |         NULL |
| 146 |           2 |          2 | 2006-04-26 18:26:37 | 2006-04-26 18:26:37 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |           2 | NULL                |            2 | 293 |               146 |         51 |  40.0000 |   39.0000 | NULL                |                   0 |         NULL |
| 147 |           7 |          2 | 2006-04-26 18:33:28 | 2006-04-26 18:33:28 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |        NULL | NULL                |            2 | 294 |               147 |         40 | 120.0000 |   13.0000 | NULL                |                   0 |         NULL |
| 148 |           5 |          2 | 2006-04-26 18:33:52 | 2006-04-26 18:33:52 |         1 | NULL          |       0.0000 | 0.0000 | NULL         |         0.0000 | NULL           | NULL                                  |        NULL | NULL                |            2 | 295 |               148 |         72 |  40.0000 |   26.0000 | NULL                |                   0 |         NULL |
+-----+-------------+------------+---------------------+---------------------+-----------+---------------+--------------+--------+--------------+----------------+----------------+---------------------------------------+-------------+---------------------+--------------+-----+-------------------+------------+----------+-----------+---------------------+---------------------+--------------+
```

### 10. Using a left outer join or  a right outer join and the customers and orders tables, find all the customers that have no orders. Display the company name, the first name and last name, and the business phone number .

SQL:
```sql
SELECT customers.company,
    customers.first_name,
    customers.last_name,
    customers.business_phone
FROM customers
LEFT OUTER JOIN orders ON orders.customer_id = customers.id
WHERE orders.id IS NULL;
```

Output:
```
+-----------+---------------+------------------+----------------+
| company   | first_name    | last_name        | business_phone |
+-----------+---------------+------------------+----------------+
| Company B | Antonio       | Gratacos Solsona | (123)555-0100  |
| Company E | Martin        | O’Donnell        | (123)555-0100  |
| Company M | Andre         | Ludick           | (123)555-0100  |
| Company N | Carlos        | Grilo            | (123)555-0100  |
| Company O | Helena        | Kupkova          | (123)555-0100  |
| Company P | Daniel        | Goldschmidt      | (123)555-0100  |
| Company Q | Jean Philippe | Bagel            | (123)555-0100  |
| Company R | Catherine     | Autier Miconi    | (123)555-0100  |
| Company S | Alexander     | Eggerer          | (123)555-0100  |
| Company T | George        | Li               | (123)555-0100  |
| Company U | Bernard       | Tham             | (123)555-0100  |
| Company V | Luciana       | Ramos            | (123)555-0100  |
| Company W | Michael       | Entin            | (123)555-0100  |
| Company X | Jonas         | Hasselberg       | (123)555-0100  |
+-----------+---------------+------------------+----------------+
```
