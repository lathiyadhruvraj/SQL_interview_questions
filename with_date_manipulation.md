####  Show unique birth years from patients and order them by ascending.

```
SELECT distinct YEAR(birth_date) as birth_year
FROM patients
order by birth_year asc;
```

|  patients       |
|-----------------|
| patient_id INT  |
first_name	TEXT 
last_name	TEXT 
gender	CHAR(1)
birth_date	DATE
city	TEXT
primary key icon	province_id	CHAR(2)
allergies	TEXT
height	INT
weight	INT


---
