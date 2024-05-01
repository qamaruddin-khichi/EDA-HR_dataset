# HR Dataset SQL Queries and Analysis

## Table of Contents

1. [About](#about)
2. [Purpose of Project](#purpose-of-project)
3. [HR DataBase Model](#hr-database-model)
4. [SQL Queries](#sql-queries)
    - [Easy Level (10 Questions)](#easy-level-10-questions)
    - [Intermediate Level (20 Questions)](#intermediate-level-20-questions)
    - [Advanced Level (4 Questions)](#advanced-level-4-questions)
5. [SQL Code](#sql-code)
6. [Conclusion](#conclusion)

## About

This project focuses on querying and analyzing the HR dataset using SQL. The dataset includes information about employees, departments, job titles, salaries, commissions, and more. By executing SQL queries on this dataset, various aspects of HR management and employee information are explored, providing insights and valuable information for decision-making processes.

## Purpose of Project

The primary purpose of this project is to demonstrate the capabilities of SQL in querying and analyzing HR-related data. By addressing a wide range of questions, from basic queries to more complex analyses, the project aims to showcase the versatility of SQL in extracting meaningful insights from structured datasets. Additionally, the project serves as a learning resource for SQL enthusiasts and professionals seeking to enhance their skills in data analysis and database management.

## HR DataBase Model

![hr-database-model](https://github.com/qamaruddin-khichi/EDA-HR_dataset/assets/155871872/cf7d370d-9f46-404d-8a86-d8f1fc75bbc1)

## SQL Queries

### Easy Level (10 Questions)

1. List the names of all countries in the countries table.
2. How many departments are there in the departments table?
3. Find all employees from the employees table whose first name is 'Steven'.
4. What is the average salary of all employees in the company?
5. How many employees work in the 'Sales' department? (department_id = 80)
6. List the department names and the number of employees in each department.
7. Find all employees who earn more than $10,000 in salary.
8. What is the maximum salary paid in the company?
9. List all employees who were hired after '01-JUNE-1987'.
10. Find the department with the highest average salary.

### Intermediate Level (20 Questions)

11. List all employees along with their department names.
12. Find the total commission made by all employees in the company.
13. Calculate the average commission rate for each department.
14. List the manager names and the number of employees reporting to each manager.
15. Find all employees who do not have a manager assigned (manager_id is NULL).
16. For each department, find the average salary and the number of employees.
17. Find all employees who have a job title starting with 'SA' (e.g., SA_REP, SA_MAN).
18. List the department names that have more than 10 employees.
19. Find the employee with the highest salary in each department.
20. Calculate the total salary paid to all employees in each department.
21. List all departments along with the countries where their employees are located.
22. Find the average salary for employees in each country.
23. Find the manager with the most direct reports (employees who report directly to them).
24. List all employees who report to a manager named 'David' (manager_last_name = 'Austin').
25. Find the department with the lowest average salary.
26. Calculate the age (in years) of each employee based on their hire date and the current date.
27. Find all employees who joined the company in the year 1987.
28. List the top 5 departments with the highest total salary paid to their employees.
29. Find the employee with the highest commission rate in the company.
30. Calculate the total commission made by each sales representative (SA_REP job title).

### Advanced Level (4 Questions)

31. Identify all departments that have at least one employee who lives in a city named 'Tokyo' (CITY = 'Tokyo').
32. Use a subquery to find all departments that have an average salary higher than the company's overall average salary.
33. Find all managers who have at least one employee reporting to them and a total commission rate exceeding 0.10 PCT.
34. Write a query to display department names and the salary range (minimum and maximum) for employees in each department.

## SQL Code

```
-- HR_Practice Dataset
USE hr_practice;

-- Easy Level (10 Questions)

-- 1. List the names of all countries in the countries table.

SELECT countries.COUNTRY_NAME
FROM countries;

-- 2. How many departments are there in the departments table?

SELECT COUNT(DEPARTMENT_NAME) AS total_departments
FROM departments;

-- 3. Find all employees from the employees table whose first name is 'Steven'.

SELECT employees.FIRST_NAME
FROM employees
WHERE FIRST_NAME = 'Steven';

-- 4. What is the average salary of all employees in the company?

SELECT ROUND(AVG(SALARY),2) AS average_salary
FROM employees;

-- 5. How many employees work in the 'Sales' department? (department_id = 80)

-- Method 1
SELECT COUNT(EMPLOYEE_ID) AS total_employees
FROM employees
WHERE DEPARTMENT_ID = 80;

-- Method 2
SELECT 
    COUNT(E.EMPLOYEE_ID) AS total_employees
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE
    D.DEPARTMENT_NAME = 'Sales';

-- 6. List the department names and the number of employees in each department.

SELECT 
    D.DEPARTMENT_NAME,
    COUNT(E.EMPLOYEE_ID) AS total_employees
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	total_employees DESC;

-- 7. Find all employees who earn more than $10,000 in salary.

SELECT employees.FIRST_NAME, employees.LAST_NAME, SALARY
FROM employees
WHERE SALARY > 10000
ORDER BY SALARY DESC;

-- 8. What is the maximum salary paid in the company?

SELECT MAX(SALARY) AS maximum_salary
FROM employees;

-- 9. List all employees who were hired after '01-JUNE-1987'.

SELECT *, employees.FIRST_NAME, employees.LAST_NAME
FROM employees
WHERE HIRE_DATE > '1987-06-01';
 
-- 10. Find the department with the highest average salary.

SELECT 
    D.DEPARTMENT_NAME,
    ROUND(AVG(E.SALARY),2) AS average_salary
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	average_salary DESC
    LIMIT 1;

-- Intermediate Level (20 Questions)

-- 11.	List all employees along with their department names. (Join employees and departments tables)

SELECT 
	E.FIRST_NAME,
    E.LAST_NAME,
    D.DEPARTMENT_NAME
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID;

-- 12.	Find the total commission made by all employees in the company.

SELECT SUM(COMMISSION_PCT) AS total_commission_pct
FROM employees;

-- 13.	Calculate the average commission rate for each department.

SELECT 
    D.DEPARTMENT_NAME,
    AVG(E.COMMISSION_PCT) AS total_commission_pct
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	total_commission_pct DESC;

-- 14.	List the manager names and the number of employees reporting to each manager.

SELECT 
	DISTINCT E2.FIRST_NAME AS Manager_FirstName,
    COUNT(E1.DEPARTMENT_ID) AS COUNT
FROM
    employees E1
        JOIN
    employees E2 ON E1.EMPLOYEE_ID = E2.MANAGER_ID
GROUP BY Manager_FirstName
ORDER BY COUNT DESC;

-- 15.	Find all employees who do not have a manager assigned (manager_id is NULL).

-- Method 1
SELECT 
	E.FIRST_NAME,
    E.LAST_NAME
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE D.DEPARTMENT_ID IS NULL;

-- Method 2
SELECT *
FROM employees
WHERE DEPARTMENT_ID IS NULL;

-- 16.	For each department, find the average salary and the number of employees.

SELECT 
    D.DEPARTMENT_NAME,
    COUNT(E.EMPLOYEE_ID) AS total_employees,
    ROUND(AVG(E.SALARY),2) AS average_salary
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	total_employees DESC, average_salary DESC;

-- 17.	Find all employees who have a job title starting with 'SA' (e.g., SA_REP, SA_MAN).

SELECT 
    E.FIRST_NAME,
    E.LAST_NAME,
    J.JOB_TITLE
FROM
    employees E
        JOIN
    jobs J ON E.JOB_ID = J.JOB_ID
WHERE
    J.JOB_TITLE LIKE 'Sa%';

-- 18.	List the department names that have more than 10 employees.

SELECT 
    D.DEPARTMENT_NAME,
    COUNT(E.EMPLOYEE_ID) AS total_employees
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
HAVING total_employees > 10
ORDER BY
	total_employees DESC;

-- 19.	Find the employee with the highest salary in each department.

SELECT 
    D.DEPARTMENT_NAME,
    MAX(E.SALARY) AS maximum_salary
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	maximum_salary DESC;

-- 20.	Calculate the total salary paid to all employees in each department.

SELECT 
    D.DEPARTMENT_NAME,
    SUM(E.SALARY) AS total_salary
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	total_salary DESC;

-- 21.	List all departments along with the countries where their employees are located. (Join employees, departments, and countries tables)

-- Method 1
SELECT 
    employees.FIRST_NAME,
    employees.LAST_NAME,
    departments.DEPARTMENT_NAME,
    countries.COUNTRY_NAME
FROM
    locations
        JOIN
    countries ON locations.COUNTRY_ID = countries.COUNTRY_ID
        JOIN
    departments ON locations.LOCATION_ID = departments.LOCATION_ID
        JOIN
    employees ON departments.DEPARTMENT_ID = employees.DEPARTMENT_ID;

-- Method 2
SELECT 
    e.first_name,
    e.last_name,
    d.department_name,
    (SELECT 
            c.country_name
        FROM
            locations l
                JOIN
            countries c ON l.country_id = c.country_id
        WHERE
            l.location_id = d.location_id) AS country_name
FROM
    employees e
        JOIN
    departments d ON e.department_id = d.department_id;
    
-- Method 3
WITH department_locations AS (
  SELECT l.location_id, c.country_name
  FROM locations l
  JOIN countries c ON l.country_id = c.country_id
)
SELECT e.first_name, e.last_name, d.department_name, dl.country_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN department_locations dl ON d.location_id = dl.location_id;

-- 22.	Find the average salary for employees in each country.

-- Method 1
SELECT 
    countries.COUNTRY_NAME,
    ROUND(AVG(employees.SALARY),2) AS average_salary
FROM
    locations
        JOIN
    countries ON locations.COUNTRY_ID = countries.COUNTRY_ID
        JOIN
    departments ON locations.LOCATION_ID = departments.LOCATION_ID
        JOIN
    employees ON departments.DEPARTMENT_ID = employees.DEPARTMENT_ID
    GROUP BY countries.COUNTRY_NAME;

-- Method 2
SELECT country_name,
       AVG(salary) AS avg_salary
FROM (
  SELECT e.department_id, c.country_name, e.salary
  FROM employees e
  JOIN departments d ON e.department_id = d.department_id
  JOIN locations l ON d.location_id = l.location_id
  JOIN countries c ON l.country_id = c.country_id
) AS employee_data
GROUP BY country_name;

-- Method 3
WITH employee_data AS (
  SELECT e.department_id, c.country_name, e.salary
  FROM employees e
  JOIN departments d ON e.department_id = d.department_id
  JOIN locations l ON d.location_id = l.location_id
  JOIN countries c ON l.country_id = c.country_id
)
SELECT country_name,
       AVG(salary) AS avg_salary
FROM employee_data
GROUP BY country_name;

-- 23.	Find the manager with the most direct reports (employees who report directly to them).

SELECT 
    m.MANAGER_ID, e.FIRST_NAME, COUNT(*) AS num_reports
FROM
    employees e
        JOIN
    employees m ON e.manager_id = m.employee_id
GROUP BY m.MANAGER_ID , e.FIRST_NAME
ORDER BY num_reports DESC
LIMIT 1;

-- 24.	List all employees who report to a manager named 'David' (manager_last_name = 'Austin').

SELECT e.first_name, e.last_name
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE m.LAST_NAME = 'Austin';

-- 25.	Find the department with the lowest average salary.

SELECT 
    departments.DEPARTMENT_NAME,
    ROUND(AVG(SALARY), 2) AS aversge_salary
FROM
    employees
        JOIN
    departments ON employees.DEPARTMENT_ID = departments.DEPARTMENT_ID
GROUP BY departments.DEPARTMENT_NAME
ORDER BY aversge_salary;

-- 26.	26.	Calculate the age (in years) of each employee based on their hire date and the current date.

-- Method 1 -- If the year has not completed yet, Floor function will round of date and returns previous year.
SELECT 
	first_name,
	last_name,
	FLOOR(DATEDIFF(CURDATE(), hire_date) / 365.25) AS age
FROM employees;

-- Method 2 -- If the year has not completed yet, year and subtime functions will return year.
SELECT employee_id,
       first_name,
       last_name,
       YEAR(CURDATE()) - YEAR(SUBTIME(hire_date, CURTIME())) AS age
FROM employees;

-- 27.	Find all employees who joined the company in the year 1987.

SELECT FIRST_NAME, LAST_NAME, HIRE_DATE
FROM employees
WHERE YEAR(HIRE_DATE) = 1987; 

-- 28.	List the top 5 departments with the highest total salary paid to their employees.

SELECT 
    D.DEPARTMENT_NAME,
    SUM(E.SALARY) AS total_salary
FROM
    employees E
        JOIN
    departments D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
    D.DEPARTMENT_NAME
ORDER BY
	total_salary DESC
LIMIT 5;

-- 29.	Find the employee with the highest commission rate in the company.

SELECT 
	FIRST_NAME,
    LAST_NAME,
    COMMISSION_PCT
FROM employees
ORDER BY COMMISSION_PCT DESC
LIMIT 1;

-- 30.	Calculate the total commission made by each sales representative (SA_REP job title).

SELECT 
	SUM(COMMISSION_PCT) AS total_commission_pct
FROM employees
WHERE JOB_ID = 'SA_REP';

-- Advanced Level (10 Questions)
-- 31.	Identify all departments that have at least one employee who lives in a city named 'Tokyo' (CITY = 'Tokyo').

SELECT 
    departments.DEPARTMENT_NAME
FROM
    locations
        JOIN
    departments ON locations.LOCATION_ID = departments.LOCATION_ID
WHERE
    locations.CITY = 'Tokyo';

-- 32.	Use a subquery to find all departments that have an average salary higher than the company's overall average salary.

SELECT 
    departments.DEPARTMENT_NAME,
    ROUND(AVG(employees.SALARY), 2) AS average_salary
FROM
    employees
        JOIN
    departments ON employees.EMPLOYEE_ID = departments.DEPARTMENT_ID
GROUP BY departments.DEPARTMENT_NAME
HAVING average_salary > (SELECT 
        AVG(SALARY)
    FROM
        employees)
ORDER BY average_salary DESC;

-- 33.	Find all managers who have at least one employee reporting to them and a total commission rate exceeding 0.10 PCT

SELECT 
    e.FIRST_NAME, COUNT(*) AS num_reports
FROM
    employees e
        JOIN
    employees m ON e.manager_id = m.employee_id
WHERE
    e.COMMISSION_PCT > 0.10
GROUP BY m.MANAGER_ID , e.FIRST_NAME
HAVING num_reports >= 1
ORDER BY num_reports DESC;


-- 34.	Write a query to display department names and the salary range (minimum and maximum) for employees in each department.

SELECT 
    departments.DEPARTMENT_NAME,
    MIN(employees.SALARY) AS max_salary,
    MAX(employees.SALARY) AS min_salary
FROM
    employees
        JOIN
    departments ON employees.DEPARTMENT_ID = departments.DEPARTMENT_ID
GROUP BY departments.DEPARTMENT_NAME
ORDER BY min_salary DESC , max_salary DESC;
```

## Conclusion

In conclusion, this project demonstrates the versatility and power of SQL in querying and analyzing HR-related data. By addressing a wide range of questions, from basic queries to more advanced analyses, valuable insights about employees, departments, salaries, commissions, and more are extracted from the dataset. The project serves as a practical example of how SQL can be utilized in HR management, decision-making, and data analysis processes.
