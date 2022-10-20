##### Company: DoorDash

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

#### Q. Given a table of purchases by date, calculate the month-over-month percentage change in revenue. The output should include the year-month date (YYYY-MM) and percentage change, rounded to the 2nd decimal point, and sorted from the beginning of the year to the end of the year.
#### The percentage change column will be populated from the 2nd month forward and can be calculated as ((this month's revenue - last month's revenue) / last month's revenue)*100.

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


#### Company: Lyft

### [Distance travelled](https://platform.stratascratch.com/coding/10324-distances-traveled?code_type=1) Medium

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

| lyft_rides_log           |  lyft_users                |
|--------------------------|----------------------------|
|id          ->   int      |  id -> int                 |
|user_id     ->   int      |  name -> varchar           |
|distance    ->   int      |                            |


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

### [Highest Cost Orders](https://platform.stratascratch.com/coding/9915-highest-cost-orders?code_type=1) Medium

#### Q. Find the customer with the highest daily total order cost between 2019-02-01 to 2019-05-01. If customer had more than one order on a certain day, sum the order costs on daily basis. Output customer's first name, total cost of their items, and the date.

```diff
select c.first_name,  SUM(o.total_order_cost) as total_cost, o.order_date from customers c join orders o on c.id=o.cust_id
    where (timestamp '2019-02-01') < o.order_date and o.order_date < (timestamp '2019-05-01') 
    group by o.order_date, first_name
    order by total_cost desc, o.order_date
    limit 1;

```

| customers                |  orders                    |
|--------------------------|----------------------------|
| id   ->   int            |  id   ->   int             |
| first_name  ->   varchar |  cust_id -> int            |
| last_name   ->   varchar |  order_date -> datetime    |
| city      ->   varchar   |  order_details -> varchar  |
| address ->  varchar      |  total_order_cost -> int   |
| phone_number -> varchar  |


---

### Company: Amazon

### [Marketing Campaign Success [Advanced]](https://platform.stratascratch.com/coding/514-marketing-campaign-success-advanced?code_type=1) Hard

#### Q. You have a table of in-app purchases by user. Users that make their first in-app purchase are placed in a marketing campaign where they see call-to-actions for more in-app purchases. Find the number of users that made additional in-app purchases due to the success of the marketing campaign.

#### The marketing campaign doesn't start until one day after the initial in-app purchase so users that only made one or multiple purchases on the first day do not count, nor do we count users that over time purchase only the products they purchased on the first day.

```diff
select count(distinct counta.user_id) from (
 select user_id, case when min(created_at) over (Partition By user_id)
 <> min(created_at) over(partition by user_id,product_id) then 1 else 0 end as campaign
 from marketing_campaign
 ) counta
 where campaign = 1;

```

| marketing_campaign       |
|--------------------------|
|user_id     ->   int      |
|created_at  ->   datetime | 
|product_id  ->   int      | 
|quantity ->      int      |
|price ->         int      |


---

---

### Company: Amazon

### [Customer Average Orders](https://platform.stratascratch.com/coding/2013-customer-average-orders?code_type=1) Easy

#### Q. How many customers placed an order and what is the average order amount?


```diff
select count(distinct counta.user_id) from (
 select user_id, case when min(created_at) over (Partition By user_id)
 <> min(created_at) over(partition by user_id,product_id) then 1 else 0 end as campaign
 from marketing_campaign
 ) counta
 where campaign = 1;

```

| marketing_campaign       |
|--------------------------|
|user_id     ->   int      |
|created_at  ->   datetime | 
|product_id  ->   int      | 
|quantity ->      int      |
|price ->         int      |


---



