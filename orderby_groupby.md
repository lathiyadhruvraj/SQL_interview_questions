#### Company: ESPN

### [Largest Olympics](https://platform.stratascratch.com/coding/9942-largest-olympics?code_type=1) Medium

#### Q. Find the Olympics with the highest number of athletes. The Olympics game is a combination of the year and the season, and is found in the 'games' column. Output the Olympics along with the corresponding number of athletes.

```diff
select games, count(DISTINCT name) as athelete_count 
from olympics_athletes_events
group by games
order by athelete_count desc
limit 1;

```

| olympics_athletes_events |
|--------------------------|
| id: int                  |
| name: varchar            |
| sex: varchar             |
| age: float               |
| height: float            |
| weight: datetime         |
| team: varchar            | 
| noc: varchar             |  
| games: varchar           |
| year: int                |
| season: varchar          |
| city: varchar            |
| sport: varchar           |
| event: varchar           |
| medal: varchar           |



---

#### Company: Airbnb

### [Host Popularity Rental Prices](https://platform.stratascratch.com/coding/9632-host-popularity-rental-prices?code_type=1) Hard

#### Q. You’re given a table of rental property searches by users. The table consists of search results and outputs host information for searchers. Find the minimum, average, maximum rental prices for each host’s popularity rating. The host’s popularity rating is defined as below: <br>
0 reviews: New <br>
1 to 5 reviews: Rising <br>
6 to 15 reviews: Trending Up <br>
16 to 40 reviews: Popular <br>
more than 40 reviews: Hot <br>


Tip: The id column in the table refers to the search ID. You'll need to create your own host_id by concating price, room_type, host_since, zipcode, and number_of_reviews.


Output host popularity rating and their minimum, average and maximum rental prices.


```diff
WITH output AS (SELECT CONCAT(price, room_type, host_since, zipcode, number_of_reviews) as host_id,
CASE
    WHEN number_of_reviews = 0 THEN 'New'
    WHEN number_of_reviews < 6 THEN 'Rising'
    WHEN number_of_reviews < 16 THEN 'Trending Up'
    WHEN number_of_reviews < 41 THEN 'Popular'
    ELSE 'Hot'
END AS popularity,
price
FROM airbnb_host_searches
GROUP BY host_id, popularity, price)
SELECT popularity, MIN(price), AVG(price), MAX(price)
FROM output
GROUP BY popularity;
```

| airbnb_host_searches     |
|--------------------------|
| id: int                  |
| price: float
| property_type: varchar
| room_type: varchar
| amenities: varchar
| accommodates: int
| bathrooms: int
| bed_type: varchar
| cancellation_policy: varchar
| cleaning_fee: bool
| city: varchar
| host_identity_verified: varchar
| host_response_rate: varchar
| host_since: datetime
| neighbourhood: varchar
| number_of_reviews: int
| review_scores_rating: float
| zipcode: int
| bedrooms: int 
| beds: int



---

#### Company: Spotify

### [Find the top 10 ranked songs in 2010](https://platform.stratascratch.com/coding/9650-find-the-top-10-ranked-songs-in-2010?code_type=1) Medium

#### Q. What were the top 10 ranked songs in 2010? Output the rank, group name, and song name but do not show the same song twice. Sort the result based on the year_rank in ascending order.


```diff
select distinct(year_rank) as rnk, group_name, song_name 
from billboard_top_100_year_end
where year=2010 
order by year_rank asc
limit 10;

```

| billboard_top_100_year_end     |
|--------------------------------|
| id: int                        |
| year: int
| year_rank: int
| group_name: varchar
| artist: varchar
| song_name: varchar



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

```diff
with tab as (select user_id, paying_customer from 
    ms_user_dimension mud inner join ms_acc_dimension mad
    on mud.acc_id = mad.acc_id),

paying as (select mdf.date, sum(mdf.downloads) paying from
            tab inner join ms_download_facts mdf
            on tab.user_id = mdf.user_id
            where paying_customer='yes'
            group by date
            order by date
),

non_paying as (select mdf.date, sum(mdf.downloads) non_paying from
            tab inner join ms_download_facts mdf
            on tab.user_id = mdf.user_id
            where paying_customer='no'
            group by date
            order by date
),

final as(select paying.date, paying.paying, non_paying.non_paying , 
    case when paying.paying < non_paying.non_paying 
    then 1 else 0 end as mark
    from paying inner join non_paying 
    on paying.date=non_paying.date)

select final.date, final.non_paying, final.paying from final where final.mark=1;

```

