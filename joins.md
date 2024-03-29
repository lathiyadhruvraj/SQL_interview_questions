#### Company: Airbnb

### [Number Of Units Per Nationality](https://platform.stratascratch.com/coding/10156-number-of-units-per-nationality?code_type=1) Medium

#### Q. Find the number of apartments per nationality that are owned by people under 30 years old. Output the nationality along with the number of apartments. Sort records by the apartments count in descending order.

```diff
select nationality,count(distinct unit_id) from airbnb_hosts join airbnb_units on airbnb_hosts.host_id=airbnb_units.host_id
where age<30 and unit_type='Apartment'
group by 1;

```

| airbnb_hosts             | airbnb_units       |
|--------------------------|--------------------|
| host_id: int             | host_id: int       |
| nationality: varchar     | unit_id: varchar   |
| gender: varchar          | unit_type: varchar |
| age: int                 | n_beds: int        |
|                          | n_bedrooms: int    |
|                          | country: varchar   |
|                          | city: varchar      |


---

#### Company: Meta/Facebook

### [Popularity Percentage](https://platform.stratascratch.com/coding/10284-popularity-percentage?code_type=1) Hard

#### Q. Find the popularity percentage for each user on Meta/Facebook. The popularity percentage is defined as the total number of friends the user has divided by the total number of users on the platform, then converted into a percentage by multiplying by 100. Output each user along with their popularity percentage. Order records in ascending order by user id. The 'user1' and 'user2' column are pairs of friends.

```diff
with base AS (
    select user1, user2
    from facebook_friends
    UNION 
    select user2, user1
    from facebook_friends
)
SELECT user1
    , 100* count(user2) / (SELECT COUNT(DISTINCT user1) FROM base)::decimal popularity
FROM base
GROUP BY 1
ORDER BY 1;

```

| facebook_friends        |
|-------------------------|
| user1: int              |
| user2: int              |


---


#### Company: LinkedIn

### [Risky Projects](https://platform.stratascratch.com/coding/10304-risky-projects?code_type=1) Medium

#### Q. Identify projects that are at risk for going overbudget. A project is considered to be overbudget if the cost of all employees assigned to the project is greater than the budget of the project. You'll need to prorate the cost of the employees to the duration of the project. For example, if the budget for a project that takes half a year to complete is 10K, then the total half-year salary of all employees assigned to the project should not exceed 10K,thenthetotalhalf−yearsalaryofallemployeesassignedtotheprojectshouldnotexceed10K. Salary is defined on a yearly basis, so be careful how to calculate salaries for the projects that last less or more than one year. Output a list of projects that are overbudget with their project name, project budget, and prorated total employee expense (rounded to the next dollar amount).

```diff
with tab as (select lep.project_id, lp.title,lp.budget, 
            lp.budget/(lp.end_date - lp.start_date) as per_day_cost,  
            le.salary from linkedin_projects lp
            left join linkedin_emp_projects lep
            on lp.id = lep.project_id
            right join linkedin_employees le
            on le.id = lep.emp_id),

sal as(select t.project_id, sum(t.salary)/365 per_day_sal from tab t 
        group by 1 order by 1)

select distinct tab.title, tab.budget, ceil(sal.per_day_sal) expense from tab right join sal on tab.project_id = sal.project_id
    where sal.per_day_sal > tab.per_day_cost
    order by 1;

```

| linkedin_projects       | linkedin_emp_projects  | linkedin_employees      |
|-------------------------|------------------------|-------------------------|
| id: int                 | emp_id:int             | id: int                 |
| title: varchar          | project_id: int        | first_name : varchar    |
| budget: int             |                        | last_name : varchar     |
| start_date: datetime    |                        | salary : int            |
| end_date: datetime      |                        |                         |


---



#### Company: Dropbox

### [Salaries Differences](https://platform.stratascratch.com/coding/10308-salaries-differences?code_type=1) Easy

#### Q. Write a query that calculates the difference between the highest salaries found in the marketing and engineering departments. Output just the absolute difference in salaries.

```diff
with depts as(select e.salary, d.department from db_employee e
                left join
                    db_dept d 
                on e.department_id = d.id
                where department='engineering' or department='marketing') 

select abs((select max(salary) from depts where department='engineering')-(select max(salary) from depts where department='marketing'));

```

| db_employee             | db_dept                | 
|-------------------------|------------------------|
| id: int                 | id: int                | 
| first_name: varchar     | department: int        | 
| last_name: varchar      |                        |
| salary: int             |                        |
| department_id: int      |                        | 
| email: datetime         |                        |

