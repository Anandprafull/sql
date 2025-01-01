
-- Create the CUSTOMER table
CREATE TABLE CUSTOMER (
    cust_id INT PRIMARY KEY,
    cname VARCHAR(100),
    city VARCHAR(100)
);

-- Create the ORDERS table
CREATE TABLE ORDERS (
    order_id INT PRIMARY KEY,
    odate DATE,
    cust_id INT,
    ord_amt DECIMAL(10, 2),
    FOREIGN KEY (cust_id) REFERENCES CUSTOMER(cust_id)
);

-- Create the ORDER_ITEM table
CREATE TABLE ORDER_ITEM (
    order_id INT,
    item_id INT,
    qty INT,
    PRIMARY KEY (order_id, item_id),
    FOREIGN KEY (order_id) REFERENCES ORDERS(order_id)
);

-- Create the ITEM table
CREATE TABLE ITEM (
    item_id INT PRIMARY KEY,
    unit_price DECIMAL(10, 2)
);

-- Create the SHIPMENT table
CREATE TABLE SHIPMENT (
    order_id INT PRIMARY KEY,
    ship_date DATE,
    FOREIGN KEY (order_id) REFERENCES ORDERS(order_id)
);

-- Insert data into the CUSTOMER table
INSERT INTO CUSTOMER (cust_id, cname, city)
VALUES
(1, 'Alice', 'Bangalore'),
(2, 'Bob', 'Mumbai'),
(3, 'Charlie', 'Bangalore'),
(4, 'Diana', 'Delhi'),
(5, 'Eve', 'Bangalore');

-- Insert data into the ORDERS table
INSERT INTO ORDERS (order_id, odate, cust_id, ord_amt)
VALUES
(101, '2024-12-01', 1, 500),
(102, '2024-12-02', 2, 700),
(103, '2024-12-03', 3, 300),
(104, '2024-12-04', 4, 900),
(105, '2024-12-05', 5, 400);

-- Insert data into the ORDER_ITEM table
INSERT INTO ORDER_ITEM (order_id, item_id, qty)
VALUES
(101, 1, 2),
(101, 2, 3),
(102, 3, 5),
(103, 2, 1),
(105, 1, 10);

-- Insert data into the ITEM table
INSERT INTO ITEM (item_id, unit_price)
VALUES
(1, 50),
(2, 100),
(3, 150),
(4, 200),
(5, 250);

-- Insert data into the SHIPMENT table
INSERT INTO SHIPMENT (order_id, ship_date)
VALUES
(101, '2024-12-06'),
(102, '2024-12-07'),
(103, '2024-12-08'),
(104, '2024-12-09'),
(105, '2024-12-10');

-- Query to find customers with more than 2 orders
SELECT cname
FROM CUSTOMER
JOIN ORDERS ON CUSTOMER.cust_id = ORDERS.cust_id
GROUP BY cname
HAVING COUNT(order_id) > 2;

-- Query to get the total order amount by order date
SELECT odate, SUM(ord_amt) AS total_order_amount
FROM ORDERS
GROUP BY odate;

-- Query to find the customer with the highest order amount
SELECT cname, city, ord_amt
FROM CUSTOMER
JOIN ORDERS ON CUSTOMER.cust_id = ORDERS.cust_id
ORDER BY ord_amt DESC
LIMIT 1;
