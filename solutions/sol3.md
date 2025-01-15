# PostgreSQL Date/Time Functions Practice Solutions

## Basic Exercises

1. Extract year from create_date:
```sql
SELECT customer_id, EXTRACT(YEAR FROM create_date) AS creation_year
FROM customer;
```

2. Extract month and year from rental_date:
```sql
SELECT rental_id, EXTRACT(MONTH FROM rental_date) AS rental_month,
       EXTRACT(YEAR FROM rental_date) AS rental_year
FROM rental;
```

3. Format last_update using TO_CHAR:
```sql
SELECT customer_id, TO_CHAR(last_update, 'Month DD, YYYY') AS formatted_last_update
FROM customer;
```

4. Calculate customer age:
```sql
SELECT customer_id, 
       AGE(CURRENT_DATE, create_date) AS customer_age
FROM customer;
```

5. Find day of week with most rentals:
```sql
SELECT TO_CHAR(rental_date, 'Day') AS day_of_week, COUNT(*) AS rental_count
FROM rental
GROUP BY day_of_week
ORDER BY rental_count DESC
LIMIT 1;
```

## Intermediate Exercises

6. Days between create_date and last rental:
```sql
SELECT c.customer_id,
       EXTRACT(day FROM MAX(r.rental_date) - c.create_date) AS days_since_creation
FROM customer c
JOIN rental r ON c.customer_id = r.customer_id
GROUP BY c.customer_id;
```

7. Average rental duration per customer:
```sql
SELECT customer_id,
       AVG(EXTRACT(EPOCH FROM (return_date - rental_date))/3600) AS avg_duration_hours
FROM rental
WHERE return_date IS NOT NULL
GROUP BY customer_id;
```

8. Format rental_date and return_date:
```sql
SELECT rental_id,
       TO_CHAR(rental_date, 'Day, DD Mon YYYY at HH24:MI:SS') AS formatted_rental_date,
       TO_CHAR(return_date, 'Day, DD Mon YYYY at HH24:MI:SS') AS formatted_return_date
FROM rental;
```

9. Count rentals by month in 2005:
```sql
SELECT TO_CHAR(rental_date, 'Mon YYYY') AS month, COUNT(*) AS rental_count
FROM rental
WHERE EXTRACT(YEAR FROM rental_date) = 2005
GROUP BY month
ORDER BY MIN(rental_date);
```

10. Customers with more weekend rentals:
```sql
SELECT customer_id
FROM (
    SELECT customer_id,
           SUM(CASE WHEN EXTRACT(DOW FROM rental_date) IN (0, 6) THEN 1 ELSE 0 END) AS weekend_rentals,
           SUM(CASE WHEN EXTRACT(DOW FROM rental_date) NOT IN (0, 6) THEN 1 ELSE 0 END) AS weekday_rentals
    FROM rental
    GROUP BY customer_id
) subquery
WHERE weekend_rentals > weekday_rentals;
```

## Advanced Exercises

11. Total rentals per customer in 2005:
```sql
SELECT customer_id, COUNT(*) AS total_rentals
FROM rental
WHERE EXTRACT(YEAR FROM rental_date) = 2005
GROUP BY customer_id
ORDER BY total_rentals DESC;
```

12. Average rental duration in 2005:
```sql
SELECT AVG(return_date - rental_date) AS avg_duration
FROM rental
WHERE EXTRACT(YEAR FROM rental_date) = 2005
  AND return_date IS NOT NULL;
```

13. Month with highest rentals in 2005:
```sql
SELECT EXTRACT(MONTH FROM rental_date) AS month, COUNT(*) AS rental_count
FROM rental
WHERE EXTRACT(YEAR FROM rental_date) = 2005
GROUP BY month
ORDER BY rental_count DESC
LIMIT 1;
```

14. Last rental date for each customer in 2005:
```sql
SELECT customer_id, 
       TO_CHAR(MAX(rental_date), 'Month DD, YYYY') AS last_rental_date
FROM rental
WHERE EXTRACT(YEAR FROM rental_date) = 2005
GROUP BY customer_id;
```

15. Rentals on first day of each month in 2005:
```sql
SELECT EXTRACT(MONTH FROM rental_date) AS month, 
       COUNT(*) AS rental_count
FROM rental
WHERE EXTRACT(YEAR FROM rental_date) = 2005
  AND EXTRACT(DAY FROM rental_date) = 1
GROUP BY month
ORDER BY month;
```
