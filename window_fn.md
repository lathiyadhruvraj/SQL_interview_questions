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
