# SQL Query Examples

## Table of Contents

* [Easy Questions](#easy-questions)
    * [1. Show first name, last name, and gender of patients whose gender is 'M'](#easy-question-1)
    * [2. Show first name and last name of patients who does not have allergies. (null)](#easy-question-2)
    * [3. Show first name of patients that start with the letter 'C'](#easy-question-3)
    * [4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)](#easy-question-4)
    * [5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'](#easy-question-5)
    * [6. Show first name and last name concatenated into one column to show their full name.](#easy-question-6)
    * [7. Show first name, last name, and the full province name of each patient.](#easy-question-7)
    * [8. Show how many patients have a birth_date with 2010 as the birth year.](#easy-question-8)
    * [9. Show the first_name, last_name, and height of the patient with the greatest height.](#easy-question-9)
    * [10. Show all columns for patients who have one of the following patient_ids: 1, 45, 534, 879, 1000.](#easy-question-10)
    * [11. Show the total number of admissions.](#easy-question-11)
    * [12. Show all the columns from admissions where the patient was admitted and discharged on the same day.](#easy-question-12)
    * [13. Show the patient id and the total number of admissions for patient_id 579.](#easy-question-13)
    * [14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?](#easy-question-14)
    * [15. Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70.](#easy-question-15)
    * [16. Write a query to find list of patients first_name, last name, and allergies where allergies are not null and are from the city of 'Hamilton'](#easy-question-16)


* <a href="https://github.com/dudeklukasz/Portfolio/blob/main/doc/SQL%20Practice%20Medium.md" target="_blank"><b>Move to Medium tasks</b></a>
* <a href="https://github.com/dudeklukasz/Portfolio/blob/main/doc/SQL%20Practice%20Hard.md" target="_blank"><b>Move to Hard tasks</b></a>

## Easy Questions

### <a name="easy-question-1">1. Show first name, last name, and gender of patients whose gender is 'M'</a>
```sql
SELECT
  first_name,
  last_name,
  gender
FROM patients
WHERE gender = 'M';
```
### <a name="easy-question-2">2. Show first name and last name of patients who does not have allergies. (null)</a>
```sql
SELECT
  first_name,
  last_name
FROM patients
WHERE allergies is null
```

### <a name="easy-question-3">3. Show first name of patients that start with the letter 'C'</a>
```sql
SELECT
  first_name 
FROM patients
WHERE first_name like 'C%'
```
### <a name="easy-question-4">4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)</a>
```sql
SELECT
  first_name,
  last_name
FROM patients
WHERE weight between 100 and 120
```
### <a name="easy-question-5">5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'</a>
```sql
UPDATE patients
SET allergies = 'NKA'
WHERE allergies is null
```
### <a name="easy-question-6">6. Show first name and last name concatenated into one column to show their full name.</a>
```sql
SELECT
  CONCAT(first_name,' ', last_name)
FROM patients
```
### <a name="easy-question-7">7. Show first name, last name, and the full province name of each patient.</a>
```sql
SELECT
  first_name,
  last_name,
  province_name
FROM patients
  JOIN province_names ON patients.province_id = province_names.province_id
```
### <a name="easy-question-8">8. Show how many patients have a birth_date with 2010 as the birth year.</a>
```sql
SELECT COUNT(*)   
FROM patients
WHERE YEAR(birth_date) = 2010
```
### <a name="easy-question-9">9. Show the first_name, last_name, and height of the patient with the greatest height.</a>
```sql
SELECT
  first_name,
  last_name,
  height
FROM patients
ORDER BY height desc
LIMIT 1
```
### <a name="easy-question-10">10. Show all columns for patients who have one of the following patient_ids: 1, 45, 534, 879, 1000.</a>
```sql
SELECT *
FROM patients
WHERE patient_id IN(1, 45, 534, 879, 1000)
```
### <a name="easy-question-11">11. Show the total number of admissions.</a>
```sql
SELECT COUNT(*)
FROM admissions
```
### <a name="easy-question-12">12. Show all the columns from admissions where the patient was admitted and discharged on the same day.</a>
```sql
SELECT *
FROM admissions
WHERE admission_date = discharge_date
```
### <a name="easy-question-13">13. Show the patient id and the total number of admissions for patient_id 579.</a>
```sql
SELECT
  patient_id,
  COUNT(*)   
FROM admissions
WHERE patient_id = 579
```
### <a name="easy-question-14">14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'.</a>
```sql
SELECT DISTINCT(city)
FROM patients
WHERE province_id = 'NS'
```
### <a name="easy-question-15">15. Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70.</a>
```sql
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE height > 160 AND weight > 70
```
### <a name="easy-question-16">16. Write a query to find list of patients first_name, last name, and allergies where allergies are not null and are from the city of 'Hamilton'.</a>
```sql
SELECT
  first_name,
  last_name,
  allergies
FROM patients
WHERE allergies IS NOT NULL AND city = 'Hamilton'
```
