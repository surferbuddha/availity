# availity

HERE ARE THE SQL ANSWERS:

-- Write a SQL query that will produce a reverse-sorted list (alphabetically by name) of 
-- customers (first and last names) whose last name begins with the letter ‘S.’

SELECT FirstName, LastName
FROM Customer 
WHERE LastName like 'S%'
ORDER by LastName, FirstName Desc;

-- Write a SQL query that will show the total value of all orders each customer 
-- has placed in the past six months. Any customer without any orders should show a $0 value.

SELECT c.CustID, FORMAT(COALESCE(SUM(ol.Cost * ol.Quantity),0), 'C') as Total
FROM Customer c 
LEFT OUTER JOIN Order o 
ON c.CustID = o.CustomerID
INNER JOIN OrderLine ol 
ON ol.OrdID = o.OrderID
WHERE OrderDate > DATEADD(month, -6, @d)
GROUP BY c.CustID

-- Amend the query from the previous question to only show those customers
-- who have a total order value of more than $100 and less than $500 in the past six months.


SELECT c.CustID, FORMAT(COALESCE(SUM(ol.Cost * ol.Quantity),0), 'C') as Total
FROM Customer c 
LEFT OUTER JOIN Order o 
ON c.CustID = o.CustomerID
INNER JOIN OrderLine ol 
ON ol.OrdID = o.OrderID
WHERE OrderDate > DATEADD(month, -6, @d)
GROUP BY c.CustID
HAVING SUM(ol.Cost * ol.Quantity) BETWEEN 100 AND 500
