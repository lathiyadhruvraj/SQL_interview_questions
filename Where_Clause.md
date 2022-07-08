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
