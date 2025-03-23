Got it! Here’s a **theoretical summary** of MySQL’s **Date and Time Data Types**:

---

### 📚 **Overview of Date and Time Data Types**

MySQL provides several data types for storing date and time values. Each type has a defined range, format, and storage requirement. 

### ✅ **1. DATE Type**
- Format: `YYYY-MM-DD`  
- Range: `1000-01-01` to `9999-12-31`  
- Storage: 3 bytes  
- **Zero Value:** `'0000-00-00'`  
- Used to store only date values.

---

### ✅ **2. DATETIME Type**
- Format: `YYYY-MM-DD HH:MM:SS`  
- Range: `1000-01-01 00:00:00` to `9999-12-31 23:59:59`  
- Storage: 8 bytes  
- **Zero Value:** `'0000-00-00 00:00:00'`  
- Stores date and time together without considering time zones.

---

### ✅ **3. TIMESTAMP Type**
- Format: `YYYY-MM-DD HH:MM:SS`  
- Range: `1970-01-01 00:00:01 UTC` to `2038-01-19 03:14:07 UTC`  
- Storage: 4 bytes  
- **Zero Value:** `'0000-00-00 00:00:00'`  
- Automatically records the current timestamp when rows are inserted or updated.
- TIMESTAMP values are stored in UTC and converted to the session’s time zone when retrieved.

---

### ✅ **4. TIME Type**
- Format: `HH:MM:SS`  
- Range: `-838:59:59` to `838:59:59`  
- Storage: 3 bytes  
- **Zero Value:** `'00:00:00'`  
- Used to store only time values, including negative intervals.

---

### ✅ **5. YEAR Type**
- Format: `YYYY` (4-digit year)  
- Range: `1901` to `2155`  
- Storage: 1 byte  
- **Zero Value:** `0000`  
- Used to store year values only.

---

### 🕰️ **Fractional Seconds Support**
- `DATETIME`, `TIMESTAMP`, and `TIME` can store fractional seconds (microseconds) with precision up to 6 digits.  
- Specified using the syntax: `DATETIME(fsp)` or `TIMESTAMP(fsp)` where `fsp` is the fractional seconds precision (0 to 6).

---

### 🔄 **Automatic Initialization and Updating**
- `TIMESTAMP` and `DATETIME` columns can automatically initialize and update using:
    - `DEFAULT CURRENT_TIMESTAMP`
    - `ON UPDATE CURRENT_TIMESTAMP`

---

### 🎯 **Conversion and Format Rules**
- MySQL automatically converts between date and time types depending on context.
- Date literals must be in `YYYY-MM-DD` format, and time values must be in `HH:MM:SS` format.
- Date and time values with **2-digit years** are interpreted as follows:
    - `70-99` → `1970-1999`
    - `00-69` → `2000-2069`

---

### ⚠️ **Handling Invalid or Out-of-Range Values**
- MySQL converts invalid or out-of-range values to a “zero” value unless strict SQL mode is enabled.
- Enabling `STRICT_TRANS_TABLES`, `NO_ZERO_DATE`, and `NO_ZERO_IN_DATE` ensures strict handling of invalid values.

---

### 🗓️ **Calendar System**
- MySQL uses the **Gregorian calendar** for date and time calculations.

---

### 📝 **“Zero” Values for Date and Time Types**

| **Data Type**  | **“Zero” Value**         |
|----------------|------------------------|
| `DATE`         | `'0000-00-00'`         |
| `TIME`         | `'00:00:00'`           |
| `DATETIME`     | `'0000-00-00 00:00:00'` |
| `TIMESTAMP`    | `'0000-00-00 00:00:00'` |
| `YEAR`         | `0000`                  |

---

### 🔍 **Special SQL Modes for Date Handling**
- `ALLOW_INVALID_DATES` – Allows dates with invalid day values (e.g., `2025-11-31`).
- `NO_ZERO_DATE` – Prevents the use of `'0000-00-00'` as a valid date.
- `NO_ZERO_IN_DATE` – Prevents zero parts in date values (e.g., `2025-00-00`).

---

### 🎁 **Key Takeaways**
- Always use 4-digit years to avoid ambiguity.
- Use `DATETIME` for time-insensitive values and `TIMESTAMP` for time-sensitive data.
- Enable strict SQL modes to prevent unexpected behavior with invalid or incomplete dates.

---

Let me know if you'd like more details or need assistance with anything else! 😊
