# MySQL Basics 

## `SELECT` — select data from a database.

```sql
SELECT column1, column2, ...
FROM table_name;
```

```sql
 SELECT * FROM Customers;
 SELECT City FROM Customers;
```

### `SELECT DISTINCT` — return only distinct (different) values.

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

By using the `DISTINCT` keyword in a function called `COUNT`, we can return the number of different countries. `SELECT COUNT(DISTINCT Country) FROM Customers;`

## `WHERE` — The `WHERE` clause is used to filter records.

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

```sql
SELECT * FROM Customers
WHERE City = 'Berlin';

SELECT * FROM Customers
WHERE NOT City = 'Berlin'; #Use the NOT keyword to select all records where City is NOT "Berlin".

SELECT * FROM Customers
WHERE CustomerID = 32; #Select all records where the CustomerID column has the value 32.

SELECT * FROM Customers
WHERE City = 'Berlin'
AND PostalCode = 12209;# Select all records where the City column has the value 'Berlin' and the PostalCode column has the value 12209.

SELECT * FROM Customers
WHERE City = 'Berlin'
OR City = 'London'; # Select all records where the City column has the value 'Berlin' or 'London'.
```

| Operator | Description |
| --- | --- |
| = | Equal |
| > | Greater than |
| < | Less than |
| >= | Greater than or equal |
| <= | Less than or equal |
| <> | Not equal. Note: In some versions of SQL this operator may be written as != |
| BETWEEN | Between a certain range |
| LIKE | Search for a pattern |
| IN | To specify multiple possible values for a column |

- **`ORDER BY`**
    
    
    ```sql
    SELECT column1, column2, ...
    FROM table_name
    ORDER BY column1, column2, ... ASC|DESC;
    ```
    
    - The `ORDER BY` keyword sorts the records in ascending order by default. To sort the records in descending order, use the `DESC` keyword.
    - For string values the `ORDER BY` keyword will order alphabetically:
    
    ```sql
    SELECT * FROM Customers
    ORDER BY City; #Select all records from the Customers table, sort the result alphabetically by the column City.
    
    SELECT * FROM Customers
    ORDER BY City DESC; # Select all records from the Customers table, sort the result reversed alphabetically by the column City.
    
    SELECT * FROM Customers
    ORDER BY Country, City; 
    # Select all records from the Customers table, sort the result alphabetically, first by the column Country, then, by the column City.
    # This means that it orders by Country, but if some rows have the same Country, it orders them by City:
    
    SELECT * FROM Customers
    ORDER BY Country ASC, CustomerName DESC;
    # selects all customers from the "Customers" table, sorted ascending by the "Country" and descending by the "CustomerName" column
    ```
    
- `AND` & `Or`
    - The `AND` operator displays a record if *all* the conditions are TRUE.
    - The `OR` operator displays a record if *any* of the conditions are TRUE.
    
    ```sql
    SELECT * FROM Customers
    WHERE Country = 'Spain' AND (CustomerName LIKE 'G%' OR CustomerName LIKE 'R%');
    # Select all Spanish customers that starts with either "G" or "R"
    ```
    
- `NOT` — used in combination with other operators to give the opposite result, also called the negative result.
    
    ```sql
    SELECT * FROM Customers
    WHERE NOT City = 'Berlin'; #Use the NOT keyword to select all records where City is NOT "Berlin".
    
    SELECT * FROM Customers
    WHERE CustomerName NOT LIKE 'A%';
    # Select customers that does not start with the letter 'A':
    
    SELECT * FROM Customers
    WHERE CustomerID NOT BETWEEN 10 AND 60;
    # Select customers with a customerID not between 10 and 60:
    
    SELECT * FROM Customers
    WHERE City NOT IN ('Paris', 'London');
    # Select customers that are not from Paris or London:
    
    SELECT * FROM Customers
    WHERE NOT CustomerID > 50;
    # Select customers with a CustomerId not greater than 50:
    ```
    
- `NULL` Values
    
    IS NULL Syntax — `SELECT *column_names*FROM *table_name*WHERE *column_name* IS NULL;` — used to test for empty values (NULL values).
    
    IS NOT NULL Syntax — `SELECT *column_names*FROM *table_name*WHERE *column_name* IS NOT NULL;` — used to test for non-empty values (NOT NULL values).
    
- `INSERT INTO` — insert new records in a table.
    
    1. Specify both the column names and the values to be inserted:
    
    `INSERT INTO *table_name* (*column1*, *column2*, *column3*, ...) VALUES (*value1*, *value2*, *value3*, ...);`
    
    2. If you are adding values for all the columns of the table, you do not need to specify the column names in the SQL query. However, make sure the order of the values is in the same order as the columns in the table. Here, the `INSERT INTO` syntax would be as follows:
    
    `INSERT INTO *table_name* VALUES (*value1*, *value2*, *value3*, ...);`
    
    ```sql
    INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
    VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
    
    INSERT INTO Customers (CustomerName, City, Country)
    VALUES ('Cardinal', 'Stavanger', 'Norway');
    # Insert Data Only in Specified Columns
    
    INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
    VALUES
    ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway'),
    ('Greasy Burger', 'Per Olsen', 'Gateveien 15', 'Sandnes', '4306', 'Norway'),
    ('Tasty Tee', 'Finn Egan', 'Streetroad 19B', 'Liverpool', 'L1 0AA', 'UK');
    # Make sure you separate each set of values with a comma ,.
    ```
    
- `UPDATE` — used to modify the existing records in a table.
    - **UPDATE Table**
        
        ```sql
        # updates the first customer (CustomerID = 1) with a new contact person *and* a new city.
        UPDATE Customers
        SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
        WHERE CustomerID = 1;
        ```
        
    - **UPDATE Multiple Records**
        
        ```sql
        # update the ContactName to "Juan" for all records where country is "Mexico":
        UPDATE Customers
        SET ContactName='Juan'
        WHERE Country='Mexico';
        ```
        
    - Without WHERE statement
        
        ```sql
        # If you omit the WHERE clause, ALL ContactName are updated to be Juan 
        UPDATE Customers
        SET ContactName='Juan';
        ```
        
