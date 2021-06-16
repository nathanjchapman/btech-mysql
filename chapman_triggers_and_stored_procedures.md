#  Triggers & Stored Procedures Assignment

For each of the questions in this assignment include the SQL query and the results of the query.

### 1. Create a trigger called `po_date` on the `purchase_order_table` to assign the current date to the `date_received` field in the `purchase_order_details` table when the corresponding row is updated on the `purchase_orders` table. The trigger should be activated after an update.

SQL:
```sql
CREATE TRIGGER po_date AFTER UPDATE ON purchase_orders
    FOR EACH ROW
    UPDATE purchase_order_details SET date_received = CURRENT_TIMESTAMP()
        WHERE purchase_order_id = NEW.id
;

UPDATE purchase_orders SET notes = "THIS WAS UPDATED" WHERE id = 90;
SELECT * FROM purchase_order_details WHERE purchase_order_id = 90;
```

OUTPUT:
```
+-----+-------------------+------------+----------+-----------+---------------------+---------------------+--------------+
| id  | purchase_order_id | product_id | quantity | unit_cost | date_received       | posted_to_inventory | inventory_id |
+-----+-------------------+------------+----------+-----------+---------------------+---------------------+--------------+
| 238 |                90 |          1 |  40.0000 |   14.0000 | 2021-03-25 12:26:23 |                   1 |           59 |
| 250 |                90 |         34 |  60.0000 |   10.0000 | 2021-03-25 12:26:23 |                   1 |           60 |
| 253 |                90 |         43 | 100.0000 |   34.0000 | 2021-03-25 12:26:23 |                   1 |           61 |
| 265 |                90 |         81 | 125.0000 |    2.0000 | 2021-03-25 12:26:23 |                   1 |           62 |
| 281 |                90 |          1 |  40.0000 |   14.0000 | 2021-03-25 12:26:23 |                   0 |         NULL |
+-----+-------------------+------------+----------+-----------+---------------------+---------------------+--------------+
```


### 2. Create a trigger called `update_orders_log` on orders. This trigger will activate after a new record is inserted into the `orders` table. The trigger will insert a new row into the `order_log` table and will record the `employee_id`, `order_id`, and the `current_timestamp`.

SQL:
```sql
CREATE TRIGGER update_order_log AFTER INSERT ON orders
    FOR EACH ROW INSERT INTO order_log (
        employee_id, order_id, last_change
    ) VALUES (
        NEW.employee_id, NEW.id, current_timestamp()
    )
;

-- TRIGGER
INSERT INTO `orders` (`employee_id`, `notes`) VALUES (1, "DUMMY INSERT");

-- PROOF
SELECT id, employee_id, notes FROM orders ORDER BY id DESC LIMIT 1;
SELECT * FROM order_log WHERE employee_id = 1 ORDER BY last_change DESC LIMIT 1;
```

OUTPUT:
```
+----+-------------+--------------+
| id | employee_id | notes        |
+----+-------------+--------------+
| 82 |           1 | DUMMY INSERT |
+----+-------------+--------------+

+-------------+----------+---------------------+
| employee_id | order_id | last_change         |
+-------------+----------+---------------------+
|           1 |       82 | 2021-03-25 12:30:08 |
+-------------+----------+---------------------+
```

### 3. Create a trigger called `update_order_log_2` on orders. This trigger will activate after a record in the `orders` table is updated. The trigger will insert a new row into the `order_log` table and will record the `employee_id`, `order_id`, and the `current_timestamp`.

SQL:
```sql
CREATE TRIGGER update_order_log_2 AFTER UPDATE ON orders
    FOR EACH ROW INSERT INTO order_log (
        employee_id, order_id, last_change
    ) VALUES (
        NEW.employee_id, NEW.id, current_timestamp()
    )
;

-- TRIGGER
UPDATE orders SET notes = "THIS WAS UPDATED" WHERE id = 82;

-- PROOF
SELECT id, employee_id, notes FROM orders ORDER BY id DESC LIMIT 1;
SELECT employee_id, order_id, last_change FROM order_log
    WHERE order_id = 82
    ORDER BY last_change DESC
    LIMIT 1;
```

OUTPUT:
```
+----+-------------+------------------+
| id | employee_id | notes            |
+----+-------------+------------------+
| 82 |           1 | THIS WAS UPDATED |
+----+-------------+------------------+

+-------------+----------+---------------------+
| employee_id | order_id | last_change         |
+-------------+----------+---------------------+
|           1 |       82 | 2021-03-25 12:35:37 |
+-------------+----------+---------------------+
```

