Certainly! Here’s a breakdown of examples that use **session variables** (`@var`) and **global variables** (`@@var`) in MySQL.

### 1. **Session Variables (`@var`)**

Session variables are defined and used in the scope of a single session. They are prefixed with a single `@` and are used to store values that persist across multiple queries during the session but are not accessible after the session ends.

#### Example: Using Session Variables (`@var`)

```sql
-- Set a session variable
SET @total_sales = 1000;

-- Query using the session variable
SELECT @total_sales AS TotalSales;
```

- **Explanation**:
    - `SET @total_sales = 1000;`: This statement sets a session variable `@total_sales` to 1000.
    - `SELECT @total_sales AS TotalSales;`: This retrieves the value stored in `@total_sales` and returns it as a result.

Session variables can also be used inside stored procedures.

#### Example with Stored Procedure:

```sql
DELIMITER //

CREATE PROCEDURE calculate_total_sales()
BEGIN
    -- Declare session variable to store total sales
    SET @total_sales = (SELECT SUM(amount) FROM sales);
    SELECT @total_sales AS TotalSales;
END //

DELIMITER ;

-- Call the procedure to calculate total sales
CALL calculate_total_sales();
```

- **Explanation**:
    - In the procedure, a session variable `@total_sales` is set to the sum of the `amount` column from the `sales` table.
    - The procedure then selects the value of `@total_sales`, which is the result of the summation.

### 2. **Global Variables (`@@var`)**

Global variables are system-defined variables that are accessible globally across all sessions. They typically represent server settings or global states. The prefix `@@` is used for global variables.

#### Example: Using Global Variables (`@@var`)

Global variables are typically used to retrieve information about server configuration or status. For example:

```sql
-- Get the current value of a global variable
SELECT @@global.max_connections AS MaxConnections;
```

- **Explanation**:
    - `@@global.max_connections` is a global variable that shows the maximum number of simultaneous connections allowed to the MySQL server.
    - `SELECT @@global.max_connections AS MaxConnections;`: This retrieves and displays the value of the `max_connections` global variable.

#### Example with Session and Global Variables:

In this example, we’ll compare the session variable and the global variable for the number of connections.

```sql
-- Get the session-specific max connections
SELECT @@session.max_connections AS SessionMaxConnections;

-- Get the global max connections
SELECT @@global.max_connections AS GlobalMaxConnections;
```

- **Explanation**:
    - `@@session.max_connections`: This global variable provides the maximum number of simultaneous connections allowed for the current session.
    - `@@global.max_connections`: This is the global variable that gives the maximum number of simultaneous connections allowed for all sessions.

### Combining Session and Global Variables in a Procedure:

Here's an example of a **stored procedure** that demonstrates the use of both session and global variables.

```sql
DELIMITER //

CREATE PROCEDURE compare_connections()
BEGIN
    -- Declare a session variable to store current max connections
    SET @session_max = @@session.max_connections;

    -- Compare the session and global max connections
    IF @session_max < @@global.max_connections THEN
        SELECT 'Session limit is less than global limit';
    ELSE
        SELECT 'Session limit is equal to or greater than global limit';
    END IF;
END //

DELIMITER ;

-- Call the procedure to compare session and global max connections
CALL compare_connections();
```

- **Explanation**:
    - `SET @session_max = @@session.max_connections;`: This stores the session's maximum allowed connections in the session variable `@session_max`.
    - The procedure compares the session variable (`@session_max`) to the global variable (`@@global.max_connections`).
    - Based on the comparison, it returns a message indicating whether the session's connection limit is less than, equal to, or greater than the global limit.

### Key Differences Between `@var` and `@@var`:

1. **`@var` (Session Variables)**:
   - Defined for the current session.
   - Accessible only within the session that created them.
   - Used for temporary storage of values that need to persist across queries in a single session.

2. **`@@var` (Global Variables)**:
   - Defined globally and are accessible across all sessions.
   - Typically used for server settings or configuration.
   - Can be accessed from any session or query and show global system variables like `max_connections`, `innodb_buffer_pool_size`, etc.

### More Complex Example Combining Both:

Let’s build a stored procedure that uses both session and global variables to dynamically check and adjust server settings based on certain conditions.

```sql
DELIMITER //

CREATE PROCEDURE adjust_max_connections()
BEGIN
    -- Declare a local session variable to hold the global max_connections value
    DECLARE global_max INT;

    -- Get the current global max_connections value
    SET global_max = @@global.max_connections;

    -- If the global max_connections is less than 1000, set it to 1000
    IF global_max < 1000 THEN
        SET GLOBAL max_connections = 1000;
        SELECT 'Global max_connections was set to 1000';
    ELSE
        SELECT 'Global max_connections is already greater than or equal to 1000';
    END IF;

    -- Output the session-specific max_connections value
    SELECT @@session.max_connections AS SessionMaxConnections;
END //

DELIMITER ;

-- Call the procedure
CALL adjust_max_connections();
```

- **Explanation**:
    - The procedure first retrieves the current `@@global.max_connections` value and stores it in a local variable `global_max`.
    - It then checks whether the global `max_connections` value is less than 1000. If it is, it updates the global setting to 1000.
    - Finally, it selects and shows the current `@@session.max_connections` for the session to demonstrate the difference.

---

### Summary:

- **Session Variables (`@var`)**: Used for temporary values specific to the current session, often used within stored procedures.
- **Global Variables (`@@var`)**: Used to access system-wide configuration and status variables, typically for server settings and monitoring.

In most cases, **session variables** are used inside stored procedures to maintain temporary state, while **global variables** are generally used to check or modify system-wide settings.
