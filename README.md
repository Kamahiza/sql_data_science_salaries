**-- Tampilkan Semua Data **

SELECT * FROM ds_salaries;

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/eced27e5-591e-4acd-bb97-1dd474c8e93d)



**-- 1. Apakah ada daata yang NULL?**

SELECT * FROM ds_salaries 
WHERE work_year IS NULL;

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/601f213b-11fa-485f-9c51-d7c7a9f7749e)


**-- 2. Melihat ada apa saja di job title **

SELECT distinct job_title AS total_job_title 
FROM ds_salaries 
ORDER BY job_title;

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/3f4550b7-e0d3-49ad-a2c1-aa4006e53b60)


**-- 3. Job title apa saja yang berkaitan dengan data analyst**

SELECT distinct job_title 
FROM ds_salaries 
WHERE job_title LIKE '%Data Analyst%';

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/365e4160-b703-4146-b732-0597c26d25e8)



**-- 4. berapa rata rata gaji analyst **

SELECT AVG(salary) AS avg_salary 
FROM ds_salaries 
WHERE job_title LIKE '%Data Analyst%';

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/b3f76f5a-d1be-424c-a60d-d1db80ad2341)


**-- 5. berapa rata rata gaji analyst berdasarkan level experience**

SELECT experience_level, AVG(salary) AS avg_salary FROM ds_salaries 
WHERE job_title 
LIKE '%Data Analyst%' 
GROUP BY experience_level;

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/1d40830e-8d7a-4571-9f31-541f60bfdd6d)


**-- 5. berapa rata rata gaji analyst berdasarkan level experience dan Jenis employment nya **

SELECT experience_level, employment_type, AVG(salary) AS avg_salary FROM ds_salaries 
WHERE job_title 
LIKE '%Data Analyst%' 
GROUP BY experience_level, employment_type;

![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/30e3eff5-28d5-4c97-b04f-8ff603643824)


**-- 6. Negara dengan gaji yang menarik untuk posisi data analyst, Full Time, EXP nya entry dan menengah/ mid **

SELECT company_location, AVG(salary_in_usd) AS avg_salary_in_usd
fROM ds_salaries 
WHERE job_title LIKE '%data analyst%'
 AND employment_type = 'FT'
 AND experience_level IN ('MI', 'EN')
GROUP BY company_location
 HAVING avg_salary_in_usd >= 20000;
 
 ![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/aa546c5e-f6b6-446f-afaf-e9a0c2e24e84)

 
**-- 7. Ditahun berapa, kenaikan gaji dari mid ke senior itu memiliki kenaikan tertinggi 
-- ( untuk pekerjaan yang berkaitan dengan data analyst yang penuh waktu **


SELECT DISTINCT work_year FROM ds_salaries;

WITH ds_1 AS (
	SELECT
		work_year,
		AVG(salary_in_usd) sal_in_usd_ex
	FROM
		ds_salaries
	WHERE
		employment_type = 'FT'
		AND experience_level = 'EX'
		AND job_title LIKE '%data analyst%'
	GROUP BY
		work_year
),
ds_2 AS (
	SELECT
		work_year,
		AVG(salary_in_usd) sal_in_usd_mi
	FROM
		ds_salaries
	WHERE
		employment_type = 'FT'
		AND experience_level = 'MI'
		AND job_title LIKE '%data analyst%'
	GROUP BY
		work_year
),

t_year AS (
	SELECT
		DISTINCT work_year
	FROM
		ds_salaries
)
SELECT
	t_year.work_year,
	ds_1.sal_in_usd_ex,
	ds_2.sal_in_usd_mi,
	ds_1.sal_in_usd_ex - ds_2.sal_in_usd_mi differences
FROM
	t_year
	LEFT JOIN ds_1 ON ds_1.work_year = t_year.work_year
	LEFT JOIN ds_2 ON ds_2.work_year = t_year.work_year;
	
![image](https://github.com/Kamahiza/sql_data_science_salaries/assets/55770733/369624e5-6caf-4daa-b9f5-fcc1fb687e34)

