# PostgreSQL Practice Exercises - Solutions

## Basic Queries

```sql
-- List all employees in IT department
SELECT * 
FROM employees 
WHERE department = 'IT';

-- Find employees with salaries > 70000
SELECT * 
FROM employees 
WHERE salary > 70000;

-- List active employees ordered by hire date
SELECT * 
FROM employees 
WHERE is_active = true 
ORDER BY hire_date;

-- Find employees hired between 2019 and 2020
SELECT * 
FROM employees 
WHERE hire_date BETWEEN '2019-01-01' AND '2020-12-31';
```

- - -

## Logical Expressions

```sql
-- IT or high salary
SELECT * 
FROM employees 
WHERE department = 'IT' OR salary > 70000;

-- Active and recent hires
SELECT * 
FROM employees 
WHERE is_active = true 
AND hire_date > '2019-12-31';

-- Not in Marketing
SELECT * 
FROM employees 
WHERE department != 'Marketing';

-- Salary comparison with department average
SELECT 
    e.name,
    e.salary,
    e.department,
    (e.salary > (
        SELECT AVG(salary) 
        FROM employees e2 
        WHERE e2.department = e.department
    )) as above_dept_average
FROM employees e;
```

- - -

## Aggregation

```sql
-- Average salary by department
SELECT 
    department,
    ROUND(AVG(salary), 2) as avg_salary
FROM employees
GROUP BY department;

-- Department with highest total salary
SELECT 
    department,
    SUM(salary) as total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC
LIMIT 1;

-- Projects per department
SELECT 
    d.dept_name,
    COUNT(p.project_id) as project_count
FROM departments d
LEFT JOIN projects p ON d.dept_id = p.dept_id
GROUP BY d.dept_name;

-- Total hours per project
SELECT 
    p.project_name,
    SUM(ep.hours_allocated) as total_hours
FROM projects p
LEFT JOIN employee_projects ep ON p.project_id = ep.project_id
GROUP BY p.project_name;

-- Departments with high average salary
SELECT 
    department,
    ROUND(AVG(salary), 2) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 70000;
```

- - -

