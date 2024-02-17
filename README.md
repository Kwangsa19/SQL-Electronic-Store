# SQL-Electronic-Store

## Scenario 

An online store sells various products. Customers place orders, each of which can include multiple products. The store wants to analyze its sales data to:

1. Identify the relationship between orders and customers.
2. Understand customer spending since the start of 2021.
3. Determine the top 3 most popular products based on order frequency.
4. Determine the average amount spending for each customer.
5. Understand how many times the customers has made transactions.

## Database and Tables
> Copy and paste this code into your SQL editor.

> This is conducted on MySQL/MariaDB relational database management system. 

```
CREATE DATABASE electronic_store;

-- Create Customers table
CREATE TABLE Customers (
    CustomerID INT,
    CustomerName VARCHAR(255)
);

-- Insert data into Customers
INSERT INTO Customers (CustomerID, CustomerName) VALUES
(1, 'John Doe'),
(2, 'Jane Smith'),
(3, 'Emily Johnson'),
(4, 'Michael Brown'),
(5, 'Linda Davis'),
(6, 'Robert Miller'),
(7, 'Elizabeth Wilson'),
(8, 'David Moore'),
(9, 'Jennifer Taylor'),
(10, 'Charles Anderson');

-- Create Orders table
CREATE TABLE Orders (
    OrderID INT,
    CustomerID INT,
    OrderDate DATE,
    OrderAmount DECIMAL(10, 2)
);

-- Insert data into Orders
INSERT INTO Orders (OrderID, CustomerID, OrderDate, OrderAmount) VALUES
(1, 1, '2021-02-15', 300),
(2, 2, '2021-03-20', 450),
(3, 3, '2022-01-05', 220),
(4, 4, '2022-05-11', 560),
(5, 5, '2022-07-22', 790),
(6, 6, '2021-08-30', 110),
(7, 7, '2022-02-09', 310),
(8, 8, '2021-12-15', 150),
(9, 9, '2022-03-20', 670),
(10, 10, '2022-11-05', 230),
(11, 1, '2022-08-19', 120),
(12, 2, '2022-09-25', 450),
(13, 3, '2022-04-15', 210),
(14, 4, '2021-06-07', 600),
(15, 5, '2021-11-23', 340);

-- Create Products table
CREATE TABLE Products (
    ProductID INT,
    ProductName VARCHAR(255),
    Price DECIMAL(10, 2)
);

-- Insert data into Products
INSERT INTO Products (ProductID, ProductName, Price) VALUES
(1, 'Laptop', 1000),
(2, 'Smartphone', 500),
(3, 'Keyboard', 50),
(4, 'Mouse', 25),
(5, 'Monitor', 300),
(6, 'Tablet', 450),
(7, 'Headphones', 75),
(8, 'Smartwatch', 250),
(9, 'Camera', 400),
(10, 'External Hard Drive', 100);

-- Create OrderDetails table
CREATE TABLE OrderDetails (
    OrderDetailID INT,
    OrderID INT,
    ProductID INT,
    Quantity INT
);

-- Insert data into OrderDetails
INSERT INTO OrderDetails (OrderDetailID, OrderID, ProductID, Quantity) VALUES
(1, 1, 1, 1),
(2, 1, 3, 2),
(3, 2, 2, 1),
(4, 3, 3, 1),
(5, 3, 1, 1),
(6, 4, 4, 1),
(7, 5, 5, 2),
(8, 6, 6, 1),
(9, 7, 7, 3),
(10, 8, 8, 1),
(11, 9, 9, 2),
(12, 10, 10, 1),
(13, 11, 2, 1),
(14, 12, 3, 2),
(15, 13, 4, 1),
(16, 14, 5, 1),
(17, 15, 6, 1),
(18, 11, 7, 2),
(19, 12, 8, 1),
(20, 13, 9, 1),
(21, 14, 10, 1),
(22, 15, 1, 1),
(23, 15, 2, 1),
(24, 15, 3, 1),
(25, 15, 4, 1),
(26, 15, 5, 1);
```

## Solution

1. Identify the relationship between orders and customers.

```
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

2. Understand customer spending since the start of 2021.

```
SELECT Customers.CustomerName, SUM(Orders.OrderAmount) AS TotalSpent
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
WHERE Orders.OrderDate >= '2021-01-01'
GROUP BY Customers.CustomerName
HAVING SUM(Orders.OrderAmount) > 500;
```

3. Determine the top 3 most popular products based on order frequency.

```
SELECT Products.ProductName, COUNT(OrderDetails.ProductID) AS TimesOrdered
FROM Products
INNER JOIN OrderDetails ON Products.ProductID = OrderDetails.ProductID
GROUP BY Products.ProductName
ORDER BY TimesOrdered DESC
LIMIT 3;
```

4. Determine the average amount spending for each customer.

```
SELECT Customers.CustomerName, AVG(Orders.OrderAmount) AS AverageSpent
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
GROUP BY Customers.CustomerName;
```

5. Understand how many times the customers has made transactions.

```
SELECT Customers.CustomerName, COUNT(Orders.OrderID) AS NumberOfOrders
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
GROUP BY Customers.CustomerName
HAVING COUNT(Orders.OrderID) > 1;
```
