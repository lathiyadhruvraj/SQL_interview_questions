#### Company: Airbnb

### [Number Of Units Per Nationality](https://platform.stratascratch.com/coding/10156-number-of-units-per-nationality?code_type=1) Medium

#### Q. Find the number of apartments per nationality that are owned by people under 30 years old. Output the nationality along with the number of apartments. Sort records by the apartments count in descending order.

```diff
select nationality,count(distinct unit_id) from airbnb_hosts join airbnb_units on airbnb_hosts.host_id=airbnb_units.host_id
where age<30 and unit_type='Apartment'
group by 1;

```

| airbnb_hosts             | airbnb_units       |
|--------------------------|--------------------|
| host_id: int             | host_id: int

n_bedrooms: int
country: varchar
city: varchar
| nationality: varchar     | unit_id: varchar
| gender: varchar          | unit_type: varchar

| age: int                 |n_beds: int

|



---