### 4. Create a trigger called `invoice_total` on `orders`. This trigger will activate after the `orders` table is updated. The trigger will update the invoice associated with the order and will update the `tax` and `shipping` fields.

SQL:
```sql
CREATE TRIGGER invoice_update AFTER UPDATE ON orders
    FOR EACH ROW UPDATE invoices
        SET tax = NEW.taxes, shipping = NEW.shipping_fee
        WHERE order_id = NEW.id
;

-- TRIGGER
UPDATE orders SET taxes = 2.0000, shipping_fee = 2.0000 WHERE id = 42;

-- PROOF
SELECT * FROM invoices WHERE order_id = 42;
```

OUTPUT:
```
+----+----------+---------------------+----------+--------+----------+------------+
| id | order_id | invoice_date        | due_date | tax    | shipping | amount_due |
+----+----------+---------------------+----------+--------+----------+------------+
| 36 |       42 | 2006-04-04 11:41:14 | NULL     | 2.0000 |   2.0000 |     0.0000 |
+----+----------+---------------------+----------+--------+----------+------------+
```

### 5. Create a trigger called `order_warning`. This trigger will activate before the `orders` table is updated. This trigger will check to see if the order being updated is "closed" (`status_id = 3`). If the order is closed it will display a warning message to the screen that the order has been closed.

SQL:
```sql
DELIMITER //
CREATE TRIGGER order_warning BEFORE UPDATE ON orders
FOR EACH ROW
    BEGIN
        IF (SELECT status_id FROM orders WHERE id = NEW.id) = 3 THEN
            SIGNAL SQLSTATE "45000" SET MESSAGE_TEXT = "Cannot update: order is closed.";
        END IF;
    END
//
DELIMITER ;

UPDATE orders SET taxes = 2.0000, shipping_fee = 2.0000 WHERE id = 78;
```

OUTPUT:
```
ERROR 1644 (45000): Cannot update: order is closed.
```

### 6. Create a function called `total_parts` that will accept two numbers: `quantity` and `price`, and return the product of the two numbers.

SQL:
```sql
CREATE FUNCTION total_parts(quantity INT, price DOUBLE) RETURNS DOUBLE DETERMINISTIC
RETURN quantity * price;

SELECT total_parts(quantity, unit_price) AS total_parts FROM order_details;
```

OUTPUT:
```
+--------------------+
| total_parts        |
+--------------------+
|               1400 |
|                105 |
|                300 |
|                530 |
|                 35 |
|                270 |
|                920 |
|                276 |
|                184 |
|              127.5 |
|               1930 |
|                680 |
|              13800 |
|               1275 |
|                598 |
|              13800 |
|                250 |
|                220 |
|                 92 |
|                 70 |
|              149.5 |
|                450 |
|               1150 |
|              74.75 |
|              482.5 |
|  919.9999999999999 |
|               1950 |
| 1739.9999999999998 |
|               4200 |
|               1000 |
| 229.99999999999997 |
|                200 |
|             533.75 |
|              289.5 |
|                552 |
|              127.5 |
|               1218 |
|                900 |
|               1590 |
|               1560 |
|               2250 |
|                660 |
|                510 |
|                510 |
|               96.5 |
|                230 |
|                736 |
|                800 |
|               52.5 |
|                200 |
|               1392 |
|                500 |
|                120 |
|               3240 |
|                280 |
|                380 |
|                  0 |
|                  0 |
+--------------------+
```

### 7. Create a function called `invoice_total` that will accept four numbers: `quantity`, `price`, `shipping`, and `tax`. The function will multiply the quantity and price then add the shipping and tax and return the result.

SQL:
```sql
CREATE FUNCTION invoice_total(quantity INT, price DOUBLE, shipping DOUBLE, tax DOUBLE) RETURNS DOUBLE DETERMINISTIC
RETURN (quantity * price) + shipping + tax;

-- SHOW INPUT VALUES
SELECT order_id, product_id, quantity, unit_price FROM order_details WHERE order_id = 32;
SELECT order_id, shipping, tax FROM invoices WHERE order_id = 32;

-- PROOF
SELECT order_details.order_id, order_details.product_id, invoice_total(
    order_details.quantity,
    order_details.unit_price,
    invoices.shipping,
    invoices.tax
) AS invoice_total FROM order_details, invoices
WHERE order_details.order_id = invoices.order_id AND order_details.order_id = 32 GROUP BY product_id;

```

