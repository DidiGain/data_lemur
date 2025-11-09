### Write a query to obtain the third transaction of every user. Output the user id, spend and transaction date.

 <u>*Table:* __transactions__</u>

| Column Name	| Type |
| :---:| :----: |
| user_id	| integer |
| spend	| decimal |
| transaction_date	| timestamp |

### Solution 

```
SELECT user_id, spend, transaction_date
FROM (
    SELECT 
    user_id, 
    spend, 
    transaction_date, 
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY transaction_date) as row_num
    FROM transactions
) t
WHERE row_num = 3;
```