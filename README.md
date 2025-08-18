## Requirements
- Big Query
- Looker Studio

## Identify the primary key
A primary key is a unique attribute in a database table that is used to exclusively identify each row. Each primary key value must be unique and must not be empty (null).
- Primary key table Customer : CustomerID
- Primary key table Products: ProdNumber 
- Primary key table Orders : OrderID
- Primary key table ProductCategory : CategoryID

## Relationship between tables
- Determine each primary key in the 4 datasets.
- Determine the relationship between the four tables.

```sql
#Verify the relationship between Customers and Orders (Foreign Key: CustomerID)
SELECT o.CustomerID
FROM `sales_dataset.Orders` o
LEFT JOIN `sales_dataset.Customers` c ON o.CustomerID = c.CustomerID
WHERE c.CustomerID IS NULL AND o.CustomerID IS NOT NULL;

# Verify the relationship between Orders and Products (Foreign Key: ProdNumber)
SELECT o.ProdNumber
FROM `sales_dataset.Orders` o
LEFT JOIN `sales_dataset.Products` p ON o.ProdNumber = p.ProdNumber
WHERE p.ProdNumber IS NULL AND o.ProdNumber IS NOT NULL;

# Verify the relationship between Products and ProductCategory (Foreign Key: Category)
SELECT p.Category
FROM `sales_dataset.Products` p
LEFT JOIN `sales_dataset.ProductCategory` pc ON p.Category = pc.CategoryID
WHERE pc.CategoryID IS NULL AND p.Category IS NOT NULL;

```

## Table Master
This MasterTable is created to make it easier to analyze in dashboards or business reports. This data can be used to calculate total sales, number of orders, and customer analysis by city or product category.

```sql
#TASK 3: CREATE A MASTER TABLE
CREATE TABLE `sales_dataset.MasterTable` AS
SELECT 
    o.Date AS order_date,
    pc.CategoryName AS category_name,
    p.ProdName AS product_name,
    p.Price AS product_price,
    o.Quantity AS order_qty,
    (o.Quantity * p.Price) AS total_sales,
    c.CustomerEmail AS cust_email,
    c.CustomerCity AS cust_city
FROM `sales_dataset.Orders` o
JOIN `sales_dataset.Customers` c ON o.CustomerID = c.CustomerID
JOIN `sales_dataset.Products` p ON o.ProdNumber = p.ProdNumber
JOIN `sales_dataset.ProductCategory` pc ON p.Category = pc.CategoryID
ORDER BY o.Date ASC;
```

## Dashboard
<div align="center">
<img width = "80%" src = "https://github.com/anandawln/Business-Intelligence-Bank-Muamalat/blob/main/assets/muamalat_bi.png">
</div>
