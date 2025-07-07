# Project1-Online-Retail-Sales-Database-Design

-- Customers
CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    address TEXT,
    phone VARCHAR(15)
);

-- Products
CREATE TABLE Products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    stock_qty INT DEFAULT 0
);

-- Orders
CREATE TABLE Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE NOT NULL,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- Order_Items (Junction table for Orders and Products)
CREATE TABLE Order_Items (
    order_item_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    unit_price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Payments
CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    payment_date DATE NOT NULL,
    amount DECIMAL(10, 2),
    payment_method VARCHAR(50),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);

INSERT INTO Customers (name, email, address, phone) VALUES
('Alice', 'alice@example.com', '123 Street, NY', '9876543210'),
('Bob', 'bob@example.com', '456 Avenue, LA', '9123456789');

INSERT INTO Products (name, description, price, stock_qty) VALUES
('Laptop', 'Gaming laptop', 75000, 10),
('Mouse', 'Wireless mouse', 1500, 50);

INSERT INTO Orders (customer_id, order_date, total_amount) VALUES
(1, '2025-07-01', 76500);

INSERT INTO Order_Items (order_id, product_id, quantity, unit_price) VALUES
(1, 1, 1, 75000),
(1, 2, 1, 1500);

INSERT INTO Payments (order_id, payment_date, amount, payment_method) VALUES
(1, '2025-07-02', 76500, 'Credit Card');


SELECT 
    p.name AS product_name,
    SUM(oi.quantity) AS total_sold,
    SUM(oi.unit_price * oi.quantity) AS total_revenue
FROM 
    Order_Items oi
JOIN 
    Products p ON oi.product_id = p.product_id
GROUP BY 
    p.name;
    

CREATE VIEW CustomerOrderSummary AS
SELECT 
    c.name AS customer_name,
    o.order_id,
    o.order_date,
    o.total_amount,
    pm.payment_method
FROM 
    Orders o
JOIN 
    Customers c ON o.customer_id = c.customer_id
LEFT JOIN 
    Payments pm ON o.order_id = pm.order_id;
    
    Select * from CustomerOrderSummary;
