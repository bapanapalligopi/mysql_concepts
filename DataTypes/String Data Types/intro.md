### 📚 **13.3 String Data Types in MySQL**

---

## 🔹 **13.3.1 String Data Type Syntax**

String data types in MySQL include:

- `CHAR` and `VARCHAR`
- `BINARY` and `VARBINARY`
- `BLOB` and `TEXT`
- `ENUM`
- `SET`

---

## 🔹 **13.3.2 The CHAR and VARCHAR Types**

### ✅ **Syntax:**
```sql
CHAR(M) [CHARACTER SET charset_name] [COLLATE collation_name]
VARCHAR(M) [CHARACTER SET charset_name] [COLLATE collation_name]
```

### 🎯 **CHAR:**
- Fixed-length string.
- `M` specifies length in characters (1 to 255).
- Pads with spaces to the specified length when storing.
- Example:
```sql
CREATE TABLE t1 (
    name CHAR(10)
);
```

### 🎯 **VARCHAR:**
- Variable-length string.
- `M` specifies maximum length (1 to 65,535, depending on row size).
- No padding with spaces.
- Example:
```sql
CREATE TABLE t2 (
    email VARCHAR(100)
);
```

---

## 🔹 **13.3.3 The BINARY and VARBINARY Types**

### ✅ **Syntax:**
```sql
BINARY(M)
VARBINARY(M)
```

### 🎯 **BINARY:**
- Fixed-length binary data.
- `M` specifies length in bytes (1 to 255).
- Pads with `0x00` to match the length.
- Example:
```sql
CREATE TABLE t3 (
    data BINARY(16)
);
```

### 🎯 **VARBINARY:**
- Variable-length binary data.
- `M` specifies the maximum length (1 to 65,535 bytes).
- Example:
```sql
CREATE TABLE t4 (
    file_hash VARBINARY(32)
);
```

---

## 🔹 **13.3.4 The BLOB and TEXT Types**

### ✅ **BLOB:**
- Binary Large Object.
- Stores binary data.
- Four types based on size:
  - `TINYBLOB` (up to 255 bytes)
  - `BLOB` (up to 65,535 bytes or 64 KB)
  - `MEDIUMBLOB` (up to 16,777,215 bytes or 16 MB)
  - `LONGBLOB` (up to 4,294,967,295 bytes or 4 GB)

### ✅ **TEXT:**
- Stores text data.
- Four types based on size:
  - `TINYTEXT` (up to 255 characters)
  - `TEXT` (up to 65,535 characters or 64 KB)
  - `MEDIUMTEXT` (up to 16,777,215 characters or 16 MB)
  - `LONGTEXT` (up to 4,294,967,295 characters or 4 GB)

### 🎯 **Example:**
```sql
CREATE TABLE documents (
    doc_id INT PRIMARY KEY,
    doc_content TEXT,
    doc_file BLOB
);
```

---

## 🔹 **13.3.5 The ENUM Type**

### ✅ **Syntax:**
```sql
ENUM('value1', 'value2', 'value3', ...)
```

- Stores one value from a predefined list.
- Can store up to 65,535 distinct values.
- Case-insensitive by default.
- Example:
```sql
CREATE TABLE orders (
    order_id INT,
    status ENUM('Pending', 'Shipped', 'Delivered', 'Cancelled')
);
```

---

## 🔹 **13.3.6 The SET Type**

### ✅ **Syntax:**
```sql
SET('value1', 'value2', 'value3', ...)
```

- Stores zero or more values from a predefined list.
- Can store up to 64 distinct values.
- Example:
```sql
CREATE TABLE user_preferences (
    user_id INT,
    preferences SET('Email', 'SMS', 'Push', 'Call')
);
```

---

## 🔹 **Summary:**
| Data Type    | Description                         | Max Size        |
|--------------|-------------------------------------|-----------------|
| `CHAR(M)`     | Fixed-length string                 | 255 chars       |
| `VARCHAR(M)`  | Variable-length string               | 65,535 chars    |
| `BINARY(M)`   | Fixed-length binary data             | 255 bytes       |
| `VARBINARY(M)`| Variable-length binary data          | 65,535 bytes    |
| `BLOB`        | Binary Large Object                  | Up to 4 GB      |
| `TEXT`        | Large text data                      | Up to 4 GB      |
| `ENUM`        | One value from a predefined list     | 65,535 values   |
| `SET`         | Multiple values from a predefined list | Up to 64 values |

Let me know if you want detailed examples or explanations! 😊🚀
