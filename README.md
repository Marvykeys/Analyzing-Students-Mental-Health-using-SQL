# :chart_with_upwards_trend: Analyzing-Students-Mental-Health-using-SQL :chart_with_downwards_trend:

A Japanese international university surveyed its students in
2018 and published a study the following year that was approved by
several ethical and regulatory boards.

The study found that international students have a higher risk of mental
health difficulties than the general population. 

I explore the `students`
data using PostgreSQL to find out if this is true and see if the length
of stay is a contributing factor.

Here is a data description of the fields on the dataset. The full
dataset is in one table with 50 fields and, according to the survey, 268
records. Each row is a student.

  |Field Name   |    Description                                    | 
  |-------------|---------------------------------------------------|
  |inter_dom    |  Types of students                                |
  |japanese_cate|   Japanese language proficiency                   |
  |english_cate |   English language proficiency                    |
  |academic     |   Current academic level                          |
  |age          |   Current age of student                          |
  |stay         |   Current length of stay in years                 |
  |todep        |   Total score of depression (PHQ-9 test)          |
  |tosc         |   Total score of social connectedness (SCS test)  |
  |toas         |   Total score of Acculturative Stress (ASISS test)|

I will be doing the following exploratory analysis:

-   Count the number of all records, and all records per student type
-   Filter the data to see how it differs between the student types
-   Find the summary statistics of the diagnostic tests for all students
-   Summarize the data for international students
-   See if length of stay impacts the test scores

 #

**First, lets explore the whole dataset to better understand the data we
will be working with.**

``` python
SELECT *
FROM students
LIMIT 10;
```

|          | inter_dom | region  | gender | academic | age | age_cate | stay | stay_cate |
|-------|-------|-------|--------|--------|--------|--------|--------|---------|
| 0 | inter | SEA | Male | Grad | 24 | 4 | 5 | Long |
| 1 | inter | SEA | Male | Grad | 28 | 5 | 1 | Short |
| 2 | inter | SEA | Male | Grad | 25 | 4 | 6 | Long |
| 3 | inter | EA | Female | Grad | 29 | 5 | 1 | Short |
| 4 | inter | EA | Female | Grad | 28 | 5 | 1 | Short |
| 5 | inter | SEA | Male | Grad | 24 | 4 | 6 | Long |
| 6 | inter | SA | Male | Grad | 23 | 4 | 1 | Short |
| 7 | inter | SEA | Female | Grad | 30 | 5 | 2 | Medium |
| 8 | inter | SEA | Female | Grad | 25 | 4 | 4 | Long |
| 9 | inter | Others | Male | Grad | 31 | 5 | 2 | Medium |


 #

**We count the number of records in the dataset to confirm we have the
expected number of records, then see how many records we have for each
student type.**

``` python
SELECT COUNT(*) AS total_records,
       COUNT(inter_dom) AS count_inter_dom
FROM students;
```

|          | total_records | count_inter_dom |
|-------|-------|-------|
| 0 | 286 | 286 |


 #

**We find out out the unique student types available on the dataset and
explore the data for each student type**

``` python
SELECT *
FROM students
WHERE inter_dom = 'Dom'
LIMIT 10;
```

|          | inter_dom | region  | gender | academic | age | age_cate | stay | stay_cate |
|-------|-------|-------|--------|--------|--------|--------|--------|---------|
| 0 | Dom | JAP | Female | Grad | 24 | 4 | 5 | Long |
| 1 | Dom | JAP | Female | Under | 28 | 5 | 1 | Short |
| 2 | Dom | JAP | Female | Under | 25 | 4 | 6 | Long |
| 3 | Dom | JAP | Male | Under | 29 | 5 | 1 | Short |
| 4 | Dom | JAP | Female | Under | 28 | 5 | 1 | Short |
| 5 | Dom | JAP | Male | Under | 24 | 4 | 6 | Long |
| 6 | Dom | JAP | Female | Under | 23 | 4 | 1 | Short |
| 7 | Dom | JAP | Female | Under | 30 | 5 | 2 | Medium |
| 8 | Dom | JAP | Female | Under | 25 | 4 | 4 | Long |
| 9 | Dom | JAP | Male | Under | 31 | 5 | 2 | Medium |


