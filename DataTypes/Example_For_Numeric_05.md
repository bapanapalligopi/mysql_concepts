Sure! I'll walk you through creating the table and then inserting values that would trigger **overflow** errors, along with some valid insert statements.

### Create Table:

```sql
CREATE TABLE datatype_example (
    id BIGINT UNSIGNED,
    adhar_number INT,
    phone_number MEDIUMINT,
    otp SMALLINT,
    age TINYINT
);
```

### Explanation of the Table:
- **id**: BIGINT UNSIGNED, will store large values (0 to 18,446,744,073,709,551,615).
- **adhar_number**: INT, stores a typical 10-digit Aadhar number.
- **phone_number**: MEDIUMINT, stores a 7-digit phone number.
- **otp**: SMALLINT, stores a 6-digit OTP.
- **age**: TINYINT, stores an age (0 to 255, but realistically 0 to 127).

---

### **Overflow Scenario Example**:
1. **For `age` (TINYINT)**: If you try to insert a value larger than 127 (the maximum for signed **TINYINT**), MySQL will give an overflow error.
2. **For `phone_number` (MEDIUMINT)**: If you insert a number larger than 16,777,215 (the maximum for **MEDIUMINT UNSIGNED**), it will also result in an overflow.
3. **For `otp` (SMALLINT)**: Inserting a value larger than 32,767 (maximum for signed **SMALLINT**) will cause an overflow.

---

### **Overflow Example Insertions**:

#### 1. **Overflow for `age` (TINYINT)**:
```sql
-- This will result in an overflow error as the value 130 is beyond the maximum limit of 127 for signed TINYINT
INSERT INTO datatype_example (id, adhar_number, phone_number, otp, age)
VALUES (1, 123456789, 9876543, 12345, 130);
```

#### 2. **Overflow for `phone_number` (MEDIUMINT)**:
```sql
-- This will result in an overflow error as the value 20,000,000 exceeds the maximum limit of 16,777,215 for UNSIGNED MEDIUMINT
INSERT INTO datatype_example (id, adhar_number, phone_number, otp, age)
VALUES (2, 123456789, 20000000, 54321, 25);
```

#### 3. **Overflow for `otp` (SMALLINT)**:
```sql
-- This will result in an overflow error as the value 35,000 exceeds the maximum limit of 32,767 for signed SMALLINT
INSERT INTO datatype_example (id, adhar_number, phone_number, otp, age)
VALUES (3, 987654321, 1234567, 35000, 45);
```

---

### **Valid Insert Statements** (without overflow):

#### 1. **Valid Insert for all fields**:
```sql
INSERT INTO datatype_example (id, adhar_number, phone_number, otp, age)
VALUES (1, 123456789, 9876543, 12345, 25);
```

#### 2. **Valid Insert with different values**:
```sql
INSERT INTO datatype_example (id, adhar_number, phone_number, otp, age)
VALUES (2, 987654321, 1234567, 9876, 45);
```

#### 3. **Another valid insert**:
```sql
INSERT INTO datatype_example (id, adhar_number, phone_number, otp, age)
VALUES (3, 112233445, 2345678, 1234, 35);
```

---

### **Handling Overflow Errors**:

When MySQL encounters a value that exceeds the defined range of a column type, it will return an error like:

```
ERROR 1264 (22003): Out of range value for column 'age' at row 1
```

To **avoid overflow errors**, you can:
1. **Ensure data validation**: Before inserting data into the database, validate that it falls within the acceptable range for each column.
2. **Use appropriate data types**: If a field might require larger values, adjust the data type accordingly (e.g., using `INT` instead of `TINYINT` for age).

Let me know if you'd like to explore other edge cases or error handling strategies!
