# PostgreSQL String Functions Practice Solutions

## Basic Level

1. ```sql
   SELECT UPPER(first_name) AS upper_first_name, LOWER(last_name) AS lower_last_name
   FROM customer;
   ```

2. ```sql
   SELECT first_name || ' ' || last_name AS full_name, LEFT(email, 3) AS email_start
   FROM customer;
   ```

3. ```sql
   SELECT first_name, last_name, LENGTH(first_name || last_name) AS full_name_length
   FROM customer;
   ```

4. ```sql
   SELECT email, SUBSTRING(email FROM POSITION('@' IN email) + 1) AS domain
   FROM customer;
   ```

5. ```sql
   SELECT first_name || ' ' || last_name AS full_name
   FROM customer;
   ```

## Intermediate Level

6. ```sql
   SELECT LOWER(LEFT(first_name, 3) || RIGHT(last_name, 3)) AS username
   FROM customer;
   ```

7. ```sql
   SELECT UPPER(last_name) || ', ' || LEFT(UPPER(first_name), 1) || '.' AS formatted_name
   FROM customer;
   ```

8. ```sql
   SELECT first_name, last_name
   FROM customer
   WHERE LENGTH(first_name) > 10 OR LENGTH(last_name) > 10;
   ```

9. ```sql
   SELECT email, SUBSTRING(email FROM 1 FOR POSITION('@' IN email) - 1) AS email_name
   FROM customer;
   ```

10. ```sql
    SELECT email, POSITION('.' IN email) AS dot_position
    FROM customer;
    ```

## Advanced Level

11. ```sql
    SELECT 
      email,
      LEFT(first_name, 1) || '***' || LEFT(last_name, 1) || 
      SUBSTRING(email FROM POSITION('@' IN email)) AS anonymized_email
    FROM customer;
    ```

12. ```sql
    SELECT 
      first_name, 
      last_name, 
      UPPER(LEFT(first_name, 2)) || LOWER(RIGHT(last_name, 2)) || LENGTH(email) AS customer_id
    FROM customer;
    ```

13. ```sql
    SELECT 
      first_name,
      CASE 
        WHEN LENGTH(first_name) % 2 = 0 THEN
          LEFT(first_name, LENGTH(first_name)/2 - 1) || '**' || RIGHT(first_name, LENGTH(first_name)/2 - 1)
        ELSE
          LEFT(first_name, LENGTH(first_name)/2) || '*' || RIGHT(first_name, LENGTH(first_name)/2)
      END AS masked_name
    FROM customer;
    ```

14. ```sql
    SELECT 
      last_name,
      LEFT(last_name, 1) || 
      REVERSE(SUBSTRING(last_name FROM 2 FOR LENGTH(last_name) - 2)) || 
      RIGHT(last_name, 1) AS reversed_middle
    FROM customer;
    ```

15. ```sql
    SELECT email
    FROM customer
    WHERE email LIKE first_name || '.' || last_name|| '@_%._%';
    ```
