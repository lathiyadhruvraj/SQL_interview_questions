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