- `DELETE` — delete existing records in a table.
    
    ```sql
    # deletes the customer "Alfreds Futterkiste" from the "Customers" table:
    DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
    
    # deletes all rows in the "Customers" table, without deleting the table -- delete records 
    DELETE FROM Customers;
    
    # delete the table completely
    DROP TABLE Customers;
    ```
    
- `SELECT TOP, LIMIT, FETCH FIRST or ROWNUM`
    - MySQL — `LIMIT`
        
        ```sql
        # Select only the first 3 records of the Customers table:
        SELECT * FROM Customers
        LIMIT 3;
        
        # selects the first three records from the "Customers" table, where the country is "Germany" (for SQL Server/MS Access):
        SELECT * FROM Customers
        WHERE Country='Germany'
        LIMIT 3;
        
        # Add the ORDER BY keyword when you want to sort the result, and return the first 3 records of the sorted result.
        SELECT * FROM Customers
        ORDER BY CustomerName DESC
        LIMIT 3;
        ```
        
    - SQL Server/MS Access — `SELECT TOP`
        
        ```sql
        # Select only the first 3 records of the Customers table:
        SELECT TOP 3 * FROM Customers;
        
        # selects the first 50% of the records from the "Customers" table (for SQL Server/MS Access):
        SELECT TOP 50 PERCENT * FROM Customers;
        
        # selects the first three records from the "Customers" table, where the country is "Germany" (for SQL Server/MS Access):
        SELECT TOP 3 * FROM Customers
        WHERE Country='Germany';
        
        # Add the ORDER BY keyword when you want to sort the result, and return the first 3 records of the sorted result.
        SELECT TOP 3 * FROM Customers
        ORDER BY CustomerName DESC;
        ```
        
    - Oracle — `FETCH FIRST … ROWS`
        
        ```sql
        # Select only the first 3 records of the Customers table:
        SELECT * FROM Customers
        FETCH FIRST 3 ROWS ONLY;
        
        # selects the first 50% of the records from the "Customers" table (for SQL Server/MS Access):
        SELECT * FROM Customers
        FETCH FIRST 50 PERCENT ROWS ONLY;
        
        # selects the first three records from the "Customers" table, where the country is "Germany" (for SQL Server/MS Access):
        SELECT * FROM Customers
        WHERE Country='Germany'
        FETCH FIRST 3 ROWS ONLY;
        
        # Add the ORDER BY keyword when you want to sort the result, and return the first 3 records of the sorted result.
        SELECT * FROM Customers
        ORDER BY CustomerName DESC
        FETCH FIRST 3 ROWS ONLY;
        ```
        
- MIN() and MAX()
    - The `MIN()` function returns the smallest value of the selected column.
        
        ```sql
        SELECT MIN(Price)
        FROM Products;
        ```
        
    - The `MAX()` function returns the largest value of the selected column.
        
        ```sql
        SELECT MAX(Price)
        FROM Products;
        ```
        
    
    **Set Column Name (Alias)**
    
    When you use `MIN()` or `MAX()`, the returned column will be named `MIN(*field*)` or `MAX(*field*)` by default. To give the column a new name, use the `AS` keyword:
    
    ```sql
    SELECT MIN(Price) AS SmallestPrice
    FROM Products;
    ```
    

## `COUNT()`

- returns the number of rows that match a specified criterion.
    - Find the total number of products in the `Products` table:
        
        ```sql
        SELECT COUNT(*)FROM Products;
        ```
        
    - Find the number of products where `Price` is higher than 20:
        
        ```sql
        SELECT COUNT(ProductID)FROM Products 
        WHERE Price > 20;
        ```
        
    - Find the number of products where the `ProductName` is not null: If you specify a column instead of `(*)`, NULL values will not be counted.
        
        ```sql
        SELECT COUNT(ProductName)FROM Products;
        ```
        
    - You can ignore duplicates by using the `DISTINCT` keyword in the `COUNT` function. If `DISTINCT` is specified, rows with the same value for the specified column will be counted as one.
        
        ```sql
        # How many different prices are there in the Products table:
        
        SELECT COUNT(DISTINCT Price)
        FROM Products;
        ```
        
    - Give the counted column a name by using the `AS` keyword.
        
        ```sql
        SELECT COUNT(*) AS [number of records]
        FROM Products;
        ```
        
- `SUM()` — returns the total sum of a numeric column.
    
    ```sql
    # Return the sum of all Quantity fields in the OrderDetails table:
    
    SELECT SUM(Quantity)
    FROM OrderDetails;
    
    # Return the number of orders made for the product with ProductID 11:
    
    SELECT SUM(Quantity)
    FROM OrderDetails
    WHERE ProductId = 11;
    
    # Name the column "total":
    
    SELECT SUM(Quantity) AS total
    FROM OrderDetails;
    
    # Use an expression inside the SUM() function:
    
    SELECT SUM(Quantity * 10)
    FROM OrderDetails;
    
    # Join OrderDetails with Products, and use SUM() to find the total amount:
    
    SELECT SUM(Price * Quantity)
    FROM OrderDetails
    LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID;
    ```
    
- `AVG()`  returns the average value of a numeric column.
    
    ```sql
    # Find the average price of all products:
    
    SELECT AVG(Price)
    FROM Products;
    
    # Return the average price of products in category 1:
    
    SELECT AVG(Price)
    FROM Products
    WHERE CategoryID = 1;
    
    # Name the column "average price":
    
    SELECT AVG(Price) AS [average price]
    FROM Products;
    
    # Return all products with a higher price than the average price:
    
    SELECT * FROM Products
    WHERE price > (SELECT AVG(price) FROM Products);
    ```
    
