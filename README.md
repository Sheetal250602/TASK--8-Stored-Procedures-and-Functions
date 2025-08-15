# Task 8: STORED PROCEDURES AND FUNCTIONS
# Using Database 'COMPANY' and table 'EMPLOYEES' for 'Stored Preocedure and Functions' by using MySQL Workbench.

CREATE DATABASE COMPANY;
USE COMPANY;

CREATE TABLE Employees(
employee_id INT PRIMARY KEY NOT NULL,
first_name VARCHAR(50),
last_name VARCHAR(50),
category VARCHAR(50),
department VARCHAR(50),
salary FLOAT DEFAULT 20000,
joining_date DATE
);

INSERT INTO Employees VALUES
(1, 'John', 'Smith', 'Full Time', 'Engineering', 80000, '2021-01-29'),
(2, 'Alice', 'Johnson', 'Part Time', 'HR', 30000.00, '2021-05-12'),
(3, 'Bob', 'Brown', 'Full Time', 'Finance', 90000, '2021-01-29'),
(4, 'Carol', 'White', 'Contract', 'IT', 75000, '2021-01-01'),
(5, 'David', 'Green', 'Full Time', 'Engineering', 85000, '2022-01-13'),
(6, 'Emma', 'Blue', 'Part Time', 'Finance', 32000, '2023-01-01'),
(7, 'Frank', 'Black', 'Full Time', 'HR', 60000, '2024-05-05'),
(8, 'Grace', 'Grey', 'Full Time', 'Marketing', 70000, '2021-01-22'),
(9, 'Henry', 'Red', 'Contract', 'Sales', 95000, '2021-01-08'),
(10, 'Ivy', 'Yellow', 'Full Time', 'Marketing', 28000, '2025-01-03');

SELECT * FROM Employees;

Employees table:

<img width="639" height="315" alt="image" src="https://github.com/user-attachments/assets/5f9d3f38-04bc-4783-8d81-b404c8807cf4" />

# Creating Stored Procedure called 'emp_info': 

DELIMITER $$
CREATE PROCEDURE emp_info()
BEGIN
    SELECT * FROM Employees;
END $$
DELIMITER ;

# Calling Stored Procedure 'emp_info':
CALL emp_info();

<img width="624" height="282" alt="image" src="https://github.com/user-attachments/assets/94d43f40-ebcd-464d-bf4b-f96a97ac8447" />



# Creating Stored Procedure called 'get_empid' using with 'IN' parameter:

CREATE PROCEDURE get_empid(IN p_first_name VARCHAR(50))
BEGIN
    SELECT employee_id FROM Employees
    WHERE first_name = 'Bob';
END $$
DELIMITER ;

# Calling 'get_empid':
CALL get_empid('Bob');

<img width="207" height="96" alt="image" src="https://github.com/user-attachments/assets/dafeec33-9aa0-4898-99b2-0d9acf467465" />


# Creating Stored Procedure using with 'IN' and 'OUT' Arguments:

DELIMITER $$
CREATE PROCEDURE get_sum(IN p_first_name VARCHAR(50), OUT p_sum DECIMAL(10,2))
BEGIN
    SELECT SUM(salary) INTO p_sum FROM Employees;
END $$
DELIMITER ;

# Calling 'get_sum':
SET @p_sum =0;
CALL get_sum('John', @p_sum);
SELECT @p_sum;

<img width="135" height="98" alt="image" src="https://github.com/user-attachments/assets/7525da6f-db4a-4dfb-a68d-80d1c43973d0" />

# Using Schema for Executing Stored procedure:
'Stored Procedure'--> 'get_sum'--> 'Enter_Paramenters'-->'Execute' 

<img width="1030" height="447" alt="image" src="https://github.com/user-attachments/assets/a6328cbd-1df1-4eb4-a299-230c223c5075" />


# Creating 'FUNCTIONS':

DELIMITER $$
CREATE FUNCTION max_salary() RETURNS VARCHAR(50)
DETERMINISTIC
BEGIN
DECLARE v_max INT;
SELECT MAX(salary) INTO v_max FROM Employees;
return  v_max;
END $$
DELIMITER ;

# Calling Functions:
SELECT max_salary();

<img width="171" height="99" alt="image" src="https://github.com/user-attachments/assets/a37e7bec-bf3d-4509-8505-eae97b0b3ab6" />


# 'Conditional Logic' in Stored Procedure using 'IF' statement:

DELIMITER $$
CREATE PROCEDURE salaryStatus(IN salary INT)
BEGIN
    IF salary > 100000 THEN
        SELECT 'Highest Salary';
    ELSEIF salary < 100000 THEN
        SELECT 'Average Salary';
    ELSE
        SELECT 'Lowest Salary';
    END IF;
END $$
DELIMITER ;

# Calling 'salarystatus':
CALL salaryStatus(200000);

<img width="204" height="116" alt="image" src="https://github.com/user-attachments/assets/1eede267-099f-4c39-a4fe-33e69927c1e5" />


# 'Conditional Logic' in Stored Procedure using 'CASE' Expression:

DELIMITER $$
CREATE PROCEDURE salary_status ()
BEGIN
SELECT employee_id, first_name,
CASE
WHEN salary < 10000 THEN 'Budget'
WHEN salary >= 10000 AND salary < 50000 THEN 'Standard'
ELSE 'Premium'
END AS salary_status FROM Employees;
END $$
DELIMITER ;

# Calling 'salary_status':
CALL salary_status;

<img width="340" height="304" alt="image" src="https://github.com/user-attachments/assets/1a6eb70d-0e34-4e28-b9c5-faae38a16162" />

# 'Conditional Logic' in Functions using 'IF' Statement:

DELIMITER $$
CREATE FUNCTION GetEmployeeStatus(salary DECIMAL(10, 2))
RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
DECLARE employee_status VARCHAR(20);
IF salary > 90000 THEN
SET employee_status = 'High Earner';
ELSEIF salary >= 30000 THEN
SET employee_status = 'Mid-Range Earner';
ELSE
SET employee_status = 'Low Earner';
END IF;
RETURN employee_status;
END $$
DELIMITER ;

# Calling 'GetEmployeeStatus':
SELECT GetEmployeeStatus(95000.00);

<img width="247" height="90" alt="image" src="https://github.com/user-attachments/assets/34d53ada-0a4e-4fc3-8711-4358a4b02a34" />

# 'Conditional Logic' in Functions using 'CASE' Expression:

DELIMITER $$
CREATE FUNCTION emp_status(category VARCHAR(50))
RETURNS VARCHAR(50)
DETERMINISTIC
BEGIN
RETURN CASE
WHEN Category = 'FULL TIME'  THEN 'Permanent'
WHEN Category = 'PART TIME' THEN 'Temporary'
ELSE 'Contract_based'
END;
END $$
DELIMITER ;

SELECT emp_status('CONTRACT');

<img width="277" height="113" alt="image" src="https://github.com/user-attachments/assets/fb03b5b9-ce6d-4db9-80a4-4b9b34b8f028" />

# 'DELETING' Stored Procedure:
DROP PROCEDURE procedure_name;

# 'DELETING' Functions:
DROP FUNCTION function_name;







