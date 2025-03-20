### 13.1.3 **Fixed-Point Types (Exact Value) - DECIMAL, NUMERIC**

MySQL provides the **DECIMAL** and **NUMERIC** types, which are used to store exact numeric values with **fixed precision** and **scale**. These data types are commonly used when exact precision is required, such as in financial calculations, monetary values, and other applications where approximation errors are unacceptable.

### Key Points:
- **DECIMAL** and **NUMERIC** are used for **exact numeric data types**, unlike floating-point types which are approximate.
- **NUMERIC** is implemented as **DECIMAL** in MySQL, so they are equivalent.
- These types store **decimal values** in **binary format** (for internal storage).

### Precision and Scale:
- **Precision (M)**: The total number of digits that can be stored in the number.
- **Scale (D)**: The number of digits that can be stored after the decimal point.
  
For example:
```sql
salary DECIMAL(5,2)
```
- **Precision (5)**: This means a total of 5 digits can be stored.
- **Scale (2)**: This means 2 digits can be stored after the decimal point.

#### Range of `DECIMAL(5,2)`:
- The possible values for this column would be in the range **-999.99 to 999.99**.
  
---

### **Syntax Explanation:**

1. **DECIMAL(M, D)**: 
   - `M` is the **precision** (the total number of digits), and `D` is the **scale** (the number of digits after the decimal point).
   - If **D = 0**, then the column stores **whole numbers** (i.e., no decimal places).

   Example:
   ```sql
   CREATE TABLE products (
       price DECIMAL(5, 2)
   );
   ```

   Here, the **price** column can store values ranging from **-999.99** to **999.99**.

2. **DECIMAL(M)** or **DECIMAL**:
   - If **scale** is not specified, it is assumed to be **0** (i.e., no fractional part).
   
   Example:
   ```sql
   CREATE TABLE inventory (
       quantity DECIMAL(5)
   );
   ```
   - Here, the **quantity** column will store whole numbers in the range **-99999 to 99999**.

3. **Maximum Digits**: 
   - The maximum **precision** (`M`) allowed is **65** digits, and the **scale** (`D`) can go up to **30** (but usually much lower, depending on the database application).
   
   Example:
   ```sql
   CREATE TABLE orders (
       total_amount DECIMAL(65, 30)
   );
   ```
   - This would allow for **total_amount** to store values with up to 35 digits before the decimal point and 30 digits after the decimal point.

---

### **Storage and Behavior:**
- **Binary Storage**: Internally, MySQL stores **DECIMAL** values in a binary format, ensuring exact storage without floating-point inaccuracies.
  
- **Truncation on Insertion**: When inserting a value with more digits after the decimal point than specified by the **scale**, MySQL will **truncate** the value to fit the scale. 

   Example:
   ```sql
   CREATE TABLE transactions (
       amount DECIMAL(5, 2)
   );
   
   INSERT INTO transactions (amount) VALUES (123.456);
   -- The value will be truncated to 123.45
   ```

   In this case, since the **scale** is 2, any extra digits after the second decimal place will be discarded.

---

### **Examples of Creating Tables and Inserting Data:**

1. **Creating a Table with DECIMAL Data Type:**
   ```sql
   CREATE TABLE financial_records (
       id INT PRIMARY KEY,
       salary DECIMAL(10, 2)  -- Maximum of 10 digits, with 2 decimal places
   );
   ```

2. **Inserting Values:**
   ```sql
   INSERT INTO financial_records (id, salary)
   VALUES (1, 99999.99),  -- Valid value within the range
          (2, 1234.50);   -- Valid value with two decimal places
   ```

3. **Inserting a Value That Causes Truncation (if precision/scale is violated):**
   ```sql
   INSERT INTO financial_records (id, salary)
   VALUES (3, 1234.5678);  -- This will be truncated to 1234.56
   ```

4. **Overflow Example (Value Exceeds Precision):**
   ```sql
   INSERT INTO financial_records (id, salary)
   VALUES (4, 1234567890.12); -- This will give an error since 1234567890 is beyond the maximum precision of 10
   ```

---

### **Handling Edge Cases and Best Practices**:

- **Precision Limits**: Always ensure that the precision and scale defined for DECIMAL columns match the expected values. For instance, using `DECIMAL(5, 2)` allows only values up to **999.99** (not beyond that).
- **Monetary Calculations**: This type is very useful for monetary values where exact precision is necessary. For example, the **total_price** of a product should be stored as `DECIMAL(10, 2)` to ensure accurate representation of currency.
  
Let me know if you need further details or examples on how to use DECIMAL for different scenarios!
