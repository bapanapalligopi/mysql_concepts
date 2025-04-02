Here is the **complete syntax** for creating a table in SQL, covering all the options and features described earlier, with explanations embedded within the syntax itself.

### Complete SQL Syntax for `CREATE TABLE` (with all options included):

```sql
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name (
    -- Column definitions
    col_name data_type [NOT NULL | NULL] [DEFAULT {literal | (expr)}]
          [VISIBLE | INVISIBLE]
          [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY]
          [COMMENT 'string']
          [COLLATE collation_name]
          [COLUMN_FORMAT {FIXED | DYNAMIC | DEFAULT}]
          [ENGINE_ATTRIBUTE [=] 'string']
          [SECONDARY_ENGINE_ATTRIBUTE [=] 'string']
          [STORAGE {DISK | MEMORY}]
          [reference_definition]
          [check_constraint_definition],

    -- More columns can follow...
    col_name2 data_type [OPTIONS],

    -- Index or Key Definitions
    {INDEX | KEY} [index_name] [index_type] (key_part,...)
          [index_option] ...,
    {FULLTEXT | SPATIAL} [INDEX | KEY] [index_name] (key_part,...)
          [index_option] ...,

    -- Constraints
    CONSTRAINT [symbol] PRIMARY KEY [index_type] (key_part,...) [index_option] ...,
    CONSTRAINT [symbol] UNIQUE [INDEX | KEY] [index_name] [index_type] (key_part,...) [index_option] ...,
    CONSTRAINT [symbol] FOREIGN KEY (col_name,...)
          reference_definition,

    -- Check Constraints
    CONSTRAINT [symbol] CHECK (expr) [[NOT] ENFORCED]
) 

-- Table Options
[table_options]

-- Partition Options
[partition_options];

-- Optional: Create Table from another table with SELECT
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    [IGNORE | REPLACE]
    [AS] query_expression;

-- Optional: Create Table Like Another Table
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    LIKE old_tbl_name;

-- Optional: Partitioning Options
partition_options:
    PARTITION BY
        { [LINEAR] HASH(expr)
        | [LINEAR] KEY [ALGORITHM={1 | 2}] (column_list)
        | RANGE{(expr) | COLUMNS(column_list)}
        | LIST{(expr) | COLUMNS(column_list)} }
    [PARTITIONS num]
    [SUBPARTITION BY
        { [LINEAR] HASH(expr)
        | [LINEAR] KEY [ALGORITHM={1 | 2}] (column_list) }
      [SUBPARTITIONS num]
    ]
    [(partition_definition [, partition_definition] ...)]

partition_definition:
    PARTITION partition_name
        [VALUES
            {LESS THAN {(expr | value_list) | MAXVALUE}
            |
            IN (value_list)}]
        [[STORAGE] ENGINE [=] engine_name]
        [COMMENT [=] 'string']
        [DATA DIRECTORY [=] 'data_dir']
        [INDEX DIRECTORY [=] 'index_dir']
        [MAX_ROWS [=] max_number_of_rows]
        [MIN_ROWS [=] min_number_of_rows]
        [TABLESPACE [=] tablespace_name]
        [(subpartition_definition [, subpartition_definition] ...)]

subpartition_definition:
    SUBPARTITION logical_name
        [[STORAGE] ENGINE [=] engine_name]
        [COMMENT [=] 'string']
        [DATA DIRECTORY [=] 'data_dir']
        [INDEX DIRECTORY [=] 'index_dir']
        [MAX_ROWS [=] max_number_of_rows]
        [MIN_ROWS [=] min_number_of_rows]
        [TABLESPACE [=] tablespace_name]

-- Table options
table_option: {
    AUTOEXTEND_SIZE [=] value
  | AUTO_INCREMENT [=] value
  | AVG_ROW_LENGTH [=] value
  | [DEFAULT] CHARACTER SET [=] charset_name
  | CHECKSUM [=] {0 | 1}
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'string'
  | COMPRESSION [=] {'ZLIB' | 'LZ4' | 'NONE'}
  | CONNECTION [=] 'connect_string'
  | {DATA | INDEX} DIRECTORY [=] 'absolute path to directory'
  | DELAY_KEY_WRITE [=] {0 | 1}
  | ENCRYPTION [=] {'Y' | 'N'}
  | ENGINE [=] engine_name
  | ENGINE_ATTRIBUTE [=] 'string'
  | INSERT_METHOD [=] { NO | FIRST | LAST }
  | KEY_BLOCK_SIZE [=] value
  | MAX_ROWS [=] value
  | MIN_ROWS [=] value
  | PACK_KEYS [=] {0 | 1 | DEFAULT}
  | PASSWORD [=] 'string'
  | ROW_FORMAT [=] {DEFAULT | DYNAMIC | FIXED | COMPRESSED | REDUNDANT | COMPACT}
  | START TRANSACTION
  | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
  | STATS_AUTO_RECALC [=] {DEFAULT | 0 | 1}
  | STATS_PERSISTENT [=] {DEFAULT | 0 | 1}
  | STATS_SAMPLE_PAGES [=] value
  | tablespace_option
  | UNION [=] (tbl_name[,tbl_name]...)
}

-- Tablespace Options (Optional)
tablespace_option: {
    TABLESPACE tablespace_name [STORAGE DISK]
  | [TABLESPACE tablespace_name] STORAGE MEMORY
}

-- Index options
index_option: {
    KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'
  | {VISIBLE | INVISIBLE}
  | ENGINE_ATTRIBUTE [=] 'string'
  | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
}

-- Foreign key reference definition
reference_definition:
    REFERENCES tbl_name (key_part,...)
      [MATCH FULL | MATCH PARTIAL | MATCH SIMPLE]
      [ON DELETE reference_option]
      [ON UPDATE reference_option]

-- Reference options (On Delete/Update actions)
reference_option: {
    RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT
}

-- Check constraints
check_constraint_definition: {
    [CONSTRAINT [symbol]] CHECK (expr) [[NOT] ENFORCED]
}
```

