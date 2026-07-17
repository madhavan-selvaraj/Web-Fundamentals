select * from employees;
select * from departments;

select * from customers;
select * from orders;
select * from categories;
select  * from products;
select * from order_items;

/*=========================================================
                QUESTIONS_21_30.SQL
      Multi-Table Joins + Aggregations + HAVING
=========================================================*/

---------------------------------------------------------
-- QUESTION 1
---------------------------------------------------------
-- Tables: customers, orders
-- Find the total number of orders placed by each customer.
--
-- Expected Output
-- customer_name | total_orders

select 
	c.customer_name , 
	count(c.customer_name ) as total_orders
from 
	customers c 
join 
	orders o
on 
	c.customer_id =o.customer_id
group by 
	c.customer_name ;



---------------------------------------------------------
-- QUESTION 2
---------------------------------------------------------
-- Tables: customers, orders, order_items, products
-- Find the total amount spent by each customer.
--
-- Expected Output
-- customer_name | total_spent
--
-- Hint: quantity × price, then GROUP BY customer.

select 
	c.customer_name ,
	sum(oi.quantity * p.price ) as total_spent
from 
	customers c
join 
	orders o
on 
	c.customer_id =o.customer_id 
join 
	order_items oi 
on 
	o.order_id =oi.order_id 
join 
	products p 
on 
	oi.product_id =p.product_id 
group by 
	customer_name ;


---------------------------------------------------------
-- QUESTION 3
---------------------------------------------------------
-- Tables: products, order_items
-- Find the total quantity sold for each product.
--
-- Expected Output
-- product_name | total_quantity
--
-- Hint: SUM(quantity).

select 
	p.product_name,
	sum(oi.quantity) as Total_quantity
from products p 
join order_items oi 
on p.product_id =oi.product_id 
group by product_name;

---------------------------------------------------------
-- QUESTION 4
---------------------------------------------------------
-- Tables: categories, products
-- Find the number of products in each category.
--
-- Expected Output
-- category_name | product_count
--
-- Hint: COUNT(product_id).

select 
	c.category_name,
	count(p.product_id) as Product_count
from categories c 
join products p 
on c.category_id =p.category_id 
group by c.category_name;


---------------------------------------------------------
-- QUESTION 5
---------------------------------------------------------
-- Tables: departments, employees
-- Find the average salary in each department.
--
-- Expected Output
-- department_name | average_salary

select 
	d.department_name,
	avg(salary) as AverageSalary
from employees e 
join departments d 
on e.department_id =d.department_id 
group by d.department_name ;


---------------------------------------------------------
-- QUESTION 6
---------------------------------------------------------
-- Tables: departments, employees
-- Find departments having more than 3 employees.
--
-- Expected Output
-- department_name | employee_count

select 
	d.department_name,
	count(e.employee_name) as employee_count
from departments d 
join employees e 
on d.department_id =e.department_id 
group by d.department_name 
having count(e.employee_name) > 3;


---------------------------------------------------------
-- QUESTION 7
---------------------------------------------------------
-- Tables: customers, orders
-- Find customers who have placed more than two orders.
--
-- Expected Output
-- customer_name | total_orders

select 
	c.customer_name,
	count(o.customer_id) as Total_orders
from customers c 
join orders o 
on c.customer_id =o.customer_id 
group by c.customer_id 
having count(c.customer_id) > 2;


---------------------------------------------------------
-- QUESTION 8
---------------------------------------------------------
-- Tables: products, order_items
-- Find the top 5 products based on quantity sold.
--
-- Expected Output
-- product_name | total_quantity
--
-- Hint: ORDER BY SUM(quantity) DESC.

select 
	p.product_name,
	sum(oi.quantity )
from products as p 
join order_items as oi
on p.product_id =oi.product_id 
group  by p.product_name 
order by sum(oi.quantity) desc 
limit 5;


---------------------------------------------------------
-- QUESTION 9
---------------------------------------------------------
-- Tables: customers, orders, order_items, products
-- Find the customer who spent the most money.
--
-- Expected Output
-- customer_name | total_spent

select 
	c.customer_name,
	sum(oi.quantity*p.price) as Total_spent
from customers c 
join orders o 
on c.customer_id = o.customer_id 
join order_items oi 
on o.order_id =oi.order_id 
join products p 
on oi.product_id =p.product_id 
group by c.customer_name 
order by sum(oi.quantity*p.price) desc limit 1;
---------------------------------------------------------
-- QUESTION 10
---------------------------------------------------------
-- Tables: categories, products, order_items
-- Find total revenue generated by each category.
--
-- Expected Output
-- category_name | total_revenue
--
-- Hint: quantity × price and GROUP BY category.

select 
	category_name,
	sum(quantity*price) as Total_revenue
from categories c 
join products p 
on c.category_id =p.category_id 
join order_items oi 
on p.product_id = oi.product_id
group by c.category_name ;

