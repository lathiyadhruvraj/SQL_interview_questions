#### Company: Netflix

### [Count the number of movies that Abigail Breslin nominated for oscar](https://platform.stratascratch.com/coding/10128-count-the-number-of-movies-that-abigail-breslin-nominated-for-oscar?code_type=1) Easy

#### Q. Find the titles of workers that earn the highest salary. Output the highest-paid title or multiple titles that share the highest salary.

```diff
select count(movie)
from oscar_nominees
where nominee = 'Abigail Breslin';

```

| oscar_nominees           |
|--------------------------|
|year      ->   int        | 
|category  ->   varchar    | 
|nominee   ->   varchar    |  
|movie     ->   varchar    |
|winner    ->   bool       |
|id        ->   int        |


---

#### Company: Yelp

### [Top Cool Votes](https://platform.stratascratch.com/coding/10060-top-cool-votes?code_type=1) Medium

#### Q. Find the review_text that received the highest number of  'cool' votes.
Output the business name along with the review text with the highest numbef of 'cool' votes.

```diff
select business_name, review_text
from yelp_reviews
where cool=(select max(cool) from yelp_reviews);

```

| yelp_reviews               |
|----------------------------|
| business_name ->   varchar | 
| review_id  ->   varchar    | 
| user_id   ->   varchar     |  
| stars     ->   varchar     |
| review_date ->  datetime   |
| review_text ->  varchar    |
| funny    ->  int           |
| useful  ->  int            |
| cool   ->  int             |

---


#### Company: City of San Francisco

### [Classify Business Type](https://platform.stratascratch.com/coding/9726-classify-business-type?code_type=1) Medium

#### Q. Classify each business as either a restaurant, cafe, school, or other. A restaurant should have the word 'restaurant' in the business name. For cafes, either 'cafe', 'café', or 'coffee' can be in the business name. 'School' should be in the business name for schools. All other businesses should be classified as 'other'. Output the business name and the calculated classification.

```diff
select DISTINCT business_name,
case when LOWER(business_name) like '%school%' then 'school' 
      when LOWER(business_name) like '%cafe%' OR
           LOWER(business_name) like '%café%' OR
           LOWER(business_name) like '%coffee%' then 'cafe'
      when LOWER(business_name) like '%restaurant%' then 'restaurant'
      else 'other' 
      end as category
from sf_restaurant_health_violations
;

```

| sf_restaurant_health_violations               |
|-----------------------------------------------|
|business_id: int                               |    
|business_name: varchar                   
|business_address: varchar
|business_city: varchar
|business_state: varchar
|business_postal_code: float
|business_latitude: float
|business_longitude: float
|business_location: varchar
|business_phone_number: float
|inspection_id: varchar
|inspection_date: datetime
|inspection_score: float
|inspection_type: varchar
|violation_id: varchar
|violation_description: varchar
|risk_category: varchar

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

### Company: Lyft

### [Lyft Driver Wages](https://platform.stratascratch.com/coding/10003-lyft-driver-wages?code_type=1) Easy

#### Q. Find all Lyft drivers who earn either equal to or less than 30k USD or equal to or more than 70k USD. Output all details related to retrieved records.

```diff
select * from lyft_drivers
where yearly_salary <= 30000 or yearly_salary >= 70000 ;
```

| lyft_drivers         |
|----------------------|
| index: int           |
| start_date: datetime |
| end_date: datetime   |
| yearly_salary: int   |


---

### Company: City of San Francisco


### [Find the base pay for Police Captains](https://platform.stratascratch.com/coding/9972-find-the-base-pay-for-police-captains?code_type=1) Easy


#### Q. Find the base pay for Police Captains. Output the employee name along with the corresponding base pay.

```diff
select employeename, basepay
from sf_public_salaries
where jobtitle = 'CAPTAIN III (POLICE DEPARTMENT)';
```

| sf_public_salaries   |
|----------------------|
| id: int              |  
| employeename: varchar
| jobtitle: varchar
| basepay: float
| overtimepay: float
| otherpay: float 
| benefits: float
| totalpay: float
| totalpaybenefits: float
| year: int
| notes: datetime
| agency: varchar
| status: varchar

---


### Company: City of San Francisco


### [Find libraries who haven't provided the email address in circulation year 2016 but their notice preference definition is set to email](https://platform.stratascratch.com/coding/9924-find-libraries-who-havent-provided-the-email-address-in-2016-but-their-notice-preference-definition-is-set-to-email?code_type=1) Easy


#### Q. Find libraries who haven't provided the email address in circulation year 2016 but their notice preference definition is set to email. Output the library code.

```diff
select distinct(home_library_code) from library_usage
where provided_email_address = False and circulation_active_year = 2016 and notice_preference_definition = 'email'  ;
```

| library_usage                   |
|---------------------------------|
| patron_type_code: int           |
| patron_type_definition: varchar |
| total_checkouts: int            | 
| total_renewals: int
| age_range: varchar
| home_library_code: varchar
| home_library_definition: varchar
| circulation_active_month: varchar
| circulation_active_year: float
| notice_preference_code: varchar
| notice_preference_definition: varchar
| provided_email_address: bool
| year_patron_registered: int
| outside_of_county: bool
| supervisor_district: float