---

#### Company: Airbnb

### [matching hosts and guests](https://platform.stratascratch.com/coding/10078-find-matching-hosts-and-guests-in-a-way-that-they-are-both-of-the-same-gender-and-nationality?code_type=1) Medium

#### Q. Find matching hosts and guests pairs in a way that they are both of the same gender and nationality. Output the host id and the guest id of matched pair.

```diff
select distinct ah.host_id, ag.guest_id 
from airbnb_hosts ah
join airbnb_guests ag 
on ah.nationality = ag.nationality and  ah.gender = ag.gender;

```

| airbnb_hosts            | airbnb_guests          | 
|-------------------------|------------------------|
| host_id: int            | guest_id: int          | 
| nationality: varchar    | nationality: varchar   |
| gender: varchar         | gender: varchar        |
| age: int                | age: int               |

---


#### Company: Meta/Facebook

### [Find all posts which were reacted to with a heart](https://platform.stratascratch.com/coding/10087-find-all-posts-which-were-reacted-to-with-a-heart?code_type=1) Easy

#### Q. Find all posts which were reacted to with a heart. For such posts output all columns from facebook_posts table.

```diff
select distinct fp.post_id, fp.poster, fp.post_text, fp.post_keywords, fp.post_date
from facebook_reactions fr 
join facebook_posts fp
on fr.post_id = fp.post_id
where fr.reaction = 'heart';

```

| facebook_reactions      | facebook_posts          | 
|-------------------------|-------------------------|
| poster: int             | post_id: int            | 
| friend: int             | poster: int             |
| reaction: varchar       | post_text: varchar      |
| date_day: int           | post_keywords: varchar  |
| post_id: int            | post_date: datetime     |


---


#### Company: Dropbox

### [Employee and Manager Salaries](https://platform.stratascratch.com/coding/9894-employee-and-manager-salaries?code_type=1) Medium

#### Q. Find employees who are earning more than their managers. Output the employee's first name along with the corresponding salary.

```diff
select e1.first_name as name, e1.salary as salary from employee e1 
join employee e2
on e1.manager_id = e2.id
where e1.salary > e2.salary ;

```

| employee                |
|-------------------------|
| id: int                 |
| first_name: varchar     |
| last_name: varchar      |
| age: int                |
| sex: varchar            |
| employee_title: varchar |
| department: varchar     |
| salary: int             |
| target: int             |
| bonus: int              |
| email: varchar          |
| city: varchar           |
| address: varchar        |
| manager_id: int         |

---


#### Company: Meta/Facebook

### [Acceptance Rate By Date](https://platform.stratascratch.com/coding/10285-acceptance-rate-by-date?code_type=1) Medium

#### Q. What is the overall friend acceptance rate by date? Your output should have the rate of acceptances by the date the request was sent. Order by the earliest date to latest.
Assume that each friend request starts by a user sending (i.e., user_id_sender) a friend request to another user (i.e., user_id_receiver) that's logged in the table with action = 'sent'. If the request is accepted, the table logs action = 'accepted'. If the request is not accepted, no record of action = 'accepted' is logged.


```diff
with q1 as (select fr1.date, count(fr1.date) as dcnt
from fb_friend_requests fr1
join fb_friend_requests fr2
on fr1.user_id_sender = fr2.user_id_sender and 
    fr1.user_id_receiver = fr2.user_id_receiver and 
    fr1.action != fr2.action
where fr1.action = 'sent'
group by fr1.date),

q2 as (select q.date, count(q.action) as cnt
from (select * from fb_friend_requests where action='sent')q
group by q.date)

select q1.date, (q1.dcnt*1.0)/(1.0*q2.cnt)::float as acc_Rate
from q1 join q2 on q1.date=q2.date;


```

| fb_friend_requests      |
|-------------------------|
| id: int                 |
| user_id_sender: varchar |
| user_id_receiver: varchar
| date: datetime
| action: varchar

---



#### Company: Lyft

### [Distances Traveled](https://platform.stratascratch.com/coding/10324-distances-traveled?code_type=1) Medium

#### Q. Find the top 10 users that have traveled the greatest distance. Output their id, name and a total distance traveled.

```diff
select q1.user_id, q2.name, q1.distance
from lyft_users as q2
right join
(select user_id, distance from lyft_rides_log 
order by distance desc
limit 10)q1
on q1.user_id = q2.id;


```
| lyft_rides_log      | lyft_users          | 
|---------------------|---------------------|
| id: int             | id: int             | 
| user_id: int        | name: varchar       |
| distance: int       | post_text: varchar  |


