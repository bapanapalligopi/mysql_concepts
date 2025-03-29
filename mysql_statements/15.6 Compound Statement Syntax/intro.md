A **compound statement** in MySQL is a block of code enclosed by `BEGIN ... END`, used in stored programs (procedures, functions, triggers) to group multiple SQL statements together, supporting declarations, flow control, condition handling, and more.

Example:
```sql
BEGIN
    DECLARE counter INT DEFAULT 0;
    SET counter = counter + 1;
    IF counter > 5 THEN
        LEAVE; 
    END IF;
END;
```
