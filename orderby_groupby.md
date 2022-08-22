##### Company: ESPN

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