``` python
SELECT *
FROM students
WHERE inter_dom = 'Inter'
LIMIT 10;
```

|          | inter_dom | region  | gender | academic | age | age_cate | stay | stay_cate |
|-------|-------|-------|--------|--------|--------|--------|--------|---------|
| 0 | inter | SEA | Male | Grad | 24 | 4 | 5 | Long |
| 1 | inter | SEA | Male | Grad | 28 | 5 | 1 | Short |
| 2 | inter | SEA | Male | Grad | 25 | 4 | 6 | Long |
| 3 | inter | EA | Female | Grad | 29 | 5 | 1 | Short |
| 4 | inter | EA | Female | Grad | 28 | 5 | 1 | Short |
| 5 | inter | SEA | Male | Grad | 24 | 4 | 6 | Long |
| 6 | inter | SA | Male | Grad | 23 | 4 | 1 | Short |
| 7 | inter | SEA | Female | Grad | 30 | 5 | 2 | Medium |
| 8 | inter | SEA | Female | Grad | 25 | 4 | 4 | Long |
| 9 | inter | Others | Male | Grad | 31 | 5 | 2 | Medium |


 #

**We Calculate the summary statistics of the diagnostic tests for all
students**

``` python
SELECT MIN(todep) AS min_phq,
       MAX(todep) AS max_phq,
	   ROUND(AVG(todep),2) AS avg_phq,
	   MIN(tosc) AS min_scs,
       MAX(tosc) AS max_scs,
	   ROUND(AVG(tosc),2) AS avg_scs,
	   MIN(toas) AS min_as,
       MAX(toas) AS max_as,
	   ROUND(AVG(toas),2) AS avg_as
FROM students;
```

| min_phq | max_phq | avg_phq | min_scs | max_scs | avg_scs | min_as | max_as | avg_as |
|-------|-------|-------|--------|--------|--------|--------|--------|---------|
| 0 | 25 | 8.19 | 8 | 48 | 37.47 | 36 | 145 | 72.38 |


 #

**Lets filter for only International Students**

``` python
SELECT MIN(todep) AS min_phq,
       MAX(todep) AS max_phq,
	   ROUND(AVG(todep),2) AS avg_phq,
	   MIN(tosc) AS min_scs,
       MAX(tosc) AS max_scs,
	   ROUND(AVG(tosc),2) AS avg_scs,
	   MIN(toas) AS min_as,
       MAX(toas) AS max_as,
	   ROUND(AVG(toas),2) AS avg_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY inter_dom;
```

| min_phq | max_phq | avg_phq | min_scs | max_scs | avg_scs | min_as | max_as | avg_as |
|-------|-------|-------|--------|--------|--------|--------|--------|---------|
| 0 | 25 | 8.04 | 11 | 48 | 37.42 | 36 | 145 | 75.56 |


 #

**how does length of stay of an international student impact the average
diagnostic scores**

``` python
SELECT stay,
	   ROUND(AVG(todep),2) AS average_phq,
	   ROUND(AVG(tosc),2) AS average_scs,
	   ROUND(AVG(toas),2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

|          | stay | average_phq | average_scs | average_as |
|-------|-------|-------|--------|--------|
| 0 | 10 | 13 | 32 | 50 |
| 1 | 8 | 10 | 44 | 65 |
| 2 | 7 | 4 | 48 | 45 |
| 3 | 6 | 6 | 38 | 58.67 |
| 4 | 5 | 0 | 34 | 91 |
| 5 | 4 | 8.57 | 33.93 | 87.71 |
| 6 | 3 | 9.09 | 37.13 | 78 |
| 7 | 2 | 8.28 | 37.08 | 77.67 |
| 8 | 1 | 7.48 | 38.11 | 72.8 |

