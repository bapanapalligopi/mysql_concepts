A **stored procedure** in SQL is a set of SQL statements that are precompiled and stored in a database, allowing you to execute them repeatedly with different inputs (parameters). Instead of writing SQL statements one by one, you define a stored procedure once, and then you can call it with a simple command. This makes the code easier to reuse, maintain, and manage. Stored procedures are often used to perform complex operations, handle transactions, or ensure consistent behavior.

### Key Parts of a Stored Procedure:

1. **Procedure Name**: 
   Every stored procedure must have a unique name within the schema. The name is used to identify and call the procedure.

2. **Parameters**: 
   Stored procedures can take parameters, which can be used to customize the behavior of the procedure. These parameters are typically passed in when the procedure is called. You can specify:
   - **IN parameters**: These are inputs to the procedure. The caller provides the values when calling the procedure.
   - **OUT parameters**: These allow the procedure to return a value to the caller. The procedure modifies the value of the OUT parameter, which is then available to the caller after the procedure executes.
   - **INOUT parameters**: These act like both IN and OUT parameters, meaning that the caller can provide an initial value, and the procedure can modify and return a new value.

3. **BEGIN/END Block**: 
   If your stored procedure has multiple statements, you wrap them in a `BEGIN ... END` block. This is similar to using braces `{ }` in programming languages to define a block of code. If the procedure only has one statement, the `BEGIN ... END` block is optional.

4. **Statements**: 
   The body of the stored procedure consists of SQL statements that perform actions, like inserting data, updating records, or even calling other procedures. These statements end with a semicolon (`;`).

5. **DELIMITER Command**: 
   The `DELIMITER` command is used in MySQL to change the statement delimiter temporarily, especially when creating a stored procedure. By default, MySQL uses `;` to end statements, but a stored procedure itself contains `;` inside its body, so you need to tell MySQL to treat `//` (or another character) as the delimiter for the stored procedure definition.

### Example Breakdown (From Your Question):

Let's go over the first example you provided and explain each part of the procedure.

```sql
delimiter //
CREATE PROCEDURE new_employee(
    first CHAR(100),
    last CHAR(100),
    birthday DATE)
BEGIN
    INSERT INTO employees (first_name, last_name) VALUES (first, last);  -- Step 1
    SET @id = (SELECT LAST_INSERT_ID());  -- Step 2
    INSERT INTO birthdays (emp_id, birthday) VALUES (@id, birthday);  -- Step 3
END //
delimiter ;
```

- **`delimiter //`**: Changes the delimiter from the default `;` to `//`. This tells MySQL that everything between `//` is part of the stored procedure, rather than each statement being executed individually.
  
- **`CREATE PROCEDURE new_employee(...)`**: 
  - `new_employee` is the name of the procedure.
  - The parameters (`first`, `last`, and `birthday`) specify the input values the procedure will accept when called.

- **`BEGIN` and `END`**: These mark the start and end of the procedure body.

- **SQL Statements Inside the Procedure**:
  - **First Statement (`INSERT INTO employees`)**: This inserts the employee's first name and last name into the `employees` table.
  - **Second Statement (`SET @id = (SELECT LAST_INSERT_ID())`)**: After the first insert, this statement captures the auto-generated `id` of the newly inserted employee. The `LAST_INSERT_ID()` function returns the ID of the most recent auto-increment value inserted into a table.
  - **Third Statement (`INSERT INTO birthdays`)**: This inserts a new record into the `birthdays` table using the `id` from the previous statement and the birthday provided as an input.

- **`delimiter ;`**: Resets the delimiter back to the default `;`.

When you call this procedure with `CALL new_employee("tim", "sehn", "1980-02-03");`, it inserts a row in both the `employees` and `birthdays` tables.

### OUT Parameters:

You also explained how **OUT parameters** work. These are parameters that the stored procedure can use to return a value to the caller. Let's break it down:

```sql
delimiter //

CREATE PROCEDURE birthday_count(
    IN bday DATE,
    OUT count INT)
BEGIN
    SET count = (SELECT COUNT(*) FROM birthdays WHERE birthday = bday);
END //

delimiter ;
```

- **IN Parameter (`bday`)**: This is the input parameter that specifies the birthday you're looking for in the `birthdays` table.
- **OUT Parameter (`count`)**: This will store the count of how many people have the specified birthday. After the procedure runs, the value of `count` will be available to the caller.

### Example Usage of the `birthday_count` Procedure:

```sql
-- Call the new_employee procedure to add two people with the same birthday
CALL new_employee('aaron', 'son', '1985-01-10');
CALL new_employee('brian', 'hendriks', '1985-01-10');

-- Declare a variable to hold the OUT value
SET @count = 0;

-- Call the birthday_count procedure to get the number of people with that birthday
CALL birthday_count('1985-01-10', @count);

-- Retrieve the value stored in the OUT parameter
SELECT @count;
```

- When the `birthday_count` procedure is called, it looks for how many people have the birthday `1985-01-10` and assigns the result to the `count` variable (which is passed as an OUT parameter).
- After the procedure runs, the value of `count` is retrieved with `SELECT @count`, and it shows the number of people with that birthday (in this case, 2).

### When to Use OUT Parameters vs. Functions:

- **OUT parameters** are useful when you need to return a value from a procedure but don't want to use a `SELECT` query to return the result (as you'd typically do with a function).
- If the procedure needs to return a single value as its main result, it's often better to use a **function** instead, because functions are designed for that purpose.

However, in some cases (like when you need to return multiple values or modify data within a procedure), using OUT parameters can be a useful way to return values.

### Summary:

Stored procedures allow you to encapsulate logic and SQL statements in a reusable way. Parameters (IN, OUT, INOUT) let you customize the behavior of the procedure. OUT parameters specifically are used to return values to the caller, and they allow you to pass back results without using the `RETURN` statement.
