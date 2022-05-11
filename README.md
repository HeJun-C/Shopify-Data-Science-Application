# Shopify Data Science Internship Application - Data Challenge

## Question 1:

### a. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.

    We got the AOV of $3145.13 because we mistakenly used the COUNT() function in Excel for the total_items column, which gave us 5000. Given the sum of total orders is $15725640, if we calculate $15725640/5000, we get $3145.13, the wrong number. Instead, if we apply the SUM() function for the total_items column, we get 43936, which is the correct number for total items.

### b. What metric would you report for this dataset?

    For the AOV, we should do the following calculation:

    (sum of order amounts) / (sum of items amounts)

    If we do it in the given Excel sheet, the function to calculate AOV will be:

    SUM(D2:D5001) / SUM(E2:E5001)

### c. What is the value?

    Based on the calculation in 2, the Average Order Value (AOV) is $357.92.


## Question 2: (MySQL Syntax)

### How many orders were shipped by Speedy Express in total?

    Answer:
    54
    Queries:
    SELECT COUNT(OrderID) AS total_SE_orders
    FROM Orders o LEFT JOIN Shippers s
    ON o.ShipperID = s.ShipperID
    WHERE ShipperName = 'Speedy Express';

### What is the last name of the employee with the most orders?

    Answer:
    Last Name: Peacock; Order Numbers: 40
    Queries:
    SELECT e.LastName, COUNT(OrderID) as order_num
    FROM Orders o LEFT JOIN Employees e
    ON o.EmployeeID = e.EmployeeID
    GROUP BY LastName
    ORDER BY COUNT(OrderID) DESC LIMIT 1;

### What product was ordered the most by customers in Germany?

    Answer:
    Product: Boston Crab Meat; Total Ordered Quantity:160

    Queries:
    WITH t1 AS 
    (SELECT o.OrderID, c.country 
     FROM Orders o LEFT JOIN Customers c
     ON o.CustomerID = c.CustomerID)
 
     SELECT p.ProductName, SUM(Quantity) AS total_orders
     FROM 
     (SELECT t1.OrderId, t1.country, od.ProductID, od.Quantity
     FROM t1 LEFT JOIN OrderDetails od 
     ON t1.OrderID = od.OrderID) AS t2 LEFT JOIN Products p
     ON t2.ProductID = p.ProductID
     WHERE t2.country = 'Germany'
     GROUP BY ProductName
     ORDER BY SUM(Quantity) DESC LIMIT 1;


