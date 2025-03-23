### 📅 **The `YEAR` Type in MySQL**

The `YEAR` type is a **1-byte** data type used to represent year values in `YYYY` format.

---

### ✅ **Key Points:**

1. **Format and Range:**
   - Stored and displayed in `YYYY` format.
   - Valid range:  
     `1901` to `2155` and `0000` (used for invalid or zero values).
   - Takes **4 characters** for display.

2. **Deprecated Display Width:**
   - `YEAR(4)` with an explicit display width is **deprecated** as of MySQL 8.0.19.
   - Use just `YEAR` without display width.  
     ✅ Correct: `YEAR`  
     ❌ Deprecated: `YEAR(4)`

3. **2-Digit YEAR Support:**
   - MySQL 8.0 **does not support** the `YEAR(2)` type, which was available in older versions.
   - For migrating from 2-digit `YEAR` to 4-digit `YEAR`, refer to MySQL 5.7 documentation.

---

### 📝 **Accepted Input Formats:**

1. **4-Digit Strings:**
   - String values within the valid range:  
     `'1901'` to `'2155'`  
     Example:  
     ```sql
     INSERT INTO events (event_year) VALUES ('2025');
     ```

2. **4-Digit Numbers:**
   - Integer values between `1901` and `2155`:  
     Example:  
     ```sql
     INSERT INTO events (event_year) VALUES (2030);
     ```

3. **1- or 2-Digit Strings:**
   - Strings between `'0'` and `'99'`:
     - `'0'` to `'69'` → Converted to `2000` to `2069`
     - `'70'` to `'99'` → Converted to `1970` to `1999`
     Example:  
     ```sql
     INSERT INTO events (event_year) VALUES ('22'); -- Inserts 2022
     INSERT INTO events (event_year) VALUES ('99'); -- Inserts 1999
     ```

4. **1- or 2-Digit Numbers:**
   - Numeric values between `0` and `99`:
     - `1` to `69` → Converted to `2001` to `2069`
     - `70` to `99` → Converted to `1970` to `1999`
     Example:  
     ```sql
     INSERT INTO events (event_year) VALUES (18); -- Inserts 2018
     INSERT INTO events (event_year) VALUES (75); -- Inserts 1975
     ```

5. **Zero Values:**
   - Numeric `0` inserts `0000` internally and displays `0000`.  
   - To insert `2000`, use a string format:  
     ```sql
     INSERT INTO events (event_year) VALUES ('0'); -- Inserts 2000
     INSERT INTO events (event_year) VALUES ('00'); -- Inserts 2000
     ```

6. **Functions Returning YEAR Values:**
   - Functions like `NOW()` can be used to get and insert the current year:  
     ```sql
     INSERT INTO events (event_year) VALUES (YEAR(NOW()));
     ```

---

### ⚠️ **Handling Invalid Values:**
- Without **strict SQL mode**:
  - Invalid `YEAR` values are converted to `0000`.
- With **strict SQL mode**:
  - Invalid values produce an **error**.

---

### 🎯 **Tip:**
- Enable **strict SQL mode** to prevent invalid values from being silently converted to `0000`.  
- Example:  
```sql
SET sql_mode = 'STRICT_ALL_TABLES';
```

Let me know if you need help with examples or migrations! 😊