/*=========================================================
                BONUS QUESTIONS
=========================================================*/

---------------------------------------------------------
-- QUESTION 1
---------------------------------------------------------
-- Tables: departments, employees
-- Find the highest-paid employee in each department.
--
-- Expected Output
-- department_name | employee_name | salary
--
-- Hint: ROW_NUMBER() OVER(PARTITION BY department).

select
	employee_name,
	department_name,
	salary
from (select 
	e.employee_name ,
	d.department_name,
	e.salary,
	row_number() 
	over(partition by department_name order by salary desc) 
	as rank
from employees e
join departments d 
on e.department_id =d.department_id)
where rank<=1;
---------------------------------------------------------
-- QUESTION 2
---------------------------------------------------------
-- Tables: departments, employees
-- Find the top 2 highest-paid employees in each department.
--
-- Expected Output
-- department_name | employee_name | salary
--
-- Hint: ROW_NUMBER() <= 2.

select
	employee_name,
	department_name,
	salary
from (select 
	e.employee_name ,
	d.department_name,
	e.salary,
	row_number() 
	over(partition by department_name order by salary desc) 
	as rank
from employees e
join departments d 
on e.department_id =d.department_id)
where rank<=2;

---------------------------------------------------------
-- QUESTION 3
---------------------------------------------------------
-- Tables: customers, orders
-- Find the first order date for each customer.
--
-- Expected Output
-- customer_name | first_order_date
--
-- Hint: MIN(order_date).

select
	customer_name,
	min(order_date) as Fisrt_order_date
from customers c 
join orders o 
on c.customer_id =o.customer_id 
group by c.customer_name ;
---------------------------------------------------------
-- QUESTION 4
---------------------------------------------------------
-- Tables: customers, orders
-- Find the latest order date for each customer.
--
-- Expected Output
-- customer_name | latest_order_date
--
-- Hint: MAX(order_date).

select
	customer_name,
	max(order_date) as Latest_order_date
from customers c 
join orders o 
on c.customer_id =o.customer_id 
group by c.customer_name ;
---------------------------------------------------------
-- QUESTION 5
---------------------------------------------------------
-- Tables: departments, employees
-- Find employees earning more than their department average salary.
--
-- Expected Output
-- employee_name | department_name | salary
--
-- Hint: Compare salary against AVG(salary) partitioned by department.

select 
	employee_name,
	department_name,
	salary,
	avg_salary 
from (select 
	e.employee_name,
	d.department_name,
	e.salary,
	avg(e.salary) over(partition by d.department_name) as avg_salary
from employees e 
join departments d 
on e.department_id =d.department_id)
where salary > avg_salary;
---------------------------------------------------------
-- QUESTION 6
---------------------------------------------------------
-- Tables: customers, orders
-- Find customers who have never placed an order.
--
-- Expected Output
-- customer_name
--
-- Hint: LEFT JOIN + NULL.

select 
	c.customer_name,
	count(o.customer_id)
from customers c 
left join orders o 
on c.customer_id =o.customer_id 
group by c.customer_name 
having count(o.customer_id )=0;
---------------------------------------------------------
-- QUESTION 7
---------------------------------------------------------
-- Tables: products, order_items
-- Find products that have never been ordered.
--
-- Expected Output
-- product_name
--
-- Hint: LEFT JOIN + IS NULL.

select 
	p.product_name
from products p 
left join order_items oi 
on p.product_id =oi.product_id 
where oi.product_id is null;
---------------------------------------------------------
-- QUESTION 8
---------------------------------------------------------
-- Tables: departments, employees
-- Find the total salary expenditure by department.
--
-- Expected Output
-- department_name | total_salary
--
-- Hint: SUM(salary).

select 
	d.department_name,
	sum(e.salary)  as Total_expenditure
from employees e
join departments d
on e.department_id =d.department_id
group by d.department_name ;
---------------------------------------------------------
-- QUESTION 9
---------------------------------------------------------
-- Tables: customers, orders, order_items, products
-- Find the number of distinct products purchased by each customer.
--
-- Expected Output
-- customer_name | product_count
--
-- Hint: COUNT(DISTINCT product_id).

select 
	c.customer_name ,
	count(distinct p.product_id )
from customers c 
join orders o 
on c.customer_id = o.customer_id 
join order_items oi 
on o.order_id =oi.order_id 
join products p 
on oi.product_id =p.product_id 
group by c.customer_name ;

---------------------------------------------------------
-- QUESTION 10
---------------------------------------------------------
-- Tables: customers, orders, order_items, products
-- Find the most purchased product by quantity.
--
-- Expected Output
-- product_name | total_quantity
--
-- Hint: SUM(quantity) and ORDER BY DESC LIMIT 1.

select 
	p.product_name,
	sum(oi.quantity) as Total_quantity
