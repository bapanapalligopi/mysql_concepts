Let's walk through a practical example of using `START TRANSACTION`, `COMMIT`, and `ROLLBACK` with two tables, `accounts` and `transactions`, to give you a clear understanding of how transactions work.

### Step 1: Table Setup

First, let's create two simple tables:

1. `accounts`: This table stores account information.
2. `transactions`: This table stores the history of transactions.

```sql
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    account_name VARCHAR(50),
    balance DECIMAL(10, 2)
);

CREATE TABLE transactions (
    transaction_id INT AUTO_INCREMENT PRIMARY KEY,
    account_id INT,
    amount DECIMAL(10, 2),
    transaction_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (account_id) REFERENCES accounts(account_id)
);
```

We'll populate these tables with some data:

```sql
-- Insert some sample data into the accounts table
INSERT INTO accounts (account_id, account_name, balance)
VALUES (1, 'John Doe', 1000.00),
       (2, 'Jane Smith', 2000.00);

-- Insert sample transactions
INSERT INTO transactions (account_id, amount)
VALUES (1, 200.00),
       (2, 300.00);
```

Now, we have two accounts:
- **John Doe** with a balance of 1000.00
- **Jane Smith** with a balance of 2000.00

And two transactions:
- A transaction of 200.00 for **John Doe**
- A transaction of 300.00 for **Jane Smith**

### Step 2: Transaction Example with `START TRANSACTION`, `COMMIT`, and `ROLLBACK`

#### Scenario:
We want to transfer 100.00 from **John Doe**'s account to **Jane Smith**'s account. If any part of the transaction fails (for example, if the balance of **John Doe** is insufficient), we want to cancel the entire transaction to maintain consistency.

#### Steps:

1. **Start the Transaction**:
We begin by starting a new transaction. This will group all the changes into a single transaction.

```sql
START TRANSACTION;
```

2. **Update John Doe's Balance**:
We will deduct 100.00 from **John Doe**'s account.

```sql
UPDATE accounts 
SET balance = balance - 100.00 
WHERE account_id = 1;
```

At this point, **John Doe**'s balance will be 900.00.

3. **Update Jane Smith's Balance**:
Now we will add 100.00 to **Jane Smith**'s account.

```sql
UPDATE accounts 
SET balance = balance + 100.00 
WHERE account_id = 2;
```

At this point, **Jane Smith**'s balance will be 2100.00.

4. **Check if Transaction was Successful**:
Let’s assume everything is good so far, and we want to save the transaction details. We’ll add a record in the `transactions` table.

```sql
INSERT INTO transactions (account_id, amount)
VALUES (1, -100.00),  -- John Doe's debit
       (2, 100.00);   -- Jane Smith's credit
```

Now, we have all the necessary updates in place, and the transaction was successful. 

5. **Commit the Transaction**:
Finally, we commit the transaction to make the changes permanent in the database.

```sql
COMMIT;
```

At this point, all the changes are saved. **John Doe**'s balance is 900.00, **Jane Smith**'s balance is 2100.00, and the transaction history has been recorded.

---

### Scenario 2: Handling Errors with `ROLLBACK`

Now let’s see what happens if something goes wrong. Suppose during the transaction process, we encounter an issue that makes us want to cancel all the changes made so far.

#### Example: Insufficient Funds for John Doe
Imagine we try to deduct more money from **John Doe**'s account than he has, and this causes an error. Let's simulate the situation:

1. **Start the Transaction**:
```sql
START TRANSACTION;
```

2. **Attempt to Deduct Money from John Doe's Account**:
Let’s say we try to deduct 1200.00 instead of 100.00, which would cause an error since **John Doe** only has 1000.00.

```sql
UPDATE accounts 
SET balance = balance - 1200.00 
WHERE account_id = 1;
```

At this point, **John Doe**’s balance would go below 0, which we assume is an error (a constraint violation or business logic issue).

3. **Check the Error and Rollback**:
Since this operation fails due to insufficient funds, we don’t want to continue with any other changes. We will use `ROLLBACK` to undo any changes made during the transaction so far.

```sql
ROLLBACK;
```

This will undo any changes made during the transaction (no changes to the `accounts` table or `transactions` table will be saved).

After rolling back, the balances in the `accounts` table remain unchanged, and no new transaction records are inserted.

---

### Full Example: Simulation of a Transaction with `COMMIT` and `ROLLBACK`

Here is a full flow using the tables `accounts` and `transactions`:

1. **Start Transaction**:
```sql
START TRANSACTION;
```

2. **Update John Doe's Balance**:
```sql
UPDATE accounts 
SET balance = balance - 100.00 
WHERE account_id = 1;
```

3. **Update Jane Smith's Balance**:
```sql
UPDATE accounts 
SET balance = balance + 100.00 
WHERE account_id = 2;
```

4. **Insert Transaction Records**:
```sql
INSERT INTO transactions (account_id, amount)
VALUES (1, -100.00),
       (2, 100.00);
```

5. **Commit the Transaction**:
```sql
COMMIT;
```

OR, if there is an issue:
```sql
ROLLBACK;
```

### Result:

- **Commit**: If everything works fine, the transaction is completed, and the changes are saved.
- **Rollback**: If something fails (like insufficient funds), all changes are undone, ensuring the database remains consistent.

---

### Conclusion

By using transactions in MySQL (with `START TRANSACTION`, `COMMIT`, and `ROLLBACK`), we ensure that multiple related operations are executed atomically. If something goes wrong in the middle, `ROLLBACK` allows us to undo all changes, ensuring that we don't leave the database in an inconsistent state. Conversely, if everything works as expected, `COMMIT` makes the changes permanent.
