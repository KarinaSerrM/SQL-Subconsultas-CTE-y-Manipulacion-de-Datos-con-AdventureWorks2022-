# Consultas SQL Avanzadas con AdventureWorks2022: Subconsultas, CTE y Manipulación de Datos

Este repositorio presenta una colección de ejercicios que demuestran el uso de estructuras y funciones SQL avanzadas sobre la base AdventureWorks2022. Se exploran técnicas como subconsultas, expresiones comunes con CTE, manejo de tablas temporales, funciones de fecha y manipulación de cadenas.

## Objetivo

Fortalecer habilidades en SQL mediante la resolución de consultas complejas que involucran filtrado avanzado, numeración de registros, creación de tablas temporales y modificación de strings.

## Contenido del proyecto

### 1. Subconsulta para productos ordenados
- Uso de `Sales.SalesOrderDetail` como subconsulta
- Orden alfabético por nombre de producto

### 2. Empleados y departamentos actuales
- Subconsulta para obtener el departamento más reciente por empleado
- JOIN correcto entre tablas de empleados y departamentos

### 3. CTE con ROW_NUMBER()
- Aplicación de `ROW_NUMBER()` en CTE para identificar registros más recientes
- Ordenación precisa por fecha de asignación

### 4. Creación de tablas temporales
- Definición de tabla temporal local y global
- Inserción de datos desde `Production.Product`

### 5. Órdenes del año 2011
- Uso de `YEAR(OrderDate)` para filtrar registros
- Consulta estructurada y salida clara

### 6. Manipulación de cadenas con SUBSTRING()
- Eliminación de los primeros caracteres en números de orden y compra
- Alias bien definidos y resultados precisos

## Herramientas utilizadas

- SQL Server Management Studio  
- Base de datos AdventureWorks2022  
- Scripts organizados por ejercicio  

## Conclusiones

- Las subconsultas permiten segmentar y filtrar con precisión
- Las expresiones comunes (CTE) y funciones de ventana enriquecen el análisis de registros
- Las tablas temporales son útiles para almacenar y transformar datos en sesiones controladas
- Las funciones de fecha y cadena amplían la versatilidad de las consultas SQL
