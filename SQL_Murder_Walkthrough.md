# My Solution to SQL City Murder

## First I have to create my own ESD from an unknown database:
 ```SQL
 SELECT name
   FROM sqlite_master
  WHERE type = 'table'
 ```


 | name |
 |------|
 crime_scene _report
 drivers_license
 facebook_event_checkin
 interview
 get_fit_now_member
 get_fit_now_check_in
 solution
 income
 person

 to get all of the data types contained in each table I can run:
 ```SQL
 SELECT sql
   FROM sqlite_master
  WHERE name = [insert table]
```

# Let's solve this shit

1. find the murder in question:
(gotta make sure the crime and city exist)

```SQL
SELECT distinct type
  FROM crime_scene_report
```
```SQL
SELECT distinct city
  FROM crime_scene_report
```
```SQL
SELECT * 
  FROM crime_scene_report
 WHERE type = 'murder' and city = 'SQL City'
```

 | date | type | description | city |
 |------|------|-------------|------|
20180215| murder | REDACTED REDACTED REDACTED |SQL City
20180215| murder | Someone killed the guard! He took an arrow to the knee! | SQL City
20180115 | murder | Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave". | SQL City


2. Get first witness's name:
```SQL
  SELECT *
    FROM person
   WHERE address_street_name = 'Northwestern Dr' 
ORDER BY address_number DESC LIMIT 1
```
| id | name | license_id | address_number | address_street_name | ssn |
|----|------|------------|----------------|---------------------|-----|
| 14887 | Morty Schapiro | 118009 | 4919 | Northwestern Dr | 111564949

3. Get second witness's name:
```SQL
SELECT *
  FROM person
 WHERE name LIKE '%Annabel%' AND address_street_name = 'Franklin Ave'
```
| id | name | license_id | address_number | address_street_name | ssn |
|----|------|------------|----------------|---------------------|-----|
16371 |Annabel Miller | 490173 | 103 | Franklin Ave | 318771143

4. Get their interview transcripts:

```SQL
SELECT person.name, interview.transcript
  FROM interview
  JOIN person
    ON person.id = interview.person_id
 WHERE person.name = 'Morty Schapiro' OR person.name = 'Annabel Miller'
 ```
| name | transcript |
|------|------------|
| Morty Schapiro | I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W". |
| Annabel Miller | I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th. |