- **`LIKE` —** The `LIKE` operator is used in a `WHERE` clause to search for a specified pattern in a column.
    
    There are two wildcards often used in conjunction with the `LIKE` operator:
    
    - The `%` wildcard represents any number of characters, even zero characters.
        
        ```sql
        # Select all customers that start with the letter "a":
        SELECT * FROM Customers
        WHERE CustomerName LIKE 'a%';
        ```
        
        | Return all customers that starts with 'a' or starts with 'b': | SELECT * FROM Customers
        WHERE CustomerName LIKE 'a%' OR CustomerName LIKE 'b%'; |
        | --- | --- |
        | Ends With | SELECT * FROM Customers
        WHERE CustomerName LIKE '%a'; |
        | starts with "b" and ends with "s": | SELECT * FROM Customers
        WHERE CustomerName LIKE 'b%s'; |
        | Return all customers that contains the phrase 'or’ | SELECT * FROM Customers
        WHERE CustomerName LIKE '%or%'; |
        | Return all customers that starts with "a" and are at least 3 characters in length: | SELECT * FROM Customers
        WHERE CustomerName LIKE 'a__%'; |
        | NOT LIKE  | SELECT * FROM Customers
        WHERE City NOT LIKE 'a%'; |
    - The underscore sign `_` represents one, single character. It can be any character or number, but each `_` represents one, and only one, character.
        
        ```sql
        # Return all customers from a city that starts with 'L' followed by one wildcard character, 
        # then 'nd' and then two wildcard characters:
        SELECT * FROM Customers
        WHERE city LIKE 'L_nd__';
        ```
        
- **Wildcards**
    
    
    *Not supported in PostgreSQL and MySQL databases.
    
    ** Supported only in Oracle databases.
    
    | Symbol | Description |
    | --- | --- |
    | % | Represents zero or more characters |
    | _ | Represents a single character |
    | [] | Represents any single character within the brackets * |
    | ^ | Represents any character not in the brackets * |
    | - | Represents any single character within the specified range * |
    | {} | Represents any escaped character ** |
    
    ```sql
    #Return all customers starting with either "b", "s", or "p":
    
    SELECT * FROM Customers
    WHERE CustomerName LIKE '[bsp]%';
    
    # Return all customers starting with "a", "b", "c", "d", "e" or "f":
    
    SELECT * FROM Customers
    WHERE CustomerName LIKE '[a-f]%';
    
    # Return all customers that have "r" in the second position:
    
    SELECT * FROM Customers
    WHERE CustomerName LIKE '_r%';
    ```
    
- The `IN` operator allows you to specify multiple values in a `WHERE` clause.
    
    ```sql
    # Return all customers from 'Germany', 'France', or 'UK'
    
    SELECT * FROM Customers
    WHERE Country IN ('Germany', 'France', 'UK');
    
    # Return all customers that are NOT from 'Germany', 'France', or 'UK':
    
    SELECT * FROM Customers
    WHERE Country NOT IN ('Germany', 'France', 'UK');
    
    # Return all customers that have an order in the Orders table:
    
    SELECT * FROM Customers
    WHERE CustomerID IN (SELECT CustomerID FROM Orders);
    ```
    
- The `BETWEEN` operator selects values within a given range. The values can be numbers, text, or dates.
    
    ```sql
    # Selects all products with a price between 10 and 20:
    
    SELECT * FROM Products
    WHERE Price BETWEEN 10 AND 20;
    
    # To display the products outside the range of the previous example, use NOT BETWEEN:
    
    SELECT * FROM Products
    WHERE Price NOT BETWEEN 10 AND 20;
    
    # selects all products with a price between 10 and 20. In addition, the CategoryID must be either 1,2, or 3:
    
    SELECT * FROM Products
    WHERE Price BETWEEN 10 AND 20
    AND CategoryID IN (1,2,3);
    
    # selects all products with a ProductName alphabetically between Carnarvon Tigers and Mozzarella di Giovanni:
    
    SELECT * FROM Products
    WHERE ProductName BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni'
    ORDER BY ProductName;
    
    # selects all orders with an OrderDate between '01-July-1996' and '31-July-1996':
    
    SELECT * FROM Orders
    WHERE OrderDate BETWEEN #07/01/1996# AND #07/31/1996#;
    
    # or 
    
    SELECT * FROM Orders
    WHERE OrderDate BETWEEN '1996-07-01' AND '1996-07-31';
    ```
    
- **SQL Aliases**
    - SQL aliases are used to give a table, or a column in a table, a temporary name.
        - A column
            
            ```sql
            SELECT CustomerID AS ID
            FROM Customers;
            
            # you can skip the AS keyword and get the same result:
            SELECT CustomerID ID
            FROM Customers;
            
            # 2 columns 
            SELECT CustomerID AS ID, CustomerName AS Customer
            FROM Customers;
            ```
            
        - **Concatenate Columns**
            
            ```sql
            # The following SQL statement creates an alias named "Address" that combine four columns (Address, PostalCode, City and Country):
            SELECT CustomerName, CONCAT(Address,', ',PostalCode,', ',City,', ',Country) AS Address
            FROM Customers;
            ```
            
            | CustomerName | Address |
            | --- | --- |
            | Alfreds Futterkiste | Obere Str. 57, 12209, Berlin, Germany |
            | Ana Trujillo Emparedados y helados | Avda. de la Constitución 2222, 05021, México D.F., Mexico |
            | Antonio Moreno Taquería | Mataderos 2312, 05023, México D.F., Mexico |
            | Around the Horn | 120 Hanover Sq., WA1 1DP, London, UK |
        - Table
            
            ```sql
            # with aliases:
            SELECT o.OrderID, o.OrderDate, c.CustomerName
            FROM Customers AS c, Orders AS o
            WHERE c.CustomerName='Around the Horn' AND c.CustomerID=o.CustomerID;
            # without aliases:
            SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
            FROM Customers, Orders
            WHERE Customers.CustomerName='Around the Horn' AND Customers.CustomerID=Orders.CustomerID;
            ```
            
            The  SQL statement selects all the orders from the customer with CustomerID=4 (Around the Horn). We use the "Customers" and "Orders" tables, and give them the table aliases of "c" and "o" respectively (Here we use aliases to make the SQL shorter):
            
            | OrderID | OrderDate | CustomerName |
            | --- | --- | --- |
            | 10355 | 1996-11-15 | Around the Horn |
            | 10383 | 1996-12-16 | Around the Horn |
    - Aliases are often used to make column names more readable.
        
        ```sql
        # If you want your alias to contain one or more spaces, like "My Great Products"
        SELECT ProductName AS [My Great Products]
        FROM Products;
        
        SELECT ProductName AS "My Great Products"
        FROM Products;
        ```
        
    - An alias only exists for the duration of that query.
    - An alias is created with the `AS` keyword.

