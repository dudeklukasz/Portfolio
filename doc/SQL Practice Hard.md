# SQL Query Examples

## Table of Contents

* [Hard Questions](#hard-questions)
    * [1. Categorize patients by weight range and count patients in each group.](#hard-question-1)
    * [2. Calculate obesity based on BMI and display patient information.](#hard-question-2)
    * [3. Find patients with "Epilepsy" treated by Dr. Lisa.](#hard-question-3)
    * [4. Generate temporary passwords for patients.](#hard-question-4)
    * [5. Find insurance status and calculate admission costs.](#hard-question-5)
    * [6. Identifie provinces with more male patients.](#hard-question-6)
    * [7. Find a patient based on various criteria.](#hard-question-7)
    * [8. Calculates the percentage of male patients.](#hard-question-8)
    * [9. Show daily admissions and changes from the previous day.](#hard-question-9)
    * [10. Sort provinces with "Ontario" first.](#hard-question-10)
    * [11. Analyze doctor admissions by year and specialty.](#hard-question-11)


* <a href="https://github.com/dudeklukasz/Portfolio/blob/main/doc/SQL%20Practice%20Easy.md" target="_blank"><b>Move to Easy tasks</b></a>
* <a href="https://github.com/dudeklukasz/Portfolio/blob/main/doc/SQL%20Practice%20Medium.md" target="_blank"><b>Move to Medium tasks</b></a>

### <a name="hard-question-1">1. Show all of the patients grouped into weight groups. Show the total amount of patients in each weight group.
* Order the list by the weight group decending.
* For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.</a>
```sql
SELECT
  COUNT(*),
  FLOOR(weight / 10) * 10 AS WEIGHT_GROUP
FROM patients
GROUP BY WEIGHT_GROUP
ORDER BY WEIGHT_GROUP DESC;
```
### <a name="hard-question-2">2. Show patient_id, weight, height, isObese from the patients table.

* Display isObese as a boolean 0 or 1.
* Obese is defined as weight(kg)/(height(m)2) >= 30.
* weight is in units kg.
* height is in units cm.</a>
```sql
SELECT
  patient_id,
  weight,
  height,
  POWER(height, 2),
  HEIGHT * HEIGHT,
  POWER(height, 2) / 10000,
  HEIGHT * HEIGHT / 10000,
  CASE
    WHEN weight / (POWER(height, 2) / 10000) >= 30 THEN 1
    ELSE 0
  END AS isObese
FROM patients;
```
### <a name="hard-question-3">3. Show patient_id, first_name, last_name, and attending doctor's specialty.
* Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'
* Check patients, admissions, and doctors tables for required information.</a>
```sql
SELECT
  patients.patient_id,
  patients.last_name,
  patients.first_name,
  specialty
FROM patients
  JOIN admissions ON patients.patient_id = admissions.patient_id
  JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id   
WHERE
  diagnosis = 'Epilepsy'
  AND doctors.first_name = 'Lisa';
```
### <a name="hard-question-4">4. All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.
* The password must be the following, in order:
* patient_id
* the numerical length of patient's last_name
* year of patient's birth_date</a>
```sql
SELECT
  DISTINCT patients.patient_id,
  CONCAT(
    patients.patient_id,
    LEN(last_name),
    YEAR(birth_date)
  ) AS password
FROM patients
  JOIN admissions ON patients.patient_id = admissions.patient_id;
```
### <a name="hard-question-5">5. Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.

* Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.</a>
```sql
SELECT
  CASE
    WHEN patient_id % 2 = 0 THEN 'YES'
    ELSE 'NO'
  END AS INSURED,
  CASE
    WHEN patient_id % 2 = 0 THEN COUNT(*) * 10
    ELSE COUNT(*) * 50
  END
FROM admissions
GROUP BY INSURED;
```
### <a name="hard-question-6">6. Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name</a>
```sql
SELECT province_name
FROM patients
  JOIN province_names ON patients.province_id = province_names.province_id
GROUP BY province_name   
HAVING
  COUNT(
    CASE
      WHEN patients.gender = 'M' THEN 1
    END)
      >
  COUNT(
    CASE
      WHEN patients.gender = 'F' THEN 1
    END);
```
### <a name="hard-question-7">7. We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:
* First_name contains an 'r' after the first two letters.
* Identifies their gender as 'F'
* Born in February, May, or December
* Their weight would be between 60kg and 80kg
* Their patient_id is an odd number
* They are from the city 'Kingston'</a>
```sql
SELECT *
FROM patients
WHERE
  first_name LIKE '__r%'
  AND gender = 'F'
  AND MONTH(birth_date) IN (2, 5, 12)
  AND weight BETWEEN 60 AND 80
  AND patient_id % 2 != 0
  AND city = 'Kingston';   
```
### <a name="hard-question-8">8. Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.</a>
```sql
SELECT
  CONCAT(ROUND(AVG(gender = 'M'), 4) * 100, '%')
FROM patients;
```
### <a name="hard-question-9">9. For each day display the total amount of admissions on that day. Display the amount changed from the previous date.</a>
```sql
SELECT
  admission_date,
  count(admission_date),
  count(admission_date) - LAG(count(admission_date), 1) OVER(
    ORDER BY admission_date)
FROM admissions
GROUP BY admission_date
ORDER By admission_date
```
### <a name="hard-question-10">10. Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.</a>
```sql
SELECT province_name
FROM province_names
ORDER BY
  CASE WHEN province_name = 'Ontario' THEN 0 ELSE 1 END,
  province_name;
```
### <a name="hard-question-11">11. We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.</a>
```sql
SELECT
  doctor_id,
  CONCAT(first_name, ' ', last_name),
  YEAR(admission_date),
  COUNT(admission_date),
  specialty
FROM admissions
  JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id
GROUP BY
  doctor_id,
  YEAR(admission_date)
```
