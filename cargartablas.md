Vamos a utilizar la base de datos de northwind para cargar nuestro data warehouse.
Empecemos por la tabla DimProducts

INSERT INTO [dbo].[DimProducts] ([ProductID], [ProductName], [categoryname],[UnitinStock])
SELECT p.ProductID, p.ProductName, c.CategoryName, p.UnitsInStock
FROM Northwind.dbo.Products AS p
INNER JOIN Northwind.dbo.Categories AS c ON p.CategoryID = c.CategoryID;

Employees

INSERT INTO [dbo].[DimEmployees] ([EmployeeID], [Address], [City],[Region],[Country],FullName  )
SELECT e.[EmployeeID], e.Address, e.City, e.Region, e.Country, CONCAT(e.FirstName, ' ', e.LastName) AS FullName
FROM Northwind.dbo.Employees AS e;

DimCliente
INSERT INTO [dbo].[DimCliente] ([CustomerID], [CompanyName], [Address],[City],[Region],[Country])
select c.CustomerID, c.CompanyName, c.Address, c.City, c.Region, c.Country
from Northwind.dbo.Customers AS c;

DimProveedor
	INSERT INTO [dbo].[DimProveedor] ([SupplierID], [CompanyName], [Address],[City],[Region],[Country])
select s.SupplierID, s.CompanyName, s.Address, s.City, s.Region, s.Country
from Northwind.dbo.Suppliers AS s;

DimTiempo
INSERT INTO [dbo].[DimTiempo] (TimeFecha, TimeAño, TimeTrimestre,TimeMes)
		SELECT o.OrderDate, YEAR(o.OrderDate) AS Anio,
    CASE 
        WHEN MONTH(o.OrderDate) BETWEEN 1 AND 3 THEN 1
        WHEN MONTH(o.OrderDate) BETWEEN 4 AND 6 THEN 2
        WHEN MONTH(o.OrderDate) BETWEEN 7 AND 9 THEN 3
        ELSE 4
    END AS Trimestre,
	MONTH(o.OrderDate) AS Mes
FROM Northwind.dbo.Orders AS o;