OUTPUT:
```
ORDER DETAILS:
+----------+----------+------------+
| order_id | quantity | unit_price |
+----------+----------+------------+
|       32 |  15.0000 |    18.0000 |
|       32 |  20.0000 |    46.0000 |
+----------+----------+------------+

INVOICE:
+----------+----------+--------+
| order_id | shipping | tax    |
+----------+----------+--------+
|       32 |   5.0000 | 0.0000 |
+----------+----------+--------+

PROOF:
+----------+------------+---------------+
| order_id | product_id | invoice_total |
+----------+------------+---------------+
|       32 |          1 |           275 |
|       32 |         43 |           925 |
+----------+------------+---------------+
```

### 8. Create a procedure called `getAllInvoices` that returns all the invoices from the `invoices` table.

SQL:
```sql
DELIMITER //
CREATE PROCEDURE getAllInvoices()
    BEGIN
        SELECT * FROM invoices;
    END
//
DELIMITER ;
```

OUTPUT:
```
+----+----------+---------------------+----------+--------+----------+------------+
| id | order_id | invoice_date        | due_date | tax    | shipping | amount_due |
+----+----------+---------------------+----------+--------+----------+------------+
|  5 |       31 | 2006-03-22 16:08:59 | NULL     | 0.0000 |   5.0000 |   870.0000 |
|  6 |       32 | 2006-03-22 16:10:27 | NULL     | 0.0000 |   5.0000 |     0.0000 |
|  7 |       40 | 2006-03-24 10:41:41 | NULL     | 0.0000 |   0.0000 |     0.0000 |
|  8 |       39 | 2006-03-24 10:55:46 | NULL     | 0.0000 |   0.0000 |     0.0000 |
|  9 |       38 | 2006-03-24 10:56:57 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 10 |       37 | 2006-03-24 10:57:38 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 11 |       36 | 2006-03-24 10:58:40 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 12 |       35 | 2006-03-24 10:59:41 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 13 |       34 | 2006-03-24 11:00:55 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 14 |       33 | 2006-03-24 11:02:02 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 15 |       30 | 2006-03-24 11:03:00 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 16 |       56 | 2006-04-03 13:50:15 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 17 |       55 | 2006-04-04 11:05:04 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 18 |       51 | 2006-04-04 11:06:13 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 19 |       50 | 2006-04-04 11:06:56 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 20 |       48 | 2006-04-04 11:07:37 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 21 |       47 | 2006-04-04 11:08:14 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 22 |       46 | 2006-04-04 11:08:49 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 23 |       45 | 2006-04-04 11:09:24 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 24 |       79 | 2006-04-04 11:35:54 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 25 |       78 | 2006-04-04 11:36:21 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 26 |       77 | 2006-04-04 11:36:47 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 27 |       76 | 2006-04-04 11:37:09 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 28 |       75 | 2006-04-04 11:37:49 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 29 |       74 | 2006-04-04 11:38:11 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 30 |       73 | 2006-04-04 11:38:32 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 31 |       72 | 2006-04-04 11:38:53 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 32 |       71 | 2006-04-04 11:39:29 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 33 |       70 | 2006-04-04 11:39:53 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 34 |       69 | 2006-04-04 11:40:16 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 35 |       67 | 2006-04-04 11:40:38 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 36 |       42 | 2006-04-04 11:41:14 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 37 |       60 | 2006-04-04 11:41:45 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 38 |       63 | 2006-04-04 11:42:26 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 39 |       58 | 2006-04-04 11:43:08 | NULL     | 0.0000 |   0.0000 |     0.0000 |
+----+----------+---------------------+----------+--------+----------+------------+
```

### 9. Create a procedure called `getInvoicesByDate` that accepts a `invoice_year` as a parameter and returns all the invoices created in that year.

SQL:
```sql
DELIMITER //
CREATE PROCEDURE getInvoicesByDate(invoice_year INT)
    BEGIN
        SELECT * FROM invoices WHERE YEAR(invoice_date) = invoice_year;
    END
//
DELIMITER ;

CALL getInvoicesByDate(2006);
```

