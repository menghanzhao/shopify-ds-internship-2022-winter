# shopify-ds-internship-2022-winter
## question 1:
Shopify has exactly 100 sneaker shops, each od these shops only sells one type of sneaker. They have calculated an AOV of $3145.13, which seems unreasonable given sneakers are relatively affordable items.

### a. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 
the $3145.13 came from taking an average of each order value across the 30 days window:

![image](https://user-images.githubusercontent.com/19338756/133838146-60df8bdf-7a1b-4c16-8cd3-59dbcbb0947c.png)

This number seems quite high for a relatively affordable item - sneakers. My initial thought is there must be some shops with extremely high number of items for each order.
This hypothesis is proven:

![image](https://user-images.githubusercontent.com/19338756/133870622-7bac3d8b-99b2-463f-bc8f-b8c06d2ba7ca.png)

It turns out, while most shope's average number of items per order is between 1 and 2, the outlier (shop 42) has an average of 668 items per order, with a total of 51 orders within these 30 days window, this shop's AOV is $235,101, which becomes the outlier. 

While this AOV value might be useful for this individual shop, it is not useful for looking at the 100 shoe stores as a whole, it only skews the data and creates confusion.

A better way to evaluate this data is by calculating the individual shop's AOV.

### b. What metric would you report for this dataset?
I would report each individual shop's average cost per item. Since each shop only has one type of shoe, this is equivalent to calculating average product price for these 100 shops.

### c. What is its value?
Getting average item cost per shop:

![image](https://user-images.githubusercontent.com/19338756/133870581-d4652982-5ba7-406f-bf56-1b6aa75e72ba.png)

This average cost per item per shop still looks high for affordable items like sneakers, so I did some further investigation on the data to see the cost distribution:

![image](https://user-images.githubusercontent.com/19338756/133870712-54457b25-aa0d-45d4-bef6-ce2471ff4f0f.png)

There seem to be an outlier with >$2500 unit cost for its item. Finding out which store it is:

![image](https://user-images.githubusercontent.com/19338756/133870753-c3303054-ca80-4669-bdf9-438bcef158c2.png)

Store 78 is the outlier, and it has an unreasonably high unit price. I would remove that in further analysis.

After removing store 78, the distribution looks better:

![image](https://user-images.githubusercontent.com/19338756/133870862-0363e17e-89c0-4251-bd33-01fc039534b9.png)

and the new average unit price of shoes is now $152.3.

![image](https://user-images.githubusercontent.com/19338756/133871108-d46095f3-d241-4282-91fa-ae930c995aaa.png)

Since it is against best practice to delete data, an alternative to disgarding outlier is to keep it, but use median instead the average unit price of shoes using median instead of average on original dataset is: $153.

![image](https://user-images.githubusercontent.com/19338756/133871124-fd2b6dd0-780b-4d01-b98d-f4151abc7b3a.png)

## question 2:
### a. Answer is 54

```
SELECT count(ShipperID) 
FROM Orders
WHERE ShipperID == 1;
```

### b. Answer is Peacock

```
SELECT e.LastName
FROM Employees as e
WHERE
(SELECT EmployeeID FROM
(SELECT o.EmployeeID, COUNT(o.EmployeeID)
FROM Orders AS o
GROUP BY o.EmployeeID
ORDER BY COUNT(o.EmployeeID) DESC
LIMIT 1) a) == e.EmployeeID;
```

### c. Answer is Boston Crab Meat

```
Select p.ProductName
		,sum(od.Quantity) as totalQuantity
From Products as p, Orders as o, OrderDetails as od, Customers as c
Where c.Country = 'Germany' and c.CustomerID = o.CustomerID and o.OrderID = od.OrderID and od.ProductID = p.ProductID
Group by p.ProductName
Order by totalQuantity desc
Limit 1;
```
