### Company: Yelp

### [Top 5 States With 5 Star Businesses](https://platform.stratascratch.com/coding/10046-top-5-states-with-5-star-businesses?code_type=1) Hard

#### Q. Find the top 5 states with the most 5 star businesses. Output the state name along with the number of 5-star businesses and order records by the number of 5-star businesses in descending order. In case there are ties in the number of businesses, return all the unique states. If two states have the same result, sort them in alphabetical order.

```diff
select state, cnt from
    (select state, count(stars) as cnt,
        RANK() over(order by count(stars) desc) ranked
    from yelp_business
    where stars=5
    group by state
    order by 2 desc, 1 asc) q1

where q1.ranked <= 5;


```

| yelp_business            |
|--------------------------|
| business_id: varchar     |
| name: varchar            |
| neighborhood: varchar    |
| address: varchar         |
| city: varchar            |
| state: varchar           |
| postal_code: varchar     |
| latitude: float          |
| longitude: float         |
| stars: float             |
| review_count: int        |
| is_open: int             |
| categories: varchar      |


---

#### Company: Medium

### [Ranking Most Active Guests](https://platform.stratascratch.com/coding/10046-top-5-states-with-5-star-businesses?code_type=1) Hard

#### Q. Rank guests based on the number of messages they've exchanged with the hosts. Guests with the same number of messages as other guests should have the same rank. Do not skip rankings if the preceding rankings are identical.Output the rank, guest id, and number of total messages they've sent. Order by the highest number of total messages first.

```diff
select dense_rank() over(order by sum(n_messages) desc) as ranking,
        id_guest, sum(n_messages) as sum_n_messages
from airbnb_contacts
group by 2
order by 3 desc;


```

| airbnb_contacts          |
|--------------------------|
| id_guest: varchar        |
| id_host: varchar
| id_listing: varchar 
| ts_contact_at: datetime
| ts_reply_at: datetime
| ts_accepted_at: datetime
| ts_booking_at: datetime
| ds_checkin: datetime
| ds_checkout: datetime
| n_guests: int
| n_messages: int


---

#### Company: Netflix

### [Top Percentile Fraud](https://platform.stratascratch.com/coding/10303-top-percentile-fraud?code_type=1) Hard

#### Q. ABC Corp is a mid-sized insurer in the US and in the recent past their fraudulent claims have increased significantly for their personal auto insurance portfolio. They have developed a ML based predictive model to identify propensity of fraudulent claims. Now, they assign highly experienced claim adjusters for top 5 percentile of claims identified by the model.Your objective is to identify the top 5 percentile of claims from each state. Your output should be policy number, state, claim cost, and fraud score.

with window fn -
```diff
SELECT policy_num,
       state,
       claim_cost,
       fraud_score,
FROM
  (SELECT *,
          ntile(100) over(PARTITION BY state
                          ORDER BY fraud_score DESC) AS percentile
   FROM fraud_score)a
WHERE percentile <=5;

```
Second Approach
```diff
select a.policy_num, a.state, a.claim_cost, a.fraud_score 
from
    (select *, rank() over(partition by state order by fraud_score) rnk
    from fraud_Score) a
left join
    (select ceil(0.05*count(policy_num)) as top5, state from fraud_score
    group by state order by state) b
on b.state = a.state
where a.rnk <= b.top5;

```

| fraud_score            |
|------------------------|
| policy_num : varchar   |
| state : varchar        | 
| claim_cost : int       |
| fraud_score : float    |


---

### Company: Salesforce

### [Average Salaries](https://platform.stratascratch.com/coding/9917-average-salaries?code_type=1) Easy


#### Q. Compare each employee's salary with the average salary of the corresponding department. Output the department, first name, and salary of employees along with the average salary of that department.

```
SELECT 
        department, 
        first_name, 
        salary, 
        AVG(salary) over (PARTITION BY department) 
FROM employee;
```

| employee                  |
|---------------------------|
| id: int                   |
| first_name: varchar       |
| last_name: varchar        |
| age: int                  |
| sex: varchar
| employee_title: varchar
| department: varchar
| salary: int
| target: int
| bonus: int
| email: varchar
| city: varchar
| address: varchar
| manager_id: int


---