---


#### Company: City of San Francisco

### [Income By Title and Gender](https://platform.stratascratch.com/coding/10077-income-by-title-and-gender?code_type=1) Medium

#### Q. Find the average total compensation based on employee titles and gender. Total compensation is calculated by adding both the salary and bonus of each employee. However, not every employee receives a bonus so disregard employees without bonuses in your calculation. Employee can receive more than one bonus. Output the employee title, gender (i.e., sex), along with the average total compensation.

```diff
select employee_title,sex, avg(salary+avgs) as avg_compensation from(select e.employee_title,e.sex,e.salary,sum(b.bonus) as avgs from sf_employee e
join sf_bonus b on e.id = b.worker_ref_id
where b.bonus is not null
group by 1,2,3)t
group by 1,2;

```

| sf_employee              | sf_bonus             | 
|--------------------------|----------------------|
| id: int                  | worker_ref_id: int   |
| first_name: varchar      | bonus: int           |
| last_name: varchar       | bonus_date: datetime |
| age: int
| sex: varchar
| employee_title: varchar
| department: varchar
| salary: int
| target: int
| email: varchar
| city: varchar
| address: varchar
| manager_id: int

---

#### Company: Microsoft

### [Premium vs Freemium](https://platform.stratascratch.com/coding/10300-premium-vs-freemium?code_type=3) Hard

#### Q. Find the total number of downloads for paying and non-paying users by date. Include only records where non-paying customers have more downloads than paying customers. The output should be sorted by earliest date first and contain 3 columns date, non-paying downloads, paying downloads.

```diff

SELECT date, SUM(IF(paying_customer = 'no', downloads, 0)) AS non_paying,
            SUM(IF(paying_customer = 'yes', downloads, 0)) AS paying
FROM ms_download_facts
JOIN ms_user_dimension AS mud
     USING(user_id)
JOIN ms_acc_dimension AS mad
     USING(acc_id)
GROUP BY date 
HAVING non_paying > paying
ORDER BY date ASC;

```


|    ms_user_dimension     |     ms_acc_dimension     |    ms_download_facts     |
|--------------------------|--------------------------|--------------------------|
|user_id     ->   int      |acc_id     ->   int       |date        -> datetime   |
|acc_id      ->   int      |paying_customer -> varchar|user_id     -> int        |
|                          |                          |downloads:  -> int        |

--- 

#### Company: Amazon

### [Order Details](https://platform.stratascratch.com/coding/9913-order-details?code_type=1) Easy

#### Q. Find order details made by Jill and Eva. Consider the Jill and Eva as first names of customers. Output the order date, details and cost along with the first name. Order records based on the customer id in ascending order.

```diff
select c.first_name, o.order_date, o.order_details, o.total_order_cost
from customers c join orders o 
on c.id = o.cust_id
where c.first_name in ('Jill', 'Eva')
order by c.id ;
```


| customers               |
|-------------------------|
| id: int                 |
| first_name: varchar
| last_name: varchar
| city: varchar
| address: varchar
| phone_number: varchar

| orders                  |
|-------------------------|
| id: int                 |
| cust_id: int
| order_date: datetime
| order_details: varchar
| total_order_cost: int

--- 


#### Company: Amazon

### [Order Details](https://platform.stratascratch.com/coding/9891-customer-details?code_type=1) Easy

#### Q. Find the details of each customer regardless of whether the customer made an order. Output the customer's first name, last name, and the city along with the order details. You may have duplicate rows in your results due to a customer ordering several of the same items. Sort records based on the customer's first name and the order details in ascending order.

```diff
select c.first_name, c.last_name, c.city, o.order_details
from customers c left join orders o
on c.id = o.cust_id
order by 1, 4;
```


| customers               |
|-------------------------|
| id: int                 |
| first_name: varchar
| last_name: varchar
| city: varchar
| address: varchar
| phone_number: varchar

| orders                  |
|-------------------------|
| id: int                 |
| cust_id: int
| order_date: datetime
| order_details: varchar
| total_order_cost: int

--- 


#### Company: Microsoft

### [Premium vs Freemium](https://platform.stratascratch.com/coding/10300-premium-vs-freemium?code_type=3) Hard

#### Q. Find the total number of downloads for paying and non-paying users by date. Include only records where non-paying customers have more downloads than paying customers. The output should be sorted by earliest date first and contain 3 columns date, non-paying downloads, paying downloads.