## Joins

- `(INNER) JOIN`: Returns records that have **matching values in both tables**. Same as `JOIN`
    
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/af501beb-2182-4e4f-b48b-919becc4ec2f/f3d331a4-83c9-4a3b-be76-94a10fb7280d/Untitled.png)
    
    ```sql
    SELECT 
    Products.ProductID, 
    Products.ProductName, 
    Categories.CategoryName
    FROM Products
    INNER JOIN Categories 
    ON Products.CategoryID = Categories.CategoryID;
    ```
    
    ```sql
    SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
    FROM ((Orders
    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
    INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID);
    ```
    
    ```sql
    SELECT column_name(s)
    FROM table1
    INNER JOIN table2
    ON table1.column_name = table2.column_name;
    ```
    
    | ProductID | ProductName | CategoryName |
    | --- | --- | --- |
    | 1 | Chais | Beverages |
    | 2 | Chang | Beverages |
    | 3 | Aniseed Syrup | Condiments |
    
    | OrderID | CustomerName | ShipperName |
    | --- | --- | --- |
    | 10248 | Wilman Kala | Federal Shipping |
    | 10249 | Tradição Hipermercados | Speedy Express |
- `LEFT (OUTER) JOIN`: Returns all records from the left table, and the matched records from the right table
    
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/af501beb-2182-4e4f-b48b-919becc4ec2f/24d7ddec-e2c0-4922-a077-8965e34f5f85/Untitled.png)
    
    ```sql
    SELECT column_name(s)
    FROM table1
    LEFT JOIN table2
    ON table1.column_name = table2.column_name;
    ```
    
    ```sql
    SELECT Customers.CustomerName, Orders.OrderID
    FROM Customers
    LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
    ORDER BY Customers.CustomerName;
    ```
    
    Left table: Customers
    
    Right Table: Orders
    
    The `LEFT JOIN` keyword returns all records from the left table (Customers), even if there are no matches in the right table (Orders).
    
    | CustomerName | OrderID |
    | --- | --- |
    | Alfreds Futterkiste | null |
    | Ana Trujillo Emparedados y helados | 10308 |
    | Antonio Moreno Taquería | 10365 |
- `RIGHT (OUTER) JOIN`: Returns all records from the right table, and the matched records from the left table
    
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/af501beb-2182-4e4f-b48b-919becc4ec2f/9ad7e73f-cb49-428b-b369-a54087e6c836/Untitled.png)
    
    ```sql
    SELECT column_name(s)
    FROM table1
    RIGHT JOIN table2
    ON table1.column_name = table2.column_name;
    ```
    
    ```sql
    SELECT Orders.OrderID, Employees.LastName, Employees.FirstName
    FROM Orders
    RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
    ORDER BY Orders.OrderID;
    ```
    
    The `RIGHT JOIN` keyword returns all records from the right table (Employees), even if there are no matches in the left table (Orders).
    
    | OrderID | LastName | FirstName |
    | --- | --- | --- |
    | null | West | Adam |
    | 10248 | Buchanan | Steven |
- `FULL (OUTER) JOIN`: Returns all records when there is a match in either left or right table. `FULL OUTER JOIN` and `FULL JOIN` are the same.
    
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/af501beb-2182-4e4f-b48b-919becc4ec2f/56e7dca0-e621-41ae-8c09-394a299a5605/Untitled.png)
    
    ```sql
    SELECT column_name(s)
    FROM table1
    FULL OUTER JOIN table2
    ON table1.column_name = table2.column_name
    WHERE condition;
    ```
    
    ```sql
    SELECT Customers.CustomerName, Orders.OrderID
    FROM Customers
    FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
    ORDER BY Customers.CustomerName;
    ```
    
    | CustomerName | OrderID |
    | --- | --- |
    | Null | 10309 |
    | Null | 10310 |
    | Alfreds Futterkiste | Null |
- **Self Join**
    
    ```sql
    # matches customers that are from the same city:
    SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
    FROM Customers A, Customers B
    WHERE A.CustomerID <> B.CustomerID
    AND A.City = B.City 
    ORDER BY A.City;
    ```
    
    | CustomerName1 | CustomerName2 | City |
    | --- | --- | --- |
    | Cactus Comidas para llevar | Océano Atlántico Ltda. | Buenos Aires |
    | Cactus Comidas para llevar | Rancho grande | Buenos Aires |
    | Océano Atlántico Ltda. | Cactus Comidas para llevar | Buenos Aires |
    | Océano Atlántico Ltda. | Rancho grande | Buenos Aires |
    | Rancho grande | Cactus Comidas para llevar | Buenos Aires |
    | Rancho grande | Océano Atlántico Ltda. | Buenos Aires |
