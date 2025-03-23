### ⏱️ **Fractional Seconds in Time Values for MySQL**

MySQL provides **fractional seconds support** for `TIME`, `DATETIME`, and `TIMESTAMP` types, with a precision of **up to 6 digits** (microseconds).

---

### ✅ **Key Concepts:**

1. **Defining Fractional Seconds:**
   - Use the syntax:
   ```sql
   type_name(fsp)
   ```
   - `type_name` → `TIME`, `DATETIME`, or `TIMESTAMP`
   - `fsp` → Fractional seconds precision (range: `0` to `6`)

   ### 📝 **Example:**
   ```sql
   CREATE TABLE t1 (
     t TIME(3),          -- TIME with 3 fractional digits
     dt DATETIME(6)      -- DATETIME with 6 fractional digits
   );
   ```

---

### 🔢 **Fractional Seconds Precision (`fsp`)**

- Valid range: `0` to `6`
- `0` → No fractional seconds
- If omitted, the default is `0` (which is different from the SQL standard, which defaults to `6`).

---

### 📚 **Inserting and Rounding/Truncation:**

When inserting values with fractional seconds into columns with **fewer fractional digits**, MySQL **rounds** the values.

---

### 🎯 **Example:**
```sql
CREATE TABLE fractest (
  c1 TIME(2),
  c2 DATETIME(2),
  c3 TIMESTAMP(2)
);

INSERT INTO fractest VALUES
('17:51:04.777', '2018-09-08 17:51:04.777', '2018-09-08 17:51:04.777');

SELECT * FROM fractest;
```

#### 📊 **Result (Rounded Values):**
```
+-------------+------------------------+------------------------+
| c1          | c2                     | c3                     |
+-------------+------------------------+------------------------+
| 17:51:04.78 | 2018-09-08 17:51:04.78 | 2018-09-08 17:51:04.78 |
+-------------+------------------------+------------------------+
```
- Fractional seconds are **rounded** if they exceed the specified precision.

---

### ✂️ **Truncation Instead of Rounding:**

To insert values with **truncation** (instead of rounding), enable the `TIME_TRUNCATE_FRACTIONAL` SQL mode.

```sql
SET @@sql_mode = sys.list_add(@@sql_mode, 'TIME_TRUNCATE_FRACTIONAL');
```

#### 📊 **Result (Truncated Values):**
```
+-------------+------------------------+------------------------+
| c1          | c2                     | c3                     |
+-------------+------------------------+------------------------+
| 17:51:04.77 | 2018-09-08 17:51:04.77 | 2018-09-08 17:51:04.77 |
+-------------+------------------------+------------------------+
```
- Fractional seconds are **truncated** when this mode is enabled.

---

### ⏰ **Using Fractional Seconds with Functions**

Functions that accept temporal arguments also support fractional seconds.

- **`NOW()`** with precision:
   - `NOW()` → No fractional seconds
   - `NOW(3)` → Returns fractional seconds up to 3 digits
   - `NOW(6)` → Returns full microseconds
   ```sql
   SELECT NOW(), NOW(3), NOW(6);
   ```
   📊 **Result:**
   ```
   +---------------------+-------------------------+----------------------------+
   | NOW()               | NOW(3)                  | NOW(6)                     |
   +---------------------+-------------------------+----------------------------+
   | 2025-03-23 14:30:00  | 2025-03-23 14:30:00.123 | 2025-03-23 14:30:00.123456 |
   +---------------------+-------------------------+----------------------------+
   ```

---

### 📆 **Using Temporal Literals with Fractional Seconds**

MySQL allows you to create temporal values with fractional seconds using standard SQL and ODBC syntax.

- **DATE 'str'**
- **TIME 'str'**
- **TIMESTAMP 'str'**

```sql
SELECT TIME '12:34:56.789' AS t,
       DATETIME '2025-03-23 12:34:56.789123' AS dt,
       TIMESTAMP '2025-03-23 12:34:56.789123' AS ts;
```

---

### 🕰️ **Handling Precision Mismatches**

- **Higher Precision → Lower Precision:**
   - Fractional seconds are either rounded or truncated, depending on SQL mode.
   ```sql
   CREATE TABLE t1 (
     t1 TIME(3),
     t2 DATETIME(2)
   );

   INSERT INTO t1 VALUES
   ('12:34:56.789123', '2025-03-23 12:34:56.789');
   ```

- **Lower Precision → Higher Precision:**
   - Fractional seconds are preserved up to the precision defined in the column.

---

### ⚡️ **Special Considerations:**

1. **Invalid `fsp` Values:**
   - A value of `fsp` outside the range (`0-6`) will produce an error.
   ```sql
   CREATE TABLE t1 (t TIME(7)); -- Error: Invalid precision
   ```

2. **Mixing Fractional Precision:**
   - A single column cannot mix different fractional seconds precisions.
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP(6) DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(3)
   );
   -- Error: Mixed precision in the same column definition
   ```

3. **Default Behavior:**
   - If `fsp` is **not specified**, the default is `0` (no fractional seconds).

---

### 🎯 **Summary:**
- `fsp` allows up to 6 digits of fractional seconds.
- Rounding occurs unless `TIME_TRUNCATE_FRACTIONAL` is enabled.
- Temporal functions and literals support fractional seconds.
- Be consistent when defining fractional precision for columns.

Let me know if you need help with queries or have more questions! 😊
