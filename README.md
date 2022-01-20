# Summer 2022 Data Science Intern Challenge

Question 1
---

**Q**: Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

**A**: AOV of a sneaker = $3145.13 seems unreasonably high for sneakers purchases. Average is easily skewed by very large values; there are several erroneous purchases / outliers in the database:
- There are 17 orders with 2000 items and $704000, they are all from the same shop id (42), user id (607) and even at the same time of the day (4am) -> most likely erroneous.
- Shop (id 78) seems to sell very expensive shoes, each pair is $25725. Although expensive, I don't think the values are erroneous because when a customer (46 different customers in total) purchases multiple pairs, the price also becomes a multiple of $25725. I think this store probably sells limited edition shoes. While this is not erroneous, this store is certainly an outlier.
<p>&nbsp</p>

**Q**: What metric would you report for this dataset?

**A**: In large database, corrupted data and outliers are very common; they could significantly affect the average value. It is essential to clean the data before analyzing it. In such settings, unless all outliers are removed, I believe the median provides a better representation of the general order value.
<p>&nbsp</p>

**Q**: What is its value?

**A**: The median order value is calculated to be $284.
<p>&nbsp</p>


Question 2
---
**Q**: How many orders were shipped by Speedy Express in total?

**A**: 54 orders were shipped by Speedy Express in total.
```SQL
SELECT COUNT(orderID) AS "# of Order Shipped by Speedy Express"
FROM Orders as o
JOIN Shippers as s
ON s.ShipperID = o.ShipperID
Where ShipperName = "Speedy Express"
```
<p>&nbsp</p>

**Q**: What is the last name of the employee with the most orders?

**A**: "Peacock" is the last name of the employee with the most orders, at 40 orders.
```SQL
SELECT e.LastName, COUNT(o.OrderID) as num_orders
FROM Employees as e
JOIN Orders as o
ON e.EmployeeID = o.EmployeeID
GROUP BY e.EmployeeID
ORDER BY num_orders DESC
LIMIT 1
```
<p>&nbsp</p>

**Q**: What product was ordered the most by customers in Germany?

**A**: Boston Crab Meat was ordered the most by customers in Germany, with 160 times in total.
```SQL
SELECT p.ProductName, SUM(od.Quantity) as total_quantity
FROM Products as p
JOIN OrderDetails as od
ON p.ProductID = od.ProductID
JOIN Orders as o
ON od.OrderID = o.OrderID
JOIN Customers as c
ON o.CustomerID = c.CustomerID
WHERE c.Country = "Germany"
GROUP BY p.ProductName
ORDER BY total_quantity DESC
LIMIT 1
```