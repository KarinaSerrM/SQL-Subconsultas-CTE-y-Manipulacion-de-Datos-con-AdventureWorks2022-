--Consulta de los nombres de los productos y los ID’s de productos que fueron ordenados
SELECT p.ProductID,
		p.Name
FROM Production.Product p
WHERE p.ProductID IN
					(SELECT sod.ProductID
					 FROM Sales.SalesOrderDetail sod)
ORDER BY p.Name ASC

--2.	Consulta de listado de BusinessEntityID, empleados y el nombre de su departamento actual. 
SELECT emp.BusinessEntityID,
    (SELECT p.FirstName + ' ' + p.LastName 
     FROM Person.Person p 
     WHERE p.BusinessEntityID = emp.BusinessEntityID) AS [Empleado],
    (SELECT d.Name 
     FROM HumanResources.Department d
     WHERE d.DepartmentID = (
         SELECT TOP 1 edh.DepartmentID
         FROM HumanResources.EmployeeDepartmentHistory edh
         WHERE edh.BusinessEntityID = emp.BusinessEntityID
         ORDER BY edh.EndDate DESC)) AS [Departamento]
FROM HumanResources.Employee emp
ORDER BY [BusinessEntityID];

--3.	Consulta de listado de BusinessEntityID, empleados y el nombre de su departamento actual con CTE (Common Table Expression)
WITH cte_departamento_actual AS (
    SELECT edh.BusinessEntityID,
        edh.DepartmentID,
        RANK() OVER(PARTITION BY edh.BusinessEntityID ORDER BY edh.EndDate DESC) AS rnk
    FROM HumanResources.EmployeeDepartmentHistory edh),
cte_empleados AS (
    SELECT e.BusinessEntityID,
        p.FirstName + ' ' + p.LastName AS NombreEmpleado,
        d.Name AS NombreDepartamento
    FROM HumanResources.Employee e
    JOIN Person.Person p ON e.BusinessEntityID = p.BusinessEntityID
    JOIN cte_departamento_actual cda ON e.BusinessEntityID = cda.BusinessEntityID
    JOIN HumanResources.Department d ON cda.DepartmentID = d.DepartmentID
    WHERE cda.rnk = 1  -- Solo el departamento más reciente (actual)
)
-- Query principal
SELECT BusinessEntityID,
    NombreEmpleado AS [Nombre del Empleado],
    NombreDepartamento AS [Nombre del Departamento]
FROM cte_empleados
ORDER BY BusinessEntityID;

/*4.Creación de una tabla temporal local y otra global para almacenar los productos ordenados. 
4.1 TABLA TEMPORAL LOCAL*/
CREATE TABLE #ProductosOrdenadosLocal(
	IDProducto INT,
	Name VARCHAR(245)
);
--INSERTAMOS DATOS
INSERT INTO #ProductosOrdenadosLocal
SELECT ProductID, Name
FROM Production.Product
ORDER BY Name;
--VISUALIZAMOS LOS DATOS
SELECT *
FROM #ProductosOrdenadosLocal;
--4.1 TABLA TEMPORAL GLOBAL
CREATE TABLE ##ProductosOrdenadosLocal(
	IDProducto INT,
	Name VARCHAR(245)
);
--INSERTAMOS DATOS
INSERT INTO ##ProductosOrdenadosLocal
SELECT ProductID, Name
FROM Production.Product
ORDER BY Name;
--VISUALIZAMOS LOS DATOS
SELECT *
FROM ##ProductosOrdenadosLocal;

--5.Consulta que muestra los números de orden y números de compra para el año 2011.
SELECT SalesOrderNumber,
    PurchaseOrderNumber
FROM Sales.SalesOrderHeader
WHERE YEAR(OrderDate) = 2011;

--6. Consulta de los números de orden y números de compra sin los 2 primeros caracteres (NewSalesOrderNumber y NewPurchaseOrderNumber)
SELECT SUBSTRING(SalesOrderNumber, 3, LEN(SalesOrderNumber)) NewSalesOrderNumber,
    SUBSTRING(PurchaseOrderNumber, 3, LEN(PurchaseOrderNumber)) NewPurchaseOrderNumber
FROM Sales.SalesOrderHeader;