| ms_user_dimension     | ms_acc_dimension          | ms_download_facts   |
|-----------------------|---------------------------|---------------------|
| user_id: int          | acc_id: int               | date: datetime      |
| acc_id: int           | paying_customer: varchar  | user_id: int        |
|                       |                           | downloads: int      |

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


#### Company: Forbes

### [Most Profitable Companies](https://platform.stratascratch.com/coding/9680-most-profitable-companies?code_type=1) Medium

#### Q. Find the 3 most profitable companies in the entire world. Output the result along with the corresponding company name. Sort the result based on profits in descending order.


```diff
select profits, company from forbes_global_2010_2014
order by profits desc
limit 3;

```

| forbes_global_2010_2014        |
|--------------------------------|
| company: varchar               |
| sector: varchar                |
| industry: varchar
| continent: varchar
| country: varchar
| marketvalue: float
| sales: float
| profits: float
| assets: float
| rank: int
| forbeswebpage: varchar


---

#### Company: Lyft

### [Bikes Last Used](https://platform.stratascratch.com/coding/9680-most-profitable-companies?code_type=1) Easy

#### Q. Find the last time each bike was in use. Output both the bike number and the date-timestamp of the bike's last use (i.e., the date-time the bike was returned). Order the results by bikes that were most recently used.

```diff
select bike_number, max(end_time)as time from dc_bikeshare_q1_2012
group by 1
order by 2;

```

| dc_bikeshare_q1_2012        |
|-----------------------------|
| duration: varchar           |
| duration_seconds: int       |
| start_time: datetime
| start_station: varchar 
| start_terminal: int 
| end_time: datetime
| end_station: varchar
| end_terminal: int
| bike_number: varchar
| rider_type: varchar
| id: int


---

### Company: Spotify

### [Top Ranked Songs](https://platform.stratascratch.com/coding/9991-top-ranked-songs?code_type=1) Medium


#### Q. Find songs that have ranked in the top position. Output the track name and the number of times it ranked at the top. Sort your records by the number of times the song was in the top position in descending order.

```diff
select q.trackname, sum(q.position) as times
from
    (select trackname, position
    from spotify_worldwide_daily_song_ranking
    where position=1) q
group by q.trackname
order by times desc;

```

| spotify_worldwide_daily_song_ranking |
|--------------------------------------|
| id          ->   int                 | 
| position    ->   int                 | 
| trackname   ->   varchar             |  
| artist      ->   varchar             |
| streams     ->   int                 |
| url         ->   varchar             |
| date        ->   datetime            |
| region      ->   varchar             |


---


### Company: Redfin

### [Rush Hour Calls](https://platform.stratascratch.com/coding/2023-rush-hour-calls?code_type=1) Medium


#### Q. Redfin helps clients to find agents. Each client will have a unique request_id and each request_id has several calls. For each request_id, the first call is an “initial call” and all the following calls are “update calls”.  How many customers have called 3 or more times between 3 PM and 6 PM (initial and update calls combined)?

```diff
SELECT count(*)
FROM
  (SELECT DISTINCT request_id
   FROM redfin_call_tracking
   WHERE date_part('hour', created_on::TIMESTAMP) BETWEEN 15 AND 18
   GROUP BY request_id
   HAVING count(*)>=3) sq

```

| redfin_call_tracking       |
|----------------------------|
| created_on ->   datetime   | 
| request_id ->   int        | 
| call_duration ->   int     |  
| id      ->   int           |


---

### Company: Forbes

### [Most Profitable Companies](https://platform.stratascratch.com/coding/10354-most-profitable-companies?code_type=1) Medium


#### Q. Find the 3 most profitable companies in the entire world. Output the result along with the corresponding company name. Sort the result based on profits in descending order.

```diff
select company, profits from forbes_global_2010_2014
order by profits desc
limit 3;
```

| forbes_global_2010_2014       |
|-------------------------------|
| company: varchar              |
| sector: varchar               |
| industry: varchar
| continent: varchar
| country: varchar
| marketvalue: float
| sales: float
| profits: float
| assets: float
| rank: int
| forbeswebpage: varchar


---

### Company: Yelp

