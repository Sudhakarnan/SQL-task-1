 1. Create the Database

    CREATE DATABASE ecommerce;
    USE ecommerce;

2. Create Tables

i.customers
    CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    address VARCHAR(255)
    );

ii.products
    CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10, 2),
    description TEXT
    );

iii.orders

    CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
    );

3.Insert Sample Data

i.customers
    INSERT INTO customers (name, email, address) VALUES
('Arun Kumar', 'arun.kumar@tnmail.com', '12 Gandhi Street, Chennai, Tamil Nadu'),
('Meena Ramesh', 'meena.r@tnmail.com', '45 MG Road, Madurai, Tamil Nadu'),
('Sundar Raj', 'sundar.raj@tnmail.com', '78 Anna Salai, Coimbatore, Tamil Nadu'),
('Lakshmi Priya', 'lakshmi.p@tnmail.com', '23 VOC Street, Trichy, Tamil Nadu'),
('Karthik Subramanian', 'karthik.s@tnmail.com', '90 RK Nagar, Salem, Tamil Nadu');


ii.products
    INSERT INTO products (name, price, description) VALUES
('Product A', 25.00, 'Basic tool - Tamil Nadu Made'),
('Product B', 35.00, 'Advanced gadget - Local Brand'),
('Product C', 40.00, 'Premium item - Handcrafted');


iii.orders

INSERT INTO orders (customer_id, order_date, total_amount) VALUES
(1, CURDATE() - INTERVAL 10 DAY, 60.00),
(2, CURDATE() - INTERVAL 40 DAY, 100.00),
(1, CURDATE() - INTERVAL 5 DAY, 160.00),
(3, CURDATE() - INTERVAL 20 DAY, 200.00),
(5, CURDATE() - INTERVAL 2 DAY, 250.00);

4.Normalize Orders with Order Items

CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

1. Sample data for order_items

INSERT INTO order_items (order_id, product_id, quantity) VALUES
(1, 1, 2),
(2, 2, 1),
(3, 1, 3),
(3, 3, 1),
(4, 3, 2),
(5, 2, 2); 


5.Queries

a) Customers who placed orders in the last 30 days

SELECT DISTINCT c.*
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;


b) Total amount of all orders placed by each customer

SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.id;

c) Update the price of Product C to 45.00

UPDATE products
SET price = 45.00
WHERE name = 'Product C';


d) Add a new column discount to products table

ALTER TABLE products
ADD COLUMN discount DECIMAL(5,2) DEFAULT 0.00;


e) Top 3 products with highest price

SELECT *
FROM products
ORDER BY price DESC
LIMIT 3;


f) Customers who ordered Product A

SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE p.name = 'Product A';


g) Join orders and customers to retrieve name and order date

SELECT o.id AS order_id, c.name AS customer_name, o.order_date
FROM orders o
JOIN customers c ON o.customer_id = c.id;


h) Orders with total amount greater than 150.00

SELECT *
FROM orders
WHERE total_amount > 150.00;


i) Average total of all orders

SELECT AVG(total_amount) AS average_order_total
FROM orders;





