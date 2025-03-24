### 📚 **Exception Handling in Stored Procedures in MySQL (Beginner to Advanced)**

---

## 🎯 **1. Introduction to Stored Procedures in MySQL**

A **stored procedure** is a precompiled SQL code that you can save and execute multiple times. You can pass parameters and manage exceptions gracefully.

### ✅ **Basic Syntax:**
```sql
DELIMITER //

CREATE PROCEDURE ProcedureName()
BEGIN
    -- SQL statements
END //

DELIMITER ;
```

---

## 🎯 **2. Basic Stored Procedure Without Error Handling**

### Example: Insert Record
```sql
DELIMITER //

CREATE PROCEDURE AddEmployee(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```
✅ **Usage:**
```sql
CALL AddEmployee('John Doe', 30);
```

---

## 🎯 **3. Error Handling in MySQL Stored Procedures**

MySQL uses `DECLARE`, `HANDLER`, and `CONDITION` for error handling.

---

### ✅ **4. Basic Error Handling with CONTINUE and EXIT**

### Basic Syntax
```sql
DECLARE handler_type HANDLER FOR condition_value statement;

-- handler_type: CONTINUE or EXIT
-- condition_value: SQLSTATE value or a MySQL error code
```

- **`CONTINUE`** – Ignore the error and continue.
- **`EXIT`** – Stop the procedure when an error occurs.

---

### ✅ **5. Basic Error Handling Example**

```sql
DELIMITER //

CREATE PROCEDURE AddEmployeeWithError(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Handle error silently or log error
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES ('Error inserting record', NOW());
    END;

    -- Try to insert
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```
✅ **Usage:**
```sql
CALL AddEmployeeWithError('John', NULL); -- Will insert error into ErrorLogs
```

---

## 🎯 **6. Handling Specific Errors Using SQLSTATE**

### Example: Handle Duplicate Key Error (`1062`)

```sql
DELIMITER //

CREATE PROCEDURE AddUniqueEmployee(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE EXIT HANDLER FOR SQLSTATE '23000'
    BEGIN
        -- Handle duplicate key error
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES ('Duplicate entry error', NOW());
    END;

    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```
✅ **Usage:**
```sql
CALL AddUniqueEmployee('John Doe', 25); -- Will log error if duplicate
```

---

## 🎯 **7. Advanced Error Handling with Multiple Handlers**

```sql
DELIMITER //

CREATE PROCEDURE HandleMultipleErrors(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE EXIT HANDLER FOR SQLSTATE '23000'
    BEGIN
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES ('Duplicate entry error', NOW());
    END;

    DECLARE EXIT HANDLER FOR SQLSTATE '22001'
    BEGIN
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES ('Data too long error', NOW());
    END;

    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```

---

## 🎯 **8. Using GET DIAGNOSTICS for Error Details (MySQL 5.7+)**

```sql
DELIMITER //

CREATE PROCEDURE AdvancedErrorDetails(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE error_msg TEXT;
    DECLARE error_code INT;

    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Get detailed error info
        GET DIAGNOSTICS CONDITION 1
            error_code = MYSQL_ERRNO, error_msg = MESSAGE_TEXT;

        -- Log error
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES (CONCAT('Error ', error_code, ': ', error_msg), NOW());
    END;

    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```

✅ **Usage:**
```sql
CALL AdvancedErrorDetails('John', NULL);
```

---

## 🎯 **9. Using Custom Error Messages with SIGNAL and RESIGNAL**

### ✅ **SIGNAL: Raise a Custom Error**

```sql
DELIMITER //

CREATE PROCEDURE CheckEmployeeAge(IN emp_age INT)
BEGIN
    IF emp_age < 18 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Age must be 18 or above';
    ELSE
        INSERT INTO Employees (Name, Age)
        VALUES ('New Employee', emp_age);
    END IF;
END //

DELIMITER ;
```
✅ **Usage:**
```sql
CALL CheckEmployeeAge(16); -- Will trigger custom error
```

---

### ✅ **RESIGNAL: Propagate Error**

```sql
DELIMITER //

CREATE PROCEDURE ParentProcedure(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Log and rethrow error
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES ('Error in parent procedure', NOW());
        RESIGNAL;
    END;

    CALL AddEmployeeWithError(emp_name, emp_age);
END //

DELIMITER ;
```

---

## 🎯 **10. Transaction Handling with Error Management**

```sql
DELIMITER //

CREATE PROCEDURE TransactionWithError(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Rollback if any error occurs
        ROLLBACK;
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES ('Transaction failed', NOW());
    END;

    START TRANSACTION;

    -- First insert
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);

    -- Second insert (with possible error)
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, NULL); -- Will fail and rollback entire transaction

    COMMIT;
END //

DELIMITER ;
```

✅ **Usage:**
```sql
CALL TransactionWithError('John Doe', 28);
```

---

## 🎯 **11. Debugging Tips for Stored Procedures**
- Use `SELECT` statements to print variable values.
- Check `SHOW ERRORS` or `SHOW WARNINGS` for additional details.
- Use `GET DIAGNOSTICS` for precise error tracking.

---

## 🔥 **Summary**
- `CONTINUE` or `EXIT` Handlers control the flow after errors.
- `SIGNAL` and `RESIGNAL` raise custom errors.
- `GET DIAGNOSTICS` provides detailed error information.
- Transactions with `ROLLBACK` ensure data integrity.

Let me know if you'd like help creating or optimizing any specific procedure! 🚀
### 📚 **What is `GET DIAGNOSTICS` in MySQL?**

`GET DIAGNOSTICS` is a MySQL command that retrieves detailed information about the **last error or warning** that occurred during SQL execution. It is useful in stored procedures or triggers to capture error details such as:

