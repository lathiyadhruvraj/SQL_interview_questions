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
