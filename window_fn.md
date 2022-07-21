#### Company: Yelp

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