- Error code
- Error message
- Affected rows
- Number of conditions

---

## 🎯 **1. Why Use `GET DIAGNOSTICS`?**
✅ **Purpose:**
- To **log detailed error information** in custom error-handling logic.
- To capture both **SQLSTATE values** and detailed **error messages**.
- To diagnose issues that occur during stored procedures or transactions.

---

## 🎯 **2. Basic Syntax**

```sql
GET [CURRENT] DIAGNOSTICS
    variable_name = statement_info_item [, variable_name = statement_info_item] ...
```

- `variable_name` – Stores the value of the diagnostic item.
- `statement_info_item` – Retrieves specific information about the last SQL statement.

---

## 🎯 **3. Commonly Used Statement Info Items**

| **Statement Info Item**  | **Description**                              |
|--------------------------|---------------------------------------------|
| `NUMBER`                  | Number of conditions raised.                |
| `ROW_COUNT`               | Number of rows affected by the last statement. |
| `CONDITION 1`              | Gets information about the first condition. |
| `RETURNED_SQLSTATE`        | SQLSTATE code for the error or warning.      |
| `MESSAGE_TEXT`             | Error or warning message.                   |
| `MYSQL_ERRNO`              | MySQL error number.                        |

---

## 🎯 **4. Basic Example Using `GET DIAGNOSTICS`**

```sql
DELIMITER //

CREATE PROCEDURE LogErrorDetails(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE error_msg TEXT;
    DECLARE error_code INT;

    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Get error information after the exception
        GET DIAGNOSTICS CONDITION 1
            error_code = MYSQL_ERRNO,  -- Get MySQL error number
            error_msg = MESSAGE_TEXT;  -- Get detailed error message

        -- Log error in ErrorLogs table
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES (CONCAT('Error ', error_code, ': ', error_msg), NOW());
    END;

    -- Attempt to insert a record
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```

✅ **Usage:**
```sql
CALL LogErrorDetails('John Doe', NULL); -- This will trigger an error if NULL is not allowed
SELECT * FROM ErrorLogs;
```

---

## 🎯 **5. Advanced Example with Multiple Conditions**

If you want to capture **multiple conditions or warnings**, use `NUMBER` to iterate over the conditions.

```sql
DELIMITER //

CREATE PROCEDURE CaptureMultipleErrors(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE error_msg TEXT;
    DECLARE error_code INT;
    DECLARE cond_count INT;
    DECLARE i INT DEFAULT 1;

    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Get the number of conditions raised
        GET DIAGNOSTICS cond_count = NUMBER;

        -- Loop through all conditions
        WHILE i <= cond_count DO
            GET DIAGNOSTICS CONDITION i
                error_code = MYSQL_ERRNO,
                error_msg = MESSAGE_TEXT;

            -- Log each condition
            INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
            VALUES (CONCAT('Error ', error_code, ': ', error_msg), NOW());

            SET i = i + 1;
        END WHILE;
    END;

    -- Try to insert invalid data
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```

✅ **Usage:**
```sql
CALL CaptureMultipleErrors('John Doe', NULL); -- Triggers error if Age is NULL
SELECT * FROM ErrorLogs;
```

---

## 🎯 **6. Using `ROW_COUNT` to Capture Affected Rows**

You can also use `ROW_COUNT` to get the number of affected rows in a procedure.

```sql
DELIMITER //

CREATE PROCEDURE CheckRowCount()
BEGIN
    DECLARE row_count INT DEFAULT 0;

    -- Delete some records
    DELETE FROM Employees WHERE Age < 18;

    -- Get the number of affected rows
    GET DIAGNOSTICS row_count = ROW_COUNT;

    -- Insert the row count in a log
    INSERT INTO AuditLogs (Action, AffectedRows, LogTime)
    VALUES ('Delete operation', row_count, NOW());
END //

DELIMITER ;
```

✅ **Usage:**
```sql
CALL CheckRowCount();
SELECT * FROM AuditLogs;
```

---

## 🎯 **7. Error Handling Using `RETURNED_SQLSTATE`**

To capture SQLSTATE values, use:

```sql
DELIMITER //

CREATE PROCEDURE CaptureSQLState(IN emp_name VARCHAR(50), IN emp_age INT)
BEGIN
    DECLARE sql_state CHAR(5);
    DECLARE error_msg TEXT;

    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Get SQLSTATE and error message
        GET DIAGNOSTICS CONDITION 1
            sql_state = RETURNED_SQLSTATE,
            error_msg = MESSAGE_TEXT;

        -- Log error with SQLSTATE
        INSERT INTO ErrorLogs (ErrorMessage, ErrorTime)
        VALUES (CONCAT('SQLSTATE ', sql_state, ': ', error_msg), NOW());
    END;

    -- Attempt to insert invalid record
    INSERT INTO Employees (Name, Age)
    VALUES (emp_name, emp_age);
END //

DELIMITER ;
```

✅ **Usage:**
```sql
CALL CaptureSQLState('John', NULL);
SELECT * FROM ErrorLogs;
```

---

## 🎯 **8. Best Practices for Using `GET DIAGNOSTICS`**
- Always use `CONDITION 1` to capture the most recent condition.
- Use `NUMBER` to iterate through multiple conditions if needed.
- Combine with error handlers (`HANDLER FOR SQLEXCEPTION`) for efficient error logging.
- Use `GET DIAGNOSTICS` inside `BEGIN...END` blocks only.

---

## 🚀 **Summary:**
- `GET DIAGNOSTICS` retrieves detailed information about SQL errors or warnings.
- Use it to capture error codes, messages, and SQLSTATE values.
- It enhances error logging and debugging in stored procedures.

Let me know if you'd like to optimize your stored procedures further or need help with any specific use cases! 😊
