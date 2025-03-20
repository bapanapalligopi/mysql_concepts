### 3.1 **Numeric Data Types**

MySQL supports several **numeric data types**, which can be broadly categorized into **exact numeric types** and **approximate numeric types**. Each of these data types is designed to store different kinds of numeric values, such as integers, decimals, and floating-point numbers. Below is an overview of the **numeric data types** and related syntax, storage, and handling.

---

### 13.1.1 **Numeric Data Type Syntax**

Numeric data types in MySQL follow a consistent syntax that allows you to define columns in tables with various constraints for storage and precision. Here's the general syntax for numeric data types in MySQL:

- **Integer Types (Exact Value)**: 
  ```sql
  INTEGER[(M)]
  INT[(M)]
  ```
  - `M` is the display width, but it does **not** affect the storage or range of the value (just the display of the number).

- **Fixed-Point Types (Exact Value)**:
  ```sql
  DECIMAL(M, D)
  NUMERIC(M, D)
  ```
  - `M` is the total number of digits (precision), and `D` is the number of digits after the decimal point (scale).

- **Floating-Point Types (Approximate Value)**:
  ```sql
  FLOAT(M, D)
  DOUBLE(M, D)
  ```
  - `M` and `D` have similar meanings as in the fixed-point types.

- **Bit-Value Type**:
  ```sql
  BIT[(M)]
  ```
  - `M` is the number of bits used to store the value, where `M` can be between 1 and 64.

---

### 13.1.2 **Integer Types (Exact Value)**

Integer types are used to store **whole numbers** (both positive and negative). MySQL provides several types of integers with different storage sizes and ranges:

1. **INTEGER / INT**:
   - **Signed Range**: -2,147,483,648 to 2,147,483,647.
   - **Unsigned Range**: 0 to 4,294,967,295.
   - **Example**:
     ```sql
     CREATE TABLE example (
       id INT UNSIGNED
     );
     ```
     This stores a unique identifier for a row.

2. **SMALLINT**:
   - **Signed Range**: -32,768 to 32,767.
   - **Unsigned Range**: 0 to 65,535.
   - **Example**:
     ```sql
     CREATE TABLE example (
       age SMALLINT
     );
     ```

3. **TINYINT**:
   - **Signed Range**: -128 to 127.
   - **Unsigned Range**: 0 to 255.
   - **Example**:
     ```sql
     CREATE TABLE example (
       status TINYINT UNSIGNED
     );
     ```

4. **MEDIUMINT**:
   - **Signed Range**: -8,388,608 to 8,388,607.
   - **Unsigned Range**: 0 to 16,777,215.

5. **BIGINT**:
   - **Signed Range**: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.
   - **Unsigned Range**: 0 to 18,446,744,073,709,551,615.
   - **Example**:
     ```sql
     CREATE TABLE example (
       transaction_id BIGINT UNSIGNED
     );
     ```

---

### 13.1.3 **Fixed-Point Types (Exact Value)**

Fixed-point types store **exact numeric values** and are primarily used for values that require precision, such as monetary amounts. These types store both whole numbers and decimal fractions.

1. **DECIMAL(M, D)**:
   - `M` is the total number of digits (precision), and `D` is the number of digits after the decimal point (scale).
   - **Example**: `DECIMAL(5, 2)` allows a total of 5 digits, with 2 digits after the decimal point (i.e., it can store values between -999.99 and 999.99).
   - **Usage**:
     ```sql
     CREATE TABLE example (
       price DECIMAL(10, 2)
     );
     ```

2. **NUMERIC(M, D)**:
   - This is essentially identical to **DECIMAL** in MySQL. It is another way to define fixed-point values.
   - **Usage**: 
     ```sql
     CREATE TABLE example (
       total_amount NUMERIC(8, 3)
     );
     ```

---

### 13.1.4 **Floating-Point Types (Approximate Value)**

Floating-point types are used for storing **approximate numeric values**, particularly when you need to store very large or small numbers with a variable number of decimal places.

1. **FLOAT(M, D)**:
   - `M` is the total number of digits (precision), and `D` is the number of digits after the decimal point (scale).
   - **Example**: `FLOAT(10, 2)` allows a total of 10 digits, with 2 digits after the decimal point.
   - **Usage**:
     ```sql
     CREATE TABLE example (
       temperature FLOAT(6, 2)
     );
     ```

2. **DOUBLE(M, D)** (Also referred to as **DOUBLE PRECISION**):
   - **DOUBLE** provides more precision than **FLOAT** and is generally used when greater precision is required.
   - **Usage**:
     ```sql
     CREATE TABLE example (
       area DOUBLE(15, 4)
     );
     ```

3. **REAL**:
   - **REAL** is treated as a synonym for **DOUBLE PRECISION** in MySQL. In some modes (like `REAL_AS_FLOAT`), **REAL** may be treated as **FLOAT**.

---

### 13.1.5 **Bit-Value Type (BIT)**

The **BIT** data type stores **binary data** in the form of bit values. It can hold values from 1 to 64 bits.

- **Usage**:
  ```sql
  CREATE TABLE example (
    flag BIT(1) -- Can store 1 bit (either 0 or 1)
  );
  ```

---

### 13.1.6 **Numeric Type Attributes**

- **SIGNED** (default): Can store both positive and negative values.
- **UNSIGNED**: Can only store **positive** values and effectively doubles the maximum possible value for positive numbers.
  
- **Example**: 
  - `INT UNSIGNED` will allow you to store values from **0 to 4,294,967,295** instead of the signed range of **-2,147,483,648 to 2,147,483,647**.

---

### 13.1.7 **Out-of-Range and Overflow Handling**

MySQL has built-in mechanisms to handle **out-of-range values** and **overflow** conditions during calculations or when inserting values into a table.

- **Overflow**: If an overflow happens (e.g., when inserting a value that exceeds the maximum size of a column), MySQL handles it in a way that depends on the **SQL mode**. By default, it will **truncate** or **return an error**.
- **Out-of-Range Handling**: MySQL allows for overflow by adjusting the result. It may clip values at the maximum or minimum allowable values for that data type, depending on the configuration.

---

### Conclusion

MySQL supports a wide variety of **numeric data types**, each suited for specific use cases, from storing small integers to precise decimal values or large floating-point numbers. Choosing the right numeric data type for a column is important to ensure the correct storage and precision requirements for your data, optimizing both performance and storage space.

Let me know if you'd like to dive deeper into any specific type!