OUTPUT:
```
+----+----------+---------------------+----------+--------+----------+------------+
| id | order_id | invoice_date        | due_date | tax    | shipping | amount_due |
+----+----------+---------------------+----------+--------+----------+------------+
|  5 |       31 | 2006-03-22 16:08:59 | NULL     | 0.0000 |   5.0000 |   870.0000 |
|  6 |       32 | 2006-03-22 16:10:27 | NULL     | 0.0000 |   5.0000 |     0.0000 |
|  7 |       40 | 2006-03-24 10:41:41 | NULL     | 0.0000 |   0.0000 |     0.0000 |
|  8 |       39 | 2006-03-24 10:55:46 | NULL     | 0.0000 |   0.0000 |     0.0000 |
|  9 |       38 | 2006-03-24 10:56:57 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 10 |       37 | 2006-03-24 10:57:38 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 11 |       36 | 2006-03-24 10:58:40 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 12 |       35 | 2006-03-24 10:59:41 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 13 |       34 | 2006-03-24 11:00:55 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 14 |       33 | 2006-03-24 11:02:02 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 15 |       30 | 2006-03-24 11:03:00 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 16 |       56 | 2006-04-03 13:50:15 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 17 |       55 | 2006-04-04 11:05:04 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 18 |       51 | 2006-04-04 11:06:13 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 19 |       50 | 2006-04-04 11:06:56 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 20 |       48 | 2006-04-04 11:07:37 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 21 |       47 | 2006-04-04 11:08:14 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 22 |       46 | 2006-04-04 11:08:49 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 23 |       45 | 2006-04-04 11:09:24 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 24 |       79 | 2006-04-04 11:35:54 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 25 |       78 | 2006-04-04 11:36:21 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 26 |       77 | 2006-04-04 11:36:47 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 27 |       76 | 2006-04-04 11:37:09 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 28 |       75 | 2006-04-04 11:37:49 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 29 |       74 | 2006-04-04 11:38:11 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 30 |       73 | 2006-04-04 11:38:32 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 31 |       72 | 2006-04-04 11:38:53 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 32 |       71 | 2006-04-04 11:39:29 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 33 |       70 | 2006-04-04 11:39:53 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 34 |       69 | 2006-04-04 11:40:16 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 35 |       67 | 2006-04-04 11:40:38 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 36 |       42 | 2006-04-04 11:41:14 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 37 |       60 | 2006-04-04 11:41:45 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 38 |       63 | 2006-04-04 11:42:26 | NULL     | 0.0000 |   0.0000 |     0.0000 |
| 39 |       58 | 2006-04-04 11:43:08 | NULL     | 0.0000 |   0.0000 |     0.0000 |
+----+----------+---------------------+----------+--------+----------+------------+
```

### 10. Create a procedure called `getOrdersByEmployeeAmount`. This procedure will accept two parameters: `employee_id` and `amount`, and will return all orders created by that employee that are over the amount specified. Remember that amount is the product of quantity and price.

SQL:
```sql
DELIMITER //
CREATE PROCEDURE getOrdersByEmployeeAmount(employee_id INT, amount DOUBLE)
    BEGIN
        SELECT * FROM orders, order_details
            WHERE orders.id = order_details.order_id
            AND (orders.employee_id = employee_id)
            AND (order_details.quantity * order_details.unit_price) > amount;
    END
//
DELIMITER ;

CALL getOrdersByEmployeeAmount(1, 4500);
```

OUTPUT:
```
+----+-------------+-------------+---------------------+---------------------+------------+---------------+----------------+-----------+---------------------+----------------------+---------------------+--------------+--------+--------------+-----------+-------+----------+---------------+-----------+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+
| id | employee_id | customer_id | order_date          | shipped_date        | shipper_id | ship_name     | ship_address   | ship_city | ship_state_province | ship_zip_postal_code | ship_country_region | shipping_fee | taxes  | payment_type | paid_date | notes | tax_rate | tax_status_id | status_id | id | order_id | product_id | quantity | unit_price | discount | status_id | date_allocated | purchase_order_id | inventory_id |
+----+-------------+-------------+---------------------+---------------------+------------+---------------+----------------+-----------+---------------------+----------------------+---------------------+--------------+--------+--------------+-----------+-------+----------+---------------+-----------+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+
| 41 |           1 |           7 | 2006-03-24 00:00:00 | 2016-06-24 11:53:18 |       NULL | Ming-Yang Xie | 123 7th Street | Boise     | ID                  | 99999                | USA                 |       0.0000 | 0.0000 | NULL         | NULL      | NULL  |        0 |          NULL |         0 | 42 |       41 |         43 | 300.0000 |    46.0000 |        0 |         1 | NULL           |               102 |          104 |
+----+-------------+-------------+---------------------+---------------------+------------+---------------+----------------+-----------+---------------------+----------------------+---------------------+--------------+--------+--------------+-----------+-------+----------+---------------+-----------+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+
```