---


### Company: Salesforce

### [Highest Target Under Manager](https://platform.stratascratch.com/coding/9905-highest-target-under-manager?code_type=1) Medium

#### Q. Find the highest target achieved by the employee or employees who works under the manager id 13. Output the first name of the employee and target achieved. The solution should show the highest target achieved under manager_id=13 and which employee(s) achieved it.

```diff
SELECT first_name, target
FROM salesforce_employees
WHERE target IN
    (SELECT MAX(target)
     FROM salesforce_employees
     WHERE manager_id = 13);
  
```

| salesforce_employees            |
|---------------------------------|
| id: int                         |
| first_name: varchar             |
| last_name: varchar              |
| age: int
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


### Company: Twitter

### [Highest Salary In Department](https://platform.stratascratch.com/coding/9897-highest-salary-in-department?code_type=1) Medium

#### Q. Find the employee with the highest salary per department. Output the department name, employee's first name along with the corresponding salary.

```diff
select department, first_name,  salary
from employee
where (department , salary) IN
    (   select e2.department, max(e2.salary) as sal
        from employee e2    
        group by department);
  
```

| orders                          |
|---------------------------------|
| id: int                         |
| first_name: varchar             |
| last_name: varchar
| age: int
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


### Company: City of San Francisco

### [Number of violations](https://platform.stratascratch.com/coding/9728-inspections-that-resulted-in-violations?code_type=1) Medium

#### Q. You're given a dataset of health inspections. Count the number of violation in an inspection in 'Roxanne Cafe' for each year. If an inspection resulted in a violation, there will be a value in the 'violation_id' column. Output the number of violations by year in ascending order.

```diff

select EXTRACT(YEAR from inspection_date :: DATE) as YEAR , count(inspection_date) as inspections 
from sf_restaurant_health_violations
where business_name = 'Roxanne Cafe' and violation_id is not null
group by 1
order by 1 asc;

```

| sf_restaurant_health_violation         |
|----------------------------------------|
| business_id: int                       |
| business_name: varchar
| business_address: varchar
| business_city: varchar
| business_state: varchar
| business_postal_code: float
| business_latitude: float
| business_longitude: float
| business_location: varchar
| business_phone_number: float
| inspection_id: varchar
| inspection_date: datetime
| inspection_score: float
| inspection_type: varchar
| violation_id: varchar
| violation_description: varchar
| risk_category: varchar


---

### Company: City of Los Angeles

### [Churro Activity Date](https://platform.stratascratch.com/coding/9688-churro-activity-date?code_type=1) Easy

#### Q. Find the activity date and the pe_description of facilities with the name 'STREET CHURROS' and with a score of less than 95 points.


```diff

SELECT activity_date, pe_description
FROM los_angeles_restaurant_health_inspections
WHERE facility_name = 'STREET CHURROS' AND score < 95

```

| los_angeles_restaurant_health_inspections         |
|---------------------------------------------------|
| serial_number: varchar                            |
| activity_date: datetime                           |
| facility_name: varchar
| score: int
| grade: varchar
| service_code: int
| service_description: varchar
| employee_id: varchar
| facility_address: varchar
| facility_city: varchar
| facility_id: varchar
| facility_state: varchar
| facility_zip: varchar
| owner_id: varchar
| owner_name: varchar
| pe_description: varchar
| program_element_pe: int
| program_name: varchar
| program_status: varchar
| record_id: varchar


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

```diff

select company, continent
from forbes_global_2010_2014
where profits in (
    select max(profits)
    from forbes_global_2010_2014
    where sector = 'Financials') ;

```


### Company: Apple

### [Count the number of user events performed by MacBookPro users](https://platform.stratascratch.com/coding/9653-count-the-number-of-user-events-performed-by-macbookpro-users?code_type=1) Easy

#### Q. Count the number of user events performed by MacBookPro users. Output the result along with the event name. Sort the result based on the event count in the descending order.


```diff

select event_name, count(*) 
from playbook_events
where device = 'macbook pro'
group by 1
order by 2 desc;

```

---


### Company: Wine Magazine

### [Find all wineries which produce wines by possessing aromas of plum, cherry, rose, or hazelnut](https://platform.stratascratch.com/coding/10026-find-all-wineries-which-produce-wines-by-possessing-aromas-of-plum-cherry-rose-or-hazelnut?tabname=solutions) Medium

#### Q. Find all wineries which produce wines by possessing aromas of plum, cherry, rose, or hazelnut. To make it more simple, look only for singular form of the mentioned aromas. Output unique winery values only.


```diff

SELECT DISTINCT winery
FROM winemag_p1
WHERE lower(description) ~ '\y(plum|cherry|rose|hazelnut)\y'

```


| winemag_p1             |
|------------------------|
| id: int                |
| country: varchar       |
| description: varchar
| designation: varchar
| points: int
| price: float
| province: varchar
| region_1: varchar
| region_2: varchar
| variety: varchar
| winery: varchar

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
