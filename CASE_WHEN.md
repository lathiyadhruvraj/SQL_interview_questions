
#### Company: Meta/Facebook

### [Find the rate of processed tickets for each type](https://platform.stratascratch.com/coding/9781-find-the-rate-of-processed-tickets-for-each-type?code_type=1) Medium

#### Q. Find the rate of processed tickets for each type.


```diff

SELECT type, SUM(CASE WHEN processed THEN 1 ELSE 0 END) :: NUMERIC / COUNT(*) as p_rate
FROM facebook_complaints
GROUP BY type;

```

| facebook_complaints       |
|---------------------------|
|complaint_id ->   int      |
|type         ->   int      |
|processed    ->   bool     |


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
