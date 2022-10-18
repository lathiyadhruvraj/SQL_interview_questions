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
