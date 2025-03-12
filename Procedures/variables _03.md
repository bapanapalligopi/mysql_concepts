In SQL, **variables** in stored procedures can be used to store temporary data during the execution of the procedure. These variables can be:

- **Session variables** (denoted by `@var`), which are available throughout the session.
- **Global variables** (denoted by `@@var`), which are accessible across all sessions (but these are often system-wide and not typically used in stored procedures).
- **Local variables**, which are defined within the scope of a stored procedure and can only be used during the execution of that particular procedure.

In most cases, for **stored procedures**, you'll primarily work with **local variables** that are defined using the `DECLARE` statement.

### Example Breakdown of Declaring and Using Variables:

```sql
CREATE PROCEDURE combined_birthday_count(
    IN bday1 DATE,
    IN bday2 DATE,
    OUT count INT)
BEGIN
    DECLARE count1, count2 INT;  -- Declaring two local variables (count1 and count2) of type INT
    SET count1 = (SELECT COUNT(*) FROM birthdays WHERE birthday = bday1);  -- Assign the count of bday1 to count1
    SET count2 = (SELECT COUNT(*) FROM birthdays WHERE birthday = bday2);  -- Assign the count of bday2 to count2
    SET count = count1 + count2;  -- Combine the counts and assign to the OUT parameter 'count'
END
```

### Explanation:

1. **Procedure Declaration**:
   - The procedure `combined_birthday_count` accepts two `IN` parameters (`bday1` and `bday2`), representing two birthdays to check in the `birthdays` table.
   - The `OUT` parameter `count` will store the sum of the number of people with those two birthdays.

2. **DECLARE Statement**:
   - `DECLARE count1, count2 INT;`: Declares **local variables** `count1` and `count2` of type `INT`. 
     - You can declare multiple variables of the same type in a single `DECLARE` statement, which is similar to how you'd define multiple variables in some programming languages.

3. **SET Statements**:
   - `SET count1 = (SELECT COUNT(*) FROM birthdays WHERE birthday = bday1);`: This query counts the number of people whose birthday matches `bday1` and assigns the result to `count1`.
   - `SET count2 = (SELECT COUNT(*) FROM birthdays WHERE birthday = bday2);`: Similarly, this counts the number of people whose birthday matches `bday2` and assigns the result to `count2`.

4. **Adding the Counts**:
   - `SET count = count1 + count2;`: After obtaining the two individual counts, you add them together and store the result in the `OUT` parameter `count`.

5. **Calling the Procedure**:
   ```sql
   CALL combined_birthday_count('1980-02-03', '1985-01-10', @count);
   ```
   This call will execute the procedure, passing in the two birthdays to compare and storing the result in the session variable `@count`.

6. **Retrieving the Result**:
   ```sql
   SELECT @count;
   ```
   After calling the procedure, you can select `@count` to see the combined count of people with the two specified birthdays.

### Important Notes:

- **Local Variables**: As mentioned, **local variables** (declared with `DECLARE`) are only accessible within the body of the stored procedure. They are temporary and will be discarded once the procedure execution is complete. They are also **scope-bound** to the specific procedure call and can't be accessed outside the procedure.
  
- **DECLARE Must Be at the Beginning**: In MySQL, `DECLARE` statements must be placed at the very start of the procedure (before any other logic or SQL statements). This is a rule borrowed from languages like C, where variables must be declared at the beginning of a function or block before they are used.

- **Session Variables (`@var`) vs. Local Variables**: Session variables, which are prefixed with `@`, are used when you want to share data between different SQL statements, across multiple stored procedure calls, or outside the procedure after it completes. On the other hand, **local variables** (declared with `DECLARE`) are isolated within the scope of the procedure and can’t be accessed outside of it.

- **Global Variables (`@@var`)**: Global variables are not typically used inside procedures, as they are often system-defined and can represent global settings or system states.

### Best Practice:
- It's recommended to use **local variables** (`DECLARE`) within stored procedures to avoid unintentional conflicts or side effects that may occur if you use session variables (`@var`). Use **OUT parameters** to pass back any necessary results to the caller, ensuring a clean separation between the procedure's internal logic and the calling code.

This pattern of declaring local variables and using them to store temporary data within the procedure helps make stored procedures more maintainable and easier to debug, as the state is kept contained and isolated.