### [Top Businesses With Most Reviews](https://platform.stratascratch.com/coding/10048-top-businesses-with-most-reviews?code_type=1) Medium


#### Q. Find the top 5 businesses with most reviews. Assume that each row has a unique business_id such that the total reviews for each business is listed on each row. Output the business name along with the total number of reviews and order your results by the total reviews in descending order.

```diff
select name, review_count from yelp_business
order by 2 desc
limit 5;
```

| yelp_business                 |
|-------------------------------|
| business_id: varchar          |
| name: varchar
| neighborhood: varchar
| address: varchar
| city: varchar
| state: varchar
| postal_code: varchar
| latitude: float
| longitude: float
| stars: float
| review_count: int
| is_open: int
| categories: varchar

---

### Company: Spotify

### [Find how many times each artist appeared on the Spotify ranking list](https://platform.stratascratch.com/coding/9992-find-artists-that-have-been-on-spotify-the-most-number-of-times?code_type=1) Easy


#### Q. Find how many times each artist appeared on the Spotify ranking list Output the artist name along with the corresponding number of occurrences. Order records by the number of occurrences in descending order.

```diff
select artist, count(id) as occurrences
from spotify_worldwide_daily_song_ranking
group by 1
order by 2 desc;
```

| spotify_worldwide_daily_song_ranking    |
|-----------------------------------------|
| id: int                                 |
| position: int                           |
| trackname: varchar
| artist: varchar
| streams: int
| url: varchar
| date: varchar
| region: varchar


---

### Company: Salesforce

### [Average Salaries](https://platform.stratascratch.com/coding/9917-average-salaries?code_type=1) Easy


#### Q. Compare each employee's salary with the average salary of the corresponding department. Output the department, first name, and salary of employees along with the average salary of that department.

```diff
with q1 as(
    select department as dept, avg(salary) as avg_sal
    from employee
    group by 1
)

select emp.department, emp.first_name, emp.salary, q1.avg_sal  from
employee as emp left join q1
on emp.department = q1.dept
order by 1;
```
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



### Company: Forbes

### [Find the most profitable company in the financial sector of the entire world along with its continent](https://platform.stratascratch.com/coding/9663-find-the-most-profitable-company-in-the-financial-sector-of-the-entire-world-along-with-its-continent?code_type=1) Easy

#### Q. Find the most profitable company from the financial sector. Output the result along with the continent.


```diff

select company, continent 
from forbes_global_2010_2014 
where sector = 'Financials' 
order by profits desc 
limit 1;

```


---


### Company: Airbnb

### [Number Of Bathrooms And Bedrooms](https://platform.stratascratch.com/coding/9622-number-of-bathrooms-and-bedrooms?code_type=1) Easy

#### Q. Find the average number of bathrooms and bedrooms for each city’s property types. Output the result along with the city name and the property type.


```diff

select property_type, city, sum(bathrooms), sum(bedrooms) 
from airbnb_search_details
group by 1, 2

```

|  airbnb_search_details    |
|---------------------------|
| id: int                   |
| price: float
| property_type: varchar
| room_type: varchar
| amenities: varchar
| accommodates: int
| bathrooms: int
| bed_type: varchar
| cancellation_policy: varchar
| cleaning_fee: bool
| city: varchar
| host_identity_verified: varchar
| host_response_rate: varchar
| host_since: datetime
| neighbourhood: varchar
| number_of_reviews: int
| review_scores_rating: float
| zipcode: int
| bedrooms: int
| beds: int

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

#### Company: Most Profitable Companies

### [Forbes](https://platform.stratascratch.com/coding/10354-most-profitable-companies?code_type=3) Easy

#### Q. Find the 3 most profitable companies in the entire world. Output the result along with the corresponding company name. Sort the result based on profits in descending order.


```diff
select company, profits
from forbes_global_2010_2014
order by 2 desc
limit 3;
```


|    forbes_global_2010_2014     |   
|--------------------------------|
| company: varchar               |
| sector: varchar
| industry: varchar
| continent: varchar
| country: varchar
| marketvalue: float
| sales: float
| profits: float
| assets: float
| rank: int
| forbeswebpage: varchar

---

### Find the top 3 most common pairs of departments that employees have worked in, sorted in descending order of frequency.

