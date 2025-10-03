# SQL Query Examples

## Table of Contents

* [Medium Questions](#medium-questions)
    * [1. Show unique birth years from patients and order them by ascending.](#medium-question-1)
    * [2. Show unique first names from the patients table which only occurs once in the list.](#medium-question-2)
    * [3. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.](#medium-question-3)
    * [4. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.](#medium-question-4)
    * [5. Display every patient's first_name. Order the list by the length of each name and then by alphabetically.](#medium-question-5)
    * [6. Show the total amount of male patients and the total amount of female patients in the patients table. Display the two results in the same row.](#medium-question-6)
    * [7. Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.](#medium-question-7)
    * [8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.](#medium-question-8)
    * [9. Show the city and the total number of patients in the city. Order from most to least patients and then by city name ascending.](#medium-question-9)
    * [10. Show first name, last name and role of every person that is either patient or doctor. The roles are either "Patient" or "Doctor".](#medium-question-10)
    * [11. Show all allergies ordered by popularity. Remove NULL values from query.](#medium-question-11)
    * [12. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.](#medium-question-12)
    * [13. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order.](#medium-question-13)
    * [14. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.](#medium-question-14)
    * [15. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'](#medium-question-15)
    * [16. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.](#medium-question-16)
    * [17. Show all columns for patient_id 542's most recent admission_date.](#medium-question-17)
    * [18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria: 1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19. 2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.](#medium-question-18)
    * [19. Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.](#medium-question-19)
    * [20. For each doctor, display their id, full name, and the first and last admission date they attended.](#medium-question-20)
    * [21. Display the total amount of patients for each province. Order by descending.](#medium-question-21)
    * [22. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.](#medium-question-22)
    * [23. display the first name, last name and number of duplicate patients based on their first name and last name.](#medium-question-23)
    * [24. Display patient's full name, height in the unit feet rounded to 1 decimal, weight in the unit pounds rounded to 0 decimals, birth_date, gender non abbreviated. Convert CM to feet by dividing by 30.48. Convert KG to pounds by multiplying by 2.205.](#medium-question-24)
    * [25. Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows).](#medium-question-25)


* <a href="https://github.com/dudekluk/Portfolio/blob/main/doc/SQL%20Practice%20Easy.md" target="_blank"><b>Move to Easy tasks</b></a>
* <a href="https://github.com/dudekluk/Portfolio/blob/main/doc/SQL%20Practice%20Hard.md" target="_blank"><b>Move to Hard tasks</b></a>

## Medium Questions

### <a name="medium-question-1">1. Show unique birth years from patients and order them by ascending.</a>
```sql
SELECT DISTINCT(YEAR(birth_date))
FROM patients
ORDER BY birth_date ASC;
```
### <a name="medium-question-2">2. Show unique first names from the patients table which only occurs once in the list.</a>
```sql
SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(*) = 1;
```
### <a name="medium-question-3">3. Show patient_id and first_name from patients where their first_name starts and ends with 's' and is at least 6 characters long.</a>
```sql
SELECT
  patient_id,
  first_name
FROM patients
WHERE first_name LIKE 'S____%S';
```
### <a name="medium-question-4">4. Show patient_id, first_name, last_name from patients whose diagnosis is 'Dementia'.</a>
```sql
SELECT
  patients.patient_id,
  first_name,
  last_name
FROM patients
  JOIN admissions ON patients.patient_id = admissions.patient_id
WHERE diagnosis = 'Dementia';
```
### <a name="medium-question-5">5. Display every patient's first_name. Order the list by the length of each name and then by alphabetically.</a>
```sql
SELECT first_name
FROM patients
ORDER BY
  LEN(first_name),   
  first_name; 
```
### <a name="medium-question-6">6. Show the total amount of male patients and the total amount of female patients in the patients table. Display the two results in the same row.</a>
```sql
SELECT
  (SELECT COUNT(*) FROM patients WHERE gender = 'M') AS male,   
  (SELECT COUNT(*) FROM patients WHERE gender = 'F') AS female;
```
### <a name="medium-question-7">7. Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.</a>
```sql
SELECT
  first_name,
  last_name,
  allergies
FROM patients
WHERE allergies IN ('Penicillin', 'Morphine')
ORDER BY
  allergies ASC,
  first_name,
  last_name;
```
### <a name="medium-question-8">8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.</a>
```sql
SELECT
  patient_id,
  diagnosis,   
  COUNT(diagnosis)
FROM admissions
GROUP BY
  patient_id,
  diagnosis
HAVING COUNT(diagnosis) > 1;
```
### <a name="medium-question-9">9. Show the city and the total number of patients in the city. Order from most to least patients and then by city name ascending.</a>
```sql
SELECT
  city,
  COUNT(*)
FROM patients
GROUP BY city
ORDER BY
  COUNT(*) DESC,   
  city 
```
### <a name="medium-question-10">10. Show first name, last name and role of every person that is either patient or doctor. The roles are either "Patient" or "Doctor"</a>
```sql
SELECT
  first_name,
  last_name,
  'Patient' AS role
FROM patients
UNION ALL
SELECT
  first_name,
  last_name,
  'Doctor' AS role
FROM doctors;   
```
### <a name="medium-question-11">11. Show all allergies ordered by popularity. Remove NULL values from query.</a>
```sql
SELECT
  allergies,
  COUNT(*)   
FROM patients
WHERE allergies IS NOT NULL
GROUP BY allergies
ORDER BY COUNT(*) DESC;
```
### <a name="medium-question-12">12. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.</a>
```sql
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE YEAR(birth_date) BETWEEN 1970 AND 1979
ORDER BY birth_date;
```
### <a name="medium-question-13">13. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order EX: SMITH,jane</a>
```sql
SELECT
  CONCAT(UPPER(last_name), ',', LOWER(first_name))
FROM patients
ORDER BY first_name DESC;
```
### <a name="medium-question-14">14. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.</a>
```sql
SELECT
  province_id,
  SUM(height)
FROM patients
GROUP BY province_id
HAVING SUM(height) >= 7000;
```
### <a name="medium-question-15">15. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'</a>
```sql
SELECT
  MAX(weight) - MIN(weight) 
FROM patients
WHERE last_name = 'Maroni';
```
### <a name="medium-question-16">16. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.</a>
```sql
SELECT
  DAY(admission_date) AS day,
  COUNT(*)
FROM admissions
GROUP BY day
ORDER BY COUNT(*) DESC;
```
### <a name="medium-question-17">17. Show all columns for patient_id 542's most recent admission_date.</a>
```sql
SELECT *
FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1;
```
### <a name="medium-question-18">18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
* patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
* attending_doctor_id contains a 2 and the length of patient_id is 3 characters.</a>
```sql
SELECT
  patient_id,
  attending_doctor_id,
  diagnosis
FROM admissions
WHERE
  patient_id % 2 != 0
  AND attending_doctor_id IN (1, 5, 19)
  OR attending_doctor_id LIKE '%2%'   
  AND patient_id LIKE '___';
```
### <a name="medium-question-19">19. Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.</a>
```sql
SELECT
  first_name,
  last_name,
  COUNT(admission_date)   
FROM admissions
  JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id
GROUP BY attending_doctor_id;  
```
### <a name="medium-question-20">20. For each doctor, display their id, full name, and the first and last admission date they attended.</a>
```sql
SELECT
  doctor_id,
  CONCAT(first_name, ' ', last_name),
  MIN(admission_date),
  MAX(admission_date)
FROM admissions   
  JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id
GROUP BY doctor_id;
```
### <a name="medium-question-21">21. Display the total amount of patients for each province. Order by descending.</a>
```sql
SELECT
  province_name,
  COUNT(*)
FROM patients
  JOIN province_names ON patients.province_id = province_names.province_id
GROUP BY province_name
ORDER BY COUNT(*) DESC;
```
### <a name="medium-question-22">22. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.</a>
```sql
SELECT
  CONCAT(patients.first_name,' ', patients.last_name),
  diagnosis,
  CONCAT(doctors.first_name,' ', doctors.last_name)
FROM patients
  JOIN admissions ON admissions.patient_id = patients.patient_id
  JOIN doctors ON doctors.doctor_id = admissions.attending_doctor_id; 
```
### <a name="medium-question-23">23. Display the first name, last name and number of duplicate patients based on their first name and last name.</a>
```sql
SELECT
  first_name,
  last_name,
  COUNT(*)
FROM patients
GROUP BY
  first_name,
  last_name
HAVING COUNT(*) > 1
ORDER BY COUNT(*);
```
### <a name="medium-question-24">24. Display patient's full name, height in the units feet rounded to 1 decimal, weight in the unit pounds rounded to 0 decimals, birth_date, gender non abbreviated. Convert CM to feet by dividing by 30.48. Convert KG to pounds by multiplying by 2.205.</a>
```sql
SELECT
  CONCAT(first_name,' ', last_name),
  ROUND(height / 30.48, 1),
  ROUND(weight * 2.205, 0),
  birth_date,
  CASE
    WHEN gender = 'M' THEN 'MALE'
    WHEN gender = 'F' THEN 'FEMALE'
  END
FROM patients;  
```
### <a name="medium-question-25">25. Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)</a>
```sql
SELECT
  patients.patient_id,
  first_name,
  last_name
FROM patients   
  FULL JOIN admissions ON patients.patient_id = admissions.patient_id
WHERE admissions.patient_id IS NULL;
```
