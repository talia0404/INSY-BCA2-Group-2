# **What is a Stored Procedure?**

A **stored procedure** is a saved block of SQL code in the database that can be executed (called) whenever needed.
Think of it as a **function in programming** but for SQL operations.

👉 It can contain **SQL statements**, **control flow logic (IF, LOOP)**, and **parameters**.

👉 It is stored once and reused many times.

---

## **Why Do We Use Stored Procedures?**

| Benefit         | Description                                                                |
| --------------- | -------------------------------------------------------------------------- |
| Reusability     | Write once, run multiple times.                                            |
| Consistency     | Avoid mistakes by standardizing complex SQL logic.                         |
| Performance     | Reduces traffic between app and database (sends just procedure call).      |
| Security        | Users can be allowed to run procedures without giving direct table access. |
| Maintainability | Business logic lives in the database, easier to update later.              |

---

## **Example of a Simple Procedure**

### ✅ **Create a Procedure**

```sql
DELIMITER //

CREATE PROCEDURE GetAllProducts()
BEGIN
    SELECT * FROM Products;
END //

DELIMITER ;
```

* `DELIMITER //` temporarily changes the end of command marker (so `;` can be used inside the procedure).
* This procedure retrieves all records from the `Products` table.

---

### ✅ **Call (Run) the Procedure**

```sql
CALL GetAllProducts();
```

---

## **Procedures with Parameters**

### ✅ **Input Parameter**

```sql
DELIMITER //

CREATE PROCEDURE GetProductByCategory(IN cat VARCHAR(50))
BEGIN
    SELECT * FROM Products WHERE Category = cat;
END //

DELIMITER ;

-- Run it:
CALL GetProductByCategory('Electronics');
```

👉 `IN` parameter means the value is passed **into** the procedure.

---

### ✅ **Output Parameter**

```sql
DELIMITER //

CREATE PROCEDURE GetTotalProducts(OUT total INT)
BEGIN
    SELECT COUNT(*) INTO total FROM Products;
END //

DELIMITER ;

-- Declare variable and call
CALL GetTotalProducts(@count);
SELECT @count;
```

👉 `OUT` parameter returns a value **back to the caller**.

---

## **Procedures with Logic**

### ✅ **IF / ELSE Example**

```sql
DELIMITER //

CREATE PROCEDURE CheckStock(IN prod_id INT)
BEGIN
    DECLARE stock_count INT;
    SELECT Stock INTO stock_count FROM Products WHERE ProductID = prod_id;
    
    IF stock_count > 0 THEN
        SELECT 'In Stock' AS Status;
    ELSE
        SELECT 'Out of Stock' AS Status;
    END IF;
END //

DELIMITER ;

-- Run it:
CALL CheckStock(1);
```

---
