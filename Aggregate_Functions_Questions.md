#### Company: DoorDash

### [Workers With The Highest Salaries](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries?code_type=1) Medium

#### Q. Find the titles of workers that earn the highest salary. Output the highest-paid title or multiple titles that share the highest salary.

```diff
with q1 as( 
 select t2.worker_title as title, t1.salary as salary  from
 worker t1 left join title t2 on t1.worker_id = t2.worker_ref_id
)

select title from q1
where salary = (select max(salary) from worker);

```

| worker                   |  title                     |
|--------------------------|----------------------------|
|worker_id   ->   int      |  worker_ref_id -> int      |
|first_name  ->   varchar  |  worker_title -> varchar   |
|last_name   ->   varchar  |  affected_from -> datetime |
|salary      ->   int      |
|joining_date ->  datetime |
|department  ->   varchar  |


---