from products p 
join order_items oi 
on p.product_id =oi.product_id 
group by p.product_name 
order by Total_quantity desc limit 1;

/*=========================================================
                QUESTIONS_31_40.SQL
         Window Functions + CTE + Advanced SQL
=========================================================*/

---------------------------------------------------------
-- QUESTION 1
---------------------------------------------------------
-- Find the highest-paid employee in each department.
--
-- Expected Output
-- department_name | employee_name | salary

select
	employee_name,
	department_name,
	salary
from (select 
	e.employee_name,
	d.department_name,
	e.salary,
	row_number() over (partition by d.department_name order by e.salary desc) as rank
from employees e 
join departments d 
on e.department_id =d.department_id) as ranked
where rank <= 1;

---------------------------------------------------------
-- QUESTION 2
---------------------------------------------------------
-- Find the top 2 highest-paid employees in each department.
--
-- Expected Output
-- department_name | employee_name | salary

select
	employee_name,
	department_name,
	salary
from (select 
	e.employee_name,
	d.department_name,
	e.salary,
	row_number() over (partition by d.department_name order by e.salary desc) as rank
from employees e 
join departments d 
on e.department_id =d.department_id) as ranked
where rank <= 2;


---------------------------------------------------------
-- QUESTION 3
---------------------------------------------------------
-- Rank employees by salary within each department.
--
-- Expected Output
-- department_name | employee_name | salary | rank

select 
	e.employee_name,
	d.department_name,
	e.salary,
	rank() over(partition by d.department_name order by e.salary desc) as rank
from employees e 
join departments d 
on e.department_id =d.department_id ;
---------------------------------------------------------
-- QUESTION 4
---------------------------------------------------------
-- Assign dense rank to employees by salary within each department.
--
-- Expected Output
-- department_name | employee_name | salary | dense_rank

select 
	e.employee_name,
	d.department_name,
	e.salary,
	dense_rank() over(partition by d.department_name order by e.salary desc) as rank
from employees e 
join departments d 
on e.department_id =d.department_id ;
---------------------------------------------------------
-- QUESTION 5
---------------------------------------------------------
-- Calculate the running total of salaries ordered by joining date.
--
-- Expected Output
-- employee_name | joining_date | salary | running_total

select 
	employee_name,
	joining_date,
	salary,
	sum(salary) over(order by joining_date) as RunningTotal
from employees;
---------------------------------------------------------
-- QUESTION 6
---------------------------------------------------------
-- Find the difference between each employee's salary and the previous employee's salary ordered by joining date.
--
-- Expected Output
-- employee_name | salary | previous_salary | difference

select 
	employee_name,
	salary,
	lag(salary) over() as previuos_employee_salary,
	abs(salary - lag(salary)over()) as difference
from employees;
---------------------------------------------------------
-- QUESTION 7
---------------------------------------------------------
-- Show each customer's previous order date.
--
-- Expected Output
-- customer_name | order_date | previous_order_date

select 
	c.customer_name,
	o.order_date,
	lag(o.order_date) over(partition by c.customer_name ) as PreviousOrderDate
from customers c
join orders o
on c.customer_id =o.customer_id ;
---------------------------------------------------------
-- QUESTION 8
---------------------------------------------------------
-- Show each customer's next order date.
--
-- Expected Output
-- customer_name | order_date | next_order_date

select 
	c.customer_name,
	o.order_date,
	lead(o.order_date) over(partition by c.customer_name ) as NextOrderDate
from customers c
join orders o
on c.customer_id =o.customer_id ;
---------------------------------------------------------
-- QUESTION 9
---------------------------------------------------------
-- Calculate the running total amount spent by each customer.
--
-- Expected Output
-- customer_name | order_date | order_amount | running_total

with temp_table as (select 
	oi.order_item_id ,
	c.customer_name,
	p.product_name,
	o.order_date,
	sum(p.price*oi.quantity) over(partition by oi.order_item_id ) as order_amount
from customers c 
join orders o 
on c.customer_id =o.customer_id 
join order_items oi 
on o.order_id =oi.order_id 
join products p 
on oi.product_id =p.product_id) 
select 
	tt.customer_name,
	tt.order_date,
	tt.order_amount,
	sum(tt.order_amount)over(partition by tt.customer_name order by tt.order_item_id) as RunningTotal
from temp_table as tt;
---------------------------------------------------------
-- QUESTION 10
---------------------------------------------------------
-- Find employees whose salary is greater than the average salary of their department.
--
-- Expected Output
-- employee_name | department_name | salary | department_average


select
	employee_name,
	department_name,
	salary,
	DepartmentAverage
from (select 
	employee_name,
	department_name,
	salary,
	avg(salary) over(partition by department_name) as DepartmentAverage
from employees e 
join departments d 
on e.department_id =d.department_id)
where salary>DepartmentAverage ;










