# :chart_with_upwards_trend: Analyzing-Students-Mental-Health-using-SQL :chart_with_downwards_trend:

Does going to university in a different country affect your mental
health? A Japanese international university surveyed its students in
2018 and published a study the following year that was approved by
several ethical and regulatory boards.

The study found that international students have a higher risk of mental
health difficulties than the general population. I explore the `students`
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

<img width="600" alt="Analysing students mental health 1" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/a9c8d686-1a54-4c89-922f-37d24d8780e1">

 #

**We count the number of records in the dataset to confirm we have the
expected number of records, then see how many records we have for each
student type.**

``` python
SELECT COUNT(*) AS total_records,
       COUNT(inter_dom) AS count_inter_dom
FROM students;
```

<img width="600" alt="Analysing students mental health 2" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/09b99466-9d74-4658-9978-3ebec3e101a6">

 #

**We find out out the unique student types available on the dataset and
explore the data for each student type**

``` python
SELECT *
FROM students
WHERE inter_dom = 'Dom'
LIMIT 10;
```

<img width="600" alt="Analysing students mental health 3" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/4f21b812-d29f-460b-9eaf-a70350fdc3b8">

``` python
SELECT *
FROM students
WHERE inter_dom = 'Inter'
LIMIT 10;
```

<img width="600" alt="Analysing students mental health 4" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/da51e4ad-3335-4df4-8359-180b3b9d001f">

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

<img width="600" alt="Analysing students mental health 5" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/c16f14c3-3634-42bc-816b-84e3524f01e5">

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

<img width="600" alt="Analysing students mental health 6" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/5a945bfa-b4a6-4498-b67f-102296e03e91">

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

<img width="600" alt="Analysing students mental health 7" src="https://github.com/Marvykeys/Analyzing-Students-Mental-Health-using-SQL/assets/130637591/dfb88db8-6843-4279-874b-fcb94e227099">

