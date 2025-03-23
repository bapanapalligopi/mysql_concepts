### 🕒 **Automatic Initialization and Updating for `TIMESTAMP` and `DATETIME` in MySQL**

In MySQL, `TIMESTAMP` and `DATETIME` columns can be automatically initialized and updated with the current date and time.

---

### ✅ **Key Concepts:**

1. **Auto-Initialized Column:**
   - Set to the **current timestamp** when a row is inserted, and no value is specified.
   - Use:  
   ```sql
   ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   ```

2. **Auto-Updated Column:**
   - Updated to the **current timestamp** when any other column in the row is modified.
   - Use:  
   ```sql
   ts TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   ```

3. **Auto-Initialized and Auto-Updated:**
   - Automatically initialized and updated with the current timestamp.
   - Use:  
   ```sql
   ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   ```

---

### 📚 **Column Definitions with Examples:**

1. **Default and Auto-Update Enabled:**
   - Auto-initializes and updates to the current timestamp.
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
     dt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   );
   ```

2. **Default Only, No Auto-Update:**
   - Auto-initializes but does not auto-update.
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
     dt DATETIME DEFAULT CURRENT_TIMESTAMP
   );
   ```

3. **Constant Default, No Auto-Update:**
   - Default is a constant value, no automatic updates.
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP DEFAULT '2000-01-01 00:00:00',
     dt DATETIME DEFAULT '2025-12-31 23:59:59'
   );
   ```

4. **Constant Default with Auto-Update:**
   - Default is a constant, but auto-updates on row modification.
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP DEFAULT 0 ON UPDATE CURRENT_TIMESTAMP,
     dt DATETIME DEFAULT '2000-01-01 00:00:00' ON UPDATE CURRENT_TIMESTAMP
   );
   ```

5. **Auto-Update Without Default:**
   - Auto-updates but does **not** auto-initialize.
   ```sql
   CREATE TABLE t1 (
     ts1 TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- default 0
     ts2 TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP -- default NULL
   );
   ```

---

### ⚠️ **Important Considerations:**

1. **Explicit Defaults:**
   - By default, `TIMESTAMP` columns are automatically assigned `DEFAULT CURRENT_TIMESTAMP` and `ON UPDATE CURRENT_TIMESTAMP` if **`explicit_defaults_for_timestamp`** is **disabled**.
   - To avoid this, enable `explicit_defaults_for_timestamp`:
   ```sql
   SET GLOBAL explicit_defaults_for_timestamp = ON;
   ```

2. **NULL Behavior:**
   - Assigning `NULL` to a `TIMESTAMP` column:
     - If the column is declared `NULL`, it sets the value to `NULL`.
     - If the column is `NOT NULL`, assigning `NULL` assigns the **current timestamp** unless `explicit_defaults_for_timestamp` is enabled.
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP NULL DEFAULT NULL,
     ts2 TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP
   );
   ```

---

### 🕰️ **Examples of Automatic Behavior:**

1. **Auto-Initialize and Auto-Update:**
   ```sql
   CREATE TABLE orders (
     id INT AUTO_INCREMENT PRIMARY KEY,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
     updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   );
   ```
   - `created_at` → Set when the row is inserted.
   - `updated_at` → Updated to the current timestamp when the row is modified.

2. **Inserting Rows:**
   ```sql
   INSERT INTO orders VALUES (NULL, NULL, NULL);
   ```
   - `created_at` and `updated_at` set to the current timestamp.
   
3. **Updating Rows:**
   ```sql
   UPDATE orders SET id = 1 WHERE id = 1;
   ```
   - `updated_at` is updated to the current timestamp.

---

### ⚡️ **Special Cases and Edge Cases:**

1. **Auto-Initialization for First `TIMESTAMP` Column:**
   - If `explicit_defaults_for_timestamp` is **disabled**, the first `TIMESTAMP` column gets:
     - `DEFAULT CURRENT_TIMESTAMP`
     - `ON UPDATE CURRENT_TIMESTAMP`
   - Example:
   ```sql
   CREATE TABLE t1 (
     ts1 TIMESTAMP, -- Implicit auto-init and auto-update
     ts2 TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   );
   ```

2. **Handling NULL Values:**
   - If a `TIMESTAMP` or `DATETIME` column is declared `NULL`:
     - Assigning `NULL` sets the value to `NULL`, not the current timestamp.
   ```sql
   CREATE TABLE t2 (
     ts TIMESTAMP NULL DEFAULT NULL
   );
   INSERT INTO t2 VALUES (NULL); -- Inserts NULL
   ```

3. **Fractional Seconds Precision:**
   - If a `TIMESTAMP` or `DATETIME` column uses fractional seconds, ensure consistency:
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP(6) DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(6)
   );
   ```
   ❌ This is invalid:  
   ```sql
   CREATE TABLE t1 (
     ts TIMESTAMP(6) DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP(3)
   );
   ```

---

### 🎯 **Summary:**
- Use `DEFAULT CURRENT_TIMESTAMP` to auto-initialize.
- Use `ON UPDATE CURRENT_TIMESTAMP` to auto-update.
- Set `explicit_defaults_for_timestamp` to control default behavior.
- Explicitly declare `NULL` to permit NULL values and avoid auto-initialization.

Let me know if you need more examples or help with SQL queries! 🚀