```diff

SELECT date, SUM(IF(paying_customer = 'no', downloads, 0)) AS non_paying,
            SUM(IF(paying_customer = 'yes', downloads, 0)) AS paying
FROM ms_download_facts
JOIN ms_user_dimension AS mud
     USING(user_id)
JOIN ms_acc_dimension AS mad
     USING(acc_id)
GROUP BY date 
HAVING non_paying > paying
ORDER BY date ASC;

```


|    ms_user_dimension     |     ms_acc_dimension     |    ms_download_facts     |
|--------------------------|--------------------------|--------------------------|
|user_id     ->   int      |acc_id     ->   int       |date        -> datetime   |
|acc_id      ->   int      |paying_customer -> varchar|user_id     -> int        |
|                          |                          |downloads:  -> int        |

---

#### List the names of employees who have worked in at least 3 different departments, with their current department and their previous department(s) listed in chronological order.

```diff 
WITH RECURSIVE employee_history(id, department_list) AS (
    SELECT id, ARRAY[department] AS department_list
    FROM employees
    UNION ALL
    SELECT e.id, e.department || eh.department_list
    FROM employee_history eh
    JOIN employees e ON eh.id = e.manager_id AND NOT e.department = ALL(eh.department_list)
)
SELECT e.name, e.department, eh.department_list
FROM employee_history eh
JOIN employees e ON eh.id = e.id
WHERE array_length(eh.department_list, 1) >= 3;
```

---

#### Given a table containing employee names, their respective salaries, and their department IDs, write a SQL query to find the highest-paid employee in each department:
```diff 
SELECT e1.employee_id, e1.name, e1.salary, e1.department_id
FROM employees e1
JOIN (
  SELECT department_id, MAX(salary) AS max_salary
  FROM employees
  GROUP BY department_id
) e2 ON e1.department_id = e2.department_id AND e1.salary = e2.max_salary

```

---
#### Company: Amazon

### [Best Selling Item](https://platform.stratascratch.com/coding/10172-best-selling-item?code_type=1) Hard

#### Q. Find the best selling item for each month (no need to separate months by year) where the biggest total invoice was paid. The best selling item is calculated using the formula (unitprice * quantity). Output the description of the item along with the amount paid.

```diff

SELECT 
    MONTH(invoicedate) AS month, 
    description AS best_selling_item, 
    MAX(total_amount_paid) AS amount_paid 
FROM (
    SELECT 
        MONTH(invoicedate) AS month, 
        stockcode, 
        description, 
        SUM(quantity * unitprice) AS total_amount_paid 
    FROM online_retail 
    GROUP BY 
        MONTH(invoicedate), 
        stockcode, 
        description
) AS monthly_sales 
JOIN (
    SELECT 
        MONTH(invoicedate) AS month, 
        MAX(total_amount_paid) AS max_total_amount_paid 
    FROM (
        SELECT 
            MONTH(invoicedate) AS month, 
            SUM(quantity * unitprice) AS total_amount_paid 
        FROM online_retail 
        GROUP BY MONTH(invoicedate)
    ) AS monthly_totals
    GROUP BY MONTH(invoicedate)
) AS max_monthly_totals 
ON monthly_sales.month = max_monthly_totals.month 
    AND monthly_sales.total_amount_paid = max_monthly_totals.max_total_amount_paid 
GROUP BY MONTH(invoicedate)


```
online_retail |
invoiceno: varchar|
stockcode: varchar
description: varchar
quantity: int
invoicedate: datetime
unitprice: float
customerid: float
country: varchar

---


#### Company: Microsoft

### [Premium vs Freemium](https://platform.stratascratch.com/coding/10300-premium-vs-freemium?code_type=1) Hard

#### Q. Find the total number of downloads for paying and non-paying users by date. Include only records where non-paying customers have more downloads than paying customers. The output should be sorted by earliest date first and contain 3 columns date, non-paying downloads, paying downloads.

###### Optimal solution
```diff
with out AS(select date
, Sum (downloads) Filter(Where paying_customer = 'no') as non_paying
, Sum (downloads) Filter(Where paying_customer = 'yes') as paying
From  ms_download_facts fact
Left Join ms_user_dimension a
on fact.user_id = a.user_id
Join ms_acc_dimension acc
on a.acc_id = acc.acc_id
Group by date
order by date)
Select date , non_paying , paying
From out
Where non_paying > paying;
```

---


#### Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'. Primary diagnosis is stored in the admissions table.


```diff
SELECT p.patient_id, p.first_name, p.last_name 
FROM patients p 
INNER JOIN admissions a ON p.patient_id = a.patient_id 
WHERE a.diagnosis = 'Dementia';
```

---
