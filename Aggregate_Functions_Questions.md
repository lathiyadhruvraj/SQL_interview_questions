#### Company: DoorDash

### [Workers With The Highest Salaries](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries?code_type=1) Medium

#### Q. Find the titles of workers that earn the highest salary. Output the highest-paid title or multiple titles that share the highest salary.

```diff
with q1 as( 
 select t2.worker_title as title, t1.salary as salary  from
 worker t1 left join title t2 on t1.worker_id = t2.worker_ref_id
)

select title from q1
where salary = (select max(salary) from worker);

```

| worker                   |  title                     |
|--------------------------|----------------------------|
|worker_id   ->   int      |  worker_ref_id -> int      |
|first_name  ->   varchar  |  worker_title -> varchar   |
|last_name   ->   varchar  |  affected_from -> datetime |
|salary      ->   int      |
|joining_date ->  datetime |
|department  ->   varchar  |


---


#### Company: Amazon

### [Monthly Percentage Difference](https://platform.stratascratch.com/coding/10319-monthly-percentage-difference?code_type=1) Hard

#### Q. 
Given a table of purchases by date, calculate the month-over-month percentage change in revenue. The output should include the year-month date (YYYY-MM) and percentage change, rounded to the 2nd decimal point, and sorted from the beginning of the year to the end of the year.
The percentage change column will be populated from the 2nd month forward and can be calculated as ((this month's revenue - last month's revenue) / last month's revenue)*100.

```diff
SELECT 
    year_month,
    ROUND((value - LAG(value) OVER (ORDER BY year_month)) / LAG(value) OVER (ORDER BY year_month) * 100, 2) revenue_diff_pct 
FROM (
select to_char(created_at, 'YYYY-MM') as year_month, SUM(value) AS value 
from sf_transactions
GROUP BY 1
) x

```

| worker                   |
|--------------------------|
|id          ->   int      |
|created_at  ->   datetime | 
|value       ->   int      | 
|purchase_id ->   int      |



---

