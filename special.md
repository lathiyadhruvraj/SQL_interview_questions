
#### Company: Yelp

### [Reviews of Categories](https://platform.stratascratch.com/coding/10049-reviews-of-categories?code_type=1) Medium

#### Q. Find the top business categories based on the total number of reviews. Output the category along with the total number of reviews. Order by total reviews in descending order.


```diff

WITH cte AS(
    SELECT
        unnest(string_to_array(categories, ';')) AS category ,
        review_count
    FROM yelp_business
)

SELECT 
    category,
    SUM(review_count) AS review_cnt
FROM cte
GROUP BY category
ORDER BY review_cnt DESC
;

```


| airbnb_contacts          |
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
