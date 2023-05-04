##### Company: Google

### [Counting Instances in Text](https://platform.stratascratch.com/coding/9814-counting-instances-in-text?code_type=1) Hard

#### Q. Find the number of times the words 'bull' and 'bear' occur in the contents. We're counting the number of times the words occur so words like 'bullish' should not be included in our count. Output the word 'bull' and 'bear' along with the corresponding number of occurrences.


```diff
select word, nentry
from 
ts_stat('select to_tsvector(contents)  from google_file_store')
where word like 'bull' or word like 'bear';
```

```diff
select 'bull' as sign, count(*)
from google_file_store
where contents like 'bull %' or contents like '% bull %' or contents like '% bull'
union
select 'bear' as sign, count(*)
from google_file_store
where contents like 'bear %' or contents like '% bear %' or contents like '% bear';
```


| google_file_store        |
|--------------------------|
| filename: varchar        |
| contents: varchar        |

---

#### Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

```diff
SELECT patient_id, first_name 
FROM patients 
WHERE first_name LIKE 's%_s' AND LENGTH(first_name) >= 6;

```


---

