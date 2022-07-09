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
