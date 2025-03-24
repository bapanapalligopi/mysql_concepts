### 📚 **MySQL String Data Types in a Single Table with Examples**

---

### 🎯 **Table Creation:**
```sql
CREATE TABLE string_data_types (
    id INT PRIMARY KEY,
    char_col CHAR(10),
    varchar_col VARCHAR(50),
    tinytext_col TINYTEXT,
    text_col TEXT,
    mediumtext_col MEDIUMTEXT,
    blob_col BLOB,
    enum_col ENUM('small', 'medium', 'large'),
    set_col SET('a', 'b', 'c', 'd')
);
```

---

### 📌 **Insert Sample Data:**
```sql
INSERT INTO string_data_types (
    id, char_col, varchar_col, tinytext_col, text_col, mediumtext_col, blob_col, enum_col, set_col
)
VALUES
(1, 'ABC', 'Variable text', 'Short txt', 'This is TEXT data', REPEAT('M', 500), 'binarydata1', 'small', 'a,d'),
(2, 'XYZ12', 'Another text', 'Tiny text', REPEAT('LARGE TEXT ', 50), 'Medium text here', 'binarydata2', 'medium', 'b,c'),
(3, 'PQR', 'Short txt', 'Text data', 'This is a longer TEXT', REPEAT('T', 1000), 'binarydata3', 'large', 'a,c,d'),
(4, '', 'Empty varchar', '', '', '', '', NULL, ''),
(5, '12345', 'Test123', 'Test text', 'Another TEXT example', 'Medium sample text', 'binarydata4', 'small', 'b')
```

---

### ✅ **Retrieve Data:**
```sql
SELECT * FROM string_data_types;
```

---

### 📊 **Sample Output:**

| id  | char_col  | varchar_col      | tinytext_col | text_col            | mediumtext_col | blob_col     | enum_col | set_col |
|-----|-----------|------------------|--------------|---------------------|----------------|--------------|----------|---------|
| 1   | ABC       | Variable text    | Short txt    | This is TEXT data   | MMMMM...       | binarydata1  | small    | a,d     |
| 2   | XYZ12     | Another text     | Tiny text    | LARGE TEXT ...      | Medium text... | binarydata2  | medium   | b,c     |
| 3   | PQR       | Short txt        | Text data    | This is a longer... | TTTTTT...      | binarydata3  | large    | a,c,d   |
| 4   |           | Empty varchar    |              |                     |                |              | NULL     |         |
| 5   | 12345     | Test123          | Test text    | Another TEXT ex... | Medium sample  | binarydata4  | small    | b       |

---

### ⚡️ **Explanation:**
1. `char_col` – Fixed-length string, right-padded with spaces.
2. `varchar_col` – Variable-length string with no padding.
3. `tinytext_col` – Small text string (up to 255 characters).
4. `text_col` – Large text string (up to 65,535 characters).
5. `mediumtext_col` – Very large text (up to 16MB).
6. `blob_col` – Binary data storage.
7. `enum_col` – Single choice from predefined options.
8. `set_col` – Multiple choice from predefined options.

✅ Let me know if you want to modify this or add more examples! 😎
