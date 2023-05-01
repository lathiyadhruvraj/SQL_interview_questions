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

####  Show unique first names from the patients table which only occurs once in the list.

####  For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.


```
SELECT first_name
FROM patients
group by first_name
having count(first_name) = 1;
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