### Key Elements of This Syntax:

#### 1. **Column Definition:**
   Each column can be defined with:
   - **data_type**: Type of data (e.g., `VARCHAR`, `INT`).
   - **NOT NULL / NULL**: If the column can accept NULL values.
   - **DEFAULT**: A default value for the column if no value is provided.
   - **AUTO_INCREMENT**: Automatically increments for integer columns (commonly used for primary keys).
   - **UNIQUE**: Ensures all values in the column are unique.
   - **PRIMARY KEY**: Marks the column as the primary key.
   - **COMMENT**: Adds a comment about the column.
   - **COLLATE**: Defines the collation for character string columns.

#### 2. **Indexes:**
   - **INDEX / KEY**: Used to create indexes.
   - **FULLTEXT / SPATIAL**: For full-text or spatial indexes.
   - **index_type**: Type of index (e.g., `BTREE`, `HASH`).
   - **key_part**: The column(s) included in the index.
   - **index_option**: Additional settings for the index.

#### 3. **Constraints:**
   - **PRIMARY KEY**: Uniquely identifies rows in the table.
   - **UNIQUE**: Ensures all values in a column are unique.
   - **FOREIGN KEY**: Links to another table’s primary key, ensuring referential integrity.
   - **CHECK**: Ensures that column values meet a specified condition.

#### 4. **Table Options:**
   - **ENGINE**: Specifies the storage engine (e.g., `InnoDB`, `MyISAM`).
   - **CHARACTER SET / COLLATE**: Defines the character set and collation for the table.
   - **ROW_FORMAT**: Defines the storage format (e.g., `DYNAMIC`, `FIXED`).
   - **MAX_ROWS / MIN_ROWS**: Limits the number of rows for the table.
   - **COMMENT**: Adds a comment for the table.
   - **STATS_AUTO_RECALC**: Auto recalculation of table statistics.

#### 5. **Partitioning:**
   - **PARTITION BY**: Defines the partitioning strategy for the table (e.g., `RANGE`, `LIST`, `HASH`).
   - **SUBPARTITION BY**: Defines subpartitions for partitions.
   - **partition_definition**: Details each partition.
   - **subpartition_definition**: Details subpartitions.

#### 6. **Table Creation from Query:**
   - **AS query_expression**: Creates a table from the result of a query (e.g., `SELECT` statement).

---

This complete syntax covers all aspects of `CREATE TABLE`, including partitioning, indexes, constraints, and advanced options like encryption, row formats, and statistics. If you need clarification on any specific part, feel free to ask!
