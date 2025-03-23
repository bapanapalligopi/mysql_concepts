### 🔄 **13.2.8 Conversion Between Date and Time Types in MySQL**

MySQL allows **conversion** between different temporal types (DATE, TIME, DATETIME, and TIMESTAMP), but such conversions may result in:

- ✅ **Loss of Information** (truncation of time or date parts).
- ⚠️ **Alteration of Values** (rounding or shifts due to time interpretation).
- ❗ **Invalid Values** (out-of-range conversions are replaced with `0` or `0000-00-00`).

---

## 📅 **Conversion of DATE Values**

1. **DATE → DATETIME or TIMESTAMP:**
   - A DATE value has no time part.
   - When converted to DATETIME or TIMESTAMP, MySQL **adds a time part of `'00:00:00'`**.
   ```sql
   SELECT CAST('2025-03-23' AS DATETIME); 
   -- Result: 2025-03-23 00:00:00
   ```

2. **DATE → TIME:**
   - Conversion results in `'00:00:00'` since DATE has no time component.
   ```sql
   SELECT CAST('2025-03-23' AS TIME); 
   -- Result: 00:00:00
   ```

---

## ⏰ **Conversion of DATETIME and TIMESTAMP Values**

1. **DATETIME/TIMESTAMP → DATE:**
   - **Rounding Behavior:**
   - Time part is rounded when converting to DATE.
   - If the time is ≥ `12:00:00`, the date is **rounded up** to the next day.
   ```sql
   SELECT CAST('1999-12-31 23:59:59.499' AS DATE); 
   -- Result: 1999-12-31
   SELECT CAST('1999-12-31 23:59:59.500' AS DATE);
   -- Result: 2000-01-01
   ```

2. **DATETIME/TIMESTAMP → TIME:**
   - The date part is **discarded**, and only the time is retained.
   ```sql
   SELECT CAST('2025-03-23 18:45:30' AS TIME);
   -- Result: 18:45:30
   ```

---

## ⏳ **Conversion of TIME Values**

### 1. **TIME → DATETIME or TIMESTAMP:**
- The date part is taken from the **current date** (`CURRENT_DATE()`), and the TIME is interpreted as:
  - ✅ **Elapsed Time** – Added to the current date.
  - ⏫ **Time Greater Than 24 Hours:** Adds extra days.
  - ⏬ **Negative Time Values:** Subtracts days.

```sql
-- Assume CURRENT_DATE() = '2025-03-23'
SELECT CAST('12:00:00' AS DATETIME);
-- Result: 2025-03-23 12:00:00

SELECT CAST('24:00:00' AS DATETIME);
-- Result: 2025-03-24 00:00:00

SELECT CAST('-12:00:00' AS DATETIME);
-- Result: 2025-03-22 12:00:00
```

### 2. **TIME → DATE:**
- Similar behavior, but only the date part is returned.
```sql
SELECT CAST('12:00:00' AS DATE);
-- Result: 2025-03-23

SELECT CAST('-12:00:00' AS DATE);
-- Result: 2025-03-22
```

---

## 🔢 **Conversion to Numeric Values**

### 1. **TIME → Integer or Decimal:**
- Conversion behavior depends on whether the TIME value has **fractional seconds**.
- ✅ No fractional seconds: Converted to an integer.
- ✅ Fractional seconds: Converted to a **DECIMAL** with the appropriate precision.

```sql
SELECT CURTIME(), CURTIME() + 0, CURTIME(3) + 0;
```
Result:
```
+-----------+-------------+--------------+
| CURTIME() | CURTIME()+0 | CURTIME(3)+0 |
+-----------+-------------+--------------+
| 09:28:00  |       92800  |    92800.887 |
+-----------+-------------+--------------+
```

### 2. **DATETIME/TIMESTAMP → Numeric:**
- Follows the same rules as TIME.
```sql
SELECT NOW(), NOW() + 0, NOW(3) + 0;
```
Result:
```
+---------------------+----------------+--------------------+
| NOW()               | NOW()+0        | NOW(3)+0           |
+---------------------+----------------+--------------------+
| 2025-03-23 09:28:00 | 20250323092800 | 20250323092800.889 |
+---------------------+----------------+--------------------+
```

---

## 🎯 **Explicit Conversion with `CAST()` or `CONVERT()`**

You can use **explicit conversion** to override implicit conversions.

```sql
-- Convert DATETIME to DATE
SELECT CAST('2025-03-23 12:34:56' AS DATE);
-- Result: 2025-03-23

-- Convert DATE to DATETIME
SELECT CAST('2025-03-23' AS DATETIME);
-- Result: 2025-03-23 00:00:00

-- Convert TIME to DATETIME
SELECT CAST('12:34:56' AS DATETIME);
-- Result: 2025-03-23 12:34:56 (Assumes current date)
```

---

## ⚠️ **Limitations and Errors**

1. **TIMESTAMP Range Limitation:**
   - TIMESTAMP values must be between:
   - ✅ **1970-01-01 00:00:01 UTC** and **2038-01-19 03:14:07 UTC**.
   - Dates outside this range convert to `0`.
   ```sql
   SELECT CAST('1968-01-01' AS TIMESTAMP);
   -- Result: 0000-00-00 00:00:00
   ```

2. **Implicit Conversion May Cause Unintended Results:**
   ```sql
   SELECT '2025-03-23' = CAST('2025-03-23 12:00:00' AS DATE);
   -- Result: 1 (TRUE)

   SELECT '2025-03-23' = '2025-03-23 12:00:00';
   -- Result: 0 (FALSE, because types are different)
   ```

---

## 🚀 **Summary:**
- Conversion between temporal types is possible but may result in **truncation, rounding, or value alteration**.
- Use **explicit conversion** (via `CAST()` or `CONVERT()`) for control over the conversion.
- TIMESTAMP values are restricted to the range **1970–2038**.
- Be mindful of implicit conversions in comparisons.

If you need more examples or have questions about specific scenarios, let me know! 😊