- **UNION —** The `UNION` operator is used to combine the result-set of two or more `SELECT` statements. The `UNION` operator selects only distinct values by default. To allow duplicate values, use `UNION ALL`
    
    
    ```sql
    SELECT column_name(s) FROM table1
    UNION
    SELECT column_name(s) FROM table2;
    ```
    
    Table: employees
    
    | id | name | department |
    | --- | --- | --- |
    | 1 | John | HR |
    | 2 | Emily | Marketing |
    | 3 | Michael | Sales |
    
    Table: contractors
    
    | id | name | department |
    | --- | --- | --- |
    | 1 | Sarah | IT |
    | 2 | Emily | Marketing |
    | 3 | David | Sales |
    
    ```sql
    SELECT name, department FROM employees
    UNION
    SELECT name, department FROM contractors;
    ```
    
    | name | department |
    | --- | --- |
    | John | HR |
    | Emily | Marketing |
    | Michael | Sales |
    | Sarah | IT |
    | David | Sales |
    
    ```sql
    SELECT column_name(s) FROM table1
    UNION ALL
    SELECT column_name(s) FROM table2;
    ```
    
    Table: employees
    
    | id | name | department |
    | --- | --- | --- |
    | 1 | John | HR |
    | 2 | Emily | Marketing |
    | 3 | Michael | Sales |
    
    Table: contractors
    
    | id | name | department |
    | --- | --- | --- |
    | 1 | Sarah | IT |
    | 2 | Emily | Marketing |
    | 3 | David | Sales |
    | 4 | John | HR |
    
    ```sql
    SELECT name, department FROM employees
    UNION ALL
    SELECT name, department FROM contractors;
    ```
    
    The result of this query would be:
    
    | name | department |
    | --- | --- |
    | John | HR |
    | Emily | Marketing |
    | Michael | Sales |
    | Sarah | IT |
    | Emily | Marketing |
    | David | Sales |
    | John | HR |
    
    **SQL UNION With WHERE**
    
    Table: employees
    
    | id | name | department | salary |
    | --- | --- | --- | --- |
    | 1 | John | HR | 50000 |
    | 2 | Emily | Marketing | 55000 |
    | 3 | Michael | Sales | 60000 |
    | 4 | Anna | HR | 48000 |
    
    ```sql
    SELECT name, department, salary FROM employees
    WHERE salary > 55000
    
    UNION
    
    SELECT name, department, salary FROM contractors
    WHERE salary > 55000;
    ```
    
    Table: contractors
    
    | id | name | department | salary |
    | --- | --- | --- | --- |
    | 1 | Sarah | IT | 60000 |
    | 2 | Emily | Marketing | 52000 |
    | 3 | David | Sales | 58000 |
    | 4 | John | HR | 49000 |
    
    The result dataset would be:
    
    | name | department | salary |
    | --- | --- | --- |
    | Michael | Sales | 60000 |
    | Sarah | IT | 60000 |
    | David | Sales | 58000 |
    
    **UNION AS Type**
    
    ```sql
    SELECT 'Customer' AS Type, ContactName, City, Country
    FROM Customers
    UNION
    SELECT 'Supplier', ContactName, City, Country
    FROM Suppliers;
    ```
    
    **`SELECT 'Customer' AS Type, ContactName, City, Country FROM Customers`**: This part of the query selects data from the **`Customers`** table. It retrieves the columns **`ContactName`**, **`City`**, and **`Country`** from the **`Customers`** table and adds a new column **`Type`** with the value 'Customer' for each row.
    
    | Type | ContactName | City | Country |
    | --- | --- | --- | --- |
    | Customer | Alejandra Camino | Madrid | Spain |
    | Customer | Alexander Feuer | Leipzig | Germany |
    | Customer | Ana Trujillo | México D.F. | Mexico |

## **SQL GROUP BY Statement**

The `GROUP BY` statement groups rows that have the same values into summary rows, like "find the number of customers in each country".

- `COUNT`
    
    
    ```sql
    SELECT COUNT(CustomerID), Country
    FROM Customers
    GROUP BY Country
    ORDER BY COUNT(CustomerID) DESC;
    ```
    
    | CustomerID | Country |
    | --- | --- |
    | 1 | USA |
    | 2 | USA |
    | 3 | Canada |
    | 4 | UK |
    | 5 | Canada |
    | 6 | USA |
    
    | COUNT(CustomerID) | Country |
    | --- | --- |
    | 3 | USA |
    | 2 | Canada |
    | 1 | UK |
    
- More…
    
    
    | ProductID | Category | Price |
    | --- | --- | --- |
    | 1 | Electronics | 500 |
    | 2 | Clothing | 75 |
    | 3 | Electronics | 200 |
    | 4 | Clothing | 120 |
    | 5 | Electronics | 350 |
    | 6 | Clothing | 90 |
    1. `SUM` — ****Total Sales Amount for Each Category****
        
        
        ```sql
        SELECT Category, SUM(Price) AS TotalSales
        FROM Sales
        GROUP BY Category;
        ```
        
        | Category | TotalSales |
        | --- | --- |
        | Electronics | 1050 |
        | Clothing | 285 |
    2. `COUNT` — ****Count of Products in Each Category****
        
        
        ```sql
        SELECT Category, COUNT(*) AS TotalProducts
        FROM Products
        GROUP BY Category;
        ```
        
        | Category | TotalProducts |
        | --- | --- |
        | Electronics | 3 |
        | Clothing | 3 |
    3. `MAX` — ****Maximum Price in Each Category****
        
        
        ```sql
        SELECT Category, MAX(Price) AS MaxPrice
        FROM Products
        GROUP BY Category;
        ```
        
        | Category | MaxPrice |
        | --- | --- |
        | Electronics | 500 |
        | Clothing | 120 |
    4. `AVG` — ****Average Price of Products in Each Category****
        
        
        ```sql
        SELECT Category, AVG(Price) AS AvgPrice
        FROM Products
        GROUP BY Category;
        ```
        
        | Category | AvgPrice |
        | --- | --- |
        | Electronics | 350.00 |
        | Clothing | 95.00 |
