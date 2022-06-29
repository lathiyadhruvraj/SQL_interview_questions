#### Company: DoorDash

### Workers With The Highest Salaries

#### Q. Find the titles of workers that earn the highest salary. Output the highest-paid title or multiple titles that share the highest salary.

-->

with q1 as(                                                               <br>
    select t2.worker_title as title, t1.salary as salary  from            <br>
    worker t1 left join title t2 on t1.worker_id = t2.worker_ref_id       <br>
)                                                                         <br>

select title from q1                                                      <br>
where salary = (select max(salary) from worker);