```diff
SELECT dept1, dept2, COUNT(*) AS frequency
FROM (
    SELECT e1.department AS dept1, e2.department AS dept2
    FROM employees e1
    JOIN employees e2 ON e1.id = e2.manager_id
) AS department_pairs
GROUP BY dept1, dept2
ORDER BY frequency DESC
LIMIT 3;
```


---

### Find the top 5 pairs of employees who have worked together in the most number of departments, sorted in descending order of frequency.

```diff
SELECT e1.name AS employee1, e2.name AS employee2, COUNT(*) AS frequency
FROM employees e1
JOIN employees e2 ON e1.id < e2.id AND e1.department = e2.department
GROUP BY e1.id, e2.id
ORDER BY frequency DESC
LIMIT 5;

```

---

### Given a table containing the number of orders for each customer, write a SQL query to find the top 10% of customers who have made the most orders:

```diff
SELECT customer_id, SUM(order_count) as total_orders
FROM orders
GROUP BY customer_id
ORDER BY total_orders DESC
LIMIT (SELECT COUNT(DISTINCT customer_id) * 0.1 FROM orders)


```

---


### Given two tables "orders" and "order_items", write a SQL query to find the top 5 products with the highest revenue in the last 30 days:

```diff
SELECT oi.product_id, SUM(oi.price * oi.quantity) as revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.order_date >= DATEADD(day, -30, GETDATE())
GROUP BY oi.product_id
ORDER BY revenue DESC
LIMIT 5



```

---
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


### Company: Meta/Facebook

### [Customer Revenue In March](https://platform.stratascratch.com/coding/9782-customer-revenue-in-march?code_type=1) Medium

#### Q. Calculate the total revenue from each customer in March 2019. Include only customers who were active in March 2019. Output the revenue along with the customer id and sort the results based on the revenue in descending order.

```diff
SELECT cust_id, SUM(total_order_cost) AS revenue
FROM orders
-- WHERE DATE(order_date) BETWEEN '2019-03-01' AND  '2019-03-31'
WHERE TO_CHAR(order_date, 'YYYY-MM') = '2019-03'
GROUP BY cust_id
ORDER BY revenue DESC;
  
```

| orders                          |
|---------------------------------|
| id: int                         |
| cust_id: int
| order_date: datetime
| order_details: varchar
| total_order_cost: int

---


### Company: Zillow

### [Cities With The Most Expensive Homes](https://platform.stratascratch.com/coding/10315-cities-with-the-most-expensive-homes?code_type=1) Medium

#### Q. Write a query that identifies cities with higher than average home prices when compared to the national average. Output the city names.

```diff
SELECT city
FROM zillow_transactions
WHERE mkt_price > (
    SELECT AVG(mkt_price)
    FROM zillow_transactions
  )
GROUP BY city
HAVING AVG(mkt_price) > (
    SELECT AVG(mkt_price)
    FROM zillow_transactions
  );

  
```
|zillow_transactions |
|--------------------|
|id: int             |
|state: varchar
|city: varchar
|street_address: varchar
|mkt_price: int

---



####  Show unique first names from the patients table which only occurs once in the list.

####  For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.


```
SELECT first_name
FROM patients
group by first_name
having count(first_name) = 1;
```

|  patients       |
|-----------------|
| patient_id INT  |
first_name	TEXT 
last_name	TEXT 
gender	CHAR(1)
birth_date	DATE
city	TEXT
primary key icon	province_id	CHAR(2)
allergies	TEXT
height	INT
weight	INT


---


#### Show first name, last name, and gender of patients who's gender is 'M'

```diff
SELECT first_name, last_name, gender
FROM patients
where gender="M";

```


|  patients       |
|-----------------|
| patient_id INT  |
first_name	TEXT 
last_name	TEXT 
gender	CHAR(1)
birth_date	DATE
city	TEXT
primary key icon	
province_id	CHAR(2)
allergies	TEXT
height	INT
weight	INT


---


#### Display every patient's first_name. Order the list by the length of each name and then by alphbetically.

```diff
SELECT first_name
FROM patients
order by len(first_name), first_name ASC;

```


|  patients       |
|-----------------|
| patient_id INT  |
first_name	TEXT 
last_name	TEXT 
gender	CHAR(1)
birth_date	DATE
city	TEXT
primary key icon	
province_id	CHAR(2)
allergies	TEXT
height	INT
weight	INT


---