- **`HAVING` Clause —** The `HAVING` clause was added to SQL because the `WHERE` keyword cannot be used with aggregate functions.
    1. 
        
        
        ```sql
        SELECT COUNT(CustomerID), Country
        FROM Customers
        GROUP BY Country
        HAVING COUNT(CustomerID) > 2;
        ```
        
        | CustomerID | Country |
        | --- | --- |
        | 1 | USA |
        | 2 | USA |
        | 3 | Canada |
        | 4 | Canada |
        | 5 | Canada |
        | 6 | USA |
        | 7 | Germany |
        | 8 | Germany |
        | 9 | France |
        | 10 | France |
        | 11 | France |
        | 12 | France |
        
        1. **`SELECT COUNT(CustomerID), Country**:` This part selects two things:
            - The count of CustomerIDs (number of customers) for each country.
            - The country name.
        2. **`FROM Customers**:` It specifies that the data is being retrieved from a table named "Customers."
        3. **`GROUP BY Country**:` This clause groups the data by the "Country" column. It means that the count will be calculated for each unique country in the dataset.
        4. **``HAVING COUNT(CustomerID) > 2**:` This filters the grouped results. It ensures that only groups (countries) with a count (number of customers) greater than 2 will be included in the final result.
        
        | COUNT(CustomerID) | Country |
        | --- | --- |
        | 3 | Canada |
        | 4 | France |
        | 3 | USA |
        
    2. 
        
        ```sql
        SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
        FROM (Orders
        INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID)
        GROUP BY LastName
        HAVING COUNT(Orders.OrderID) > 10;
        ```
        
        Employees table:
        
        | EmployeeID | LastName |
        | --- | --- |
        | 1 | Smith |
        | 2 | Johnson |
        | 3 | Williams |
        
        Orders table:
        
        | OrderID | EmployeeID |
        | --- | --- |
        | 101 | 1 |
        | 102 | 1 |
        | 103 | 2 |
        | 104 | 2 |
        | 105 | 2 |
        | 106 | 3 |
        | 107 | 3 |
        
        1. **`SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders:`** This part of the query selects the last names of employees from the Employees table and counts the number of orders they've handled from the Orders table. It uses an alias **`NumberOfOrders`** to represent the count of orders.
        2. **`FROM (Orders INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID):`** Here, it specifies the tables involved in the query - Orders and Employees. It performs an INNER JOIN to connect the two tables using the common field **`EmployeeID`**.
        3. **`GROUP BY LastName:`** This clause groups the results by the last name of the employees. It means that the subsequent operations are performed on each unique last name in the result set.
        4. **`HAVING COUNT(Orders.OrderID) > 10:`** This clause filters the grouped results. It only includes the groups (employees) where the count of orders (NumberOfOrders) is greater than 10. This ensures that only employees who have handled more than 10 orders will be displayed in the final result.
        
        | LastName | NumberOfOrders |
        | --- | --- |
        | Smith | 2 |
        | Johnson | 3 |
    3. 
        
        ```sql
        SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
        FROM Orders
        INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
        WHERE LastName = 'Davolio' OR LastName = 'Fuller'
        GROUP BY LastName
        HAVING COUNT(Orders.OrderID) > 25;
        ```
        
        | EmployeeID | LastName |
        | --- | --- |
        | 1 | Davolio |
        | 2 | Fuller |
        | 3 | Smith |
        | 4 | Johnson |
        
        | OrderID | EmployeeID |
        | --- | --- |
        | 101 | 1 |
        | 102 | 1 |
        | 103 | 2 |
        | 104 | 2 |
        | 105 | 2 |
        | 106 | 2 |
        | 107 | 2 |
        | 108 | 1 |
        | 109 | 1 |
        | 110 | 2 |
        | 111 | 2 |
        | 112 | 2 |
        | 113 | 2 |
        | 114 | 2 |
        
        - It selects the last names from the Employees table and counts the number of orders handled by employees with those last names.
        - The INNER JOIN connects the Orders table with the Employees table based on the EmployeeID.
        - The WHERE clause filters the employees whose last names are 'Davolio' or 'Fuller'.
        - GROUP BY LastName groups the results by the last name.
        - HAVING COUNT(Orders.OrderID) > 25 specifies that it only shows results where the count of orders is greater than 25.
        
        | LastName | NumberOfOrders |
        | --- | --- |
        | Fuller | 32 |
- **EXISTS Operator —** The `EXISTS` operator is used to test for the existence of any record in a subquery. The `EXISTS` operator returns TRUE if the subquery returns one or more records.
    
    ```sql
    SELECT SupplierName
    FROM Suppliers
    WHERE EXISTS (
        SELECT ProductName
        FROM Products
        WHERE Products.SupplierID = Suppliers.supplierID
        AND Price < 20
    );
    ```
    
    Suppliers: 
    
    | SupplierID | SupplierName |
    | --- | --- |
    | 1 | Supplier A |
    | 2 | Supplier B |
    | 3 | Supplier C |
    
    Products 
    
    | ProductID | SupplierID | ProductName | Price |
    | --- | --- | --- | --- |
    | 101 | 1 | Product 1 | 15 |
    | 102 | 1 | Product 2 | 25 |
    | 103 | 2 | Product 3 | 18 |
    | 104 | 2 | Product 4 | 30 |
    | 105 | 3 | Product 5 | 22 |
    
    Results: 
    
    SupplierName
    
    ---
    
    Supplier A
    
    ---
    
    Supplier B
    
    ---
    
    Explanation:
    
    - The query checks each supplier from the **`Suppliers`** table.
    - For each supplier, it looks in the **`Products`** table to see if there exists at least one product with a price less than 20 associated with that supplier.
    - Suppliers A and B have at least one product each with a price less than 20, so they satisfy the condition and are included in the final result.
    
- **ANY and ALL Operators —** The *operator* must be a standard comparison operator (=, <>, !=, >, >=, <, or <=)
    
    
    **Products**
    
    | ProductID | ProductName | SupplierID | CategoryID | Unit | Price |
    | --- | --- | --- | --- | --- | --- |
    | 1 | Chais | 1 | 1 | 10 boxes x 20 bags | 18 |
    | 2 | Chang | 1 | 1 | 24 - 12 oz bottles | 19 |
    | 3 | Aniseed Syrup | 1 | 2 | 12 - 550 ml bottles | 10 |
    | 4 | Chef Anton's Cajun Seasoning | 2 | 2 | 48 - 6 oz jars | 22 |
    | 5 | Chef Anton's Gumbo Mix | 2 | 2 | 36 boxes | 21.35 |
    | 6 | Grandma's Boysenberry Spread | 3 | 2 | 12 - 8 oz jars | 25 |
    | 7 | Uncle Bob's Organic Dried Pears | 3 | 7 | 12 - 1 lb pkgs. | 30 |
    | 8 | Northwoods Cranberry Sauce | 3 | 2 | 12 - 12 oz jars | 40 |
    | 9 | Mishi Kobe Niku | 4 | 6 | 18 - 500 g pkgs. | 97 |
    
    **OrderDetails**
    
    | OrderDetailID | OrderID | ProductID | Quantity |
    | --- | --- | --- | --- |
    | 1 | 10248 | 11 | 12 |
    | 2 | 10248 | 42 | 10 |
    | 3 | 10248 | 72 | 5 |
    | 4 | 10249 | 14 | 9 |
    | 5 | 10249 | 51 | 40 |
    | 6 | 10250 | 41 | 10 |
    | 7 | 10250 | 51 | 35 |
    | 8 | 10250 | 65 | 15 |
    | 9 | 10251 | 22 | 6 |
    | 10 | 10251 | 57 | 15 |
    
    ---
    
    - `ALL` means that the condition will be true only if the operation is true for all values in the range.
        
        ```sql
        SELECT column_name(s)
        FROM table_name
        WHERE column_name operator ALL
          (SELECT column_name
          FROM table_name
          WHERE condition);
        ```
        
        ```sql
        SELECT ProductName
        FROM Products
        WHERE ProductID = ALL
          (SELECT ProductID
          FROM OrderDetails
          WHERE Quantity = 10);
        ```
        
        - **`ANY`** and **`ALL`** are used with subqueries that return a list of **`ProductID`**.
        - The first two queries use **`ANY`**, looking for products where the **`ProductID`** matches any **`ProductID`** found in the subquery results that satisfy the specified conditions.
        - The last query uses **`ALL`** to find products associated with every row in the subquery where the **`Quantity`** is 10.
            
            ProductName
            
            ---
            
            Chang
            
            ---
            
            Chef Anton's Gumbo Mix
            
            ---
            
            Mishi Kobe Niku
            
            ---
            
    
    - `ANY` means that the condition will be true if the operation is true for any of the values in the range.
        
        ```sql
        SELECT column_name(s)
        FROM table_name
        WHERE column_name operator ANY
          (SELECT column_name
          FROM table_name
          WHERE condition);
        ```
        
        ```sql
        SELECT ProductName
        FROM Products
        WHERE ProductID = ANY
          (SELECT ProductID
          FROM OrderDetails
          WHERE Quantity > 1000);
        ```
        
        This query retrieves the names of products where the **`Quantity`** in the **`OrderDetails`** table is greater than 1000.
        
        ---
        
        Based on the provided data, there are no products with a quantity greater than 1000 in the **`OrderDetails`** table, so this query would return an empty result set.
        
- **SELECT INTO Statement —** The `SELECT INTO` statement copies data from one table into a new table.
    1. Copy all columns into a new table:
        
        ```sql
        SELECT *
        INTO newtable [IN externaldb]
        FROM oldtable
        WHERE condition;
        
        # creates a backup copy of Customers:
        SELECT * INTO CustomersBackup2017
        FROM Customers;
        
        # the IN clause to copy the table into a new table in another database:
        SELECT * INTO CustomersBackup2017 IN 'Backup.mdb'
        FROM Customers;
        ```
        
    2. Copy only some columns into a new table:
        
        ```sql
        SELECT column1, column2, column3, ...
        INTO newtable [IN externaldb]
        FROM oldtable
        WHERE condition;
        
        # copies only a few columns into a new table:
        SELECT CustomerName, ContactName INTO CustomersBackup2017
        FROM Customers;
        ```
        
    3. copies data from more than one table into a new table:
        
        ```sql
        SELECT Customers.CustomerName, Orders.OrderID
        INTO CustomersOrderBackup2017
        FROM Customers
        LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
        ```
        
    4. `SELECT INTO` can also be used to create a new, empty table using the schema of another. Just add a `WHERE` clause that causes the query to return no data:
        
        ```sql
        SELECT * INTO newtable
        FROM oldtable
        WHERE 1 = 0;
        ```
        
- **INSERT INTO SELECT Statement —** The `INSERT INTO SELECT` statement copies data from one table and inserts it into another table. The `INSERT INTO SELECT` statement requires that the data types in source and target tables match.
    1. Copy all columns from one table to another table:
        
        ```sql
        INSERT INTO table2
        SELECT * FROM table1
        WHERE condition;
        # Copy "Suppliers" into "Customers" (fill all columns):
        INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
        SELECT SupplierName, ContactName, Address, City, PostalCode, Country FROM Suppliers;
        ```
        
    2. Copy only some columns from one table into another table:
        
        ```sql
        INSERT INTO table2 (column1, column2, column3, ...)
        SELECT column1, column2, column3, ...
        FROM table1
        WHERE condition;
        # Copy only the German suppliers into "Customers":
        INSERT INTO Customers (CustomerName, City, Country)
        SELECT SupplierName, City, Country FROM Suppliers
        WHERE Country='Germany';
        ```
        
- **The SQL CASE Expression**
    
    ```sql
    CASE
        WHEN condition1 THEN result1
        WHEN condition2 THEN result2
        WHEN conditionN THEN resultN
        ELSE result
    END;
    ```
    
    | OrderID | Quantity | QuantityText |
    | --- | --- | --- |
    | 10248 | 12 | The quantity is under 30 |
    | 10249 | 40 | The quantity is greater than 30 |
    | 10250 | 10 | The quantity is under 30 |
    | 10250 | 35 | The quantity is greater than 30 |
    
    ```sql
    SELECT OrderID, Quantity,
    CASE WHEN Quantity > 30 THEN 'The quantity is greater than 30'
    WHEN Quantity = 30 THEN 'The quantity is 30'
    ELSE 'The quantity is under 30'
    END AS QuantityText
    FROM OrderDetails;
    ```
    
- **NULL Functions**
    
    
    | P_Id | ProductName | UnitPrice | UnitsInStock | UnitsOnOrder |
    | --- | --- | --- | --- | --- |
    | 1 | Jarlsberg | 10.45 | 16 | 15 |
    | 2 | Mascarpone | 32.56 | 23 |  |
    | 3 | Gorgonzola | 15.67 | 9 | 20 |
    
    ```sql
    SELECT ProductName, 
    UnitPrice * (UnitsInStock + UnitsOnOrder)
    FROM Products;
    ```
    
    - **MySQL**
        - `IFNULL()` function lets you return an alternative value if an expression is NULL:
            
            ```sql
            # if UnitsOnOrder is NULL, considering it as 0.
            SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
            FROM Products;
            ```
            
            **Mascarpone:**
            
            - **`UnitPrice`**: $32.56
            - **`UnitsInStock`**: 23 units
            - **`UnitsOnOrder`**: (NULL - considered as 0)
            - Calculation: $32.56 * (23 + 0) = $32.56 * 23 = $749.68
        - or we can use the `COALESCE()` function, like this:
            
            ```sql
            SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
            FROM Products;
            ```
            
    - **MS Access**
        - The MS Access `IsNull()` function returns TRUE (-1) if the expression is a null value, otherwise FALSE (0):
            
            ```sql
            SELECT ProductName, UnitPrice * (UnitsInStock + IIF(IsNull(UnitsOnOrder), 0, UnitsOnOrder))
            FROM Products;
            ```
            
    - **Oracle**
        - The Oracle `NVL()` function achieves the same result:
            
            ```sql
            SELECT ProductName, UnitPrice * (UnitsInStock + NVL(UnitsOnOrder, 0))
            FROM Products;
            ```
            
        - or we can use the `COALESCE()` function, like this:
            
            ```sql
            SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
            FROM Products;
            ```
            
- **Stored Procedure**
    
    > if you have an SQL query that you write over and over again, save it as a stored procedure, and then just call it to execute it.
    > 
    > 
    > You can also pass parameters to a stored procedure, so that the stored procedure can act based on the parameter value(s) that is passed.
    > 
    - **Stored Procedure Syntax**
        
        ```sql
        CREATE PROCEDURE procedure_name
        AS
        sql_statement
        GO;
        ```
        
    - **Execute a Stored Procedure**
        
        ```sql
        EXEC procedure_name;
        ```
        
    1. **Stored Procedure With Multiple Parameters** 
    CREATE PROCEDURE SelectAllCustomers @City nvarchar(30), @PostalCode nvarchar(10)
    AS
    SELECT * FROM Customers WHERE City = @City AND PostalCode = @PostalCode
    GO;
        
        ```sql
        CREATE PROCEDURE SelectAllCustomers @City nvarchar(30), @PostalCode nvarchar(10)
        AS
        SELECT * FROM Customers WHERE City = @City AND PostalCode = @PostalCode
        GO;
        
        EXEC SelectAllCustomers @City = 'London', @PostalCode = 'WA1 1DP';
        ```
        
        - **`CREATE PROCEDURE`** is the SQL statement used to create a new stored procedure named **`SelectAllCustomers`**.
        - **`@City nvarchar(30), @PostalCode nvarchar(10)`** declares parameters for the stored procedure. Parameters allow you to pass values into the stored procedure when it's executed. Here, **`@City`** and **`@PostalCode`** are parameters, specifying their data types (**`nvarchar`**) and maximum lengths (**`30`** and **`10`** characters, respectively).
        - **`AS`** is used to begin the body of the stored procedure.
        - **`SELECT * FROM Customers WHERE City = @City AND PostalCode = @PostalCode`** is the SQL query inside the stored procedure. It selects all columns (**``**) from the **`Customers`** table where the **`City`** matches the passed **`@City`** parameter and the **`PostalCode`** matches the passed **`@PostalCode`** parameter.
        - **`GO`** signifies the end of the stored procedure definition.
        - Once this stored procedure is created, it can be executed by providing values for **`@City`** and **`@PostalCode`** to retrieve specific customer information from the **`Customers`** table based on the provided city and postal code parameters.
- **SQL Comments**
    1. Single line comments start with `--`.
        
        ```sql
        --SELECT * FROM Customers;
        SELECT * FROM Products;
        ```
        
    2. Multi-line comments start with `/*` and end with `*/`.
        
        ```sql
        /*Select all the columns
        of all the records
        in the Customers table:*/
        SELECT * FROM Customers;
        ```
        
- Operators
    
    
    | Operator | Description |
    | --- | --- |
    | + | Add |
    | - | Subtract |
    | * | Multiply |
    | / | Divide |
    | % | Modulo |
    
    | Operator | Description |
    | --- | --- |
    | & | Bitwise AND |
    | | | Bitwise OR |
    | ^ | Bitwise exclusive OR |
    
    | Operator | Description |
    | --- | --- |
    | = | Equal to |
    | > | Greater than |
    | < | Less than |
    | >= | Greater than or equal to |
    | <= | Less than or equal to |
    | <> | Not equal to |
    
    | Operator | Description |
    | --- | --- |
    | ALL | TRUE if all of the subquery values meet the condition |
    | AND | TRUE if all the conditions separated by AND is TRUE |
    | ANY | TRUE if any of the subquery values meet the condition |
    | BETWEEN | TRUE if the operand is within the range of comparisons |
    | EXISTS | TRUE if the subquery returns one or more records |
    | IN | TRUE if the operand is equal to one of a list of expressions |
    | LIKE | TRUE if the operand matches a pattern |
    | NOT | Displays a record if the condition(s) is NOT TRUE |
    | OR | TRUE if any of the conditions separated by OR is TRUE |
    | SOME | TRUE if any of the subquery values meet the condition |
    
    | Operator | Description |
    | --- | --- |
    | += | Add equals |
    | -= | Subtract equals |
    | *= | Multiply equals |
    | /= | Divide equals |
    | %= | Modulo equals |
    | &= | Bitwise AND equals |
    | ^-= | Bitwise exclusive equals |
    | |*= | Bitwise OR equals |
