### 13.1.2 **Integer Types (Exact Value) - INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT**

MySQL provides several integer types for storing **whole numbers** (both positive and negative). These integer types vary in **storage size** and the **range of values** they can hold. Here's a breakdown of the different integer types in MySQL:

---

### **Integer Types Supported by MySQL**

| **Type**       | **Storage (Bytes)** | **Minimum Value (Signed)** | **Minimum Value (Unsigned)** | **Maximum Value (Signed)** | **Maximum Value (Unsigned)** |
|----------------|---------------------|----------------------------|------------------------------|----------------------------|------------------------------|
| **TINYINT**    | 1                   | -128                       | 0                            | 127                        | 255                          |
| **SMALLINT**   | 2                   | -32,768                    | 0                            | 32,767                     | 65,535                       |
| **MEDIUMINT**  | 3                   | -8,388,608                 | 0                            | 8,388,607                  | 16,777,215                   |
| **INT**        | 4                   | -2,147,483,648             | 0                            | 2,147,483,647              | 4,294,967,295                |
| **BIGINT**     | 8                   | -9,223,372,036,854,775,808 | 0                            | 9,223,372,036,854,775,807  | 18,446,744,073,709,551,615  |

---

### **Explanation of Each Integer Type**

1. **TINYINT (1 Byte)**:
   - **Storage Size**: 1 byte (8 bits).
   - **Signed Range**: From **-128** to **127**.
   - **Unsigned Range**: From **0** to **255**.
   - **Use Case**: Best for storing small numbers (e.g., boolean flags, status codes).

   Example:
   ```sql
   CREATE TABLE example (
     status TINYINT UNSIGNED
   );
   ```

2. **SMALLINT (2 Bytes)**:
   - **Storage Size**: 2 bytes (16 bits).
   - **Signed Range**: From **-32,768** to **32,767**.
   - **Unsigned Range**: From **0** to **65,535**.
   - **Use Case**: Suitable for values like short codes, small counters, or limits.

   Example:
   ```sql
   CREATE TABLE example (
     age SMALLINT
   );
   ```

3. **MEDIUMINT (3 Bytes)**:
   - **Storage Size**: 3 bytes (24 bits).
   - **Signed Range**: From **-8,388,608** to **8,388,607**.
   - **Unsigned Range**: From **0** to **16,777,215**.
   - **Use Case**: Can store medium-range values, like population counts or sales figures.

   Example:
   ```sql
   CREATE TABLE example (
     product_quantity MEDIUMINT UNSIGNED
   );
   ```

4. **INT (4 Bytes)**:
   - **Storage Size**: 4 bytes (32 bits).
   - **Signed Range**: From **-2,147,483,648** to **2,147,483,647**.
   - **Unsigned Range**: From **0** to **4,294,967,295**.
   - **Use Case**: Commonly used for general-purpose integer fields, such as user IDs or item counts.

   Example:
   ```sql
   CREATE TABLE example (
     user_id INT UNSIGNED
   );
   ```

5. **BIGINT (8 Bytes)**:
   - **Storage Size**: 8 bytes (64 bits).
   - **Signed Range**: From **-9,223,372,036,854,775,808** to **9,223,372,036,854,775,807**.
   - **Unsigned Range**: From **0** to **18,446,744,073,709,551,615**.
   - **Use Case**: Best for very large numbers, like large-scale transaction systems, or unique identifiers in very large databases.

   Example:
   ```sql
   CREATE TABLE example (
     transaction_id BIGINT UNSIGNED
   );
   ```

---

### **Choosing the Right Integer Type**

- **Memory Optimization**: Use the smallest integer type that fits your data requirements. For instance, use **TINYINT** if you need to store values between **0** and **255**.
- **Unsigned vs Signed**: If your values will only be positive, consider using **unsigned** integers to double the range of positive values.
- **Use Case Examples**:
  - **TINYINT**: Use for fields like **status flags** (e.g., 0 or 1) or small categorical values.
  - **BIGINT**: Use for **unique identifiers** in systems that require a large number of entries, such as in large-scale transaction systems.

Let me know if you need any further details or examples!
