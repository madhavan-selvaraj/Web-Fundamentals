CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    joining_date DATE
);

INSERT INTO employees VALUES
(1,'John','IT',80000,'2022-01-10'),
(2,'Mary','HR',60000,'2021-06-15'),
(3,'David','IT',90000,'2020-02-20'),
(4,'Emma','Finance',75000,'2023-01-01'),
(5,'Robert','IT',85000,'2019-11-05');

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_name VARCHAR(50),
    order_date DATE,
    amount DECIMAL(10,2)
);

INSERT INTO orders VALUES
(101,'Alice','2025-01-01',100),
(102,'Alice','2025-01-05',200),
(103,'Alice','2025-01-10',300),
(104,'Bob','2025-01-03',150),
(105,'Bob','2025-01-08',250);

CREATE TABLE customers(
    customer_id INT,
    customer_name VARCHAR(50)
);

INSERT INTO customers VALUES
(1,'John'),
(2,'Mary'),
(3,'John'),
(4,'David'),
(5,'Mary');

select * from employees;
select * from orders;
select * from customers;

/*=========================================================
                    QUESTIONS
=========================================================*/

--------------------------------------------------------
-- QUESTION 1
--------------------------------------------------------
-- Add an email column to employees table.

alter table employees 
add email_id varchar(100) default null;

--------------------------------------------------------
-- QUESTION 2
--------------------------------------------------------
-- Update email address of employee_id = 1.

update employees 
set email_id='john.it@gamil.com'
where employee_id=1;

--------------------------------------------------------
-- QUESTION 3
--------------------------------------------------------
-- Give 10% salary increment to all IT employees.

update employees
set salary = salary + salary * 10/100
where department = 'IT';

--------------------------------------------------------
-- QUESTION 4
--------------------------------------------------------
-- Find employees earning more than the average salary.
-- Hint: Compare salary with AVG(salary).

select
	employee_name,
	salary ,
	(select AVG(salary) as average from employees)
from employees 
where salary > (select
					avg(salary) as average
				from employees);

--------------------------------------------------------
-- QUESTION 5
--------------------------------------------------------
-- Find the highest-paid employee in each department.

select 
	employee_name,
	department,
	salary
from (select 
	*,
	row_number() over (partition by department order by salary desc) as high_salary
from employees) as sort
where high_salary =1;

--------------------------------------------------------
-- QUESTION 6
--------------------------------------------------------
-- Find the top 2 highest-paid employees in each department.

select 
	employee_name,
	department,
	salary
from ( select
		* ,
		rank() over (partition by department order by salary desc) as rank
		from employees) as high_salary
	where rank<=2;

--------------------------------------------------------
-- QUESTION 7
--------------------------------------------------------
-- Rank employees by salary within each department.

select 
	employee_name,
	department,
	salary,
	rank() over (partition by department order by salary) as rank
from employees;
--------------------------------------------------------
-- QUESTION 8
--------------------------------------------------------
-- Assign dense rank to employees based on salary.

select 
	employee_name,
	department,
	salary,
	dense_rank() over (partition by department order by salary) as rank
from employees;

--------------------------------------------------------
-- QUESTION 9
--------------------------------------------------------
-- Calculate running total of salaries ordered by joining date.
-- Hint: Use SUM() OVER(ORDER BY joining_date).

select 
	employee_name,
	joining_date,
	salary,
	sum(salary) over (order by joining_date) as Total
from employees;

--------------------------------------------------------
-- QUESTION 10
--------------------------------------------------------
-- Show previous order amount for each customer.

select 
	customer_name,
	order_date,
	amount,
	lag(amount) over(partition by customer_name order by order_date) as previous_amount
from orders;
	

--------------------------------------------------------
-- QUESTION 11
--------------------------------------------------------
-- Show next order amount for each customer.

select 
	customer_name,
	order_date,
	amount,
	lead(amount) over(partition by customer_name order by order_date) as next_amount
from orders;

--------------------------------------------------------
-- QUESTION 12
--------------------------------------------------------
-- Find customers whose total purchase amount exceeds 300.


select 
	customer_name,
	sum(amount)
from orders 
group by customer_name 
having sum(amount)>300;


--------------------------------------------------------
-- QUESTION 13
--------------------------------------------------------
-- Find duplicate customer names.

select 
	customer_name,
	count(customer_name) over() as count
from customers
group by customer_name
having count(customer_name) >1;

--------------------------------------------------------
-- QUESTION 14
--------------------------------------------------------
-- Find the second highest salary.

select 
	employee_name,
	salary
from employees
order by salary desc
limit 1
offset 1;

--------------------------------------------------------
-- QUESTION 15
--------------------------------------------------------
-- Remove duplicate customers keeping only the first occurrence.
-- Hint: Use ROW_NUMBER() and delete rows > 1.
 delete from customers
 where customer_id in(
 	select customer_id
 	from(
 		select 
 			customer_id,
 			row_number() over(partition by customer_name order by customer_id ) as rank
 		from customers) 
	as ranked
	where rank > 1);



select * from customers;
insert into customers values (3,'John'),(5,'Mary');
--------------------------------------------------------
-- QUESTION 16
--------------------------------------------------------
-- Find the employee who joined earliest in each department.
-- Hint: Order by joining_date ASC inside ROW_NUMBER().

select
	employee_name,
	department,
	joining_date
from(select *,
	row_number() over(partition by department order by joining_date asc ) as rank
	from employees)
as rows_num
where rank=1;

--------------------------------------------------------
-- QUESTION 17
--------------------------------------------------------
-- Calculate salary difference between an employee and the previous employee ordered by joining date.
-- Hint: Use LAG(salary).

select 
	employee_name,
	joining_date,
	salary,
	lag(salary) over(order by joining_date) as PreviousEmployeeSalary,
	ABS(salary - lag(salary) over(order by joining_date)) as difference
from employees;

--------------------------------------------------------
-- QUESTION 18
--------------------------------------------------------
-- Find percentage contribution of each employee salary to total salary expenditure.
-- Hint: Divide salary by SUM(salary) OVER().

select 
	employee_name,
	salary,
	sum(salary) over( ) as Total_salary,
	round((salary*100)/sum(salary) over(),2) as percentage
from employees e ;


--------------------------------------------------------
-- QUESTION 19
--------------------------------------------------------
-- Calculate running total of order amount for each customer.
-- Hint: Use PARTITION BY customer_name.

select 
	customer_name,
	order_date,
	amount,
	sum(amount) over(partition by customer_name order by order_date desc) as running_total 
from orders;

--------------------------------------------------------
-- QUESTION 20
--------------------------------------------------------
-- Find customers who placed more than one order.

 select 
 	customer_name,
 	count(customer_name) over() as No_of_orders
 from orders
 group by customer_name 
 having count(customer_name)>1;







