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
